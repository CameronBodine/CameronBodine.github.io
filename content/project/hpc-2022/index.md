---
title: 'Flagging poor-quality scientific imagery in distributed memory'
authors:
- Cameron S. Bodine
date: '2022-05-06'
publishDate: '2023-11-26'
summary: 'Final project for CS 599: High-Performance Computing at Northern Arizona University'


tags:
- high performance computing
- distributed memory
- algorithms
- side-scan sonar
- C
featured: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
# image:
#   caption: 'Image credit: **Reich et al. (2017)**'
#   focal_point: ""
#   preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
# projects:
# - internal-project

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
# slides: example
---
*This is a final project for CS 599: High-Performance Computing at Northern Arizona University. The final script is shown below. The linked pdf contains the final project write-up.*

```
//mpic++ -I /scratch/csb67/HPC/Final/eigen -O3 CS599_project_csb67.cpp -lm -o CS599_project_csb67

//srun -n<# of Ranks> <Program> <IMAGE DIRECTORY> <# of Dimensions for clustering> <# of Clusters>
//srun -n 10 CS599_project_csb67 ./Images 2 10

#include <stdio.h>
#include <unistd.h>
#include <mpi.h>
#include <stdlib.h>
#include <math.h>
#include <string.h>
#include <dirent.h>
#include <iostream>

// Define / Includes for Image IO
#define STB_IMAGE_IMPLEMENTATION
#include "stb_image.h"

// Include for PCA with Eigen
#include <Eigen/Dense>
#include <Eigen/Eigenvalues>

using namespace Eigen;
using namespace std;

//function prototypes
double calcDist(int iA, double** datasetA, int iB, double ** datasetB, int DIM);


#define CHANNEL_NUM 1 // Number of image bands
#define KMEANSITERS 10 // Total kmeans iterations

int main(int argc, char **argv){

    int my_rank, nprocs;

    MPI_Init(&argc,&argv);
    MPI_Comm_rank(MPI_COMM_WORLD,&my_rank);
    MPI_Comm_size(MPI_COMM_WORLD,&nprocs);

    //Process command-line arguments
    char inD[500];
    int DIM;
    int KMEANS;

    strcpy(inD, argv[1]);
    sscanf(argv[2], "%d", &DIM);
    sscanf(argv[3], "%d", &KMEANS);

    // Variables
    int fileCnt=0, i=0;
    DIR *d;
    struct dirent *dir;

    int * buckets=(int*)malloc(sizeof(int)*(nprocs*2));
    int * myRange = (int*)malloc(sizeof(int)*(2)); //local lower/uper bucket range
    int myCount = 0; //Local image file count

    int * imgCluster; // store image cluster assignment
    int * globalClusterCnt = (int*)malloc(sizeof(int)*KMEANS); // store global count of points per cluster

    // Allocate array to store mean centers
    double ** meanCenters = (double**)malloc(sizeof(double*)*KMEANS);
    for (int i=0; i<KMEANS; i++){
        meanCenters[i]=(double*)malloc(sizeof(double)*DIM);
    }
    double * initCenters=(double*)malloc(sizeof(double)*(KMEANS*DIM)); // Store initial mean centers

    // For timers
    double start, end, totalTime, totalTimeMax;
    double allocS, allocE, allocTime, allocTimeMax;
    double pcaS, pcaE, pcaTime, pcaTimeMax;
    double kmeanS, kmeanE, kmeanTime, kmeanTimeMax;


    //Begin Timer
    start=MPI_Wtime();

    ////////////////////////////////////////////////////////////////////////
    // All ranks store filenames
    d = opendir(inD);

    while((dir=readdir(d)) != NULL){
        if(dir->d_type == DT_REG){
            fileCnt++;
        }
    }
    rewinddir(d);

    char * fileList[fileCnt];

    while((dir = readdir(d)) != NULL){
        if(dir->d_type == DT_REG){
            fileList[i] = (char*)malloc(strlen(dir->d_name)+1);
            strcpy (fileList[i], dir->d_name);
            i++;
        }
    }

    //Change directory to image directory
    chdir(inD);

    // Begin workload distribution timer
    allocS=MPI_Wtime();

    ////////////////////////////////////////////////////////////////////////////
    // Root determines total workload and assigns ~even work to each rank
    if (my_rank==0){
        printf("\nFile Count: %d\n", fileCnt);

        ////////////////////////////////////////////////////////////////////////
        // Determine File Size
        unsigned long totalSize=0;
        long * fileSize = (long*)malloc(sizeof(long)*fileCnt);
        for (int i=0; i<fileCnt; i++){

            FILE* fp = fopen(fileList[i], "rb");
            fseek(fp, 0L, SEEK_END);
            long int res = ftell(fp);
            totalSize += res;
            fileSize[i] = res;

            fclose(fp);
        }
        printf("Total Bytes: %d\n", totalSize);
        printf("Bytes per Processor: ~%d\n\n", totalSize/nprocs);

        ////////////////////////////////////////////////////////////////////////
        // Allocate the work ~evenly

        int start = 0, end = 0, rank = 0;
        unsigned long rankSize = 0; //Store current rank's workload in bytes

        for (int i = 0; i<fileCnt; i++){
            if ( (rankSize + fileSize[i]) < (totalSize/nprocs) ){
                rankSize += fileSize[i];
            }else{
                buckets[rank*2]=start;
                buckets[rank*2+1]=i;
                start=i+1;
                rank++;
                rankSize=0;
            }
        }

        // Assign leftovers to last rank
        buckets[(nprocs*2)-2] = (buckets[(nprocs*2)-3])+1;
        buckets[(nprocs*2)-1] = fileCnt-1;
    }

    ////////////////////////////////////////////////////////////////////////////
    // Broadcast bucket workload to all ranks
    MPI_Bcast(buckets, nprocs*2, MPI_INT, 0, MPI_COMM_WORLD);
    myRange[0] = buckets[my_rank*2]; // Store my lower bucket val
    myRange[1] = buckets[my_rank*2+1]; // Store my upper bucket val
    myCount = myRange[1] - myRange[0] + 1;

    // End workload distribution timer
    allocE=MPI_Wtime();

    //Begin PCA Timer
    pcaS=MPI_Wtime();

    ////////////////////////////////////////////////////////////////////////////
    // Iterate each image and calculate PC's

    // Store reduced values from data reduction with PC's
    double ** dataReduct = (double**)malloc(sizeof(double*)*myCount);
    for (int i=0; i<myCount; i++){
        dataReduct[i]=(double*)malloc(sizeof(double*)*DIM);
    }

    // Iterate each image
    for (int i=0; i<myCount; i++){
        int im_row, im_col, bpp;
        // Open the image
        uint8_t * image = stbi_load(fileList[i+myRange[0]], &im_col, &im_row, &bpp, CHANNEL_NUM);

        // Allocate matrix to store pixel values
        MatrixXd pixM(im_row, im_col);

        // Store pixel values
        int p = 0;
        for (int r=0; r<im_row; r++){
            for (int c=0; c<im_col; c++){
                pixM(r, c)=image[p];
                p++;
            }
        }

        // Normalize pixel values
        pixM.normalize();

        // Close image
        stbi_image_free(image);

        // Find the mean
        double M = pixM.mean();

        // Center values on the mean
        for (int r=0; r<im_row; r++){
            for (int c=0; c<im_col; c++){
                pixM(r,c)=pixM(r,c)-M;
            }
        }

        // Calculate variance-covariance matrix
        MatrixXd varCov = MatrixXd::Zero(im_row, im_row);
        for (int colA=0; colA<im_row; colA++){
            for (int colB=0; colB<im_col; colB++){
                varCov(colA, colB) = ( pixM.col(colA).cwiseProduct(pixM.col(colB)) ).sum() / (pixM.rows()-1);
            }
        }

        // Calculate eigenvectors
        SelfAdjointEigenSolver<MatrixXd> es(varCov);
        // EigenSolver<MatrixXd> es(pixM);

        // Store eigenvectors
        MatrixXd eig_v(im_row, DIM);
        for (int k=0; k<im_row; k++){
            for (int p=0; p<DIM; p++){
                eig_v(k,p)=es.eigenvectors().col(im_row-1-p)[k];
            }
        }

        // Data reduction with PC's
        VectorXd pixR = (pixM*eig_v).colwise().sum();

        // Store reduced dimensions
        for (int p=0; p<DIM; p++){
            dataReduct[i][p]=pixR[p];
        }


    }

    //End PCA Timer
    pcaE=MPI_Wtime();

    //Begin KMEAN Timer
    kmeanS=MPI_Wtime();

    ////////////////////////////////////////////////////////////////////////////
    // Prep for Kmeans clustering

    // Allocate memory for image cluster assignment
    imgCluster=(int*)malloc(sizeof(int*)*myCount);

    // Rank 0 initializes mean centers
    if (my_rank==0){
        // initCenters=(double*)malloc(sizeof(double)*(KMEANS*DIM));

        for (int k=0; k<KMEANS; k++){
            for (int d=0; d<DIM; d++){
                initCenters[(k*DIM)+d]=dataReduct[k][d];
            }
        }
    }

    // Broadcast initial mean centers
    MPI_Bcast(initCenters, (KMEANS*DIM), MPI_DOUBLE, 0, MPI_COMM_WORLD);

    // Store new mean centers
    for (int k = 0; k<KMEANS; k++){
        for (int d = 0; d<DIM; d++){
            double val = initCenters[(k*DIM)+d];
            meanCenters[k][d] = val;
        }
    }
    if (my_rank==0){
        printf("\n\n*******\nInitial\n*******\n");
        for (int k = 0; k<KMEANS; k++){
            printf("Cluster: %d | Centroid: (%f, %f)\n", k, meanCenters[k][0], meanCenters[k][1]);
        }
        printf("\n\n");
    }

    ////////////////////////////////////////////////////////////////////////////
    // Do K-Means clustering
    for (int kIters=0; kIters<KMEANSITERS; kIters++){ // Total Iterations
        for (int p=0; p<myCount; p++){ // Iterate each image
            double shortDist=-1;
            for (int k=0; k<KMEANS; k++){ // Iterate each kmean centroid

                // Calculate distance between point and cluster centroid
                double imgToClust = calcDist(p, dataReduct, k, meanCenters, DIM);

                // Store distance if it is less then the previous distance and update cluster assignment
                if ( (imgToClust < shortDist) || (shortDist == -1)){
                    shortDist = imgToClust; // Store distance
                    imgCluster[p] = k; // Store cluster assignment
                }
            }
        }
        if (kIters<KMEANSITERS-1){
            ////////////////////////////////////////////////////////////////////
            // Determine local number of points per cluster
            int * localClustCnt = (int*)calloc(KMEANS,sizeof(int)); // store local count of points per cluster

            for (int i = 0; i<myCount; i++){
                localClustCnt[imgCluster[i]]++;
            }

            ////////////////////////////////////////////////////////////////////////
            // Determine global number of points per cluster
            MPI_Reduce(localClustCnt, globalClusterCnt, KMEANS, MPI_INT, MPI_SUM, 0, MPI_COMM_WORLD);

            // Free local cluster count
            free(localClustCnt);

            // Update local points per cluster with global points per cluster
            MPI_Bcast(globalClusterCnt, KMEANS, MPI_INT, 0, MPI_COMM_WORLD);

            ////////////////////////////////////////////////////////////////////////
            // Calculate local weighted mean
            double * localCenters = (double*)malloc(sizeof(double)*KMEANS*DIM);
            double * newCenters = (double*)malloc(sizeof(double)*KMEANS*DIM);

            for (int k=0; k<KMEANS; k++){ // Iterate each cluster
                for (int d=0; d<DIM; d++){ // Iterate each dimension
                    double sum = 0; // Store sum of points in cluster
                    for (int i = 0; i<myCount; i++){ // Iterate each point
                        if (imgCluster[i] == k){ // If current point belongs to cluster k
                            sum += dataReduct[i][d]; // Add point value to sum
                        }
                    }
                    // To avoid divide by 0
                    if (globalClusterCnt[k] > 0){
                        localCenters[(k*DIM)+d] = sum/globalClusterCnt[k];
                    }else{
                        localCenters[(k*DIM)+d] = 0;
                    }
                }
            }

            // Sum each ranks weighted mean to get global mean centers
            MPI_Reduce(localCenters, newCenters, (KMEANS*DIM), MPI_DOUBLE, MPI_SUM, 0, MPI_COMM_WORLD);

            // Broadcast new mean centers
            MPI_Bcast(newCenters, (KMEANS*DIM), MPI_DOUBLE, 0, MPI_COMM_WORLD);

            // Store new mean centers
            for (int k = 0; k<KMEANS; k++){
                for (int d = 0; d<DIM; d++){
                    double val = newCenters[(k*DIM)+d];
                    meanCenters[k][d] = val;
                }
            }


            free(localCenters);
            free(newCenters);
        }
    }
    if (my_rank==0){
        printf("\n\n*****\nFINAL\n*****\n");
        for (int k = 0; k<KMEANS; k++){
            printf("Cluster: %d | Centroid: (%f, %f) | Count: %d\n", k, meanCenters[k][0], meanCenters[k][1], globalClusterCnt[k]);
        }
        printf("\n\n");
    }

    //End KMEAN Timer
    kmeanE=MPI_Wtime();

    ////////////////////////////////////////////////////////////////////////////
    // Timers
    // Workload distribution time
    allocTime= allocE-allocS;
    MPI_Reduce(&allocTime, &allocTimeMax, 1, MPI_DOUBLE, MPI_MAX, 0, MPI_COMM_WORLD);

    // PCA Time
    pcaTime = pcaE-pcaS;
    MPI_Reduce(&pcaTime, &pcaTimeMax, 1, MPI_DOUBLE, MPI_MAX, 0, MPI_COMM_WORLD);

    // Kmean Time
    kmeanTime = kmeanE-kmeanS;
    MPI_Reduce(&kmeanTime, &kmeanTimeMax, 1, MPI_DOUBLE, MPI_MAX, 0, MPI_COMM_WORLD);

    // Total Response Time
    end=MPI_Wtime();
    totalTime = end-start;
    MPI_Reduce(&totalTime, &totalTimeMax, 1, MPI_DOUBLE, MPI_MAX, 0, MPI_COMM_WORLD);

    if (my_rank==0){
        printf("\n\n******\nTIMERS\n******\n");
        printf("Total Response Time (s): %f\n", totalTimeMax);
        printf("Total Distribution Time (s): %f\n", allocTimeMax);
        printf("Total PCA Time (s): %f\n", pcaTimeMax);
        printf("Total KMEAN Time (s): %f\n", kmeanTimeMax);
        printf("\n\n");
    }

    ////////////////////////////////////////////////////////////////////////////
    // Free memory
    free(globalClusterCnt);
    for (int i =0; i<KMEANS; i++){
        free(meanCenters[i]);
    }
    free(meanCenters);
    free(myRange);
    free(buckets);
    free(imgCluster);

    MPI_Finalize();
    return 0;
}

// Function to calculate distance between two points
double calcDist(int iA, double** datasetA, int iB, double ** datasetB, int DIM){
    // iA & iB : row index of point in dataset
    double * pA = datasetA[iA];
    double * pB = datasetB[iB];

    double sqSum = 0;

    for (int i = 0; i<DIM; i++){
        sqSum += pow( (pA[i] - pB[i]), 2);
    }
    return sqrt(sqSum);
}

```
