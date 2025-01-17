# SkinLesion-Segm-Classif-UNet-FocusNet-ResNet50

This is the final year project completed as dissertation for my BEng Hons Computers Science degree at University of York in (2018-2021).
<br>
<br><h2>EXECUTIVE SUMMARY</h2>
With this project I aim to contribute in filling the gap between the segmentation and classification tasks: it will evaluate the impact of the segmentation task as a pre-processing step for improving classification accuracy. This will be done by implementing well-known segmentation neural network, producing new sets of images, and train a classification network on both the original dataset and the ones generated by the segmentation models. 
<br><br>
Malignant melanoma is the deadliest type of skin cancer: it accounts for 79% of skin cancer deaths [1], [2] and between 2015-2017, 44 cases every day (16000 in total) with a further 7% increase predicted in the next decade. It represents an abnormal production of melanocytes cells, and despite its survival rate ranging between 15% to 95% [3] depending on which stage it is diagnosed in, what makes it so dangerous is that in most cases it is extremely challenging to recognize with naked eye the key clues that help distinguish a malignant cancerous mole from a harmless mole. Furthermore, even professional clinicians’ examinations are subjects to flaws, with diagnosis accuracies ranging from 75% to 84% [4], [5]. The explosion of technological advancements that started in 2012 with the advancements of deep learning has spurred the development of fully automated computer-aided diagnosis systems: many researchers are now channelling their efforts into producing state-of-the-art neural network architectures for efficiently performing the various steps that are required to construct fully automated systems. This led to a production of very competitive tools and methods at an extremely fast pace. However, the division of the problem in smaller sub-tasks, despite generally being a wise approach, also leads to a confinement of the results obtained to that specific task, overlooking the importance of verifying that such results also translate to the classification task which, in the medical context, is the true final goal.
<br><br>
Many network architectures have been proposed to deal with segmentation task and proven to produce state-of-the-art results compared to previous architectures; the assumption that such results improvements will directly translate to the skin moles classification task too, often without testing or providing evidence, is then made. The key objective for this project is to produce authentic and sensible results for arguing in favour or against the assumption made regarding the transferability of segmentation improvements in the context of medical skin cancer lesion classification. Favourable results are defined as results that show a clear increase in classification’s accuracy proportional to the increase in segmentation’s accuracy given by the progression of models’ complexity; on the other hand, unfavourable results are defined as results that show no clear correlation between classification accuracy improvements and segmentation performance improvements. 
<br><br>
Four different implementations of segmentation architectures will be discussed, analysed and tested in this project. One of them is a popular and widely employed segmentation network, U-Net [6], that was published in 2015 and has been among the state-of-the-arts ever since and being still employed as a benchmark for segmentation problems. Two of the remaining architectures evaluated are advanced revisitations of the U-Net architecture: from 2015 to date, novel methods for increasing performance of Convolutional Neural Networks (CNNs) have been proposed. Batch Normalisation [7], Residual Networks [8] and Squeeze-and-Excite Networks [9] are among these. Finally, the last architecture that will be explored is the novel network FocusNet [10] developed at the University of York. A Residual CNN is used for the classification task and will serve as the baseline for fairly testing the different segmentation models. 
<br><br>
The results show a strong connection between the use of segmentation as a pre-processing step and the performance in skin lesion classification task. Performing an upper one-tailed Student’s t-test between the baseline ResNet and the results produced by the classification networks produce t-values of 0.4, 2.93, 2.33 and 2.64 for the model trained on the masks produced by U-Net, U-Net BatchNorm, U-Net ResSE and FocusNet respectively. These results prove that a positive correlation exists between the use of segmentation as a data pre-processing technique and the improvement of a image skin lesion classification model performance. The results obtained from the training on the various segmentation models also expose the inadequateness of the U-Net architecture for circumstances in which applying heavy data pre-processing transformations is not a viable option. 
<br><br>
The use of the ISIC datasets is permitted under the CC-0 license, which effectively waives as many rights as legally possible, allowing the almost indiscriminate use of the datasets by both researchers and amateurs. 
<br>
<br>

<h2>THE IMPORTANT PART: SEGMENTATION-TO-CLASSIFICATION PIPELINE</h2>
On a high-level, the approach taken for achieving these self-defined goals consists of the manual implementation of four artificial neural network architectures for the segmentation task, which produce new sets of predicted segmentation masks each. These masks are used to crop the original dataset, hence producing, for each segmentation model, a new cropped dataset. 
<br> An example of cropping lesion images using the predicted segmentation masks is shown below:
<br>
![Cropping](/images/crop.png)

<br><br>
This is then fed into a respective pre-trained classification network: each segmentation model will have its own classification model trained on the generated dataset. The version of the classification model trained on the original dataset is used as baseline for the evaluation. An example pipeline using U-Net architecture is shown below:
<br>
![Pipeline](/images/pipeline.png)
<br><br>
To test whether using segmented images for training a classification network yields better results than simply using the original images with simple augmentation transformations, all the results produced by the different ResNets are then evaluated against the “baseline” instance of ResNet trained on the original, non-cropped, augmented images i.e. the same images used for training the segmentation networks.
<br>
<br>
<h2>RESULTS</h2>
A statistical hypothesis test is used to evaluate the probability of observing a pair of data samples under the assumption that they were drawn from the same distribution (null hypothesis). If rejected, it means that the difference between the two data samples is statistically significant enough to indicate that the difference in results implies a difference in models’ abilities rather than statistical chance. <br>
The results obtained from the multiple training iterations of each model were used as samples for performing the statistical tests.
<br>Furthermore, a Shapiro-Wilk test is used to test for normality.
<br>Visual analysis is also performed using Box-&-Whiskers Plots and QQ-plots.
<br><br>
An upper, one-tailed test was used over the two-tailed: this is because the objective of this project is to prove the presence of a positive correlation between the use of segmentation as a pre-processing step for skin lesion classification. As such, not only are we interested in verifying that segmentation does affect the classification (two-tailed test), but also that it improves it (upper one-tailed test). Using the Student’s t-tables [91], the critical value for 18 degrees of freedom for an upper one-tailed test and p=0.05 is 1.734. If the t-value is greater than the critical value, then we reject the null hypothesis i.e. there is strong evidence for results not being due to statistical chance. <br>
Performing the test between the results obtained from the baseline ResNet and the other ResNets one at a time returns the respective t-values, which are shown in the table below:
<br>
![Student T-test](/images/studenttest.png)

<br><br>
Except for the traditional U-Net, which was not able to learn the segmentation task (producing instead all white or all black images) the null hypothesis is rejected for all the models. 
<br><br>
These results prove that, in the context of skin lesion imaging, the use of image segmentation as a pre-processing step has a positive effect on the classification performance which is variable in effect based on the segmentation neural network architecture used. <br>
As such the self-defined goals have been achieved and all the goals for the projects have been met. Furthermore, the results highlight another interesting element: some segmentation architectures might be unable to learn any function at all, in which case the classification performance is obviously affected as a consequence. 



