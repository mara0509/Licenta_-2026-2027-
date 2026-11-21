## Chapter 4: Object detection on edge devices and edge–cloud processing

### 1. Alqahtani, Cheema & Toosi (2024): Benchmarking Deep Learning Models for Object Detection on Edge Computing Devices
**Reference:** Alqahtani, D. K., Cheema, A. & Toosi, A. N. (2024). Benchmarking Deep Learning Models for Object Detection on Edge Computing Devices. *arXiv preprint* arXiv:2409.16808. https://doi.org/10.48550/arXiv.2409.16808

**Description:** The study compares how fast, how accurate and how energy-hungry popular object detectors are on cheap edge devices. Eight COCO-pretrained models (YOLOv8 n/s/m, EfficientDet Lite0–2, two SSD variants) were run on Raspberry Pi 3, 4 and 5, with and without a Google Coral TPU accelerator, and on an NVIDIA Jetson Orin Nano. Each model ran as a web service, received images continuously for 5 minutes, and every test was repeated three times. Idle energy was subtracted to get the energy per request. On the plain Pi 5, unoptimised YOLOv8 was the slowest family, while the TPU made it much faster but roughly halved YOLOv8n's accuracy. The authors conclude that edge deployment is a trade-off between accuracy, speed and energy, and that accelerators help only if the conversion keeps accuracy.

- **Method / technology:**
  - YOLOv8 n/s/m, EfficientDet Lite0/1/2, SSD MobileNet V1, SSDLite MobileDet.
  - Formats: PyTorch on the Pis, TFLite for the TPU (input reduced to 320×320), TensorRT on the Jetson.
  - Flask service on each device, load generated with Locust.
  - Energy measured with a USB power meter, minus idle energy.
- **Setting and data:** COCO validation set (5,000 images); no custom training; Pi 3 (1 GB), Pi 4 (4 GB), Pi 5 (4 GB), Jetson Orin Nano (4 GB); 3 repetitions of 5 minutes each.
- **Metrics / results:**
  - Pi 5 inference: YOLOv8n ≈ 300–400 ms (read from a figure), YOLOv8m 1,348 ms, SSD V1 93 ms.
  - With the TPU: YOLOv8 ≈ 21–97 ms, but YOLOv8n mAP fell from ≈ 30 to 16.
  - Pi 5 mAP without the TPU: YOLOv8n ≈ 30, YOLOv8m 44, SSD V1 19.
  - Energy per request on the Pi 5: 1.02–3.58 mWh (YOLOv8), 0.47–0.62 mWh with the TPU; the TPU raised idle energy by 20%.
  - Jetson Orin Nano: YOLOv8n 16 ms, but the highest idle consumption.
- **What's missing:**
  - YOLOv8 ran in unoptimised PyTorch on the Pis, so CPU speed is underestimated.
  - RAM use and model size not reported; quantization not studied systematically.
  - COCO only, no real application; several values readable only from charts; preprint.
- **What I take:**
  - the nano model as the right size for a Pi 5;
  - the warning that compression can destroy accuracy, so accuracy is measured before and after quantization;
  - energy per inference with idle power subtracted;
  - three repetitions per test;
  - a plain Pi 5 without an accelerator, since detection runs only after a PIR trigger.
- **How I improve:**
  - YOLO26n exported to ONNX/NCNN instead of PyTorch;
  - INT8 tested with accuracy, speed and RAM measured;
  - RAM and model size reported;
  - evaluation on feeder images with animals, people and empty frames.

### 2. Ranjan et al. (2026): Does YOLO26 Truly Offer Advantages Over Its Predecessors for Edge Deployment?
**Reference:** Ranjan, R., Kothawade, G. S., Sharrer, K., Tsukuda, S. & Good, C. (2026). Does YOLO26 Truly Offer Advantages Over Its Predecessors for Edge Deployment? A Benchmark Study in Aquaculture. *arXiv preprint* arXiv:2607.09835. https://doi.org/10.48550/arXiv.2607.09835

