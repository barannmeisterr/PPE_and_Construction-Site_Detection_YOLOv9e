# Human-Machine Distance Monitoring and PPE Checking System

##  Project Description

This is a computer vision project that uses YOLOv9-e model and checks if workers wear their **Personal Protective Equipment (PPE)** and if they keep a safe distance from work machines. It helps to make workplaces safer by watching videos and giving alerts when there is a dangerous situation.

The system can:
- Check if people wear PPE equipments..
- See if workers are too close to the construction heavy machines.
- Give live warnings and save logs for safety reports.

##  Author

- **Arda Baran**

## Datasets Used

 
Two separate image datasets are used for this project whose are “SH17” dataset for PPE detection and “ACID” dataset for operational machines in construction areas detection.”SH17” dataset includes images of 17 different objects related to person and PPE items.”ACID” dataset includes images of 10 different objects related to heavy machines in construction areas.  
The role of the “SH17” dataset is to check workers for proper PPE compliance by detecting whether they are correctly wearing essential safety equipment such as helmets, vests, gloves, and other protective items. By identifying PPE on individuals in real-time video feeds, the system can automatically verify if workers meet the required safety standards.  
Meanwhile, the “ACID” dataset is utilized to detect and recognize heavy construction machinery and vehicles. This allows the system to perform proximity analysis between workers and machinery, ensuring that safe distances are maintained to prevent accidents due to human-machine interactions.  
Thus, both datasets complement each other:  
• SH17 supports PPE violation detection to protect workers against environmental hazards,  
• ACID supports proximity violation detection to prevent machinery-related accidents.  
By combining insights from both datasets, the system creates a comprehensive safety monitoring solution that addresses both human-based (PPE) and environment-based (machinery) risks in construction and industrial sites.  
1) SH17 Dataset  
• Dataset Name and Source: SH17, Source: https://www.kaggle.com/datasets/mugheesahmad/sh17-dataset-for-ppe-detection                            
• Number of Samples and Features: 8099 samples of 17 different objects related to person and PPE items  
• Number of Images:8099
• Number of classes:17
• Type of Data: Image  
• Target Variable: Object Detection  
2) ACID Dataset  
• Dataset Name and Source: ACID, Source: https://universe.roboflow.com/test-blhxw/acid-dataset/dataset/1  
• Number of Samples and Features: 8927 samples(only train and val images are used) of 10 different objects related to heavy operational machines in construction areas.  
• Number of Images:8927
• Number of classes:10
• Type of Data: Image
• Target Variable: Object Detection

## Data Processing

My final dataset named "PPE_And_Construction_Datasets_Merged" dataset that used for training the model is the merge of two separate datasets: the "SH17" dataset for PPE and person and PPE-related items, and the "ACID" dataset for operational heavy machinery in construction areas.There are 17026 images after merging these two datasets.The most significant problem encountered was the presence of missing labels. In the "ACID" dataset, only heavy construction machines were labeled, while there were no annotations for persons, body parts, or PPE items and In the "SH17" dataset, only persons, body parts, or PPE items were labeled, while there were no annotations for heavy construction machines.This inconsistency created a major challenge for training a reliable object detection model, as the model could incorrectly learn background regions or treat unlabeled objects as negatives.To address this issue, I applied a three-step correction process:

To address this issue, I applied a three-step correction process:
1)Initial Detection using Official YOLOv9-e Model:I utilized the official pretrained YOLOv9-e model, which is highly accurate for detecting persons, body parts, and common PPE equipment. I applied it on the ACID dataset images to automatically detect missing persons and PPE-related objects that were originally unlabeled.
2)Label Correction: Based on the detections, I generated synthetic labels for the missing objects.If a person, helmet, or other PPE equipment was confidently detected but missing from the original annotations, I added them manually or programmatically. This ensured label consistency across the merged dataset.
3)Manual Review and Cleaning:After automatic correction, I performed a manual inspection on a subset of the corrected data to verify the quality of the generated labels.
Images that had uncertain detections or low-confidence corrections were either manually labeled or excluded from the training set to maintain high annotation quality.

By combining automatic detection, smart relabeling, and manual verification, I ensured that the merged dataset was properly balanced, representative, and contained minimal missing labels.
This ultimately improved the robustness and generalization ability of my object detection model, especially in real-world construction site scenarios where PPE compliance monitoring is critical. 

PPE_And_Construction_Datasets_Merged:

• Number of Images:17026
• Number of classes:27
• Types of Features: Image Data (Numerical pixel values)
• Target Variable :Object Detection
• Duplicate Entries: No

## Model Training and Results

I trained my model with Nvidia A100 40 GB GPU in Google Colab Pro+ environment by using train.py script during 200 epochs with special augmentations and albumenations and 17026 images in the "PPE_And_Construction_Datasets_Merged" dataset.I used this GPU in full capacity.I used YOLOv9 object detection model.I applied special augmentations and albumenations.The results are very satisfactory after training.
------------------------------------------------
| Highest mAP50            | Highest mAP50-95  |
|--------------------------|------------------ |
|  0.78587(Epoch 155)      | 0.60521(Epoch 153)|
------------------------------------------------

All results , trained model(best.pt) and metrics are stored in "Model Evaluation Metrics After Training" folder and "runs" folder.



## Used Tools

- **Ultraliytics YOLOv9e Model** for object detection  
- **Python Programming Language** for backend  
- **PyTorch** for real-time inference engine 
- **Firebase and JSON file structure** for saving logs and alerts  
- **Google Colab Pro+ Environment** for training model with A100 Nvidia GPU(40 GB) with cuda support.  

##  Features

- Object detection with bounded boxes
- Person,body parts and PPE eqipment detection on images and videos(Person,Body Parts,PPE Detection Module)
- Heavy construction vehicles detection on images and videos(Heavy Construction Vehicle Detection Module)
- Real-time alerts for missing PPE(PPE Violation Detection Module)
- Real-time alerts for unsafe distances between human and construction heavy machines(Proximity Violation Detection Module)
- Logs saved in JSON format(Violation Logging Module)



##  How to Use

1. Download "SH17 and "ACID" datasets from given sources above.
2. Copy all images from SH17 dataset(total 8099 images) to "PPE_and_Construction-Site_Detection_YOLOv9e\datasets\PPE_And_Construction_Datasets_Merged\images" folder.
3. Copy only train and val images from ACID dataset(1985 images from val folder + 6942 images from train folder)  to "PPE_and_Construction-Site_Detection_YOLOv9e\datasets\PPE_And_Construction_Datasets_Merged\images" folder.
4. extract the labels from labels.zip to "PPE_and_Construction-Site_Detection_YOLOv9e\datasets\PPE_And_Construction_Datasets_Merged\labels" folder.
5. Use scripts in the "PPE_and_Construction-Site_Detection_YOLOv9e\src" folder to perform operations.


