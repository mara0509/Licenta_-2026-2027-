## Chapter 4: Object detection on edge devices and edge–cloud processing

### 1. Ranjan et al. (2026): Does YOLO26 Truly Offer Advantages Over Its Predecessors for Edge Deployment?
**Reference:** Ranjan, R., Kothawade, G. S., Sharrer, K., Tsukuda, S. & Good, C. (2026). Does YOLO26 Truly Offer Advantages Over Its Predecessors for Edge Deployment? A Benchmark Study in Aquaculture. *arXiv preprint* arXiv:2607.09835. https://doi.org/10.48550/arXiv.2607.09835

**Description:** The study tests whether YOLO26, released by Ultralytics in January 2026, is really better than older YOLO versions on a low-cost CPU-only device. Twelve models (YOLOv5u, YOLOv8, YOLO11 and YOLO26 in nano, small and medium) were trained to detect dead fish, an animal welfare indicator, and run on a Raspberry Pi 5. All reached almost the same accuracy, but YOLO26n was the fastest nano model on the Pi, thanks to its NMS-free design, which removes a post-processing step that is slow on CPUs. Small and medium models were too slow for real-time use, and GPU speed rankings did not carry over to the Pi. It is the most direct evidence for running YOLO26n on a Raspberry Pi 5.

- **Method / technology:** 12 COCO-pretrained YOLO models exported to ONNX (FP32) and run with ONNX Runtime on a Raspberry Pi 5 (8 GB, CPU only, active cooling), after 3 warm-up runs.
- **Setting and data:** salmon tank, 2,000 images from a Raspberry Pi camera, 2 classes (live, dead); 200 test images on the Pi.
- **Metrics / results:**
  - YOLO26n: 133.2 ms average, 145.1 ms 90th percentile, 7.51 FPS, 9.8 MB ONNX; mAP50 94.32%, precision 90.79%, recall 89.29%.
  - YOLOv8n 153.6 ms; small models 2.5–2.9 FPS, medium 1.04–1.25 FPS.
  - Without cooling, the Pi throttled above 80 °C.
- **What's missing:** RAM not measured (8 GB board); no NCNN or INT8; accuracy not re-checked on the Pi after export; preprint.
- **What I take:**
  - YOLO26n as the fastest nano model on a Pi 5 CPU, with 133 ms / 7.5 FPS / 9.8 MB as reference values;
  - latency measured on the Pi itself after warm-up, reported as average, maximum and 90th percentile;
  - active cooling.
- **How I improve:**
  - NCNN and INT8 compared with FP32 ONNX;
  - RAM measured on a 2 GB Pi 5.

### 2. Palazzetti et al. (2025): AntPi, a Raspberry Pi based edge–cloud system for real-time ant species detection using YOLO
**Reference:** Palazzetti, L., Giannetti, D., Verolino, A., Grasso, D. A., Pinotti, C. M. & Betti Sorbelli, F. (2025). AntPi: A Raspberry Pi based edge–cloud system for real-time ant species detection using YOLO. *Ecological Informatics*, 91, 103383. https://doi.org/10.1016/j.ecoinf.2025.103383

**Description:** AntPi is a low-cost field device that detects ant species with YOLO on a Raspberry Pi 5 and sends images and results to a cloud server with a live web gallery. The best nano model (YOLO11n) was exported to several formats and quantized, and inference time, RAM and power were measured on the Pi. NCNN and OpenVINO were about four times faster than PyTorch, RAM stayed around 600–700 MB in most formats, and INT8 caused the largest accuracy drop. Recall fell sharply on real field images, mainly because of blur. It is the only reviewed paper that measures RAM, power and export formats on a Raspberry Pi 5.

- **Method / technology:** Raspberry Pi 5 (8 GB) + Camera v3; YOLOv9/10/11 (COCO-pretrained); YOLO11n exported to PyTorch, NCNN, OpenVINO and TFLite, with FP16 and INT8 quantization; HTTP upload to a Flask server.
- **Setting and data:** 1,253 images of 5 ant species; 129 field images; 400 empty images; 250 detections per measurement.
- **Metrics / results:**
  - Pi 5 inference: PyTorch 346.5 ms, NCNN 94.6 ms, OpenVINO 79.4 ms.
  - RAM average / max: NCNN 611 / 767 MB, TFLite 1,163 / 1,390 MB.
  - Power peaks 12.7 W; third-party chargers caused shutdowns.
  - Recall 74.9% on the test set vs ~42% on field images; ≤ 0.1 false positives per empty image.
  - INT8 gave the largest mAP drop.
- **What's missing:** 8 GB board; INT8 speed and RAM not measured; no YOLO26; every image uploaded, so bandwidth savings were not studied.
- **What I take:**
  - NCNN export instead of PyTorch, and no TFLite;
  - about 600–700 MB RAM for a nano model, so it should fit a 2 GB Pi 5 powered by the official supply;
  - false positives on empty frames as a test of the filter.
- **How I improve:**
  - INT8 measured for speed and RAM as well as accuracy;
  - bandwidth saving measured as the share of frames discarded on the Pi.

### 3. Viswanathan et al. (2026): Two-Stage Wildlife Event Classification for Edge Deployment
**Reference:** Viswanathan, A. S., Bock, A., Bent, Z., Peyton, M. A., Tartakovsky, D. M. & Santos, J. E. (2026). Two-Stage Wildlife Event Classification for Edge Deployment. *Sensors*, 26(4), 1366. https://doi.org/10.3390/s26041366