**Description:** The study tests whether YOLO26, released by Ultralytics in January 2026, is really better than older YOLO versions on a low-cost CPU-only device. Twelve models (YOLOv5u, YOLOv8, YOLO11 and YOLO26, each in nano, small and medium) were trained to detect dead fish in a fish farm, an animal welfare indicator, and then run on a Raspberry Pi 5. All models reached almost the same accuracy, but YOLO26n was the fastest nano model on the Pi. This comes from its NMS-free design, which removes a post-processing step that is slow on CPUs. However, YOLO26 needed about 2.5 times more training images than YOLOv8 to reach the same accuracy, and GPU speed rankings did not carry over to the Pi. It is the most direct evidence available for running YOLO26n on a Raspberry Pi 5.

- **Method / technology:**
  - YOLOv5u, YOLOv8, YOLO11, YOLO26 (n/s/m), COCO-pretrained, 100 epochs, trained on an NVIDIA A100.
  - Exported to ONNX (FP32), run with ONNX Runtime on a Raspberry Pi 5 (8 GB, CPU only, active cooling).
  - Three warm-up runs; learning curve over 7 dataset sizes (100–2,800 images).
- **Setting and data:** salmon tank, 2,000 images from a Raspberry Pi camera over 90 days under two lighting conditions, 2 classes (live, dead), split 70/20/10, training set augmented to 2,800; 200 test images on the Pi.
- **Metrics / results:**
  - YOLO26n on the Pi 5: 133.2 ms average, 145.1 ms 90th percentile, 7.51 FPS, ONNX size 9.8 MB.
  - YOLOv8n: 153.6 ms / 12.3 MB; YOLO11n: 149.0 ms / 10.6 MB.
  - Accuracy: YOLO26n mAP50 94.32%, mAP50–95 71.27%, precision 90.79%, recall 89.29%; all 12 models within 1.04 mAP50 points.
  - Small models 2.5–2.9 FPS, medium 1.04–1.25 FPS.
  - Images needed for ≥ 90% mAP50: YOLO26n about 1,000, YOLOv8n 400.
  - Without cooling, the Pi 5 throttled above 80 °C; with a fan, it stayed below 70 °C.
- **What's missing:**
  - RAM use not measured (8 GB board).
  - No INT8/FP16, NCNN or OpenVINO.
  - Energy not measured; accuracy not re-checked on the Pi after export.
  - One species, two classes; preprint.
- **What I take:**
  - YOLO26n as the fastest nano model on a Pi 5 CPU;
  - 133 ms, 7.5 FPS and 9.8 MB as reference values;
  - latency reported as average, maximum, 90th percentile and FPS after warm-up;
  - testing on the target device, not on a GPU;
  - active cooling and temperature logging;
  - planning the training data size around YOLO26's higher data needs.
- **How I improve:**
  - NCNN and INT8 compared with FP32 ONNX;
  - RAM measured on a 2 GB Pi 5;
  - accuracy re-checked after export;
  - tested on feeder-camera images of cats and dogs, including empty and person-only frames.

### 3. Palazzetti et al. (2025): AntPi, a Raspberry Pi based edge–cloud system for real-time ant species detection using YOLO
**Reference:** Palazzetti, L., Giannetti, D., Verolino, A., Grasso, D. A., Pinotti, C. M. & Betti Sorbelli, F. (2025). AntPi: A Raspberry Pi based edge–cloud system for real-time ant species detection using YOLO. *Ecological Informatics*, 91, 103383. https://doi.org/10.1016/j.ecoinf.2025.103383

**Description:** AntPi is a low-cost, solar-powered field device that detects and classifies ant species on a Raspberry Pi 5 and sends the results to the cloud. The Pi captures an image, adds GPS and weather data to it, and runs YOLO on the device. It then sends the image and results to a Flask server that updates a web gallery in real time. Nine YOLO models were trained, and the best nano model (YOLO11n) was exported to several formats and quantized, with inference time, RAM, CPU load and power measured on the Pi. NCNN and OpenVINO were about four times faster than PyTorch, and RAM stayed around 600–700 MB in most formats. Accuracy dropped sharply on real field images. It is the only paper that measures the RAM, power and export-format numbers missing from the others.

