## Chapter 3: Individual animal identification

### 1. De Marsico, Jain & Miglino (year): Beyond Humans: Multispecies Animal Face Recognition Using Transfer Learning
**Reference:** De Marsico, M., Jain, A. K. & Miglino, A. (year). Beyond Humans: Multispecies Animal Face Recognition Using Transfer Learning. *Journal / Conference*, volume, pages. DOI

**Description:** The study tests whether models already trained on very large datasets can recognize individual animals by their face, as a non-invasive alternative to microchips, ear tags and collars. Two pre-trained models are compared as "fingerprint" extractors: FaceNet, trained on human faces, and a Vision Transformer (ViT), trained on ImageNet objects and animals. They are tested on dogs, three primate species and cattle, always on individuals never seen during training. On dogs, ViT clearly beat both FaceNet and dog-specific networks trained from scratch, reaching 96.8% verification accuracy and 84.34% Rank-1 identification. The authors also warn that accuracy alone is misleading for recognition, and that the false acceptance rate at a fixed threshold should be reported. Their error analysis shows that head pose, lighting, blur and similar coat colours cause most mistakes.

- **Method / technology:**
  - Transfer learning with two pre-trained models used as feature extractors: FaceNet (CASIA-WebFace, last block retrained, 512-number fingerprint) and ViT (ImageNet-21k → ImageNet, fully retrained from pre-trained weights).
  - Triplet training (same animal close, different animals far), moving from medium to hard examples.
  - Evaluated as verification (1:1) and identification (1:N).
- **Setting and data:**
  - Extended DogFaceNet dataset: 8,363 internet face photos of 1,393 dogs (2–41 photos per dog), aligned and resized to 224×224.
  - Split: training 6,128 photos / 1,252 dogs; validation 1,538 / 869; test 537 photos / 93 dogs never seen in training.
  - Also primates (lemurs, golden monkeys, chimpanzees) and cattle.
- **Metrics / results:**
  - Dog verification (100 draws of 2,500 genuine + 2,500 impostor pairs): ViT 96.8%, FaceNet 88.2%, previous best (Yoon et al.) 88.8%, DogFaceNet 76.9%.
  - Dog identification: ViT Rank-1 84.34%, Rank-5 96.97%; FaceNet about 38%; dog-specific models trained from scratch 39.74% and 10.96%.
  - Cattle: ViT 99.4% accuracy, EER 0.82%. Primates mixed; chimpanzees 47% frozen → 63% retrained.
- **What's missing:**
  - The dog accuracy is the best value over thresholds chosen on the test data, so it is optimistic.
  - Impostor pairs are random dogs, not look-alikes of the same breed.
  - No cats; internet photos rather than a fixed camera.
- **What I take:**
  - a large pre-trained model used as a fingerprint extractor instead of a network built from scratch;
  - general visual knowledge (ImageNet ViT) over human-face knowledge;
  - new animals registered with a few photos, no retraining;
  - verification (1:1) and identification (1:N) as two separate tasks;
  - FAR at a fixed threshold, FRR and EER, plus Rank-1 and Rank-5;
  - frozen model first, triplet fine-tuning if accuracy is not enough;
  - about 5 photos per animal.
- **How I improve:**
  - cats as well as dogs;
  - images from a fixed camera at the bowl, which reduces the pose, lighting and blur problems they found;
  - testing on look-alike pairs (same colour, similar breed);
  - identification linked to daily intake, welfare monitoring and medication safety.

### 2. Fernigrini & Yun (2026): A Dual-Stream CLIP–ViT Framework for Open-Set Animal Re-Identification
**Reference:** Fernigrini, … & Yun, … (2026). A Dual-Stream CLIP–ViT Framework for Open-Set Animal Re-Identification. *Journal of Imaging*, 12(8), 354. https://doi.org/10.3390/jimaging12080354

**Description:** The study proposes a two-stage system that recognizes individual cats and dogs from ordinary photos, for example to help find lost pets. In stage 1, YOLOv8 finds the animal and crops it with a 15% margin. In stage 2, a frozen CLIP model and a ViT focused on distinctive body parts together produce a 512-number identity fingerprint, which is compared with stored photos. On 173 animals never seen in training, the first guess was correct 97–98% of the time, stable over three runs. The system was tested honestly: with rejection of unregistered animals, a background check and nine types of damaged images. Accuracy fell sharply on 14,716 unseen dogs, and low resolution and motion blur were the largest causes of error.

- **Method / technology:**
  - Stage 1: YOLOv8 (COCO, then fine-tuned on cats and dogs from OpenImages), permissive threshold, soft crop with a 15% margin, image centre used if nothing is detected.
  - Stage 2: frozen CLIP + small trained projection layer; ImageNet ViT that selects the 6 most informative patches, frozen for 10 epochs and then fine-tuned slowly; outputs fused into a 512-number fingerprint.
  - Matching by similarity to stored fingerprints; no random erasing in augmentation.
- **Setting and data:**
  - CatIndividuals: real phone photos with clutter; 354 cats for training (about 26 photos each), 77 for testing.
  - MultiposeDog: whole-body dog crops in different poses; 95 dogs for training (about 10 photos each), 96 for testing.
  - PetFace: dog face crops; 3–5 photos per dog for training, 14,716 dogs for testing.
  - Main test: 96 dogs + 77 cats = 173 unseen animals, 181 query photos vs 2,319 gallery photos.
