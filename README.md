# Change-Detection-in-Remote-Sensing-Images

#### Important used packages and their versions
   - numpy==1.19.4
   - opencv-python==4.4.0.46
   - scikit-learn==0.23.2
   - scipy==1.1.0
   - matplotlib==3.3.3
   - notebook==6.1.5
   - python==3.7.9
   
   
We will be outlining an unsupervised method for change detection. It involves the automatic analysis of the change data, i.e. the **difference image**, constructed using the **multi temporal images**. A difference image is the pixel-by-pixel subtraction of the 2 images. **Eigen vectors** of pixel blocks from the difference image will then be extracted by **Principal Component Analysis** (PCA). Subsequently, a feature vector is constructed for each pixel in the difference image by projecting that pixel’s neighbourhood onto the Eigen vectors. The feature vector space, which is the collection of the feature vectors for all the pixels, upon clustering by K-means algorithm gives us two clusters – one representing pixels belonging to the changed class, and other representing pixels belonging to the unchanged class. Each pixel will belong to either of the clusters and hence a **change map** can be generated. So, the steps towards implementing this application are:

    1)Difference image generation and Eigen vector space (EVS)
    2)Building the feature vector space (FVS)
    
    3)Clustering of the feature vector space and change map
   
### 1. Difference image and the Eigen vector space

In this method,a non-overlapping blocks are taken  of size 5 x 5 from the difference image and flatten them into row vectors. The image can be resized to make both the dimensions a multiple of 5 by **scipy.misc.imresize()**. Collection of these row vectors forms a vector set. In change_detection.py script, find_vector_set() does exactly this. If the size of our difference image is m x n, then the number of rows in the vector set would be  **{m x n}/{5 x 5} .**

### 2. Building the feature vector space

Building the FVS involves again taking 5 x 5 blocks from the difference image, flattening them, and lastly projecting them onto the EVS, only this time, the blocks will be overlapping. A vector space (VS) is first made by constructing one vector for each pixel of the difference image such a way that one 5 x 5 block is actually a pixel’s 5 x 5 neighborhood. It is to be noted here that by this logic, 4 boundary rows and 4 boundary columns pixels won’t get any feature vectors since they won’t have a 5 x 5 neighborhood. (We can manage with this exclusion of these pixels, since it is safe to assume here that any changes occurring would be concentrated in the middle regions of the images, rather than the edges). So, we will have **(m x n)- 8** feature vectors in the FVS, all 25 dimensional. Projecting the FVS to the **25 dimensional EVS** simply means to perform the following matrix multiplication

(VS)((m x n - 8) x 25) .(EVS)(25 x 25) = (FVS)(m x n - 8) x 25

Function **find_FVS()** determines the feature vector space for us. The function is similar to find_vector_set(), but extracts overlapping blocks from the difference image.

### 3. Clustering of the feature vector space, and change map

The feature vectors for the pixels carry information whether the pixels have characteristics of a changed pixel or an unchanged one. Having constructed the feature vector space, we now need to cluster it so that the pixels can be grouped into two disjoint classes. K-means algorithm is used to do that. Thus each pixel will get assigned to a cluster in such a way that the distance between the cluster’s mean vector and the pixel’s feature vector is the least. Each pixel gets a label from 1 to K, which denotes the cluster number that they belong to.