- **Method / technology:**
  - Raspberry Pi 5 (8 GB) + Camera v3, temperature/humidity sensor, GPS, power bank, solar panel.
  - YOLOv9 (t/s/m), YOLOv10 (n/s/m) and YOLO11 (n/s/m), COCO-pretrained, trained on an RTX 3060.
  - YOLO11n exported to PyTorch, NCNN, OpenVINO, TFLite and MNN; FP16 and INT8 post-training quantization (252 calibration images).
  - HTTP upload to a Flask server with server-triggered updates.
- **Setting and data:**
  - 1,253 images of 5 ant species + 1 morphotype (2,873 labelled ants), split 70/20/10, training set augmented to 3,508.
  - 129 field images; 400 ant-free images for robustness.
  - 250 detections per measurement on the Pi.
- **Metrics / results:**
  - YOLO11n test set: precision 87.3%, recall 74.9%, mAP50 84.1%. Field images: precision ~89%, recall ~42%, mAP50 44.6%.
  - At most 0.1 false positives per empty image.
  - Pi 5 inference: PyTorch 346.5 ms (2.8 FPS), NCNN 94.6 ms (9.6 FPS), OpenVINO 79.4 ms (11.2 FPS), TFLite 298.9 ms.
  - RAM (average / max): NCNN 611 / 767 MB, PyTorch 625 / 798 MB, OpenVINO 690 / 867 MB, TFLite 1,163 / 1,390 MB.
  - Power 6.2–7.1 W average, peaks up to 12.7 W; third-party chargers caused shutdowns.
  - Energy per detection: YOLO11n 2.14 J vs YOLO11m 15.42 J.
  - FP16 reduced mAP50 moderately, INT8 the most; INT8 speed gains could not be measured.
- **What's missing:**
  - 8 GB board, not 2 GB.
  - Quantization reported only as accuracy, in a figure.
  - No YOLO26 or ONNX.
  - Every image is uploaded, so bandwidth savings were not studied; the cloud only stores and displays data.
- **What I take:**
  - the Pi → cloud backend → dashboard structure, with images tagged with metadata;
  - NCNN (or ONNX/OpenVINO) instead of PyTorch;
  - about 600–700 MB RAM for a nano model, which should fit a 2 GB board;
  - avoiding TFLite;
  - the official 5 V / 5 A power supply, with power logged;
  - false positives on empty frames as a test;
  - RAM, CPU and power as metrics.
- **How I improve:**
  - YOLO26n on a 2 GB Pi 5;
  - INT8 tested for speed and RAM as well as accuracy;
  - only frames with an animal uploaded, so bandwidth savings can be measured;
  - the cloud runs extra AI models (emotion, pain), not just storage;
  - evaluation on feeder-camera images, including blurred and moving animals.

### 4. Viswanathan et al. (2026): Two-Stage Wildlife Event Classification for Edge Deployment
**Reference:** Viswanathan, A. S., Bock, A., Bent, Z., Peyton, M. A., Tartakovsky, D. M. & Santos, J. E. (2026). Two-Stage Wildlife Event Classification for Edge Deployment. *Sensors*, 26(4), 1366. https://doi.org/10.3390/s26041366

**Description:** The study builds a low-cost, offline vision sensor that recognizes a puma within seconds of a motion trigger, so people can be alerted while the animal is still there. Motion-triggered cameras produce mostly empty or irrelevant images, a detector alone mislabels species, and a classifier on the whole image raises many false alarms. The authors therefore use a two-stage cascade on a Raspberry Pi 5. A permissive YOLO detector only locates animals and discards empty images. A specialised classifier then decides on each cropped animal. The cascade far outperformed either stage alone, and in field deployment it raised false alerts on less than 1% of triggers. YOLO26 was also tested and was slightly faster than YOLOv8 at similar accuracy.

