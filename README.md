# Pineapple Plantation Classification with U-Net Model and Sentinel-2 Data  

**Sage McGinley-Smith**  
*Mordecai Lab, Stanford University*  
*CS 191W Fall 2024*  

---

## Project Background  
This is a project that I began working on last spring quarter, advised by Aly Singleton and Caroline Glidden in the Mordecai Lab. The project goal is to use 2019 maps of pineapple plantations in Costa Rica to generate maps of the plantation distribution for 2020-present based on Sentinel-2 satellite data for those years. In the spring, I built a random forest model that was able to classify the satellite images with reasonable accuracy. However, further research and reviews of the literature revealed that a better approach for such a project would be to leverage deep learning models to do more accurate image segmentation. 

---

## Machine Learning Approaches: Deep Learning vs. Random Forest  

### Baseline and Random Forest Models  
Through this project, I have tested multiple machine learning models on the question of satellite image segmentation. The logistic regression baseline, which represents the most basic version of a deep learning model, performs poorly on subsets of the data, with recall of only 0.1, meaning only 10% of the pixels that were pineapple plantations were actually classiified as plantations in the test set. A brief example of this linear baseline can be found [here](https://colab.research.google.com/drive/15PRkwwH_VYkhsdaJLXnFg37ElMhEaYo9?usp=sharing). Random forest is a more advanced machine learning model, capable of capturing non-linear patterns in the data due to it's decision tree structure. As previously mentioned, Random Forest performed reasonably well for this project, but the results were not on par with the creation of a scientifically useful dataset. More details about that project and the Random forest implementation can be found [here](https://github.com/sagems/pineapple_classification). It is important to note that both the Random Forest model and the Logistic regression model use pixel-based classification, meaning they classify each pixel individually using the input features, without consideration for the classification of the surrounding pixels.

### Deep Learning and U-Net Models  
By contrast, deep learning models are advanced machine learning systems that use neural networks to process and analyze data. A simple neaural network essentially consists of series of logistic regression layers with non-linear activation functions between each layer. For some further intuition about the structure of basic neural networks, I reccomend watching this [video](https://www.youtube.com/watch?v=aircAruvnKk&t=1003s). In a neural network, each layer extracts increasingly complex features from the input, enabling tasks like image recognition, natural language processing, and more. These models excel at handling large, unstructured datasets, with different kinds of models working better on different kinds of tasks. For image processing in particular, a specific kind of network called a Convolutional Neural Net (or CNN) is used. A Convolutional Neural Network (CNN) is a type of deep learning model designed to process grid-like data, such as images. It uses convolutional layers (2 and 3 dimensional filters) to automatically detect patterns like edges or textures by applying the filters to scan over the input. These patterns are then combined in deeper layers to recognize more complex features, making CNNs particularly effective for image classification, segmentation, and object detection tasks. 

Although there is much more to say about deep learning and CNNs, for this particular project, it is most important to understand the U-Net model. The U-Net model is a kind of CNN that is often used to identify and outline specific regions in pictures, such as tissues in biomedical images, or pineapple plantations in satellite images. It works by first "zooming out" and expanding the images to have more channels, and then "zooming in" to output a final mask segmenting the image into different classifications. In the case of this project, the final mask image is binary, representing areas classified as pineapple plantations and non-pineapple plantations. I find this [youtube series](https://www.youtube.com/watch?v=ArPaAX_PhIs&list=PLkDaE6sCZn6Gl29AoE31iwdVwSG-KnDzF&index=1), which is the basis for the deep learning class I took this fall, very useful in explaining the concept of a CNN as it applies to image segmentation.

In terms of modeling platforms, most of the experts I talked to, as well as the intructors in my deep learning class, reccommended using the keras library with TensorFlow for building models. Tensorflow is ideal for building models in Python because it abstracts away many of the complicated implementatio details of a machine learning algorithm (backpropogation, input manipulation, etc). PyTorch is a similar platform that does essentially the same thing as TensorFlow, and is preferred by some experts in the field, however I chose to use TensorFlow when building my models. 

---

## Data Collection and Google Cloud Platform (GCP) Setup  

I used an NDVI composite in Google Earth Engine to get a relatively cloud-free image for my model. I then selected four quadrants in different areas of the country to take data from. I chose these quadrants because they had cloud-free imagery, contained representative samples of topography, and had a mix of plantation and non-plantation cover. The shapefiles for the quadrants can be found in the shapefiles folder. I exported the Sentinel-2 imagery for each of the regions with red, green, blue, near-infrared, and short-wave infrared bands, as well as NDVI, NDWI, SAVI, and NDMI indices. Finally, I rasterized the feature collection containing the plantation locations and exported the masks of the regions to use as labels in the model. The full Google Earth Engine code can be found [here](https://code.earthengine.google.com/0d678008835c1601629c868fcc5240a1).

Once the full images were exported, I set up a Google Cloud bucket for storage and made a folder for each quadrant for both the Sentinel images and the mask images. I then used the rasterio library and the Google Drive and GCP integration features of Colab to chip the larger images into 128 x 128 chips, which I then exported into the folders in my bucket. The code for this step can be found in the Data_Preparation_For_UNet_Model.ipynb file. One important note for this step of the process is to properly authenticate for access to the GCP bucket. It is a straightforward process but essential for this part of the code to work. Instruction for this step can be found in the GCP_Authentication_Instructions image.

---

## Model Implementation  

Once the data is in the GCP buckets with a consistent naming scheme, it can then be pulled back into Colab to train a model. It is useful, if possible, to initialize the weights of the model based on a previous model, as that will allow for faster training. It is also possible to apply transfer learning with this kind of problem, which is essentially retraining the final layers of a model on new training data. Although this project did not focus on transfer learning, it is something to consider for the future. The basic code outline for the model can be found [here](https://colab.research.google.com/drive/1HhO45GIW1zwEXomkEq-9NaHphisHwpDP?usp=sharing).

---

## Useful Resources  
Through the research process for this project, I found many valuable resources and met with researchers across the realms of spatial data and deep learning. I've attached a document with the most relevant notes from those meetings, and have linked several useful resources below. 

- [Consultation Notes](https://docs.google.com/document/d/1puVxFoWywQZErhmyTF4738MD0djCUVpMdM4hhl3lGUM/edit?usp=sharing)
- [Github Repository on Satellite Data + Deep Learning](https://github.com/satellite-image-deep-learning)
- [Colab notebook walking through a basic classification process](https://colab.research.google.com/github/climatechange-ai-tutorials/aquaculture-mapping/blob/main/Aquaculture_Mapping_Detecting_and_Classifying_Aquaculture_Ponds_using_Deep_Learning.ipynb#scrollTo=rSRCNgYzUwaf)
- [Vertex AI Guides](https://developers.google.com/earth-engine/guides/ml_examples)
- [Guide to Earth Engine and PyTorch CNN (Pixel-Based)](https://colab.research.google.com/github/google/earthengine-community/blob/master/guides/linked/Earth_Engine_PyTorch_Vertex_AI.ipynb)
