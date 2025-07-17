# Evaluation of Multimodal Generative AI for Esophageal Cancer Staging Using FDG-PET: Diagnostic Accuracy and Comparison with Physicians

🔬 This repository contains the Python code and methodology for the research study evaluating the performance of Large Language Models (LLMs) in staging esophageal cancer from FDG-PET Maximum Intensity Projection (MIP) images.
________________________________________
### ⚠️ Disclaimer: For Research Purposes Only
**This code is intended for research and educational purposes only. It is a proof-of-concept and has NOT been validated for clinical use. The model's performance, as detailed in the study, does not yet meet the standard of care. Do not use the output for actual medical diagnosis, patient care, or treatment planning.**
________________________________________
## Abstract

**Objective:** This study evaluated the diagnostic accuracy of large language models (LLMs) in staging esophageal cancer using FDG-PET images, focusing on their ability to assess lymph node (cN) and distant metastases (cM) for automated radiology reporting.

**Methods:** This retrospective study included 120 consecutive adult patients who were diagnosed with esophageal squamous cell carcinoma (SCC) and underwent FDG PET/CT at Tohoku University Hospital between January 2019 and December 2021. Patients with prior treatment, non-SCC histology, or blood glucose levels ≥200 mg/dL were excluded. Frontal maximum intensity projection (MIP) PET images were extracted, standardized, and analyzed alongside tumor location information. Five LLMs (ChatGPT-4.5, ChatGPT-4.1, OpenAI o3, o1, and ChatGPT-4 Turbo) and four blinded human evaluators (a nuclear medicine specialist, a gastrointestinal surgeon, and two radiology residents) assessed the presence of thoracic and abdominal lymph node metastases and determined cN and cM staging. Model analyses were performed via API in a zero-shot setting. Diagnostic agreement and accuracy were evaluated using Cohen’s Kappa, Cochran’s Q test, and post hoc McNemar tests with Holm–Bonferroni correction; significance was set at p < 0.05.

**Results:** The average accuracy was 39-77% for LLMs and 60-85% for physicians, with significantly higher accuracy for physicians in Thoracic LN, Abdominal LN, and cN-stage. Newer LLMs approached physician-level performance in identifying abdominal lymph node metastases and cM staging but showed weaker consistency for cN staging.

**Conclusion:** Though current LLMs do not yet rival physicians in comprehensive staging, recent models show promising potential for assisting in specific diagnostic tasks.
________________________________________
## How It Works

The script iterates through a series of PET MIP images. For each image, it performs the following steps:
  1.	**Encodes** the image into a base64 string.
  2.	**Determines** the primary tumor location (upper, middle, or lower esophagus) based on the case number. This information is hardcoded.
  3.	**Constructs a detailed prompt** for an OpenAI vision model (e.g., GPT-4.5). The prompt instructs the model to act as a radiology assessment tool and identify:
o	Thoracic lymph node metastases (TXN)
o	Abdominal lymph node metastases (AXN)
o	Distant metastases (MX)
4.	**Calculates** the final N-stage (NX) and M-stage (MX) based on the counts, following rules defined in the prompt.
5.	**Sends** the image and prompt to the OpenAI API and prints the structured output.
________________________________________
## Requirements
- A Google Account (the script is designed for Google Colab)
- Python 3.x
- An OpenAI API Key with access to GPT-4 vision models (gpt-4-turbo, gpt-4.5-preview, etc.).
- A dataset of PET MIP images.
________________________________________
## 🚀 Setup and Usage
This code is designed to run in a Google Colab environment.
### 1. Prepare Your Data
The script is hardcoded to work with a specific dataset structure.
  1.	In the root of your Google Drive, create a folder containing your PET MIP images.
  2.	The images must be named sequentially from 001.jpg to 120.jpg.
  3.	Modify the IMAGE_PATH1 variable in the code if your folder has a different name or path.
```
# Modify this path to match your folder in Google Drive
IMAGE_PATH1 = "/content/drive/MyDrive/Your_Image_Folder/" + str(i).zfill(3) + ".jpg"
```

### 2. Configure the Script
1.	Open the .ipynb file in Google Colab.
2.	**Set your API Key:** It is highly recommended to use Colab's "Secrets" feature (look for the key icon 🔑 in the left sidebar) to store your OPENAI_API_KEY. The script will access it via os.environ.
```
import os
from google.colab import userdata

API_KEY = userdata.get('OPENAI_API_KEY')
```

3.	Select the Model
Choose which OpenAI model to use by uncommenting the desired MODEL line.
```
# MODEL = "gpt-4.5-preview"
MODEL = "gpt-4-turbo-2024-04-09"
# ... and so on
```

### 3. Run the Analysis
  1.	Run the first cell to mount your Google Drive and grant permissions.
  2.	Run the subsequent cells to install the openai library and execute the analysis loop.
  3.	The output for each case will be printed to the console.
________________________________________
## Limitations
- **Dataset Specificity:** The code is tightly coupled to a dataset of 120 images with pre-defined tumor locations. To use with your own data, you must **manually modify the if/elif block** that assigns the position variable for each case.
- **Prompt-Based Logic:** The staging rules (e.g., how NX is calculated from lymph node counts) are defined within the text prompt sent to the LLM. This is not easily scalable or modifiable without editing the prompt itself.
- **Performance:** As noted in the results, the diagnostic accuracy of the LLMs, while promising, is currently inferior to that of human specialists.
________________________________________
## License
This project is licensed under the Apache License. See the LICENSE file for details.

