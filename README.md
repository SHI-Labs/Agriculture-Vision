# Agriculture-Vision

Agriculture-Vision Dataset, Challenge and Workshop (CVPR 2020)

## Content

- [Papers](#papers)
- [Challenge](#challenge)
    * [Agriculture-Vision Challenge dataset](#agriculture-vision-challenge-dataset)
    * [Evaluation metric](#evaluation-metric)
    * [Submission](#submission)
    * [Methods](#methods)
    * [Leaderboard](#leaderboard)
- [Workshop](#workshop)
    * [Paper track](#paper-track)
    * [Topic description](#topic-description)
    * [IMPORTANT DATES](#important-dates)

## Papers:
CVPR paper on Agriculture-Vision
```BibTex
@article{chiu2020agriculture,
  title={Agriculture-Vision: A Large Aerial Image Database for Agricultural Pattern Analysis},
  author={Chiu, Mang Tik and Xu, Xingqian and Wei, Yunchao and Huang, Zilong and Schwing, Alexander and Brunner, Robert and Khachatrian, Hrant and Karapetyan, Hovnatan and Dozier, Ivan and Rose, Greg and others},
  journal={arXiv preprint arXiv:2001.01306},
  year={2020}
}
```
CVPR Workshop paper on the 1st Agriculture-Vision Challenge (challenge dataset, methods and results)
```BibTex
@article{chiu20201st,
  title={The 1st Agriculture-Vision Challenge: Methods and Results},
  author={Chiu, Mang Tik and Xu, Xingqian and Wang, Kai and Hobbs, Jennifer and Hovakimyan, Naira and Huang, Thomas S. and Shi, Honghui and others},
  journal={arXiv preprint arXiv:2004.09754},
  year={2020}
}
```

## Challenge
### Agriculture-Vision Challenge Dataset

<img src="images/cs.gif" width="300" title="Cloud shadow"> <img src="images/dp.gif" width="300" title="Double plant"> <img src="images/ps.gif" width="300" title="Planter skip">
<img src="images/sw.gif" width="300" title="Standing water"> <img src="images/ww.gif" width="300" title="Waterway"> <img src="images/wc.gif" width="300" title="Weed cluster">

The dataset used in this challenge is a subset of the [Agriculture-Vision dataset](https://arxiv.org/abs/2001.01306). The challenge dataset contains 21,061 aerial farmland images captured throughout 2019 across the US. Each image consists of four 512x512 color channels, which are RGB and Near Infra-red (NIR). Each image also has a boundary map and a mask. The boundary map indicates the region of the farmland, and the mask indicates valid pixels in the image. Regions outside of either the boundary map or the mask are not evaluated.

This dataset contains six types of annotations: Cloud shadow, Double plant, Planter skip, Standing Water, Waterway and Weed cluster. These types of field anomalies have great impacts on the potential yield of farmlands, therefore it is extremely important to accurately locate them. In the Agriculture-Vision dataset, these six patterns are stored separately as binary masks due to potential overlaps between patterns. Users are free to decide how to use these annotations.

Each field image has a file name in the format of (**field id**)_(**x1**)-(**y1**)-(**x2**)-(**y2**).(**jpg**/**png**). Each field id uniquely identifies the farmland that the image is cropped from, and (x1, y1, x2, y2) is a 4-tuple indicating the position in which the image is cropped. Please refer to our paper for more details regarding how we construct the dataset.
#### Download

The challenge dataset contains images, boundaries and masks for train, val and test set. It also contains labels for the train and val set only. The dataset .tar.gz file is around 4.4 GB. Please [contact us](https://www.agriculture-vision.com/contact-us) to get access.

### Evaluation metric

We use mean Intersection-over-Union (mIoU) as our main quantitative evaluation metric, which is one of the most commonly used measures in semantic segmentation datasets. The mIoU is computed as:

<img src="images/metric.png">

Where **c** is the number of annotation types (**c** = 7 in our dataset, with 6 patterns + background), P<sub>c</sub> and T<sub>c</sub> are the predicted mask and ground truth mask of class c respectively.

Since our annotations may overlap, we modify the canonical mIoU metric to accommodate this property. For pixels with multiple labels, a prediction of either label will be counted as a correct pixel classification for that label, and a prediction that does not contain any ground truth labels will be counted as an incorrect classification for all ground truth labels.

Concretely, we construct the confusion matrix **M<sup>c×c</sup>** with the following rules:

For each prediction x and label set Y:

1. If **x** ⊆ **Y**, then **M<sub>y,y</sub>** = **M<sub>y,y</sub>** + 1 for each **y** in **Y**<br/>
2. Otherwise, **M<sub>x,y</sub>** = **M<sub>x,y</sub>** + 1 for each **y** in **Y**<br/>

The mIoU is finally computed by (**true_positive**) / (**prediction** + **target** - **true_positive**), averaged across all classes.

### Submission

#### Registration
We are now hosting our challenge on **Codalab**. The competition page can be found here. Each participating team is required to register for the challenge. To register your team, fill out the registration form here, then register on the competition page.

*Make sure your Codalab account email matches one of the member emails in the registration form. Each team can only register once.

#### Codalab submission
All registered teams can evaluate their results on Codalab and publish their results on the leaderboard. The submission file should be a compressed **.zip** file that contains all prediction images. All prediction images should be in **png** format and the file names and image sizes should match the input images exactly. The prediction images will be converted to a 2D numpy array with the following code:

`numpy.array(PIL.Image.open(‘field-id_x1-y1-x2-y2.png’))`

In the loaded numpy array, only 0-6 integer labels are allowed, and they represent the annotations in the following way:

0 - **background**<br/>
1 - **cloud_shadow**<br/>
2 - **double_plant**<br/>
3 - **planter_skip**<br/>
4 - **standing_water**<br/>
5 - **waterway**<br/>
6 - **weed_cluster**<br/>

This label order will be strictly followed during evaluation.

All teams can have 2 submissions per day and 20 submissions in total.

#### Final submission and prize reward
The Codalab leaderboard will be closed after the deadline. Top-tier teams will be invited through email to provide their final submission for the prize reward. The final submission will be a compressed **.zip** file that contains the following materials:

<pre><b>submission/
    results/
        (field id #1)_(x1)-(y1)-(x2)-(y2).png</b>   (label predictions that matche the best mIoU on the leaderboard)<b>
        (field id #2)_(x1)-(y1)-(x2)-(y2).png
        ... etc.
    code/ </b>                                      (the training and inference code for the method)<b>
    models/ </b>                                    (pretrained model (if applicable) and the final model)<b>
    challenge_report.pdf</b>                        (detailed description of the method)</pre>

Note: Submission terms can be found [here](https://www.agriculture-vision.com/submission-terms).

To be considered as a valid submission for the prize reward, all submissions must satisfy the following requirements:

- <b>Model size will be limited below 150M parameters in total.</b>
- <b>The mIoU derived from the "results/" folder in the final submission should match the mIoU on the leaderboard.</b>
- <b>Predictions in "results/" in the final submission can be reproduced with the resources in "code/" and "models/".</b>
- <b>The training process of the method can be reproduced and the retrained model should have a similar performance.</b>
- <b>The test set is off-limits in training.</b>
- <b>For fairness, teams need to specify what public datasets are used for training/pre-training their models in their challenge_ report.pdf. Results that are generated from models using private datasets, and results without such details will be excluded from prize evaluation. (Results using private datasets can still be included in the report.)</b>
</b>
The prize award will be granted to the top 3 teams on the leaderboard that provide a valid final submission.

### Methods

Please refer to our [workshop summary paper](https://arxiv.org/abs/2004.09754) for details regarding notable methods.

### Leaderboard

|Team|mIoU (%)|Background|Cloud shadow|Double plant|Planter skip|Standing water|Waterway|Weed cluster
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|Hyunseong|63.9|80.6|56.0|57.9|57.5|75.0|63.7|56.9|
|seungjae|62.2|79.3|44.4|60.4|65.9|76.9|55.4|53.2|
|yjl912.2|61.5|80.1|53.7|46.1|48.6|76.8|71.5|53.6|
|ddcm|60.8|80.5|51.0|58.6|49.8|72.0|59.8|53.8|
|RodrigoTrevisan|60.5|80.2|43.8|57.5|51.6|75.3|66.2|49.2|
|SYDu|59.5|81.3|41.6|50.3|43.4|73.2|71.7|55.2|
|agri|59.2|78.2|55.8|42.9|42.0|77.5|64.7|53.2|
|Tennant|57.4|79.9|36.6|54.8|41.4|69.8|66.9|52.0|
|celery03.0|55.4|79.1|38.9|43.3|41.2|73.0|61.5|50.5|
|stevenwudi|55.0|77.4|42.0|54.4|20.1|69.5|67.7|53.8|
|PAII|55.0|79.9|38.6|47.6|26.2|74.6|62.1|55.7|
|agrichallenge1.2|54.6|80.9|50.9|39.3|29.2|73.4|57.8|50.5|
|hui|54.0|80.2|41.6|46.4|20.8|72.8|64.8|51.4|
|shenchen61.6|53.7|79.4|36.7|56.3|21.6|67.0|61.8|52.8|
|NTU|53.6|79.8|41.4|49.4|13.5|73.3|61.8|56.0|
|tpys|53.0|81.1|50.5|37.1|25.9|67.4|58.7|50.1|
|Simple|52.7|80.2|40.0|45.2|24.6|70.9|57.6|50.4|
|Ursus|52.3|78.9|36.3|37.8|34.4|69.3|57.1|52.3|
|liepieshov|52.1|77.2|40.2|46.0|16.0|71.3|62.9|51.1|
|Lunhao|49.4|79.5|40.4|38.8|10.5|69.4|58.3|49.1|
|tetelias-mipt|49.2|80.4|37.8|34.8|04.6|70.6|62.5|53.8|
|Dataloader|48.9|79.1|42.0|35.8|09.1|68.7|56.7|51.3|
|Hakjin|46.4|78.6|32.0|38.3|01.8|66.2|58.0|49.9|
|JianyuTANG|44.6|78.1|37.9|31.8|15.4|47.3|54.8|46.9|
|haossr|43.9|79.2|21.4|28.1|02.7|67.5|56.4|52.3|
|rpartsey|41.5|72.5|21.6|36.2|09.1|59.7|40.7|50.6|
|baidujjwal|40.8|75.2|26.1|40.1|09.9|48.0|37.1|49.5|
|Chaturlal|40.7|77.7|23.0|20.4|05.0|55.0|51.0|52.9|
|Sciforce|40.2|80.5|29.6|24.4|0.0|41.2|55.9|50.0|
|MustafaA|40.1|76.5|34.4|25.6|11.1|46.0|36.5|50.3|
|HaotianYan|36.8|77.1|21.9|25.1|13.7|57.5|24.3|37.9|
|gro|36.3|76.4|37.5|08.4|0.0|60.3|29.7|41.8|
|oscmansan|35.5|71.6|29.6|03.0|0.0|52.4|46.2|45.9|
|ThorstenC|33.6|72.3|22.3|10.0|02.0|40.8|40.1|47.8|
|ZHwang|33.5|76.5|32.4|12.9|0.0|57.2|15.9|39.9|
|fayzur2.0|22.1|65.4|21.8|02.2|00.2|23.3|13.4|28.7|
|gaslen.2|21.5|71.0|03.3|17.9|00.8|10.2|06.9|40.1|
|dvkhandelwal|16.3|71.5|0.0|0.0|0.0|42.6|0.0|0.0|
|ajeetsinghiitd|10.3|56.9|00.2|00.4|0.0|0.0|00.1|14.5|

### Teams

## Workshop

### Paper Track

- We accept papers related to Agriculture-Vision with a rigorous peer-review process with program committee members from  multiple research communities including computer vision, machine learning, image processing, remote sensing, agriculture etc.
- All accepted paper will be published in IEEE CVPR 2020 Workshop Proceedings. 
- Several top accepted papers will be given oral presentation opportunity at our workshop, the rest accepted papers will be given poster presentation slots during the workshop.

### Topic Description

The Workshop will be open to the broader community addressing **various technical and application aspects of challenges and opportunities in computer vision research for agriculture.** We aim to provide a venue to both show current relevant efforts in interdisciplinary areas between computer vision and agriculture, and to encourage further research and conversations within the computer vision community to tackle impactful agriculture-vision problems. We will solicit papers from the following wide range of topics:

- Computer vision research for agricultural imagery in general
- Resources and benchmarks for agricultural imagery based pattern analysis
- Farmland pattern classification and segmentation from LSHR aerial imagery
- Efficient data sampling methods for effective training on agricultural imageries
- Effective data fusion of multi-spectral image data such as RGB and Near-Infrared (RGB-NIR)
- Robust and stable pattern recognition on sparse and imbalanced annotations
- Other novel vision applications in agriculture

Please feel free to [contact us](https://www.agriculture-vision.com/contact-us) if you have any additional questions.

### IMPORTANT DATES

#### Paper submission:
- Workshop paper submission deadline: March 30, 2020 (11:59PM PDT)
- Notification to authors: April 15, 2020 (11:59PM PDT)
- Camera ready deadline: April 20, 2020 (10:00 AM PDT)
- Poster & Oral presentation during workshop, June 14, 2020
 
#### Challenge related:
- Challenge opens to public: January 27, 2020 (11:59PM PDT)
- Challenge reports submission by email: April 15, 2020 (11:59 PDT)
- Challenge results submission deadline: April 20, 2020 (10:00 AM PDT)
- Challenge awards announcement, prize winners presentations, June 14, 2020

### Accepted Papers and Posters

#### Accepted Papers

[1] Effective Data Fusion with Generalized Vegetation Index: Evidence from Land Cover Segmentation in Agriculture, *Hao Sheng,
Xiao Chen, Jingyi Su, Ram Rajagopal, Andrew Ng* **(Stanford University) [Link]**

### Accepted Posters