- **Method / technology:**
  - Stage 1: YOLOv8s (640 px, confidence threshold 0.25, NMS IoU 0.45) used only to locate animals; boxes expanded by 15% and cropped; YOLO class labels ignored.
  - Stage 2: EfficientNetV2-S binary classifier (384×384, ImageNet-pretrained, two-step training), action at probability ≥ 0.5.
  - PIR WiFi camera → FTP → Raspberry Pi 5 with a local hotspot and Flutter interface.
  - Every event logged; Grad-CAM heatmaps.
- **Setting and data:**
  - Training: 1,103 puma + 1,693 no-puma images from New Mexico (80/20).
  - Test: 479 puma + 955 no-puma images from California, a different site with different cameras.
  - Day colour and night infrared images; field deployment since May 2025 (12,436 triggers).
- **Metrics / results:**
  - Two-stage: precision 0.983, recall 0.975, F1 0.979, balanced accuracy 0.983, 8 false positives / 12 false negatives.
  - YOLO only: recall 0.334 (319 pumas missed, labelled as dog, sheep or elephant).
  - Classifier only: 171 false alarms (precision 0.723).
  - Pi 5 time: Stage 1 0.85 s (YOLOv8s) or 0.75 s (YOLO26), Stage 2 ≈ 0.55 s.
  - End to end ≈ 4 s, mostly the FTP transfer.
  - Field false alerts 0.8% (98 / 12,436); about 8 W total.
- **What's missing:**
  - One target species; most missed animals were in low-contrast night images.
  - RAM, export format and quantization not reported (times suggest unoptimised models).
  - YOLO26 tested only in the YOLO-only baseline.
  - Fully offline, no cloud.
- **What I take:**
  - YOLO as a permissive locator and empty-frame filter, with the decision left to a specialised model;
  - a low confidence threshold to keep recall high;
  - crops with a 15% margin;
  - PIR → capture → filter → decision;
  - end-to-end latency broken down by step;
  - baselines (detector only, classifier only);
  - a field false alert rate;
  - event logging and automatic recovery after outages;
  - Grad-CAM to explain errors.
- **How I improve:**
  - Stage 2 (emotion and pain) runs in the cloud;
  - YOLO26n is optimised (NCNN/INT8) and its RAM measured on a 2 GB Pi 5;
  - the task is welfare assessment rather than species confirmation;
  - only the crop is uploaded.

### Chapter summary
The reviewed studies show that a Raspberry Pi 5 can run a nano YOLO model fast enough for motion-triggered detection:
- YOLO26n reached 133 ms per image with ONNX, the fastest nano model on the Pi.
- Optimised formats run about four times faster than PyTorch.
- A nano model uses about 600–700 MB RAM.

They also show that larger models are too slow on the Pi (1–3 FPS) and that compression can cut accuracy sharply. A permissive detector followed by a specialised model on the crop clearly beats either stage alone.

The gaps across the papers are that none measures INT8 YOLO26n for speed, RAM and accuracy together, none uses a 2 GB board, and none measures the bandwidth saved by uploading only relevant frames. My feeder fills these gaps and keeps only the animal filter on the edge, sending the crops to cloud models for emotion and pain.

