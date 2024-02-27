# SRIP-24
## This is the Submission for the Selection Task of the Project - *ML for Sustainability: Satellite Data Processing for Detecting Pollution Sources*
### A. Dataset Preparation: 
  The dataset contained the total number of 90 classes where each class possessed image samples of the animal which the class is comprised.
  Moreover, the first task was to organize the Dataset of 90 classes into a One vs Rest dataset for the purpose of Binary Classification.                           
  Here, One vs Rest is a type of multiclass classification schemes built on top of real-valued binary classifiers is to train N different binary classifiers, each one trained to distinguish the examples in a single class from the examples in all remaining classes [^1]

  Secondly, the next task was to organize the Dataset of 90 total classes into 5 condensed classes, where the 90 class classification problem would be turned into 5 class classification problem.

  All of this was done with the help of [dataset_controller](dataset_controller.ipynb), where the 90 classes were organised for One vs Rest classification which created a binary classification problem for all the 90 different classes, where the primary class consisted the image samples of the animal the folder is comprised of while the other class contained all the other images samples of the remaining 89 classes. Upon further analysis, it is found that the primary class would only possess 60 image samples whereas the other(rest) class have 5340 image samples, This creates a huge class imbalance [^2]
Hence, to solve the problem of class imbalance, I augmented the samples of Primary class 30 times. i.e For every sample we create 30 different augmented sample, this artificially increases the samples approximately to 1800. At the same time, we select 20 samples from each remaining class so that we have somewhat equal amount of training samples in both the 'Primary' and 'rest' class.

Furthermore, I decided to have different ecological niche like Terrestrial, Aquatic, Aerial, Arboreal and Burrowing as the Classification Schema for condensing and organizing 90 different classes into 5 classes[^3]

### B. Model Development:
  Here, the input shape is set to be (128,128) and 3 color channels. Further, the samples are loaded with a Batch size of 32, the Learning rate is set to 1e-3. Also, when training for OvR the model is compiled with the 'adam' optimizer and _binary_crossentropy_ [^4] as the classification loss. Where as, when training for 5 class classification the model is compiled with  _categorical_crossentropy_ [^5] as the classification loss

The model consists of 3 Convolution layers followed by 3 Fully Connected dense layers as classifier. The Convolution layers have activation function as 'Relu' (Rectified Linear Unit). Morevoer the last dense layer possesses 'softmax' as activation function. Each Convolution layer consists of a Maxpooling2D and BatchNormalization followed by a Dropout layer(p = 0.25)

![Architecture of Custom CNN model](https://github.com/Abeey04/SRIP-24/blob/main/Visualize_Convolution_layer/Architecture%20of%20Custom%20CNN%20model.png?raw=true)

### C. Training and Evaluation:
  The K Fold Cross-validation is done with the help of sklearn library where we evaluate the model 3 times (different model instances) with 3 different splits of shuffled Training data. 

During each fold the classification metrics of the model is calculated and recorded and saved into the .csv file. Moreover, Confusion Matrix to visualize the perfomance of the model is computed and saved for each fold.

For One vs Rest : [Classification_report_of_all_OvR_classes](https://github.com/Abeey04/SRIP-24/blob/5066b114f224951d31403c142cca726d3fd87986/One%20vs%20Rest%20Classification/Classification_report_of_all_OvR_classes.csv)

For 5 class classification : [Classification_report_of_5_class_classification](https://github.com/Abeey04/SRIP-24/blob/5066b114f224951d31403c142cca726d3fd87986/5_class_classification/Classification_report_of_5_class_classification.csv)

### D. Convolutional Layer Visualization:
  The activation maps, called feature maps, capture the result of applying the filters to input, such as the input image or another feature map.

The idea of visualizing a feature map for a specific input image would be to understand what features of the input are detected or preserved in the feature maps. The expectation would be that the feature maps close to the input detect small or fine-grained detail, whereas feature maps close to the output of the model capture more general features.

The Custom CNN model's Convolution layers are :
![Conv1](https://github.com/Abeey04/SRIP-24/blob/82e2b0a9d54d16a4261cf267dace131146b0d518/Visualize_Convolution_layer/conv1.png)

![Conv2](https://github.com/Abeey04/SRIP-24/blob/82e2b0a9d54d16a4261cf267dace131146b0d518/Visualize_Convolution_layer/conv2.png)

![Conv3](https://github.com/Abeey04/SRIP-24/blob/82e2b0a9d54d16a4261cf267dace131146b0d518/Visualize_Convolution_layer/conv3.png)

We can see that the feature maps closer to the input of the model capture a lot of fine detail in the image and that as we progress deeper into the model, the feature maps show less and less detail.

This pattern was to be expected, as the model abstracts the features from the image into more general concepts that can be used to make a classification. Although it is not clear from the final image, we generally lose the ability to interpret these deeper feature maps.

  [^1]: [Rifkin, Ryan & Klautau, Aldebaro. (2004). In Defense of One-Vs-All Classification. Journal of Machine Learning Research. 5. 101-141.](https://www.researchgate.net/publication/220320940_In_Defense_of_One-Vs-All_Classification)
  [^2]: [Johnson, J.M., Khoshgoftaar, T.M. Survey on deep learning with class imbalance. J Big Data 6, 27 (2019).](https://doi.org/10.1186/s40537-019-0192-5)
  [^3]: [A Comprehensive Guide to Classifying and Understanding Animal Anatomy, Habitats, Communication, Behavior and Survival Strategies](https://www.scribd.com/document/246340404/Classification-of-Animals-Based-on-Their-Habitat) 
  [^4]: [Ruby, Usha & Yendapalli, Vamsidhar. (2020). Binary cross entropy with deep learning technique for Image classification. International Journal of Advanced Trends in Computer Science and Engineering. 9. 10.30534/ijatcse/2020/175942020.](https://www.researchgate.net/publication/344854379_Binary_cross_entropy_with_deep_learning_technique_for_Image_classification)
  [^5]: [Generalized Cross Entropy Loss for Training Deep Neural Networks with Noisy Labels](https://doi.org/10.48550/arXiv.1805.07836)

