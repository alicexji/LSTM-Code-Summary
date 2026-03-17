# LSTM Code Summarization

## Overview

This project implements an LSTM-based sequence-to-sequence model that generates natural language summaries for Java methods. The model takes a Java method as input and produces a short textual description of the method's functionality.


## Setup and Reproduction

The notebook is designed to run in **Google Colab** and uses Google Drive for accessing the provided resources and storing outputs.

### 1. Mount Google Drive

At the beginning of the notebook, Google Drive is mounted:

```python
from google.colab import drive
drive.mount('/content/drive')
```
This allows the notebook to access datasets and store output files.

### 2. Required Folder Structure

Two directories must exist in Google Drive before running the notebook.

1. Shared Resources Directory

This directory contains the files provided with the assignment, including:
- get_codet5_embeddings.py
- requirements.txt
- sample datasets
- test dataset

The notebook expects these resources at the path:
```
/content/drive/MyDrive/Assignment-2/
```
2. Working Directory
** **ACTION** Create an empty folder in Google Drive named: genai_assignment2_alice
This folder will be used to store all generated outputs, including:
- tokenized datasets
- model checkpoints
- generated predictions
- evaluation results

The notebook accesses this folder through:
```
/content/drive/MyDrive/genai_assignment2_alice/
```

3. Install Dependencies
The notebook installs the required Python packages using:
```
!pip install transformers==4.37.2
!pip install sentence-transformers==2.6.1
!pip install peft==0.8.2
!pip install accelerate
```

Additional dependencies are listed in requirements.txt and installed using
```
!pip install -r {SHARED_DIR}requirements.txt
```


### 3. Running the Notebook
Once Google Drive is mounted and the directory structure is set up, the notebook can be executed top-to-bottom.
The notebook performs the following steps:
- Load training and validation datasets
- Generate CodeT5+ embeddings
- Train the LSTM encoder–decoder model
- Save the best model checkpoint
- Generate summaries for the test set
- Evaluate predictions using BLEU, METEOR, BERTScore, and SIDE metrics

Enable GPU Runtime (Recommended)

Training and inference are significantly faster when using a GPU in Google Colab.

To enable a GPU runtime:

1. Open the notebook in Google Colab.
2. Click **Runtime → Change runtime type**.
3. Set **Hardware accelerator** to **GPU**.
4. Click **Save**.

The notebook automatically detects whether a GPU is available and will work regardless of CPU or GPU

## Output Files

All generated data and intermediate files are written to the working directory:
```
/content/drive/MyDrive/genai_assignment2_alice/
```
The notebook writes several intermediate and final outputs during execution.
1. Cloned GitHub Repositories:
   When mining Java repositories, the notebook clones repositories into:
   ```
   genai_assignment2_alice/repos/
   ```
   These repositories are used to extract Java source files.

2. List of Java Files:
   After scanning repositories, a JSON file containing all discovered Java files is saved to:
   ```
   genai_assignment2_alice/java_files.json
   ```
   This allows the notebook to reload the file list without rescanning repositories.
3. Extracted Code–Summary Pairs:
   During extraction of (method, summary) pairs, checkpoint files are periodically written to avoid losing progress:
   ```
   genai_assignment2_alice/code_summary_pairs_partial.json
   ```
   After extraction completes, the final dataset is saved as:
   ```
   genai_assignment2_alice/code_summary_pairs.json
   ```
4. Train / Validation Dataset Files:
   After preprocessing and filtering, the dataset is split into training and validation sets. The following files are written to the working directory:
   ```
   genai_assignment2_alice/train_code.txt
   genai_assignment2_alice/train_summary.txt
   genai_assignment2_alice/val_code.txt
   genai_assignment2_alice/val_summary.txt
   ```
5. Tokenized Dataset and Embeddings:
   The script get_codet5_embeddings.py converts the dataset into token IDs and generates a pretrained embedding matrix. The resulting files are saved as:
   ```
   genai_assignment2_alice/train_code.pt
   genai_assignment2_alice/train_summary.pt
   genai_assignment2_alice/val_code.pt
   genai_assignment2_alice/val_summary.pt
   ```
   These .pt files are loaded by the LSTM model during training.
6. Model Checkpoint:
   During training, the model checkpoint is saved to:
   ```
   genai_assignment2_alice/lstm_checkpoint.pt
   ```
   This file contains the trained model weights and is used for evaluation and summary generation.
7. Generated Summaries and Evaluation Inputs:
   After generating summaries for the test dataset, the notebook writes the following files:
   ```
   predictions.txt
   references.txt
   code.txt
   ```

   They are used as input for the evaluation metrics.

8. Evaluation Metrics:
   The notebook prints the final evaluation scores directly in the notebook output, including:
   - BLEU-1, BLEU-2, BLEU-3, BLEU-4
   - METEOR
   - BERTScore
   - SIDE score

  These summary statistics appear in the final cells of the notebook.

  ### Outputs
  The repository has an outputs folder where I've placed some txt output files from the most recent local run for reference. To see all the outputs, run the full notebook and view in google drive.
   