**Design decisions from this chapter**
- YOLO26n (nano) as the edge animal filter → fastest nano model on a Pi 5 CPU, 133.2 ms vs 153.6 ms for YOLOv8n, thanks to its NMS-free design (Ranjan et al., 2026); slightly faster than YOLOv8 at similar accuracy (Viswanathan et al., 2026)
- Nano instead of small or medium models → larger models run at only 1–3 FPS on the Pi (Ranjan et al., 2026) and use about 7× more energy per detection (Palazzetti et al., 2025); YOLOv8m took 1,348 ms (Alqahtani et al., 2024)
- Export to NCNN / ONNX instead of PyTorch, no TFLite → NCNN was about 3.7× faster than PyTorch, and TFLite used 1.1–1.4 GB RAM (Palazzetti et al., 2025); PyTorch is a weak baseline (Alqahtani et al., 2024)
- INT8 quantization tested, not assumed → TPU compression dropped YOLOv8n mAP from 30 to 16 (Alqahtani et al., 2024); INT8 gave the largest mAP drop (Palazzetti et al., 2025)
- A plain Raspberry Pi 5 (2 GB), no Coral TPU or Jetson → a nano model needs about 600–700 MB RAM (Palazzetti et al., 2025); accelerators add cost and idle power and can hurt accuracy (Alqahtani et al., 2024)
- Active cooler and the official 5 V / 5 A power supply → the Pi 5 throttled above 80 °C without cooling (Ranjan et al., 2026); peaks reached 12.7 W, and third-party supplies caused shutdowns (Palazzetti et al., 2025)
- Two stages: YOLO26n filter on the Pi, emotion and pain models on the crop in the cloud → F1 0.979 for the cascade vs 0.495 (detector only) and 0.814 (classifier only) (Viswanathan et al., 2026)
- YOLO used only for "animal present + where", not for the species or welfare decision → YOLO labelled pumas as dogs, sheep or elephants (Viswanathan et al., 2026)
- Permissive confidence threshold (about 0.25) and crop with a 15% margin → keeps recall high while Stage 2 removes extra candidates (Viswanathan et al., 2026)
- PIR sensor triggers the camera, so detection runs only on motion → avoids continuous inference on mostly empty frames (Viswanathan et al., 2026); a slow CPU model is acceptable for event-based detection (Alqahtani et al., 2024)
- Only animal frames uploaded, tagged with time and kennel ID → AntPi uploads every image, so bandwidth savings were never measured (Palazzetti et al., 2025)
- Every event logged, with automatic restart after power or network loss (plus the SQLite buffer) → the system recovered from two power and four WiFi outages unattended (Viswanathan et al., 2026)
- Detector evaluated on images from the feeder's own camera, including empty, person-only and blurred frames → recall fell from ~75% to ~42% on real field images (Palazzetti et al., 2025)

**Metrics I will measure (from this chapter)**
- **YOLO26n inference time** (ms) **and FPS:** average, maximum and 90th percentile over the test images on the Pi 5, after 3 warm-up runs, for ONNX FP32, NCNN and INT8. Reference: 133.2 ms / 145.1 ms 90th percentile / 7.51 FPS, ONNX (Ranjan et al., 2026); 94.6 ms NCNN for YOLO11n (Palazzetti et al., 2025).
- **RAM use** (MB): average and maximum during inference on the 2 GB Pi 5, logged with a monitoring script. Reference: 611 / 767 MB NCNN, 625 / 798 MB PyTorch for YOLO11n (Palazzetti et al., 2025).
- **Animal-filter accuracy:** precision, recall and F1 (%) on feeder-camera images labelled as animal / no animal (empty, person-only, blurred), plus false positives per empty frame. Reference: precision 90.79%, recall 89.29% (Ranjan et al., 2026); ≤ 0.1 false positives per empty image (Palazzetti et al., 2025).
- **Effect of INT8 quantization:** change in recall / mAP50 (points), inference time (ms), model size (MB) and RAM (MB), INT8 vs FP32. Reference: 9.8 MB FP32 ONNX (Ranjan et al., 2026); mAP 30 → 16 after TPU compression (Alqahtani et al., 2024).
- **End-to-end latency** (s): from PIR trigger → capture → YOLO26n → upload → cloud result stored, broken down by step, over 20+ events. Reference: ≈ 4 s, mostly image transfer (Viswanathan et al., 2026).
- **Power** (W): average and peak during idle and inference, measured with a USB power meter; CPU temperature logged. Reference: 6.2–7.1 W average, 12.7 W peak (Palazzetti et al., 2025); throttling above 80 °C (Ranjan et al., 2026).