- **Metrics / results:**
  - Rank-1 97–98% on the 173 animals, stable over 3 runs; Rank-5 and mAP also reported (mAP is their main metric).
  - With strict rejection of unregistered animals, only about 68% of the right animals are accepted.
  - Low resolution −16 points, motion blur −13 points.
  - PetFace (14,716 unseen dogs): 54% with ≥ 5 photos per dog, 59% in the 2-photo version.
  - The model attends to the animal, not the background.
- **What's missing:**
  - Designed for random owner photos, not a fixed camera.
  - Cat images dominate training (91% of images).
  - Performance collapses with thousands of similar dogs.
  - Strict open-set operation costs a lot of accuracy.
- **What I take:**
  - detection and identification as separate steps;
  - a 15% soft crop and a permissive detector;
  - a frozen pre-trained model with a small trained layer;
  - fingerprint comparison, so new arrivals need no retraining;
  - no random erasing;
  - 2–5 registration photos per animal;
  - sharp, high-resolution frames;
  - an "unknown animal" answer instead of a forced match;
  - Rank-1, Rank-5 and mAP on unseen animals.
- **How I improve:**
  - a fixed camera with the same angle and background;
  - tens of shelter animals instead of thousands;
  - when unsure, the system asks the caretaker instead of choosing between strict rejection and a wrong match;
  - identification used for daily monitoring and medication, not lost-pet search.

### Chapter summary
Both studies show that a large pre-trained model, used to turn each animal into a fingerprint and compare it with registered photos, recognizes individuals far better than networks trained from scratch on small animal datasets. Performance is high for small groups and good images: Rank-1 84–98%. It drops with blur, low resolution, unusual head poses, look-alike animals and very large galleries, and strict rejection of unknown animals costs accuracy. Neither study uses a fixed camera or links identity to care decisions.

In my system:
- **Phase 1:** each kennel is assigned to one animal.
- **Phase 2 (future work):** verification checks that the animal at the bowl is the assigned one. It uses a frozen DINOv2 fingerprint and a strict threshold, with caretaker confirmation when the system is unsure.

**Design decisions from this chapter**
- Phase 1 with one animal per kennel; individual recognition left to Phase 2 → recognition drops with look-alike animals, blur and pose, and strict rejection of unknown animals accepts only about 68% of the right animals (Fernigrini & Yun, 2026; De Marsico et al.)
- Phase 2 as verification (1:1: "is this the animal assigned to this kennel?") rather than identification (1:N) → the two tasks are evaluated differently, and a feeder per kennel only needs the simpler check (De Marsico et al.)
- DINOv2 as a frozen fingerprint extractor, not a classifier or a network trained from scratch → a pre-trained ViT reached 84.34% Rank-1 vs 39.74% and 10.96% for dog-specific models trained from scratch (De Marsico et al.); a frozen backbone keeps its general knowledge when few animals are available (Fernigrini & Yun, 2026)
- Triplet fine-tuning only if frozen DINOv2 is not accurate enough → retraining improved chimpanzee results from 47% to 63% (De Marsico et al.)
- New animals registered with about 5 photos from the feeder camera, with no retraining → fingerprint comparison works on unseen animals; ≥ 5 photos per animal is the usual convention, and 2 photos already work (De Marsico et al.; Fernigrini & Yun, 2026)
- Fingerprints stored and compared by cosine similarity in pgvector (already used for RAG) → identification by comparing fingerprints with registered ones (De Marsico et al.; Fernigrini & Yun, 2026)
- YOLO26n with a permissive threshold and a crop with a 15% margin → a missed animal cannot be identified, while extra background is absorbed by the margin (Fernigrini & Yun, 2026)
- No random erasing during training → it can hide the small markings that distinguish individuals (Fernigrini & Yun, 2026)
- The sharpest frame captured while the animal is still at the bowl, at the camera's full resolution → blur (−13 points) and low resolution (−16 points) were the largest causes of error (Fernigrini & Yun, 2026); head pose, lighting and blur caused most wrong rejections (De Marsico et al.)
- Strict threshold for medication, with caretaker confirmation when the system is unsure → accuracy hides false acceptances, and giving medication to the wrong animal is the dangerous error (De Marsico et al.)

**Metrics I will measure (from this chapter, if Phase 2 is implemented)**
- **Verification error:** FAR and FRR (%) at the chosen threshold, and EER (%). Measured on genuine pairs (same animal) and impostor pairs, including look-alike pairs of similar colour and breed. Reference: 96.8% dog verification accuracy (De Marsico et al.); cattle EER 0.82% (De Marsico et al.).
- **Rank-1 and Rank-5** (%): each query image from the feeder camera compared with the registered photos of all shelter animals, using animals not seen in training. Reference: 84.34% / 96.97% for dogs (De Marsico et al.); 97–98% Rank-1 on 173 animals (Fernigrini & Yun, 2026).
- **Effect of registration photos:** Rank-1 (%) with 1, 2 and 5 registered photos per animal. Reference: 54–59% on 14,716 dogs with few photos (Fernigrini & Yun, 2026).
- **Unknown-animal rejection:** the share of unregistered animals correctly rejected, and the share of registered animals still accepted, at the same threshold (%). Reference: about 68% of the right animals accepted under strict rejection (Fernigrini & Yun, 2026).
