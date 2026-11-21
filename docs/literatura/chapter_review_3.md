## Chapter 3: Individual animal identification

### 1. De Marsico, Jain & Miglino (year): Beyond Humans: Multispecies Animal Face Recognition Using Transfer Learning
**Reference:** De Marsico, M., Jain, A. K. & Miglino, A. (year). Beyond Humans: Multispecies Animal Face Recognition Using Transfer Learning. *Journal / Conference*, volume, pages. DOI

**Description:** The study tests whether models pre-trained on very large datasets can recognize individual animals by their face, as a non-invasive alternative to microchips and tags. Two pre-trained models were used as fingerprint extractors: FaceNet, trained on human faces, and a Vision Transformer (ViT), trained on ImageNet. They were tested on dogs, primates and cattle, always on individuals never seen in training. On dogs, ViT clearly beat FaceNet and dog-specific networks trained from scratch. The authors separate verification (is this the claimed animal?) from identification (which animal is this?). They warn that accuracy alone hides false acceptances, so the false acceptance rate at a fixed threshold should be reported.

- **Method / technology:** FaceNet and ImageNet ViT as feature extractors, adapted with triplet training; evaluated as verification (1:1) and identification (1:N).
- **Setting and data:** 8,363 internet face photos of 1,393 dogs (2–41 per dog); test on 93 dogs never seen in training; also primates and cattle.
- **Metrics / results:**
  - Dog verification: ViT 96.8%, FaceNet 88.2%, dog-specific DogFaceNet 76.9%.
  - Dog identification Rank-1: ViT 84.34% vs 39.74% and 10.96% for dog-specific models trained from scratch.
  - Head pose, lighting, blur and similar coat colours caused most errors.
- **What's missing:** the dog accuracy uses the best threshold chosen on the test data, so it is optimistic; impostors are random dogs, not look-alikes; no cats.
- **What I take:**
  - a large pre-trained ViT used as a fingerprint extractor instead of a network trained from scratch;
  - verification (1:1) as the task for a feeder assigned to one animal, measured with FAR and FRR at a fixed threshold rather than accuracy.
- **How I improve:**
  - cats as well as dogs;
  - frontal faces from a fixed camera at the bowl instead of internet photos.

### 2. Fernigrini & Yun (2026): A Dual-Stream CLIP–ViT Framework for Open-Set Animal Re-Identification
**Reference:** Fernigrini, … & Yun, … (2026). A Dual-Stream CLIP–ViT Framework for Open-Set Animal Re-Identification. *Journal of Imaging*, 12(8), 354. https://doi.org/10.3390/jimaging12080354

**Description:** The study proposes a system that recognizes individual cats and dogs from ordinary photos, for example to help find lost pets. After YOLOv8 crops the animal, a frozen CLIP model and a ViT produce an identity fingerprint, which is compared with stored photos. Because the main model stays frozen, new animals are added by registering photos, with no retraining. On 173 animals never seen in training, the first guess was correct 97–98% of the time. Accuracy fell sharply when unregistered animals had to be rejected strictly, with blurred or low-resolution images, and with thousands of dogs.

- **Method / technology:** YOLOv8 crop → frozen CLIP + partly fine-tuned ViT → 512-number fingerprint, matched by similarity to stored fingerprints.
- **Setting and data:** CatIndividuals (phone photos of cats), MultiposeDog and PetFace (dogs); main test on 96 dogs + 77 cats never seen in training.
- **Metrics / results:**
  - Rank-1 97–98% on 173 animals.
  - Only about 68% of the right animals accepted when unregistered animals are rejected strictly.
  - Low resolution −16 points, motion blur −13 points.
  - 54–59% on 14,716 unseen dogs with 2–5 photos each.
- **What's missing:** designed for random owner photos; performance collapses with very large galleries; strict rejection costs much accuracy.
- **What I take:**
  - a frozen model with fingerprint comparison, so a new animal is added by registering 2–5 photos;
  - an "unsure" outcome instead of a forced match;
  - sharp, full-resolution frames, since blur and low resolution caused the most errors.
- **How I improve:**
  - each fingerprint compared only with the one animal assigned to the kennel, captured by a fixed camera;
  - when unsure, the caretaker confirms instead of the system choosing between strict rejection and a wrong match.

### Chapter summary
Both studies show that a large pre-trained model used as a fingerprint extractor recognizes individual animals far better than networks trained from scratch, and that new animals can be added without retraining. Performance drops with blur, low resolution, unusual head poses and look-alike animals, and strictly rejecting unknown animals costs accuracy. Neither uses a fixed camera or links identity to care decisions.

In my system, Phase 1 takes the animal's identity from the kennel assignment: the camera only detects that an animal is present. Phase 2 (future work) verifies that the animal at the bowl is the assigned one, using a frozen DINOv2 fingerprint.

**Design decisions from this chapter**
- Phase 1: one animal per kennel; the camera only detects an animal, and the identity comes from the kennel assignment → individual recognition drops with look-alikes, blur and pose, and strict rejection accepts only about 68% of the right animals (De Marsico et al.; Fernigrini & Yun, 2026)
- Phase 2 (future work) as verification against the assigned animal, not identification among all animals → a feeder per kennel only needs the 1:1 check (De Marsico et al.)
- Frozen DINOv2 fingerprints with no training, about 5 registration photos per animal, stored with the animal's profile → a pre-trained ViT reached 84.34% vs 39.74% for networks trained from scratch (De Marsico et al.); a frozen model adds new animals without retraining, and 2–5 photos work (Fernigrini & Yun, 2026)
- The sharpest frame of each visit is used → blur and low resolution were the largest causes of error (Fernigrini & Yun, 2026; De Marsico et al.)
- Low similarity sends a warning to the caretaker, never an automatic action; medication is always confirmed by the caretaker → accuracy hides false acceptances, and the wrong animal accepted is the dangerous error (De Marsico et al.)
- Species check (YOLO says cat or dog, compared with the assigned animal) shown only as a warning → YOLO's class labels are unreliable (Viswanathan et al., 2026, Chapter 4)

**Metrics I will measure (from this chapter, if Phase 2 is implemented)**
- **Verification error:** FAR and FRR (%) at the chosen threshold, and EER (%), on genuine pairs (same animal) and impostor pairs, including look-alikes of similar colour. Reference: 96.8% dog verification accuracy, cattle EER 0.82% (De Marsico et al.).
- **Effect of registration photos:** FRR at the same threshold with 1, 2 and 5 registered photos per animal. Reference: 2–5 photos already work, 54–59% Rank-1 on a very large gallery (Fernigrini & Yun, 2026).
