# Retinoblastoma
Research Project 06/2022-03/2023: Retinoblastoma Detection via Image Processing and Explainable AI.

**Abstract:**
Retinoblastoma (RB) is a treatable ocular melanoma that is diagnosed early and subsequently cured in the United States but has a poor prognosis in low- and middle-income countries. This study hopes to streamline the process of diagnosing RB in LMICs. Transfer learning methods were utilized to detect RB from fundus imaging. 140 RB+ and 140 RB- images were acquired. Then, five models were used: VGG16, VGG19, Xception, Inception v3, and ResNet50 to train them on the dataset. To evaluate these models, there were two metrics used that are considered excellent for medical-image classification: Dice Similarity Coefficient (DSC) and Intersection-over-Union (IoU). Explainable AI techniques such as SHAP and LIME were implemented into VGG16 and VGG19 to increase the transparency of their decision-making frameworks, which is critical for the use of AI in medicine. Finally, diagnostic biomarkers were explored for their feasibility in improving the models. VGG16 was the best at identifying RB with a 0.9890 DSC and 0.9787 IoU value. VGG19 was a close runner-up with a 0.9885 DSC and a 0.9786 IoU value. Inception v3 had a 0.9663 DSC and 0.9374 IoU value, with Xception having DSC and IoU values of 0.9647 and 0.9371 respectively. ResNet50 performed the worst with a 0.8400 DSC and 0.7052 IoU value. SHAP values typically ranged from -0.06 to 0.10. Transfer learning methods were effective at identifying RB, and explainable AI made the models more viable for clinical settings.

**Background Information:**
Retinoblastoma (RB) is a treatable ocular melanoma and is considered an orphan disease (impacts less than 200,000 people nationwide) caused by a mutation in both RB1 genes within a cell (The American Cancer Society medical and editorial content team, 2018); RB typically occurs under 2 years of age and is diagnosed early and subsequently cured in the United States. Retinoblastoma (RB) disproportionately impacts low and middle-income countries (LMICs). In a study where 4064 patients with RB participated, 642 (15.8%) were from high-income countries, 1151 (28.3%) were from upper-middle-income countries, 1791 (44.1%) from lower-middle-income countries, and 480 (11.8%) from low-income countries (Fabian et al., 2022). As the income classes progress from high to low, the percentage of mortality increases almost exponentially. This difference in death percentages can be most directly attributed to the lack of advanced technologies and healthcare services present in LMICs, for which this project aims to provide a solution for.

**Engineering Goals:**
Current machine learning work on the topic of Retinoblastoma has reasonable accuracy. However, in clinical settings, accuracy is never sufficient until perfect. This project wishes to advance RB diagnostic accuracy by utilizing transfer learning techniques. Explainable AI (XAI) is also an unexplored sector of RB research; this project also wishes to also see the feasibility of using XAI to increase the informational applications of the models to help doctors find unique features they have not detected before, alongside making the CNN model’s process as transparent as possible to maximize its applications in clinical settings. This project also wishes to explore the viability of diagnostic biomarkers to see if it has any plausibility to improve the accuracy of the models. In addition, if cheaper and more accessible methods to collect fundus imaging (and diagnostic biomarker data) from patients can be obtained, then the transparent model can be utilized to aid LMICs in getting diagnoses faster.

**Data Analysis:**
Retinoblastoma has scarce data available for it. However, utilizing a previous study on deep learning for RB, 140 RB-positive fundus images were obtained. To find 140 RB-negative fundus images to have an even split of data and to avoid unbalanced data, research papers were scraped to find healthy benign images. Although this could cause issues due to differences in how the fundus images were taken, this was the best dataset that could be acquired and since all the models run with the same dataset, it should not cause a drastic change in which architecture prevails as the best one. Data augmentation was considered, but was decided not to be used as medical imaging is more high-stakes than other image classification tasks. If the data augmentation causes any slight resolution issues, it can drastically affect the accuracy of its predictions; this is proved in the results where resolution plays a big part in accuracy. The RB-positive images were taken with a Retcam pediatric camera and with an age group of 12 to 20 years. The RB-negative images may not have all been taken in the same manner. All images were resized to the proper input size during image and label preprocessing. The tumor is circled in the RB-positive image. Although it is more easily distinguishable between these two images, in other cases, it isn’t as easy. Another issue we encountered was regarding how to tie the images and labels together; data was manipulated using a plethora of functions.

