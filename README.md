## Organization

This repository includes two main folders: 

* **ml_folder**: containing the source-code of our main experiments;
* **preprocessing_folder:** containing the code of our feature extractor and some attacks;

In the root folder of this repository, we have also provided a “requirements.txt” file, specifying which Python libraries will be used to carry out all our experiments. Moreover, we also provided a document ("get_data.md") explaining how to retrieve the data for our experiments. This artifact entirely runs on CPU.

In what follows, we will first provide a high-level overview of the data folder, and then explain how to use the corresponding code for a practical evaluation.



### data_folder

#### Preliminary Information

Let us provide some essential background information for those who are not experts in the specific problem tackled by our paper.

* **Sources.** Our paper entails experiments carried out on two datasets: [DeltaPhish](https://link.springer.com/chapter/10.1007/978-3-319-66402-6_22) and [Zenodo](https://dl.acm.org/doi/abs/10.1145/3465481.3470112) (both of which are publicly available), containing “raw data” of webpages (benign or phishing). For transparency, we include in this repository all such “raw data”, which will be deleted after the review of the artifact (to avoid potential copyright violations). We will, however, maintain the preprocessed version of each webpage. 
* **Attacks**. Our paper entails “adversarial attacks against machine learning”, whose basic principle is to (i) take a sample, (ii) manipulate such sample in some way, and (iii) assess whether the “adversarial sample” evades a given ML model or not. Specifically in our case, we consider a total of 12 adversarial attacks, meaning that we artificially create 12 “adversarial variants” of each “original sample” (i.e., a phishing webpage). Some of these variants are created “at runtime”, whereas the others are created “in advance” (we did this by manually manipulating each raw sample). 

#### Structure

The folder is organized depending on the dataset(Zenodo or Deltaphish), the format (raw or preprocessed). Let us explain both of these:

* **raw:** this folder contains the “original” data as well as the adversarial variants of each sample.
	* normal. This folder contains information on the “original” webpages. It contains a JSON file with the URLs of each sample; and an “HTML” folder containing the raw HTML of each sample
	* wa. This folder refers to the “cheap” attacks considered in our paper. It contains files including the HTML of phishing webpages after applying the “cheap” HTML manipulation.
	* wa+. This folder refers to (a subset of) the “advanced” attacks of our paper. It contains files including the HTML of phishing webpages after applying the “advanced” HTML manipulations.
* **Preprocessed:** this folder includes data describing the “preprocessed” format of each sample in the “raw” folder after the application of our custom-built feature extractor.
	* normal. This folder contains a single JSON file describing the feature representation of each “normal” sample (benign and phishing)
	* wa and wa+. These folders contain three subfolders ("u, r, c") each referring to a specific variant of our wa/wa+ attacks. Each subfolder has a single JSON file, which contains the feature representation of each (phishing) sample after applying adversarial manipulation.
	* phish_sub_test_x_100.pkl. This is a “pickle” file including the 100 samples used as basis for our our adversarial attacks in the preprocessing space. 

### preprocessing_folder

This folder contains 4 files:

* extractor.py: this python script analyzes a sample (a phishing webpage) and extracts its feature representation. 
* feature_extraction.ipynb: is a (small) notebook showcasing the application of extractor.py on a single sample.
* PA_PSP.ipynb: is a notebook that applies the perturbations related to the preprocessing attacks considered in our paper.

### ml_folder

This folder contains 4 files, which refer to the experiments performed on the “DeltaPhish” dataset:

* ML.py, containing some custom-defined functions for developing our ML models and printing their results
* RF/CN/LR_experiments.ipynb, which are notebooks containing the experiments for each of the 3 main ML algorithms considered in our paper (RF=random forest, LR=logistic regression, CN=convolutional neural network).




## INSTRUCTIONS

Let us explain how to use our artifact.

1. **Get the data, and install requirements**. This is self explanatory; we recommend creating an ad-hoc virtual environment for this purpose (PyCharm works very well). Important: the data_folder should be placed in the root directory!
2. **Test the feature extractor.** Simply run the preprocessing_folder/feature_extraction.ipynb notebook once. It should prove that the feature extractor “works”.
3. **Create the adversarial samples**
   * Simply run the preprocessing_folder/PA_PSP.ipynb_ once.
4. **Test the attacks.** Consider any of the three notebooks (e.g., “ml_folder/experiments_RF.ipynb”) and run all of its cells. The LR and RF do not take long to train, whereas the CN can take several minutes (the runtime on our platform is provided in the documentation). Every cell reports the part in the paper in which the corresponding result is “shown”. Due to randomness, the results can differ from those in the paper (which are provided just as the average and std. dev.)