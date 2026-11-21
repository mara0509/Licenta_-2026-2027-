## Chapter 2: Recognition of emotions and pain from animals' faces

### 1. Su et al. (2026): AI-Based Decoding of Animal Micro-Expressions (review)
**Reference:** Su, F., Wang, Y., Li, X., Wei, Y. & Tian, Y. (2026). Artificial Intelligence-Based Decoding of Animal Micro-Expressions: A Review of Methodological Advances and Translational Applications. *Animals*, 16, 2578. https://doi.org/10.3390/ani16162578

**Description:** This review summarizes how AI turns brief, subtle facial changes in animals into measurable indicators of pain, stress, fear or comfort, across companion, farm, laboratory and primate species. It groups the methods into five families and gives a practical rule for choosing between them: image classification, facial landmarks, video models, multimodal fusion and large pre-trained models. With small labeled datasets, pre-trained image encoders or landmark models are the most defensible choice, and lightweight image pipelines are the most feasible on edge devices. The authors stress that dogs and cats differ: dog expressions are easy for people to read, while cat pain signals are subtle and often missed, which is where AI helps most. They also find that most published accuracies come from small, single-site datasets and are likely inflated. The review frames my method choices and names shelters as a setting for continuous monitoring.

- **Method / technology:** structured narrative review. Main source PubMed (249 records), plus Web of Science, Scopus and IEEE; two authors screened independently; search last rerun in May 2026.
- **Setting and data:** studies on dogs, cats, horses, farm animals, rodents and primates.
- **Metrics / results:** no pooled figures (narrative synthesis). Main findings:
  - Five method families, compared by their strengths and weaknesses.
  - Late fusion (combining separately trained predictions) is the easiest to deploy when sensors are not synchronized; feeding records are named as a modality to combine with the face.
  - Large vision-language models fell to near chance on controlled, cropped dog faces.
  - Most informative region: ears in dogs, whiskers in cats.
- **What's missing:** no meta-analysis or performance numbers; screening not a full PRISMA protocol; limited dog- and cat-specific detail because of the broad scope.
- **What I take:**
  - the method rule (pre-trained static encoder + lightweight pipeline for small data);
  - YOLO detection + classifier as an established pattern;
  - late fusion of face predictions with intake data;
  - vision-language models used to explain results, not to decide the state;
  - companion-animal priorities: cross-breed testing, uncertainty reporting, unobtrusive capture, vet in the loop;
  - evaluation beyond accuracy (false alarm rate).
- **How I improve:**
  - a working system in a shelter setting, which the review names but finds no system for;
  - face predictions combined with food and water intake by the AI agent;
  - false alarm rate reported alongside accuracy.

### 2. Boneh-Shitrit et al. (2022): Explainable recognition of emotional states from canine facial expressions
**Reference:** Boneh-Shitrit, T., Feighelstein, M., Bremhorst, A., et al. (2022). Explainable automated recognition of emotional states from canine facial expressions: the case of positive anticipation and frustration. *Scientific Reports*, 12, 22611. https://doi.org/10.1038/s41598-022-27079-w

**Description:** This is the first study to recognize dog emotions from facial expressions using data from a controlled experiment, so the labels do not depend on someone judging the photo. Twenty-nine Labrador Retrievers were filmed while waiting for food behind a window. Food delivered as expected gave the positive state (anticipation); blocked access gave the negative state (frustration). The face was cropped from each frame, and two routes were compared: expert-coded facial movements with a decision tree, and pre-trained deep networks with only a small classifier trained on top. A self-supervised DINO-ViT was clearly the best, and majority voting over the frames of each video raised accuracy further. Heatmaps showed that the ViT focused on the eyes, ears and nose, and the network even classified videos where the expert had coded no facial movement.

- **Method / technology:**
  - Mask R-CNN to crop the face.
  - Two routes: 39 DogFACS movements + decision tree; or a frozen backbone (ResNet50 or ViT, supervised or DINO self-supervised) + one trained classification layer.
  - Video label by majority vote over frames; EigenCAM heatmaps.
