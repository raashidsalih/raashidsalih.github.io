+++
title = 'Modelling COVID Mortality Using Chest X-Rays'
date = 2023-09-07T17:04:00+05:30
draft = false
tags = ["Project"]
+++

{{< github repo="raashidsalih/COVID-Mortality" >}}

This repository contains a ResNet model and an associated Gradio-based interface that can predict the mortality risk of COVID-19 patients based on their chest X-ray images. The model was trained using two datasets of chest X-ray images of COVID-19 patients, one from the [IEEE8023 public open dataset](https://github.com/ieee8023/covid-chestxray-dataset) and another from [The Cancer Imaging Archive (TCIA) repository](https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=89096912). The model achieves a decent accuracy of 86% along with high sensitivity. Please check the repository Readme for more information.

![Interface Preview](https://github.com/raashidsalih/COVID-Mortality/blob/main/assets/Interface.png)
You can try out the model [here](https://huggingface.co/spaces/raashidsalih/COVID-Mortality). You can also run the model locally by either:
1. Using the associated DockerFile
2. Using virtual environment in conjunction with requirements.txt

## Data

The data used to train the model consists of two datasets of chest X-ray images of COVID-19 patients:

- **IEEE8023 dataset**: This dataset was obtained from a [public GitHub repository](https://github.com/ieee8023/covid-chestxray-dataset) that contains chest X-ray and CT images of COVID-19 cases, as well as other diseases. The survival labels were obtained from the metadata provided in the repository, while the occasional missing labels were imputed using a Random Forest Classifier that was trained on the other available metadata describing the patients’ medical conditions.
- **Stony Brook University (SBU) dataset**: This dataset was retrieved and transformed from [The Cancer Imaging Archive (TCIA) repository](https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=89096912) that contains chest X-ray images of COVID-19 patients collected by the Stony Brook Medicine Hospital. The associated survival labels were obtained from the clinical reports provided in the repository. This dataset is rather rich relative to the previous one, however, the images are in the DICOM format which makes it difficult to work with especially due to the large size. The images were transformed to the PNG format along with some other alterations to make them viable for this use case and the results are [publicly hosted here](https://www.kaggle.com/datasets/toxite/covid-19-cxr-ny-sbu).

## Model

The model used to predict the mortality risk of COVID-19 patients is ResNet18 which is based on the convolutional neural network (CNN) architecture. The model was trained in two iterations:

- **First iteration**: The model was trained on the IEEE8023 dataset, with class weight adjustment to overcome class imbalance.
- **Second iteration**: The model was fine-tuned on the SBU dataset and achieved an accuracy of 86% on a test set created from a portion of the SBU dataset.

## Interface

The model is deployed for demonstration purposes and external testing. Gradio was used for the front-end interface, while Huggingface Spaces was used for hosting and deployment.

The interface provides the user with the ability to upload their own image to get predictions from the model. Besides that, one can also visualize the activation layer as an overlay to see which features of the image led to the prediction.

The interface can be accessed [through this link](https://huggingface.co/spaces/raashidsalih/COVID-Mortality).

## References

**IEEE 8023 Data Citation**
> COVID-19 Image Data Collection: Prospective Predictions Are the Future
Joseph Paul Cohen and Paul Morrison and Lan Dao and Karsten Roth and Tim Q Duong and Marzyeh Ghassemi
arXiv:2006.11988, <https://github.com/ieee8023/covid-chestxray-dataset>, 2020

**TCIA Data Citation**

> Saltz, J., Saltz, M., Prasanna, P., Moffitt, R., Hajagos, J., Bremer, E., Balsamo, J., & Kurc, T. (2021). Stony Brook University COVID-19 Positive Cases [Data set]. The Cancer Imaging Archive.  [https://doi.org/10.7937/TCIA.BBAG-2923](https://doi.org/10.7937/TCIA.BBAG-2923)

**TCIA Citation**
> Clark K, Vendt B, Smith K, Freymann J, Kirby J, Koppel P, Moore S, Phillips S, Maffitt D, Pringle M, Tarbox L, Prior F. The Cancer Imaging Archive (TCIA): Maintaining and Operating a Public Information Repository, Journal of Digital Imaging, Volume 26, Number 6, December, 2013, pp 1045-1057. DOI: 10.1007/s10278-013-9622-7
