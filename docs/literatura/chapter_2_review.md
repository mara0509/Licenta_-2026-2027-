## Chapter 2: Recognition of emotions and pain from animals' faces

### 1. Su et al. (2026): AI-Based Decoding of Animal Micro-Expressions (review)
**Reference:** Su, F., Wang, Y., Li, X., Wei, Y. & Tian, Y. (2026). Artificial Intelligence-Based Decoding of Animal Micro-Expressions: A Review of Methodological Advances and Translational Applications. *Animals*, 16, 2578. https://doi.org/10.3390/ani16162578

**Description:** This review summarizes how AI turns brief, subtle facial changes in animals into measurable indicators of pain, stress or comfort, across companion, farm, laboratory and primate species. It groups the methods into five families (image classification, landmarks, video models, multimodal fusion, large pre-trained models) and gives a rule for choosing between them. With small labeled datasets, pre-trained image encoders are the most defensible choice. The authors stress that cat pain signals are subtle and often missed by people, which is where AI helps most. They also find that most published accuracies come from small, single-site datasets and are likely inflated. Shelters are named as a setting for continuous monitoring, but no system there is described.

- **Method / technology:** structured narrative review; PubMed (249 records), Web of Science, Scopus and IEEE; two independent screeners; search rerun in May 2026.
- **Setting and data:** studies on dogs, cats, horses, farm animals, rodents and primates.
- **Metrics / results:** no pooled figures.
  - Late fusion (combining separately trained predictions) is the easiest to deploy, and feeding records are named as a signal to combine with the face.
  - Large vision-language models fell to near chance on controlled, cropped dog faces.
- **What's missing:** no meta-analysis; limited dog- and cat-specific detail; screening was not a full PRISMA protocol.
- **What I take:**
  - a pre-trained image encoder as the method for small labeled datasets;
  - late fusion: face results and intake data combined afterwards by the AI agent;
  - the LLM interprets results but never classifies the images.
- **How I improve:**
  - a working monitoring system in a shelter, the setting the review names but finds no system for.

### 2. Boneh-Shitrit et al. (2022): Explainable recognition of emotional states from canine facial expressions
**Reference:** Boneh-Shitrit, T., Feighelstein, M., Bremhorst, A., et al. (2022). Explainable automated recognition of emotional states from canine facial expressions: the case of positive anticipation and frustration. *Scientific Reports*, 12, 22611. https://doi.org/10.1038/s41598-022-27079-w

**Description:** This is the first study to recognize dog emotions from facial expressions using data from a controlled experiment, so the labels do not depend on someone judging the photo. Twenty-nine Labrador Retrievers were filmed waiting for food: food delivered as expected gave the positive state, blocked access the negative one. The face was cropped from each frame and passed to pre-trained networks with only a small classifier trained on top. A self-supervised DINO-ViT was clearly the best, and a majority vote over the frames of each video raised accuracy further. It shows that a binary positive / negative target is feasible from the face alone.

- **Method / technology:** Mask R-CNN face crop; frozen backbone (ResNet50 or ViT, supervised or DINO) + one trained layer; majority vote over frames; EigenCAM heatmaps.
- **Setting and data:** laboratory, 29 Labradors, 164 three-second videos (82 positive, 82 negative); labels from the experimental condition; dataset not public.
- **Metrics / results:**
  - DINO-ViT: 85% per frame, 89% per video (F1 0.89 / 0.90).
  - Supervised ViT: 79% / 82%.
  - Expert-coded facial movements: 71%.
- **What's missing:** one breed, one lab, two food-related emotions; it is unclear whether the deep models were tested on unseen dogs; the dataset is not public.
- **What I take:**
  - cropping the face before classification;
  - a frozen self-supervised ViT with a small trained classifier on top;
  - a majority vote over the frames of one visit.
- **How I improve:**
  - mixed-breed dogs and feeder-camera images instead of one breed in a lab.

