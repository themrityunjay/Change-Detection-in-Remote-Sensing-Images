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
   
