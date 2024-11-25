# Satellite Image Segmentation with U-Net Model and Sentinel-2 Data  

**Sage McGinley-Smith**  
*Mordecai Lab, Stanford University*  
*CS 191W Fall 2024*  

This GitHub is intended as a resource for implementing U-Net binary image segmentation of Sentinel 2 satellite imagery. It uses the example of classifying pineapple plantations in Costa Rica as demonstration of the pipeline and modeling decisions. Below is a brief outline of the contents of this document, which contain implementation instructions as well as background and context and useful resources. This project was completed as a senior project in the Mordecai Lab at Stanford University. It is organized as follows: 
- Section 1: Implementation Instructions
- Section 2: Project Context and Background
- Section 3: Why Deep Learning + U-Net Explanation
- Section 4: Resources and Future Directions

---

## Section 1: Implementation Instructions
| Step and Platform       | Explanation                                                                                                                                                     | Code                                                                 |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------|
| **Data Export in Google Earth Engine** | Follow the GEE code script to obtain an NDVI composite of Sentinel 2 imagery. The script rasterizes the label layer and exports it separately from the Sentinel layer, for which it exports red, green, blue, near-infrared, and short-wave infrared bands, as well as NDVI, NDWI, SAVI, and NDMI indices. It is important that you export these nine bands in order for the model to receive proper dimension images. If desired, select different geographic boundaries for export, different time interval for compositing, or different spatial resolution. Ensure the mask and all bands of the image are exported at the same resolution (script should do this automatically but is a good check). Export to desired folder in Drive. | [GEE Code](https://code.earthengine.google.com/0d678008835c1601629c686fc5240a1) |
| **Set up GCP bucket structure** | Set up a GCP bucket by going to the GCP Dashboard and creating a new bucket. Create one folder called `sentinel-images` and one called `mask-images`. Within each folder, create a folder for each quadrant. Finally, create an authentication method and key using the instructions in the `GCP_Authentication_Instructions` image. | [GCP How-To?](#) |
| **Tile the images and store in GCP** | Use the Colab Notebook to chip up the Sentinel images into size 128 (height) × 128 (width) × 9 (bands) and the label images into size 128 × 128 × 1 (classification layer). Make sure that the GCP account, the Drive account where the images are stored, and the account executing the notebook are all the same. This step may take a while depending on how large your quadrants are. | [Colab Notebook](https://colab.research.google.com/drive/1HhO45GlW1zwEXomkEq) |
| **Split data into test, train, and dev folders.** |                                                                                                                                                                 |                                                                      |
| **Build, Train, and Export Model** | Use the Colab Notebook to build, train, and export a U-Net model. The notebook contains comments and instructions that explain how to make changes to training parameters and process as well as model structure, if desired. | [Colab Notebook](https://colab.research.google.com/drive/1HhO45GlW1zwEXomkEq) |

---

## Useful Resources  
Through the research process for this project, I found many valuable resources and met with researchers across the realms of spatial data and deep learning. I've attached a document with the most relevant notes from those meetings, and have linked several useful resources below. 

- [Consultation Notes](https://docs.google.com/document/d/1puVxFoWywQZErhmyTF4738MD0djCUVpMdM4hhl3lGUM/edit?usp=sharing)
- [Github Repository on Satellite Data + Deep Learning](https://github.com/satellite-image-deep-learning)
- [Colab notebook walking through a basic classification process](https://colab.research.google.com/github/climatechange-ai-tutorials/aquaculture-mapping/blob/main/Aquaculture_Mapping_Detecting_and_Classifying_Aquaculture_Ponds_using_Deep_Learning.ipynb#scrollTo=rSRCNgYzUwaf)
- [Vertex AI Guides](https://developers.google.com/earth-engine/guides/ml_examples)
- [Guide to Earth Engine and PyTorch CNN (Pixel-Based)](https://colab.research.google.com/github/google/earthengine-community/blob/master/guides/linked/Earth_Engine_PyTorch_Vertex_AI.ipynb)
