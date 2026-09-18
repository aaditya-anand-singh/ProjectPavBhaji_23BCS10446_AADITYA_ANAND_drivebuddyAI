Instagram Pav-Bhaji Classification

A machine learning project for classifying Instagram posts as Pav-Bhaji or Not Pav-Bhaji using textual metadata such as captions and hashtags.

Project Overview

The objective of this project is to determine whether an Instagram image belongs to the Pav-Bhaji category without directly using image pixels or CNN-based visual features.

Instead, the model uses the textual metadata associated with each Instagram post:

Captions
Tags / Hashtags

The task is formulated as a binary text classification problem.
Dataset

The provided JSON dataset contains:

1,500 Instagram posts
18 metadata fields

After matching the Instagram metadata with the supplied labeled image folders, 452 labeled samples were obtained.

Class	Label	Samples
Not Pav-Bhaji	0	269
Pav-Bhaji	1	183
Total		452

The original dataset contains more posts than the final supervised dataset because only posts that could be successfully matched with the supplied labels were used for model training and evaluation.
Features Used

The model uses:

Caption + Tags

Other metadata fields such as likes, comments, timestamps, URLs, image dimensions, and video information were not used as direct predictive features.

Text Preprocessing

The textual data was cleaned using the following steps:

Convert text to lowercase.
Remove URLs.
Remove user mentions.
Remove the # symbol from hashtags.
Remove punctuation and unnecessary special characters.
Normalize whitespace.
Preserve Unicode characters.

The cleaned caption and tag information was combined into a single text representation.

Feature Extraction

The cleaned text was converted into numerical features using TF-IDF (Term Frequency-Inverse Document Frequency).

Configuration:

N-gram range:       (1, 2)
Maximum features:   2,000
Minimum document frequency: 2
Stop words:         English

Both individual words and two-word combinations were considered.

Models Evaluated

Three machine learning algorithms were evaluated:

Logistic Regression
Linear Support Vector Machine
Multinomial Naive Bayes
Model Performance

Results on the 80:20 stratified test split:

Model	Accuracy	Precision	Recall	F1-Score
Logistic Regression	64.84%	54.72%	78.38%	64.44%
Linear SVM	60.44%	51.02%	67.57%	58.14%
Multinomial Naive Bayes	63.74%	53.70%	78.38%	63.74%

For this test split, Logistic Regression achieved the highest accuracy and F1-score among the evaluated models.
Cross-Validation

Because only 452 labeled samples were available, 5-fold stratified cross-validation was also performed.

Metric	Mean	Standard Deviation
Accuracy	68.13%	4.44%
Precision	58.15%	4.33%
Recall	76.46%	6.50%
F1-Score	65.98%	4.75%

The cross-validation results provide a more stable performance estimate than relying on a single train-test split.

Final Model

The final classification pipeline is:

Instagram Caption
        +
Instagram Tags
        ↓
Text Preprocessing
        ↓
TF-IDF
        ↓
Logistic Regression
        ↓
Prediction
        ↓
Pav-Bhaji / Not Pav-Bhaji
Final Configuration
Input:              Caption + Tags
Feature Extraction: TF-IDF
N-grams:            Unigrams + Bigrams
Maximum Features:   2,000
Classifier:         Logistic Regression
Class Weight:       Balanced
Maximum Iterations: 1,000
Random State:       42
Confusion Matrix

The final Logistic Regression model produced the following confusion matrix on the test set:

                    Predicted
                 Not Pav-Bhaji   Pav-Bhaji

Actual
Not Pav-Bhaji          30            24

Pav-Bhaji               8            29

Therefore:

True Negatives  = 30
False Positives = 24
False Negatives = 8
True Positives  = 29

The model correctly classified 59 out of 91 test samples, resulting in an accuracy of 64.84%.

Feature Analysis

The Logistic Regression coefficients were analyzed to understand the textual patterns learned by the model.

Examples of features associated with the Pav-Bhaji class include:

pavbhaji
foodlover
foodgasm
pav
monsoon
homemade
vegetables
mumbaifood
butter
bhaji
street
homecooking

Examples of features associated with the Not Pav-Bhaji class include:

fondue
vadapav
bhelpuri
panipuri
pizza
dosa
masaladosa
gobimanchurian
chicken
golgappa

These features represent statistical associations learned from this particular dataset and should not be interpreted as universal rules.

Prediction

The final notebook includes a reusable prediction function that accepts a caption and tags and returns the predicted class and predicted probability.

Example:

Input:
Hot homemade pav bhaji with butter and vegetables

Prediction:
Pav-Bhaji

Predicted probability:
70.99%

Another example:

Input:
Delicious chicken tikka with spicy sauce

Prediction:
Not Pav-Bhaji

Predicted probability:
63.92%
Limitations
Small Labeled Dataset

Although the JSON contains 1,500 posts, only 452 posts could be successfully matched with the supplied labels.

Noisy Metadata

Instagram captions and hashtags are user-generated and may contain multiple food names or information unrelated to the actual image.

Collection-Related Bias

The term pavbhaji was one of the strongest features learned by the model. Since the dataset was collected around Pav-Bhaji-related content, this may introduce a collection-related signal.

No Visual Information

The model does not analyze the image itself. It relies entirely on textual metadata.

Therefore, an image containing Pav-Bhaji but having unrelated or insufficient metadata may be classified incorrectly.

Generalization

The reported results are specific to the supplied dataset and experimental setup. Performance on completely independent Instagram data may differ.

Future Improvements

Possible improvements include:

Increasing the size and diversity of the labeled dataset.
Improving label quality through manual verification.
Using word or sentence embeddings.
Experimenting with transformer-based text representations.
Exploring character-level TF-IDF features.
Combining textual features with visual image features.
Evaluating the final system on an independent external test dataset.
Repository Structure
pavbhaji-text-classification/
│
├── PavBhaji_Classification.ipynb
├── PavBhaji_Classification_Report.pdf
├── README.md
└── requirements.txt

The dataset is not included in this repository. The notebook should be configured with the appropriate dataset path before execution.

Technologies Used
Python
Pandas
NumPy
Scikit-learn
Matplotlib
Google Colab
GitHub
How to Run
1. Open the Notebook

Open:

PavBhaji_Classification.ipynb

in Google Colab.

2. Provide the Dataset

Place the dataset in the expected Google Drive directory and update the
dataset_path variable if required.

3. Run the Notebook

Execute the notebook cells sequentially.

The notebook performs:

Data Loading
    ↓
Label Matching
    ↓
Text Extraction
    ↓
Preprocessing
    ↓
TF-IDF
    ↓
Model Training
    ↓
Evaluation
    ↓
Prediction
Author

Aaditya Anand

Computer Science and Engineering
Chandigarh University