- **Setting and data:** laboratory, 29 Labrador Retrievers, 248 three-second videos balanced to 164 (82 positive, 82 negative); labels from the experimental condition; leave-one-dog-out testing; dataset not public.
- **Metrics / results:**
  - Expert features + decision tree: 71%.
  - DINO-ViT: 85% per frame and 89% per video (F1 0.89 positive, 0.90 negative). Supervised ViT 79% / 82%; ResNet50 75–78% / 81%.
  - 77% of videos with no coded facial movement were still classified correctly.
- **What's missing:**
  - One breed, one laboratory, two food-related emotions.
  - "Eyes up" was probably an artifact of the food coming from above.
  - It is unclear whether the deep models used the strict dog-separated testing, so 89% is a best case.
  - The dataset is not public.
- **What I take:**
  - cropping the face before classification;
  - a frozen self-supervised backbone (DINO-ViT, now DINOv2) with a small classifier;
  - positive vs negative as the target, which is enough to flag discomfort;
  - classifying each frame and voting per visit;
  - heatmaps that show which facial region drove the decision.
- **How I improve:**
  - mixed breeds instead of one;
  - images from a feeder camera instead of a lab setup;
  - per-visit results stored over time and combined with intake data;
  - testing on dogs never seen in training.

### 3. Montaluisa et al. (2025): End-to-End Canine Emotion Recognition from Images to Video
**Reference:** Montaluisa, L., Andrade, R. & Grijalva, F. (2025). End-to-End Canine Emotion Recognition from Images to Video: EfficientNetV2-S and Visual Interpretability Study. *2025 IEEE Ninth Ecuador Technical Chapters Meeting (ETCM)*, Quito. https://doi.org/10.1109/ETCM67548.2025.11304379

**Description:** The authors build a pipeline that learns dog emotions from still images and then applies the model to video, with heatmaps explaining each decision. They use the public Kaggle Dog Emotion dataset (4,000 images, four balanced emotions) and fine-tune an ImageNet-pretrained EfficientNetV2-S, evaluated with cross-validation and a fixed held-out test set. For video, YOLOv8 first finds the dog, the emotion is classified inside the box, and a majority vote over 15 frames gives a stable result. The paper also compares all published results on the same dataset, showing that a DINOv2 ViT and the best CNNs perform almost identically (84.7–85.4%). The authors recommend using the predictions as decision support and name shelters as a setting where negative emotions are often misread.

- **Method / technology:**
  - EfficientNetV2-S (transfer learning from ImageNet) with dropout 0.5; images resized to 384×384.
  - 5-fold stratified cross-validation + 10% fixed test set.
  - Video: YOLOv8 dog detection → classification inside the box → majority vote over 15 frames.
  - Grad-CAM heatmaps; code public on GitHub.
- **Setting and data:** Kaggle Dog Emotion dataset, 4,000 web images (1,000 each of sad, happy, angry, relaxed). YOLOv8 found a dog in only 75% of the images.
- **Metrics / results:**
  - Test accuracy 85.43%, with F1 per class, ROC curves and a confusion matrix.
  - Earlier results on the same dataset: VGG16 70.50%, CNN from scratch 78.00%, DINOv2 ViT + face crop 84.68%, EfficientNet-B0 84.75%.
  - Heatmaps: teeth for anger, tongue for happiness, eyes for sadness, paws and posture for relaxation.
- **What's missing:**
  - Web labels with no expert validation; a quarter of the images contain no detectable dog.
  - Splits by image, not by dog.
  - No accuracy reported on video.
  - Some decisions rely on paws and posture rather than the face.
- **What I take:**
  - the Kaggle Dog Emotion dataset, grouped into positive (happy, relaxed) and negative (angry, sad);
  - 85.43% and 84.68% (DINOv2) as benchmarks;
  - detect first, then classify inside the box;
  - majority vote over consecutive frames;
  - a fixed held-out test set;
  - filtering the training images with YOLO;
  - Grad-CAM used together with metrics, never alone.
- **How I improve:**
  - only images where an animal is detected are kept, both for training and at the feeder;
  - face crops instead of the whole image;
  - per-visit accuracy measured, which the paper only shows visually;
  - results linked to each animal's history.

