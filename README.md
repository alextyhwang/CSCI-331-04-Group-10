# Traffic Sign Detection System
We are building a Traffic Sign Detection System using computer vision + machine learning

## Abstract:

A comparative study of U-Net and Mask R-CNN architectures for traffic sign detection. This repository hosts a full machine learning pipeline—from data preprocessing to evaluation—benchmarked on the Kaggle Car Detection dataset using IoU, F1-Score, and standard segmentation metrics.


**Objectives:**
* Create system that takes images of streets with vehicles and detects traffic signs in them
* Build full Machine learning pipeline (data prep, model training, evaluation, and analysis)
* Compare two different image segmentation models (U-Net vs Mask R-CNN)

**Contributors:** 

1. Ferdinand Virtudes
- Developed Project Roadmap and delegated tasks
- Researched kaggle dataset
- Implemented MASK R-CNN model
- Implemented U-NET model
- U-NET/Mask R-CNN preprocessing from Kaggle dataset
- Conducted experiments (K-fold, hyperparameter tuning, final test evaluation) 
- Created base presentation template
- Prepared presentation 

## How It's Made:

**Data used:** https://www.kaggle.com/datasets/pkdarabi/cardetection/data

**Tech used:** Python, Google Colab

# How it works

1. The U-Net Model (The "U" Shape)
<details>
<summary> We built this using an Encoder-Decoder architecture with Skip Connections. </summary>
Encoder (Downsampling): The left side of the "U". It takes the image and shrinks it down, extracting high-level features (like shapes and edges) while throwing away spatial details. Think of this like summarizing a book into bullet points.
</details>

2. The Tiny Mask R-CNN (The Custom Lightweight) 
<details> 
<summary>Instead of using the massive, standard Mask R-CNN (which is huge and slow), We built a "Tiny" custom version optimized for the M1 chip. </summary> 
Backbone: We used a simple series of Convolutional layers to look at the image and find "features" (is there a wheel? is there a window?).
RPN (Region Proposal Network) Logic: In a full R-CNN, this suggests where objects are. In the Tiny version, We implemented a simplified branch that focuses the network on the area where the car likely exists before trying to draw the mask.
Mask Head: The final layers that output the binary (black and white) mask.

</details>

3. How We Trained Them

<details>
<summary>
We preprocessed the data to suit the two models and experimented with K-fold cross validation and Hyperparameter tuning </summary>
The Data: We started with car images and YOLO text files (which are just bounding boxes). Then we wrote a script to convert those boxes into Binary Masks (black background, white square where the car is). We resized everything to 256x256 pixels and 128x128 for Mask R-CNN so the Mac could handle the math quickly.

The Process (5-Fold Cross-Validation):

We didn't just train it once. We used a technique called 5-Fold Cross-Validation.
* We cut the data into 5 equal pieces.
* We trained the model on 4 pieces and tested it on the 5th.
* We repeated this 5 times, rotating which piece was the "test" piece.

Hyperparameter Tuning: We ran a grid search to find the best settings.
* Learning Rate: We tested 0.001 vs 0.0005. (Do We try to learn fast and messy, or slow and careful?)
* Batch Size: We tested 16 vs 32. (Do We look at 16 images at a time, or 32?)
</details>

4. How We Evaluated Them

<details>
<summary> To grade the models, We used IoU (Intersection over Union) as the main score. </summary>

The Math: IoU = Area of Union / Area of Overlap
​	
In Simple Terms:
* Intersection: The area where the model and the answer key both agreed "this is a car."
* Union: The total area covered by either the model or the answer key.
* The Score: If the model draws a box perfectly over the car, the score is 1.0 (100%). If We draw a box on a tree instead of the car, the score is 0.0 (0%).

Other Metrics We used:

* Precision: When We predicted a pixel was a car, how often was We right? (Avoiding false alarms).
* Recall: Did We find all the car pixels, or did We miss some? (Avoiding misses).

</details>

## Lessons Learned:
* M1 Mac limitations for running Mask R-CNN: had to reduce batch size and learning rate to accomodate




