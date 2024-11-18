# Pineapple Plantation Classification with Unet Model and Sentinel-2 Data 
Sage McGinley-Smith \
Mordecai Lab, Stanford University\
CS 191W Fall 2024

### Project Background
This is a project that I began working on last spring quarter, advised by Aly Singleton and Caroline Glidden in the Mordecai Lab. The project goal is to use 2019 maps of pineapple plantations in Costa Rica to generate maps of the plantation distribution for 2020-present based on Sentinel-2 satellite data for those years. In the spring, I built a random forest model that was able to classify the satellite images with reasonable accuracy. However, further research and reviews of the literature revealed that a better approach for such a project would be to leverage deep learning models to do more accurate image segmentation. 

### Deep Learning vs. Random Forest 
Through this project, I have tested multiple machine learning models on this question. The logistic regression baseline, which represents the most basic version of a deep learning model, performs poorly on subsets of the data, with recall of only 0.1, meaning only 10% of the pixels that were pineapple plantations were actually classiified as plantations in the test set. A brief example of this linear baseline can be found [here](https://colab.research.google.com/drive/15PRkwwH_VYkhsdaJLXnFg37ElMhEaYo9?usp=sharing). Random forest is a more advanced machine learning model, capable of capturing non-linear patterns in the data due to it's decision tree structure. As previously mentioned, Random Forest performed reasonably well for this project, but the results were not on par with the creation of a scientifically useful dataset. More details about that project and the Random forest implementation can be found [here](https://github.com/sagems/pineapple_classification). It is important to note that both the Random Forest model and the Logistic regression model use pixel-based classification, meaning they classify each pixel individually using the input features, without consideration for the classification of the surrounding pixels.

By contrast, deep learning models are advanced machine learning systems that use neural networks to process and analyze data. A simple neaural network essentially consists of series of logistic regression layers with non-linear activation functions between each layer. For some further intuition about the structure of basic neural networks, I reccomend watching this [video](https://www.youtube.com/watch?v=aircAruvnKk&t=1003s). In a neural network, each layer extracts increasingly complex features from the input, enabling tasks like image recognition, natural language processing, and more. These models excel at handling large, unstructured datasets, with different kinds of models working better on different kinds of tasks. For image processing in particular, a specific kind of network called a Convolutional Neural Net (or CNN) is used. A Convolutional Neural Network (CNN) is a type of deep learning model designed to process grid-like data, such as images. It uses convolutional layers (2 and 3 dimensional filters) to automatically detect patterns like edges or textures by applying the filters to scan over the input. These patterns are then combined in deeper layers to recognize more complex features, making CNNs particularly effective for image classification, segmentation, and object detection tasks. 


### Data Collection and GCP Set Up

### Useful Resources
Through research process for this project, I found many valuable resources and met with researchers across the realms of spatial data and deep learning. I've attached a document with the most relevant notes from those meetings, and have linked several useful resources below. 

- [Consultation Notes](https://docs.google.com/document/d/1puVxFoWywQZErhmyTF4738MD0djCUVpMdM4hhl3lGUM/edit?usp=sharing)
- [Github Repository on Satellite Data + Deep Learning](https://github.com/satellite-image-deep-learning)
- [Colab notebook walking through a basic classification process](https://colab.research.google.com/github/climatechange-ai-tutorials/aquaculture-mapping/blob/main/Aquaculture_Mapping_Detecting_and_Classifying_Aquaculture_Ponds_using_Deep_Learning.ipynb#scrollTo=rSRCNgYzUwaf)
- [Vertex AI Guides](https://developers.google.com/earth-engine/guides/ml_examples)
- [Guide to Earth Engine and PyTorch CNN (Pixel-Based)](https://colab.research.google.com/github/google/earthengine-community/blob/master/guides/linked/Earth_Engine_PyTorch_Vertex_AI.ipynb)


### Addendum: Palm Classification
The palm plantation data that exists for this project 
