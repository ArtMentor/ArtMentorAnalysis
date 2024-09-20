<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; width: 100%;">
    <img src="./artmentor_logo.png" style="width: 60px; height: auto;"/>
    <p style="text-align: center; margin: 10px 0;"><strong>ArtMentor Data Processing and Analysis Repository</strong></p>
</div>

## Overview
This repository (**Data Analysis System**) contains datasets and data processing scripts used in the **ArtMentor** project. The project focuses on analyzing human-AI interaction during the review and evaluation of artworks. Below are detailed descriptions of the key files, folders, and their purposes. To access ArtMentor's app (**Data Collection System**), please click [here](https://github.com/ArtMentor/ArtMentorApp).
- **Paper**: [ArtMentor: AI-Assisted Evaluation of Artworks to Explore Multimodal Large Language Models Capabilities](#) (CHI 2025)
---
## `requirements.txt`
Lists all necessary Python packages for running the scripts in this repository, allowing users to set up the appropriate environment using pip.

## Contents
- [Usage](#Usage)
- [Folders](#Folders)
- [`.py` files](#py-files)
- [`.xlsx` files](#xlsx-files)


## Usage

1. **Install Required Python Packages**: Navigate to the `ArtMentorAnalysis` directory and run the following command to install all necessary Python packages:

   ```bash
   pip install -r requirements.txt
   ```
2. **Run Comprehensive Data Analysis**: Run artmentorAnalysis.py to perform a complete analysis of the data, and get the data for each of the entity recognition metrics (Accuracy, Precision, Recall, F1 Score), Artistic Style Sensitivity (ASS), Score Consistency (SC), Score Discrepancy (SD), as well as text similarity (TS) and modification rate (TMR).
## Folders

### 1. `.idea` and `__pycache__` Folder
These folders are automatically generated by the development environment (IDE) and contain project-specific settings and Python bytecode files. They are not directly related to the data processing and can be ignored.

### 2. `Dataset` Folder
This folder provides a more granular view of the user and AI interactions by storing data from each round of review and editing during an ArtMentor session. The dataset was collected from [ArtMentorApp](https://github.com/ArtMentor/ArtMentorApp). It has **three** subfolders:

- **Entities**: Stores records of entity labels recognized by the AI within the artworks. It tracks AI-generated labels, user-added labels, and user-removed labels, including special handling for "style" labels.
- **score_Review**: Records every round of scores and reviews, showing how both AI and user inputs evolved over time.
- **suggestions**: Similar to `score_Review`, but focuses on the suggestion dimension.

#### Detailed JSON Structure for `Dataset/Entities`:

```json
{
  "original": ["Face", "Black hair", "Open mouth", "Green shirt"],
  "added": ["Yellow balances", "schoolbag"],
  "removed": ["Yellow platform"],
  "style": {
    "original": ["Style: Cartoon"],
    "added": [],
    "removed": []
  }
}
```
- **`original`**: A list of entity labels initially identified by the AI.
- **`added`**: Labels added by the user during their review.
- **`removed`**: Labels removed by the user from the AI's original list.
- **`style`**: Special handling for style-related labels, tracking the original AI-detected style, any additions, and removals by the user.

#### Detailed JSON Structure for `Dataset/score_Review`:

```json
[
  {
    "round": 1,
    "data": {
      "scores": {
        "original": 0,
        "current": 0,
        "initGPTscore": null
      },
      "Reviews": {
        "original": "",
        "current": "",
        "added": "",
        "removed": ""
      }
    }
  },
  {
    "round": 2,
    "data": {
      "scores": {
        "original": 4,
        "current": 4,
        "initGPTscore": 4
      },
      "Reviews": {
        "original": "The artwork effectively uses contrasting colors...",
        "current": "The artwork effectively uses contrasting colors...",
        "added": "",
        "removed": ""
      }
    }
  }
]
```
- **`round`**: Indicates the iteration or round of interaction.
- **`data`**: Contains the same structure as individual files in the `Dataset/score_Review` folder, but captures the state at each round.
#### Detailed JSON Structure for `Dataset/suggestion`:
```
[
  {
    "round": 1,
    "data": {
      "suggestions": {
        "original": "",
        "current": "",
        "added": "",
        "removed": ""
      }
    }
  },
  {
    "round": 2,
    "data": {
      "suggestions": {
        "original": "To improve the color contrast, consider using more vibrant colors...",
        "current": "To improve the color contrast, consider using more vibrant colors...",
        "added": "",
        "removed": ""
      }
    }
  },
  {
    "round": 3,
    "data": {
      "suggestions": {
        "original": "To improve the color contrast, consider using more vibrant colors...",
        "current": "To enhance the contrast, use more vibrant colors and stronger outlines...",
        "added": "and stronger outlines",
        "removed": "consider"
      }
    }
  }
]
```
- **`round`**: Indicates the iteration or round of interaction.
- **`data`**: Contains the same structure as individual files in the `Dataset/suggestion` folder, but captures the state at each round.

## `.py` files
### 1. `StyleAnalysis.py`
This script calculates the **Art Style Sensitivity (ASS)** metric by analyzing the `style` dimension in the JSON files located in the `Dataset/Entities` folder. The results are saved to `ASS Results.xlsx`.

### 2. `EntityAnalysis.py`
A Python script that processes the JSON files in the `Dataset/Entities` folder to compute various metrics for entity recognition, including **Accuracy**, **Precision**, **Recall**, and **F1 Score**. The results are saved in `Entity Results.xlsx`.

### 4. `ScoreAnalysis.py`
Calculates the **Score Consistency (SC)** and **Score Difference (SD)** by analyzing `original` and `current` scores from the JSON files in the `Dataset/score_Review` folder.

### 5. `TextAnalysis.py`
This script analyzes the **Text Modification Rate (TMR)** and **Text Similarity (TS)** by examining the `original`, `current`, `added`, and `removed` fields in the `Reviews` and `suggestions` JSON files from the `Dataset` folder.

### 6. `artmentorAnalysis.py`
The central script for running comprehensive data analysis in the ArtMentor project. It integrates all other analysis scripts (`EntityAnalysis.py`, `ScoreAnalysis.py`, `StyleAnalysis.py`, `TextAnalysis.py`) to produce final results.

## `Results/ .xlsx` files
### 1. `ASS Results.xlsx`
This Excel file stores the results of the Art Style Sensitivity (ASS) analysis, showing whether the AI correctly identified the art style of each artwork. The file contains two columns.
- **File Name**: The name of the JSON file corresponding to the artwork.
- **Correct Recognition (1=Correct, 0=Incorrect)**: Indicates if the AI's style recognition was accurate.

### 2. `Entity Results.xlsx`
Stores the output of `EntityAnalysis.py`, listing metrics such as **Accuracy**, **Precision**, **Recall**, and **F1 Score** for each artwork.

### 3. `SC_SD Results.xlsx`
Records the **Score Consistency (SC)** and **Score Difference (SD)** for each dimension of the artwork.

### 4. `SC_SD Sequences.xlsx`
Captures the sequence of scores (`original` and `current`) for each dimension and artwork during each round of interaction. This is useful for tracking how scores evolved over time.

### 5-6. `TMR Results_rev.xlsx` and `TMR Results_sug.xlsx`
Contains the **Text Modification Rate (TMR)** for each review and suggestion in each dimension, measuring how much the text was modified by the user compared to the AI's original output.

### 6-8. `TS_Results_rev.xlsx` and `TS_Results_sug.xlsx`
Records the **Text Similarity (TS)** for each review and suggestion in each dimension, indicating how similar the final user-edited text is to the original AI-generated text.
