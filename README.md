# Pineapple Plantation Classification with U-Net Model and Sentinel-2 Data  

**Sage McGinley-Smith**  
*Mordecai Lab, Stanford University*  
*CS 191W Fall 2024*  

---

## Project Background  
This project, initiated last spring quarter under the guidance of Aly Singleton and Caroline Glidden in the Mordecai Lab, aims to generate maps of pineapple plantation distributions in Costa Rica for 2020 onward using Sentinel-2 satellite data. The starting point was a 2019 map of plantations. Initially, I developed a random forest model that classified satellite images with reasonable accuracy. However, subsequent literature reviews and discussions highlighted the potential of deep learning models to achieve more precise image segmentation.  

---

## Machine Learning Approaches: Deep Learning vs. Random Forest  

### Baseline and Random Forest Models  
In exploring methods for satellite image segmentation, I tested several machine learning approaches. The logistic regression baseline performed poorly, achieving a recall of only 0.1—correctly identifying just 10% of the plantation pixels in the test set. A demonstration of this simple model is available [here](https://colab.research.google.com/drive/15PRkwwH_VYkhsdaJLXnFg37ElMhEaYo9?usp=sharing).  

Random forest, a more advanced machine learning model, showed promise. Its decision tree structure enables it to capture non-linear patterns, outperforming the baseline model. However, the results were still insufficient for generating scientifically valuable datasets. For further details about the random forest implementation, visit the project's [GitHub repository](https://github.com/sagems/pineapple_classification). Both the logistic regression and random forest models use a pixel-based classification approach, evaluating each pixel independently of its neighbors.  

### Deep Learning and U-Net Models  
Deep learning models, such as convolutional neural networks (CNNs), excel at image-based tasks by recognizing spatial patterns and features within images. A CNN processes data through layers of filters that detect features like edges or textures, which are then combined in deeper layers to identify complex patterns. For image segmentation tasks, U-Net models are particularly effective.  

The U-Net architecture performs segmentation by first "zooming out," increasing feature richness through a contracting path, and then "zooming in" through an expanding path to generate the final segmentation mask. This binary mask distinguishes plantation from non-plantation areas. For an introduction to CNNs and segmentation concepts, I recommend this [video series](https://www.youtube.com/watch?v=ArPaAX_PhIs&list=PLkDaE6sCZn6Gl29AoE31iwdVwSG-KnDzF&index=1).  

For this project, I used TensorFlow with Keras for model development, based on recommendations from experts and my deep learning coursework. While PyTorch is another viable framework, TensorFlow’s abstraction of implementation details like backpropagation makes it well-suited for this work.  

---

## Data Collection and Google Cloud Platform (GCP) Setup  

Using Google Earth Engine (GEE), I created NDVI composites to produce cloud-free images of Costa Rica for 2020 onward. I selected four regions with representative topography and a mix of plantation and non-plantation cover. The regions were exported with the following features:  

- **Sentinel-2 bands:** red, green, blue, near-infrared, and short-wave infrared  
- **Indices:** NDVI, NDWI, SAVI, and NDMI  

Plantation locations were rasterized to create binary mask labels. GEE scripts for this process can be found [here](https://code.earthengine.google.com/0d678008835c1601629c868fcc5240a1).  

The images and masks were stored in a Google Cloud bucket, organized by quadrant. To prepare for model training, I used Rasterio in Google Colab to chip the images into 128 x 128 tiles, which were then uploaded to the GCP bucket. Code for this step is available in the `Data_Preparation_For_UNet_Model.ipynb` notebook. Proper GCP authentication, as detailed in the `GCP_Authentication_Instructions` image, is essential for accessing the bucket.  

---

## Model Implementation  

Once the data was stored in GCP, it was retrieved in Colab for model training. The U-Net model architecture was implemented with TensorFlow/Keras, with the option to initialize weights from a pre-trained model for faster convergence. Transfer learning, where the model's final layers are retrained on the new dataset, was not explored in this project but offers potential for future research.  

For a basic outline of the U-Net model implementation, see the notebook [here](https://colab.research.google.com/drive/1HhO45GIW1zwEXomkEq-9NaHphisHwpDP?usp=sharing).  

---

## Useful Resources  

Throughout this project, I benefited from numerous resources and consultations with experts in spatial data and deep learning. Below are some key references:  

- [Consultation Notes](https://docs.google.com/document/d/1puVxFoWywQZErhmyTF4738MD0djCUVpMdM4hhl3lGUM/edit?usp=sharing)  
- [Github Repository on Satellite Data + Deep Learning](https://github.com/satellite-image-deep-learning)  
- [Colab Notebook: Basic Classification Process](https://colab.research.google.com/github/climatechange-ai-tutorials/aquaculture-mapping/blob/main/Aquaculture_Mapping_Detecting_and_Classifying_Aquaculture_Ponds_using_Deep_Learning.ipynb#scrollTo=rSRCNgYzUwaf)  
- [Vertex AI Guides](https://developers.google.com/earth-engine/guides/ml_examples)  
- [Earth Engine + PyTorch CNN Guide](https://colab.research.google.com/github/google/earthengine-community/blob/master/guides/linked/Earth_Engine_PyTorch_Vertex_AI.ipynb)  
