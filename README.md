# Directories_PL

This project streamlines the process of extracting and geolocating address information from scanned historical directory books. By leveraging both traditional OCR (Tesseract) and a cutting-edge AI model (Gemini AI), it offers a robust solution for transforming these valuable historical documents into structured, geographically tagged data. 


The resulting text is transformed into a dataframe with an address on every line and geolocated using the GeoPy library and Nominatim geocoder. 

The scripts should be run in the following order:
### 1. Directories
This script creates the necessary folders in the directory

### 2. 00_Cropped_images
Crop images to minimise the noise and improve OCR performance

### 3. 01_Pre-Process
The pre_process script contains three major functions to transform PDF images into new images with less noise, easier to interpret by OCR models.
The images provided are previously cropped in the 00_Cropping_image script.

* Function 1 pre-processes the image
* Function 2 defines the boundaries
* Function 3 reads the OCR

There's a final function, which uses the three previous functions as consecutive steps and loops through all pages of a PDF document to extract all the possible information.

After this script there are two potential ways of running the OCR model. 
* Option 1: Run the "02_Post-Processing" script and then "03_Geolocation" script.
* Option 2: Run the "02_AI_OCR" script

### 4.a. 02_Post-Processing + 03_Geolocation

The Post_Process script takes the csv files created in the "01_Pre_process" script and shapes the addresses with the desired form, it corrects recurrent misspellings and mistakes from the tesseract model. To obtain relatively correct outcomes, this script has required considerable time to adjust the shape and correct for misspellings and other mistakes from the OCR.

This script has been deprecated after changing the approach to the one used in the script "02_AI_OCR". The use of the AI model to read and shape the pdf images follows the structure of  the paper "MULTIMODAL LLMS FOR OCR, OCR POST-CORRECTION, AND NAMED ENTITY RECOGNITION IN HISTORICAL DOCUMENTS" from Gavin Greif, Niclas Griesshaber and Robin Greif and has drastically reduced time in human correction.

### 4.b. 02_AI_OCR

This file follows the idea from the paper "MULTIMODAL LLMS FOR OCR, OCR POST-CORRECTION, AND NAMED ENTITY RECOGNITION IN HISTORICAL DOCUMENTS" from Gavin Greif, Niclas Griesshaber and Robin Greif.
GIT - https://github.com/niclasgriesshaber/gemini_historical_dataset_pipeline/tree/main

Using the same prompt from the paper as template and styling it to our project needs, I feed pre-processed images to the ai model, which returns a json file. Then, a second prompt is used to enforce the desired format of the final text. The result is a csv file with addresses.

The addresses which found are later golocated using geopy free open source geocoding from OpenStreetMap: Nominatim.


### 5. Control
Finally, the control file can be run to look into the performance of the model and provide basic statistics. 