**Introduction to Transfer Learning Analysis:**
In this review, the models VGG16, VGG19, ResNet50, Inception v3, and Xception are compared. All of these are trained with the ImageNet dataset, but was repurposed them for this project. A biomedical segmentation CNN such as U-net was considered, buta another area with a lack of transparency is reached (a limitation); U-net gives the diagnosis and area of infection, but does not provide sufficient detail. This novel architecture with a top-performing model & XAI points out specific contours within the fundus image that supports the diagnosis, and doctors can achieve greater confidence that the model is able to be trusted due to the detail it dives into. With U-net, it is not possible to explore any new diagnostic tumor features due to the generality of the segmentation model, but with XAI techniques, it is possible to find specific, and often minute details of the fundus image that informs the model of a benign/malignant diagnosis. First, Jupyter Notebook was used, but later in the project Google Colab was preferred due to easier library imports and additional functionalities. 

**Preliminary Testing:**
scikit-learn's tool Grid Search CV was considered to find the best hyperparameters, but was not since it is less feasible with neural networks like the ones used. One concern arised while working through this project was overfitting since high accuracy rates were obtained. Though accuracy was not the final metric, it was utilized for preliminary testing. Since all 5 provided gave a similar testing/validation accuracy/loss, it was deduced that overfitting was not a major concern. Early stopping was implemented either way to account for overfitting. However, the test dataset was still utilized to reaffirm that there was no overfitting. Since the test received similar results as both training & validation, it was authenticated that the results of these models were not overfitting. For all the models, the validation split was 0.4, and patience for early stopping was 5, with validation accuracy being monitored. Epochs were mostly only 50 (though the model stopped short due to early stopping), with ResNet50 having 4 epochs due to overfitting occurring particularly early. Pre-trained image processing models often take a long time to run, especially with more epochs, so with preliminary testing where the models were introduced, the epoch count was lowered or the models while other aspects of this project were worked on.  

**Metrics:**
For efficient transfer learning model comparisons, a need a good way to compare the models is needed. Some metrics that were considered were accuracy, specificity, and ROC AUC. Accuracy isn’t the best indicator, as in a sample of 100 patients, marking all patients as having benign growths if there were 10 patients with cancer would yield a misleading 0.9 accuracy rate. After careful review, it was decided to use Dice Similarity Metric (DSC, also known as F-score) and Intersection-over-Union (IoU), which are considered state-of-the-art metrics for medical image classification. DSC is calculated from a harmonic mean of the precision and recall of a prediction. IoU, also known as the Jaccard index, is another metric that is recently discovered to have very good potential in evaluating a model for its efficacy, with respect to the detection of diseases. IoU typically penalizes under and over segmentation more than DSC. As the name suggests, IoU is calculated by taking the intersection, union, and dividing them. In both DSC and IoU, it is noticed that a bigger weightage placed on true positives; the more true positives and less false negatives that a model gives, the higher its Jaccard index and resultant plausibility in clinical applications is. 

Utilizing PyTorch’s Jaccard function, the Jaccard index/IoU of every model was taken. The average type was set to micro (calculates metric globally, across all samples and classes) and converted the parameters to meet the parameter specifications needed to input in the y_true and y_pred. For y_pred, a plethora of functions were run in order to receive the values in a format that was rendered usable by the model.