### 3. Montaluisa et al. (2025): End-to-End Canine Emotion Recognition from Images to Video
**Reference:** Montaluisa, L., Andrade, R. & Grijalva, F. (2025). End-to-End Canine Emotion Recognition from Images to Video: EfficientNetV2-S and Visual Interpretability Study. *2025 IEEE Ninth Ecuador Technical Chapters Meeting (ETCM)*, Quito. https://doi.org/10.1109/ETCM67548.2025.11304379

**Description:** The authors train a dog emotion model on still images and apply it to video, using the public Kaggle Dog Emotion dataset (4,000 images, four balanced emotions). An ImageNet-pretrained EfficientNetV2-S is fine-tuned, with cross-validation and a fixed held-out test set. For video, YOLOv8 finds the dog and a vote over 15 frames gives a stable result. The paper compares all published results on the same dataset and shows that a DINOv2 ViT and the best CNNs perform almost identically. However, a quarter of the images contain no detectable dog, and some decisions rely on paws and posture rather than the face.

- **Method / technology:** EfficientNetV2-S (transfer learning), 5-fold stratified cross-validation + 10% fixed test set; YOLOv8 + 15-frame vote for video; Grad-CAM; code on GitHub.
- **Setting and data:** Kaggle Dog Emotion, 4,000 web images (sad, happy, angry, relaxed).
- **Metrics / results:**
  - Test accuracy 85.43%.
  - Earlier results on the same dataset: DINOv2 ViT + face crop 84.68%, EfficientNet-B0 84.75%, VGG16 70.50%.
  - YOLOv8 found a dog in only 75% of the images.
- **What's missing:** web labels with no expert check; splits by image, not by dog; no accuracy reported on video.
- **What I take:**
  - the Kaggle Dog Emotion dataset, grouped into positive (happy, relaxed) and negative (angry, sad);
  - 85.43% and 84.68% (DINOv2) as benchmarks;
  - training images passed through the same detector and crop as at the feeder, so images without a dog are removed.
- **How I improve:**
  - face crops instead of the whole image;
  - per-visit accuracy measured, which the paper only shows visually.

### 4. Kopczyński & Czubenko (2026): Enhancing human-dog interaction through deep learning and explainable AI
**Reference:** Kopczyński, M. & Czubenko, M. (2026). Enhancing human-dog interaction through deep learning and explainable AI. *Scientific Reports*, 16, 30258. https://doi.org/10.1038/s41598-026-51009-9