**Description:** The study builds a low-cost, offline sensor on a Raspberry Pi 5 that recognizes a puma within seconds of a motion trigger. Motion-triggered cameras produce mostly empty or irrelevant images, a detector alone mislabels species, and a classifier on the whole image raises many false alarms. The authors therefore use two stages: a permissive YOLO detector only locates animals and discards empty images, and a specialised classifier decides on each cropped animal. The cascade far outperformed either stage alone. YOLO26 was slightly faster than YOLOv8 at similar accuracy. The system ran unattended in the field for months, logging every event and recovering after power and WiFi outages.

- **Method / technology:**
  - PIR camera → Raspberry Pi 5.
  - Stage 1: YOLOv8s with confidence threshold 0.25, boxes expanded by 15% and cropped, class labels ignored.
  - Stage 2: EfficientNetV2-S classifier on each crop.
  - Every event logged.
- **Setting and data:** 2,796 training images from New Mexico; 1,434 test images from California (different site and cameras); field deployment since May 2025 (12,436 triggers).
- **Metrics / results:**
  - Two-stage F1 0.979 vs 0.495 for YOLO only (pumas labelled as dog, sheep or elephant) and 0.814 for the classifier only.
  - Stage 1 on the Pi: 0.85 s (YOLOv8s) vs 0.75 s (YOLO26).
  - End to end ≈ 4 s, mostly image transfer.
- **What's missing:** RAM, export format and quantization not reported; YOLO26 tested only as the detector alone; fully offline.
- **What I take:**
  - two stages: YOLO only as a permissive animal locator (threshold ≈ 0.25, crop with a 15% margin), with the decision left to a specialised model;
  - a PIR-triggered pipeline with end-to-end latency broken down by step;
  - event logging and automatic recovery after outages.
- **How I improve:**
  - Stage 2 runs in the cloud and assesses welfare (emotion, pain) instead of confirming a species;
  - YOLO26 used in the full pipeline.

### Chapter summary
A Raspberry Pi 5 can run a nano YOLO model fast enough for motion-triggered detection: YOLO26n reached 133 ms per image, optimised formats run about four times faster than PyTorch, and a nano model uses about 600–700 MB RAM. Larger models are too slow on the Pi, INT8 can cut accuracy, and a permissive detector followed by a specialised model on the crop clearly beats either stage alone. No study measures INT8 YOLO26n for speed, RAM and accuracy together, uses a 2 GB board, or measures the bandwidth saved by uploading only relevant frames. My feeder keeps only the animal filter on the Pi and sends the crops to the cloud models for emotion and pain.

**Design decisions from this chapter**
- YOLO26n as the edge animal filter → fastest nano model on a Pi 5 CPU, 133.2 ms vs 153.6 ms for YOLOv8n (Ranjan et al., 2026); faster than YOLOv8 at similar accuracy (Viswanathan et al., 2026)
- NCNN export, INT8 tested rather than assumed, no TFLite → NCNN 3.7× faster than PyTorch, TFLite used 1.1–1.4 GB RAM, INT8 gave the largest accuracy drop (Palazzetti et al., 2025)
- A plain Raspberry Pi 5 (2 GB) with no accelerator → a nano model needs about 600–700 MB RAM (Palazzetti et al., 2025), and 7.5 FPS is enough for detection that runs only after a PIR trigger (Ranjan et al., 2026)
- Active cooler and the official 5 V / 5 A power supply → throttling above 80 °C without cooling (Ranjan et al., 2026); 12.7 W peaks and shutdowns with third-party supplies (Palazzetti et al., 2025)
- PIR → camera → YOLO26n (presence and location only, threshold ≈ 0.25, crop with a 15% margin) → emotion and pain models on the crop in the cloud → F1 0.979 for two stages vs 0.495 for YOLO alone, whose class labels were unreliable (Viswanathan et al., 2026)
- Only crops containing an animal are uploaded, tagged with time and kennel ID → AntPi uploads every image, so the saving was never measured (Palazzetti et al., 2025)
- Every event logged, with automatic restart after power or network loss and the SQLite buffer → unattended recovery from two power and four WiFi outages (Viswanathan et al., 2026)
- The filter evaluated on the feeder's own images, including empty, person-only and blurred frames → recall fell from ~75% to ~42% on real field images (Palazzetti et al., 2025)

**Metrics I will measure (from this chapter)**
- **Inference time** (ms) **and FPS:** average, maximum and 90th percentile on the Pi 5 after 3 warm-up runs, for FP32 ONNX, NCNN and INT8. Reference: 133.2 ms / 145.1 ms / 7.51 FPS, ONNX (Ranjan et al., 2026); 94.6 ms NCNN for YOLO11n (Palazzetti et al., 2025).
- **RAM use** (MB): average and maximum during inference on the 2 GB Pi 5, for the same three versions. Reference: 611 / 767 MB NCNN for YOLO11n (Palazzetti et al., 2025).
- **Filter accuracy:** precision, recall and F1 (%) on feeder images labelled animal / no animal, plus false positives per empty frame, for the same three versions so the INT8 loss is visible. Reference: precision 90.79%, recall 89.29% (Ranjan et al., 2026); ≤ 0.1 false positives per empty image (Palazzetti et al., 2025).
- **End-to-end latency** (s): PIR trigger → capture → YOLO26n → upload → cloud result stored, broken down by step, over 20+ events. Reference: ≈ 4 s, mostly image transfer (Viswanathan et al., 2026).