### 4. Kopczyński & Czubenko (2026): Enhancing human-dog interaction through deep learning and explainable AI
**Reference:** Kopczyński, M. & Czubenko, M. (2026). Enhancing human-dog interaction through deep learning and explainable AI. *Scientific Reports*, 16, 30258. https://doi.org/10.1038/s41598-026-51009-9

**Description:** The authors address two problems with earlier dog emotion models: narrow single-breed datasets and labels without clear rules. Their own previous model fell from 92% to 58% on other breeds. They labeled about 10,000 multi-breed images from the Stanford Dogs dataset into five emotions, following written behavioral guidelines with trained annotators and very high agreement (Cohen's kappa 0.90). Ten pre-trained approaches were compared, and a YOLO11 classification model was the best on every metric, followed by a frozen DINOv2 with a simple SVM classifier. The authors describe YOLO11 as well suited for real-time use with limited computing resources. They insist that outputs are indicators of visible expression, not diagnoses, and recommend decision support with confidence scores for shelters.

- **Method / technology:** nine fine-tuned networks (including ResNet50, MobileNetV2, EfficientNet-B0, ConvNeXt and YOLO11-cls), an ensemble with majority vote, and frozen DINOv2 + SVM; CNN from scratch as baseline; Grad-CAM.
- **Setting and data:** about 10,000 Stanford Dogs images (120 breeds), 5 emotions (happiness, curiosity, anger, relaxation/sleepy, sadness). Augmented to about 30,000 images (6,000 per class), split 70/20/10, whole photo (no face crop). Available on request.
- **Metrics / results:**
  - YOLO11-cls: accuracy 0.84, F1 0.84.
  - Frozen DINOv2 + SVM: accuracy 0.75, F1 0.76.
  - Ensemble: 0.75; CNN from scratch: lowest.
  - Grad-CAM attention on ears, mouth and tail.
- **What's missing:**
  - Labels describe visible expressions, not verified internal states.
  - The whole photo is used, so background can influence decisions.
  - Augmentation before the split, so copies of the same photo may be in training and testing.
  - DINOv2 was not fine-tuned or tested on face crops; no cross-breed test.
- **What I take:**
  - a YOLO classification model as a strong candidate for the emotion classifier;
  - frozen DINOv2 + a simple classifier as the second candidate, to be compared under the same conditions;
  - written labeling guidelines per emotion, for checking the Kaggle labels and grouping them into positive and negative;
  - no rotation in augmentation;
  - precision, recall, F1 per class and a confusion matrix;
  - Grad-CAM as a plausibility check only;
  - confidence scores shown to staff.
- **How I improve:**
  - YOLO26-cls and DINOv2 compared on face crops, with the same split;
  - augmentation applied only after the split;
  - predictions confirmed by intake data before an alert is raised.

### 5. Li et al. (2026): Deep Learning-Based Dog Expression Recognition
**Reference:** Li, D.-L., Lee, S.-K. & Tsai, Y.-C. (2026). Deep Learning-Based Dog Expression Recognition. *IEEE Access*, 14, 5675–5683. https://doi.org/10.1109/ACCESS.2025.3650550

**Description:** The study tests whether recognizing emotion from separate facial regions gives more stable results across breeds than classifying the whole image. YOLOv8 detects the eyes, ears and muzzle and classifies the emotion of each region, and the region scores are then fused into one decision. Four fusion methods were compared, and the muzzle proved the most reliable region and the ears the least. Trained only on 360 Shetland Sheepdog images, the model kept 88.52% accuracy on 120 other breeds, while a whole-image model fell to about 51%. Pre-training on COCO was essential with so little data. The paper is the strongest evidence that region-based cues transfer across breeds, which matters for mixed-breed shelter animals.

- **Method / technology:**
  - YOLOv8 (COCO pre-trained, CSPDarkNet53 backbone) detects and classifies eyes, ears and muzzle.
  - Fusion by majority vote, confidence sum, or weights based on accuracy (muzzle 0.52, eyes 0.37, ears 0.11) or on region count.
  - Oversampling for balance; trained on an RTX 2060.
- **Setting and data:**
  - Shetland Sheepdog dataset: 360 images, 5 behavior-defined states (joy, anger, anxiety, relaxation, neutral), 550 annotated regions.
  - Cross-breed test on Stanford Dogs cleaned to 11,487 images.
  - Random 9:1 split.
- **Metrics / results:**
  - Detection mAP@0.5: 81.6 with COCO pre-training vs 62.9 from scratch; 96.4 with oversampling.
  - Accuracy: 90% on Shetland (vs 58% for mVGG16); 88.52% on 120 breeds (vs 51.36%).
- **What's missing:**
  - Very small single-breed training set.
  - States defined by behaviors (sleeping = neutral, licking = anxiety).
  - Stanford Dogs labels added without a described protocol.
  - No separation by dog; edge deployment proposed but not tested.
- **What I take:**
  - YOLO can locate facial regions, not only the whole animal;
  - region fusion, so a result is still possible when the muzzle is in the bowl;
  - the muzzle as the most informative region for dogs;
  - COCO pre-training for small datasets;
  - oversampling;
  - cross-breed testing as proof of generalization.
- **How I improve:**
  - testing on animals and breeds not seen in training;
  - deployment on a real edge–cloud pipeline;
  - the authors' own future directions (shelters, edge AI, newer YOLO, multimodal data) built into the system.

### 6. Feighelstein et al. (2022): Automated recognition of pain in cats
**Reference:** Feighelstein, M., Shimshoni, I., Finka, L. R., Luna, S. P. L., Mills, D. S. & Zamansky, A. (2022). Automated recognition of pain in cats. *Scientific Reports*, 12, 9575. https://doi.org/10.1038/s41598-022-13348-1

**Description:** This is the first study to recognize pain in cats automatically from facial images. It compares a model based on 48 manually placed facial landmarks with a deep network working directly on the image. The data come from 26 female cats photographed before and after ovariohysterectomy, with the pain label taken from the clinical timeline rather than from a person judging the face. Under strict leave-one-cat-out testing, both approaches reached about 72–74% accuracy, so deep learning matched the landmark model without any manual annotation. The authors suggest transformers as the next step and stress that pain models should prioritize recall, since a missed case of pain has a high welfare cost. The dataset is public, which makes it the main training and test set for my cat pain model.

- **Method / technology:** 48 landmarks → vectors → multilayer perceptron, vs ResNet50 on the raw face; both tested with and without alignment and augmentation; leave-one-cat-out (train 19, validate 6, test 1).
- **Setting and data:** veterinary hospital, 26 domestic short-hair female cats, 464 images (232 pain, 232 no pain); labels from the clinical timeline; public on GitLab.
- **Metrics / results:**
  - ResNet50 without alignment: accuracy 73.6%, precision 81.9%, recall 70.1%.
  - Landmarks + MLP with alignment: 72.4% accuracy, 75.1% recall.
  - Large variation between cats (±18% accuracy); augmentation did not help.
- **What's missing:**
  - Young females only, one surgical context.
  - Small dataset; landmarks placed by hand.
  - No explanation of decisions; face only, no body posture.
- **What I take:**
  - the public dataset;
  - deep learning on the raw face as a valid route;
  - leave-one-cat-out testing;
  - recall as the priority metric;
  - heatmaps as the step the authors name for clinical use;
  - 73.6% / 70.1% recall as the reference under strict testing.
- **How I improve:**
  - a DINOv2-based model, the transformer step the authors suggest;
  - the decision threshold tuned for high recall;
  - heatmaps for every decision;
  - automatic face cropping at the feeder.

### 7. Steagall et al. (2023): Automated prediction of pain using the Feline Grimace Scale
**Reference:** Steagall, P. V., Monteiro, B. P., Marangoni, S., Moussa, M. & Sautié, M. (2023). Fully automated deep learning models with smartphone applicability for prediction of pain using the Feline Grimace Scale. *Scientific Reports*, 13, 21584. https://doi.org/10.1038/s41598-023-49031-2

**Description:** The study automates the Feline Grimace Scale (FGS), a validated veterinary tool that scores five facial action units:
- ear position;
- orbital tightening;
- muzzle tension;
- whisker change;
- head position.

A total score of 0.39 or higher means the cat likely needs pain treatment. A lightweight network predicts 37 facial landmarks, 35 geometric measurements are computed from them, and an XGBoost model predicts pain, the total score and each action unit. It covers cats of any breed, age and sex with naturally occurring pain, scored by eight experienced veterinarians. It reached 95.5% accuracy, and the decision can be traced back to specific facial regions. Its main limitations are that it has no face detection step and that its test images were not separated by cat.

- **Method / technology:**
  - 11 lightweight landmark networks compared (best ShuffleNetV2 and MobileNetV3);
  - face alignment and an edge-detection filter;
  - 35 angles and ratios grouped by action unit;
  - XGBoost for binary pain (cut-off 0.4), the total score and each action unit.
- **Setting and data:** 3,447 face images (1,655 from six clinical trials, 1,092 from FGS app users, 700 from Kaggle) with 37 landmarks. 1,188 clinical images scored by 8 veterinarians; test sets of 120 and 100 images. Data available on request.
- **Metrics / results:**
  - Landmarks: ShuffleNetV2 error 16.76%, 0.046 s per image, 6.2M parameters; MobileNetV3 18.16%, 4.3M parameters.
  - Pain vs no pain: accuracy 95.5%, AUROC 0.97. Total score MSE 0.0096.
  - Without whisker or head measurements, accuracy stays at 94% and 93%, so eyes, ears and muzzle carry most of the information.
- **What's missing:**
  - No face detection; it assumes a frontal, aligned face.
  - Test sets small and not separated by cat, so results may be inflated.
  - Annotation took 69 days.
  - No comparison with end-to-end deep models.
- **What I take:**
  - the FGS as my definition of pain signs;
  - a threshold that triggers an alert, as with the 0.39 cut-off;
  - eyes, ears and muzzle as the regions to check in my heatmaps;
  - AUROC and inference time as metrics;
  - the dataset to request from the authors.
- **How I improve:**
  - the fixed camera at the bowl gives frontal faces without a person holding a phone;
  - automatic face detection and cropping;
  - an end-to-end model compared against this landmark route's reference values;
  - testing separated by cat.

### Chapter summary
The reviewed studies show that pre-trained networks can recognize positive vs negative emotion in dogs (about 85–89%) and pain in cats (about 74% under strict testing, up to 95.5% with veterinary FGS scores). They also share the same limits:
- small or single-breed datasets;
- images taken by people rather than by a fixed camera;
- splits that mix images of the same animal;
- isolated predictions that are never followed over time or combined with other signals.

No study places these models in a feeder. My system:
- crops the face automatically at the bowl;
- compares a YOLO classification model with a frozen DINOv2 + classifier;
- votes over the frames of each visit;
- explains each decision with a heatmap;
- tests on animals not seen in training;
- passes the result to an agent that combines it with food and water intake.

**Metrics I will measure (from this chapter)**
- **Classification performance:** accuracy, precision, recall and F1 per class (%), plus a confusion matrix, for the dog model (positive / negative) and the cat model (pain / no pain), on a held-out test set. Reference:
  - dogs: 85.43% on the 4-class Kaggle dataset (Montaluisa et al., 2025) and 85% per frame in the lab (Boneh-Shitrit et al., 2022);
  - cats: 73.6% accuracy, 81.9% precision, 70.1% recall (Feighelstein et al., 2022).
- **Recall on the pain / negative class and AUROC:** the threshold is chosen so that few pain cases are missed, and the resulting false alarm rate is reported. Reference: 70.1% recall (Feighelstein et al., 2022); AUROC 0.97 (Steagall et al., 2023).
- **Per-visit accuracy** (%): accuracy when all frames of one visit are combined by majority vote, compared with single-frame accuracy. Reference: 85% → 89% (Boneh-Shitrit et al., 2022); 15-frame vote (Montaluisa et al., 2025).
- **Testing protocol (not a metric):** cat results measured with leave-one-cat-out, so every test is on an unseen cat; augmentation only after the split. Reference: 92% → 58% drop on unseen breeds (Kopczyński & Czubenko, 2026) vs 88.52% with region cues (Li et al., 2026).