**Explainable AI:**
This project wishes to maximize the clinical applications of the architecture that was discovered to be most effective in RB detection. However, in medicine, after accuracy is transparency, a significant factor that influences a lot of decisions. Often AI is not trusted due to the black-box setup that is forced upon doctors when they attempt to utilize AI in healthcare applications. One solution to this is the usage of the novel explainable AI techniques of SHapley Additive Explanations (SHAP) and Local Interpretable Model Agnostic Explanations (LIME) come in handy. They help provide insight into which part of the image informed the model of the presence of RB. LIME wants to minimize the locality-aware loss L(f,g,Πx) while keeping the model complexity denoted low. SHAP was the results of our VGG16 model during preliminary testing and was used to predict a single malignant image. First, the image was segmented so each pixel didn't need to be explained. Then, the top prediction was retrieved. After visualizing our explanations, we received an image with SHAP values on it. The SHAP value talks about how much each value contributed toward the final outcome. A negative SHAP value for a feature means that the feature contributed more toward the listed diagnosis, and a positive SHAP value for a feature means that the feature contributed more away from the listed diagnosis.

**Future Research:**
Artificial intelligence in medicine often faces one common challenge; accurate data to develop effective models. One way to improve the architectures' accuracy is by acquiring data from previous deep-learning studies on RB. By using this data to feed the models larger datasets, it can be ensured that the models are trained with large, high-quality datasets. To acquire this data, it will be ensured that research ethics guidelines are followed and the necessary licensing is obtained. A correspondance was already made to a previous deep-learning study named “Automatic Retinoblastoma Screening and Surveillance Using Deep Learning” that utilizes 36,623 images. By incorporating the aforementioned data into the models, it is possible to improve the accuracy of the models, resultantly increasing the clinical viability of our AI solution for diagnosing RB in LMICs.

One hanging end of this project is creating a cost-effective device to collect fundus imaging and data for biomarkers, alongside connecting fundus imaging and biomarker data. Fundus imaging is an expensive technique utilized to diagnose ocular diseases and is not very accessible. By finding a way to train the models with iPhone images, which MDEyeCare has explored the feasibility of, this goal can be achieved. In addition,  inexpensive methods to collect HIPAA-compliant data is discovered, algorithmic effiency is maximized to decrease operating costs, and the model is trained with images and possibly biomarkers; it is possible to create a multimodal approach to maximize this solution’s applicability to LMICs. 

Overall, by incorporating data from existing studies, creating cost-effective data-collecting devices, exploring the use of biomarkers, and following HIPAA compliance guidelines, it is possible to increase the affordability and efficiency of medical models. These efforts can ultimately improve patient outcomes in LMICs and decrease the disparity in access to high-quality medical care.

**Conclusion:** 
In this project, multiple transfer learning architectures were analyzed: VGG16, VGG19, ResNet50, Inception v3, and ResNet50. It was deduced that VGG16 has the best viability in diagnosing RB with a 0.9890 DSC and 0.9787 IoU value. VGG19 was a close runner-up with a 0.9885 DSC and a 0.9786 IoU value. Inception v3 had a 0.9663 DSC and 0.9374 IoU value, with Xception being a close runner-up to that model with DSC and IoU values of 0.9647 and 0.9371, respectively. Finally, ResNet50 performed the worst with a 0.8400 DSC and 0.7052 IoU value.

SHAP was applied to VGG16 and were able to gain better insight into which contours of the image caused the diagnosis that the model gave for a single image. Since VGG19 was a close runner-up, SHAP and LIME were also run on the results of that model for a single image and the model was able to produce interesting visuals making it easier to understand how the model viewed the image that was inputted.

Through the use of metrics such as DSC and IoU, which are specifically designed for medical image classification, and by comparing popular pre-trained image processing architectures, the effectiveness of these models can be evaluated in identifying tumor features. Additionally, by employing XAI techniques such as SHAP and LIME, the transparency of the decision-making process of these models can be enhanced and promote trust in AI among clinicians. These combined efforts can improve the clinical applicability of deep learning models and assist doctors in possibly identifying novel tumor features.