**Description:** The authors build a multi-breed dog emotion dataset with a documented labeling protocol: about 10,000 Stanford Dogs images in five emotions, labeled by trained annotators (Cohen's kappa 0.90). Ten pre-trained approaches were compared, and a YOLO11 classification model was the best on every metric, ahead of a frozen DINOv2 with an SVM classifier. Their earlier single-breed model had fallen from 92% to 58% on other breeds. The authors treat Grad-CAM only as a plausibility check. They insist that outputs describe visible expression, not diagnoses, and should support staff decisions.

- **Method / technology:** nine fine-tuned networks (including YOLO11-cls, ConvNeXt, EfficientNet-B0), an ensemble, and frozen DINOv2 + SVM; Grad-CAM.
- **Setting and data:** about 10,000 Stanford Dogs images (120 breeds), augmented to about 30,000, split 70/20/10; whole photo, no face crop.
- **Metrics / results:**
  - YOLO11-cls: accuracy 0.84, F1 0.84.
  - Frozen DINOv2 + SVM: 0.75 / 0.76.
  - CNN from scratch: lowest.
- **What's missing:** augmentation before the split (possible leakage); background included; DINOv2 not tested on face crops; no cross-breed test.
- **What I take:**
  - a YOLO classification model (YOLO26-cls) as the second candidate, to compare with frozen DINOv2;
  - their written per-emotion guidelines to check the Kaggle labels when grouping them;
  - heatmaps as a plausibility check, not as proof.
- **How I improve:**
  - both candidates compared on face crops, with the same split and augmentation applied only to training images.

### 5. Li et al. (2026): Deep Learning-Based Dog Expression Recognition
**Reference:** Li, D.-L., Lee, S.-K. & Tsai, Y.-C. (2026). Deep Learning-Based Dog Expression Recognition. *IEEE Access*, 14, 5675–5683. https://doi.org/10.1109/ACCESS.2025.3650550

**Description:** The study tests whether emotion recognized from facial regions (eyes, ears, muzzle) is more stable across breeds than classification of the whole image. YOLOv8 detects and classifies each region, and the region scores are fused into one decision. Trained only on 360 Shetland Sheepdog images, the model kept 88.52% accuracy on 120 other breeds, while a whole-image model fell to about 51%. The muzzle was the most reliable region. The paper is the strongest evidence that facial cues transfer across breeds, which matters for mixed-breed shelter dogs.

- **Method / technology:** YOLOv8 (COCO-pretrained) detecting eyes, ears and muzzle, with four fusion methods.
- **Setting and data:** 360 Shetland Sheepdog images in 5 behavior-defined states; cross-breed test on 11,487 Stanford Dogs images.
- **Metrics / results:** 90% on Shetland (vs 58% for a whole-image model); 88.52% on 120 breeds (vs 51.36%).
- **What's missing:** tiny single-breed training set; states defined by behaviors (sleeping = neutral); no separation by dog; edge deployment not tested.
- **What I take:**
  - facial cues transfer across breeds when the model sees the face rather than the whole image, which supports face crops for mixed-breed dogs;
  - testing on breeds not seen in training.
- **How I improve:**
  - deployment in a working edge–cloud pipeline, which the authors only propose.

### 6. Feighelstein et al. (2022): Automated recognition of pain in cats
**Reference:** Feighelstein, M., Shimshoni, I., Finka, L. R., Luna, S. P. L., Mills, D. S. & Zamansky, A. (2022). Automated recognition of pain in cats. *Scientific Reports*, 12, 9575. https://doi.org/10.1038/s41598-022-13348-1

**Description:** This is the first study to recognize pain in cats automatically from facial images. It uses 26 female cats photographed before and after surgery, with the pain label taken from the clinical timeline. A landmark-based model and a ResNet50 on the raw face both reached about 72–74% accuracy under strict leave-one-cat-out testing, so deep learning matched landmarks without manual annotation. The authors suggest transformers as the next step and stress that pain models should prioritize recall, since a missed case of pain has a high welfare cost. The dataset is public.

- **Method / technology:** 48 landmarks + MLP vs ResNet50 on the raw face; leave-one-cat-out (train 19, validate 6, test 1).
- **Setting and data:** veterinary hospital, 26 female domestic short-hair cats, 464 images (232 pain, 232 no pain); public on GitLab.
- **Metrics / results:**
  - ResNet50: accuracy 73.6%, precision 81.9%, recall 70.1%.
  - Landmarks + MLP: 72.4%.
  - ±18% variation between cats.
- **What's missing:** young females in one surgical context; small dataset; no explanation of decisions.
- **What I take:**
  - the public dataset as the training and test set for the cat pain model;
  - leave-one-cat-out testing;
  - recall as the priority metric, with the threshold chosen so few pain cases are missed.
- **How I improve:**
  - a DINOv2-based model, the transformer step the authors suggest.

### 7. Steagall et al. (2023): Automated prediction of pain using the Feline Grimace Scale
**Reference:** Steagall, P. V., Monteiro, B. P., Marangoni, S., Moussa, M. & Sautié, M. (2023). Fully automated deep learning models with smartphone applicability for prediction of pain using the Feline Grimace Scale. *Scientific Reports*, 13, 21584. https://doi.org/10.1038/s41598-023-49031-2

**Description:** The study automates the Feline Grimace Scale (FGS), a validated veterinary tool that scores ear position, orbital tightening, muzzle tension, whisker change and head position. A score of 0.39 or higher means the cat likely needs treatment. A lightweight network predicts 37 facial landmarks, geometric measurements are computed from them, and XGBoost predicts pain, with labels from eight experienced veterinarians. Eyes, ears and muzzle carried most of the information. The system needs a person to provide a frontal, aligned face, and its test images were not separated by cat.

- **Method / technology:** landmark networks (best ShuffleNetV2), face alignment, 35 geometric measurements, XGBoost.
- **Setting and data:** 3,447 cat face images with 37 landmarks; 1,188 clinical images scored by 8 veterinarians; data available on request.
- **Metrics / results:**
  - Pain vs no pain: accuracy 95.5%, AUROC 0.97.
  - Without whisker or head measurements: 94% and 93%.
- **What's missing:** no face detection; small test sets not separated by cat; 69 days of manual landmark annotation.
- **What I take:**
  - the FGS facial signs as the definition of the pain indicator;
  - an alert when the pain score passes a threshold;
  - eyes, ears and muzzle as the regions the heatmaps should highlight.
- **How I improve:**
  - faces detected and cropped automatically by the fixed feeder camera;
  - an end-to-end model with no landmark annotation.

### Chapter summary
Pre-trained networks can recognize positive vs negative emotion in dogs (about 85–89%) and pain in cats (about 74% under strict testing, up to 95.5% with veterinary FGS scores). The studies share four limits:
- small or single-breed datasets;
- photos taken by people;
- splits that mix images of the same animal;
- predictions that are never followed over time or combined with other signals.

No study places these models in a feeder. My system crops the face at the bowl, classifies it in the cloud, votes per visit, and passes the result to the AI agent together with food and water intake.

**Design decisions from this chapter**
- The face cropped before classification → the face crop gave the best results (Boneh-Shitrit et al., 2022); whole-image models relied on paws, tail and background (Montaluisa et al., 2025; Kopczyński & Czubenko, 2026); facial cues transfer across breeds (Li et al., 2026)
- Dogs: positive / negative, trained on Kaggle Dog Emotion regrouped (happy + relaxed vs angry + sad) → a binary target reached 85–89% (Boneh-Shitrit et al., 2022); a public dataset with published benchmarks (Montaluisa et al., 2025)
- Cats: pain / no pain, trained on the public Feighelstein dataset and described through the FGS facial signs → the only public cat pain dataset with clinical labels (Feighelstein et al., 2022); FGS signs are the veterinary standard (Steagall et al., 2023)
- Two classifier candidates compared: frozen DINOv2 + small classifier vs YOLO26-cls → frozen DINO-ViT was best (Boneh-Shitrit et al., 2022); DINOv2 matched the best CNN (Montaluisa et al., 2025); YOLO11-cls beat frozen DINOv2 (Kopczyński & Czubenko, 2026); pre-trained encoders suit small datasets (Su et al., 2026)
- One result per visit, by majority vote over its frames → 85% → 89% per video (Boneh-Shitrit et al., 2022)
- Pain threshold tuned for high recall, sending an alert to the vet → a missed pain case costs more than a false alarm (Feighelstein et al., 2022); FGS cut-off as an alert trigger (Steagall et al., 2023)
- Heatmaps (Grad-CAM for CNNs, EigenCAM for ViTs) shown with each result, used as a plausibility check on eyes, ears and muzzle → EigenCAM on ViT faces (Boneh-Shitrit et al., 2022); plausibility only (Kopczyński & Czubenko, 2026); the informative regions (Steagall et al., 2023)
- Face results combined with intake by the AI agent; the LLM never classifies images → late fusion with feeding records; vision-language models near chance on cropped faces (Su et al., 2026)

**Metrics I will measure (from this chapter)**
- **Classification performance:** accuracy, precision, recall and F1 per class (%) and a confusion matrix, for both candidates, on the dog model (positive / negative) and the cat model (pain / no pain). Reference: dogs 85.43% on 4 classes (Montaluisa et al., 2025), 85% per frame (Boneh-Shitrit et al., 2022); cats 73.6% accuracy, 70.1% recall (Feighelstein et al., 2022).
- **Pain / negative recall and AUROC:** at the chosen threshold, with the resulting false alarm rate. Reference: 70.1% recall (Feighelstein et al., 2022); AUROC 0.97 (Steagall et al., 2023).
- **Per-visit accuracy** (%): majority vote over the frames of a visit vs single-frame accuracy. Reference: 85% → 89% (Boneh-Shitrit et al., 2022).
- **Testing protocol:** cats tested leave-one-cat-out; dogs tested on a held-out set; augmentation only on training images. Reference: 92% → 58% on unseen breeds (Kopczyński & Czubenko, 2026).
