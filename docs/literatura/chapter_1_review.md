## Chapter 1: IoT smart pet feeders

### 1. Pico-Valencia & Holgado-Terriza (2025): The Internet of Things Empowering the Internet of Pets
**Reference:** Pico-Valencia, P. & Holgado-Terriza, J. A. (2025). The Internet of Things Empowering the Internet of Pets—An Outlook from the Academic and Scientific Experience. *Applied Sciences*, 15(4), 1722. https://doi.org/10.3390/app15041722

**Description:** This systematic review maps how IoT has been used for pet care, based on 81 prototypes and systems published between 2010 and 2024. Following the PRISMA and Kitchenham & Charters guidelines, the authors classified the systems by function, hardware, software, communication and place of use. Feeding is the most common function (79% of prototypes). Most systems are built from low-cost boards such as ESP32, Arduino and Raspberry Pi, combined with load cells, PIR sensors and servo motors. Almost all target one pet at home, AI is used only to detect or recognize the pet, and many prototypes are never properly evaluated. The review is my main reference for where the field stands and which gaps remain.

- **Method / technology:** systematic literature review (PRISMA + Kitchenham & Charters), 5 research questions, search string ("internet of things" OR "iot") AND ("pet"), inclusion and exclusion criteria.
- **Setting and data:** 206 records from Web of Science, Scopus, Scielo, Dialnet, DOAJ, ProQuest and Google Scholar, reduced to 81 studies. 67 of the 81 were published since 2020 and 59 come from Asia. 15 target dogs, 14 cats and 43 pets in general.
- **Metrics / results:**
  - Functions: feeding 79%, surveillance 49%, health monitoring 25%.
  - Place of use: 93% at home, 6% in veterinary clinics, none in shelters.
  - Raspberry Pi used in 17 studies; medication dispensed by only 2 of 81 systems; MQTT used by only 2.
  - AI: no system analyzes the animal's face, and none reports accuracy for its AI.
  - Reported dispensing accuracy: < 5% error for portions ≥ 200 g (S30), 96.94% portion precision (S49), 98% portion accuracy (S59).
- **What's missing:** no system for shelters; no system interprets the collected data to recommend a vet visit; the quality of each study's metrics is not assessed (the abstract says 36 papers, the methodology 81).
- **What I take:**
  - the review method as the structure of my own review;
  - confirmation of my hardware (Raspberry Pi + ESP32, HX711 load cells, PIR, servos, camera);
  - dispensing accuracy as a metric, with reference values;
  - splitting processing between edge and cloud;
  - portions based on energy requirements, breed and weight.
- **How I improve:**
  - a shelter setting with many animals instead of one pet at home;
  - water intake measured with a load cell;
  - a medication dispenser;
  - emotion and pain recognition from the face;
  - an LLM agent that recommends vet visits;
  - secure MQTT to Azure IoT Hub;
  - quantitative evaluation of every part.

### 2. Aguilar Alvarez et al. (2021): Pet feeder monitoring and remote control based on IoT
**Reference:** Aguilar Alvarez, S., Hinojosa Altamirano, R., Hidalgo Lascano, P. & Cruz Dávalos, P. (2021). Monitoreo y control remoto de un dispensador de alimento para mascotas basado en IoT. *Revista de Investigación en Tecnologías de la Información (RITI)*, 9(17), 77–88. https://doi.org/10.36825/RITI.09.17.008

**Description:** The paper presents an IoT device that dispenses both food and water in one unit and is configured and monitored from an Android app over MQTT. An Arduino Mega controls all sensors and actuators, while a Raspberry Pi 3B with a camera handles communication, the feeding schedule and video streaming. Food is dosed by weight with a load cell, and the app suggests a daily ration from the dog's breed, weight and size. The evaluation is one of the most complete among feeder papers: about 300 dispensing tests, water-change and power-backup tests, and a 7-day test with a real dog. Dispensing was punctual and reliable, but accuracy depended strongly on portion size. The architecture is the closest to mine, and the paper gives the most useful reference values.

- **Method / technology:**
  - Arduino Mega 2560 (sensors and actuators) + Raspberry Pi 3B (MQTT with Paho, camera with MJPG-streamer), linked over serial.
  - Food: auger with a DC motor, 5 kg load cell, infrared level sensors, current sensor to detect jams.
  - Water: ultrasonic level sensors and three servo-valves (fill, dispense, drain).
  - Server: Mosquitto broker, Apache, MySQL. Power backup: UPS.
- **Setting and data:**
  - Lab tests with small, medium and large dog profiles: 5 tests each for 1, 2 and 3 portions per day, checked with a commercial scale.
  - 20 water-change tests.
  - 7 days with a medium-sized dog, 300 g/day in 3 portions.
- **Metrics / results:**
  - Dispensing error < 5% for portions ≥ 200 g, rising to about 30–35% around 50 g (trend R² = 0.43, very scattered).
  - On-time dispensing 306/306 (100%); food jams in 7% of about 300 tests; water changes 20/20.
  - Video delay 5–10 s over mobile data and 30–40 s over ADSL.
  - About 3.5 h of UPS autonomy; power consumption 24 W.
- **What's missing:**
  - Food and water intake are not computed: the load cell doses food, and the level sensors only refill and drain the water.
  - Small portions are inaccurate.
  - Notification delay is called "instant" but never measured.
  - No image analysis and no identification of the animal.
- **What I take:**
  - the Raspberry Pi + microcontroller split;
  - MQTT topics prefixed with the device ID, so many feeders share one broker;
  - separate command and data topics;
  - a ration suggested from breed, weight and size;
  - jam and low-level alerts;
  - draining stale water;
  - their test protocol and reference values.
- **How I improve:**
  - food and water intake measured per meal with load cells;
  - dispensing accuracy tested on small portions (25–100 g) with enough repetitions;
  - notification latency measured;
  - one analyzed snapshot per visit instead of a live stream;
  - a platform with several roles instead of one owner's app.

### 3. Mahadiputra et al. (2024): Automatic Pet Feeder Rotational Model Using MQTT and Mobile Application
**Reference:** Mahadiputra, P. G. K., Suarjaya, I. M. A. D. & Wibawa, K. S. (2024). Automatic Pet Feeder Rotational Model Using MQTT and Mobile Application. *Jurnal Ilmiah Merpati*, 12(2), 113–124. e-ISSN 2685-2411.

**Description:** The authors built a rotational cat feeder with three pre-filled compartments, so it can hold any type of food, including wet food, unlike typical dry-food dispensers. An ESP32 reads one load cell under each compartment, keeps time with an RTC, and drives a stepper motor that rotates the lid to open the next compartment at the times set in an Android app. The app and the feeder communicate over secure MQTT (port 8883) through a HiveMQ broker, with eight topics split into control and monitoring. The evaluation is detailed and transparent, with full tables for load-cell error, transmission delay, motor rotation and scheduled feeding. It is my main reference for load-cell accuracy and MQTT delay.

- **Method / technology:**
  - ESP32, 3 load cells + HX711, 28BYJ-48 stepper motor, DS3231 RTC, 3D-printed container.
  - HiveMQ broker, MQTT over TLS with per-client credentials, 8 topics.
  - Android app (Java, Paho); schedules stored on the phone.
- **Setting and data:** lab tests only:
  - 20 objects (8.5–169.9 g) per compartment, compared with a kitchen scale;
  - 20 delay measurements per compartment;
  - 20 motor rotations;
  - 10 scheduled feedings.
- **Metrics / results:**
  - Average load-cell error 0.68%, 1.46% and 1.91% for the three compartments. Readings were equal to or slightly below the real weight.
  - Errors of 4.4–7.1% for objects under about 15 g, mostly < 1% above about 50 g.
  - Device → app delay 1.0–1.2 s on average.
  - Motor: only 2 of 20 rotations reached exactly 90° (mean deviation 5.4°).
  - Scheduled feedings 9/10 (90%). The one failure was a command sent while the motor was still turning.
- **What's missing:**
  - Food eaten is not calculated; no water module; no camera or animal identification.
  - Data is stored only on one phone.
  - Small inconsistencies between the abstract and the tables.
- **What I take:**
  - the load-cell test protocol (about 20 known weights vs a reference scale, with focus on small weights);
  - separate control and monitoring topics;
  - MQTT over TLS with per-device credentials;
  - QoS as an explicit design choice;
  - delay measured over repeated trials;
  - feeding success rate as a metric;
  - the welfare argument (freedom from hunger; low water intake linked to urinary disease in cats).
- **How I improve:**
  - a cloud database shared by several roles;
  - actual intake per meal, including water;
  - a servo instead of a stepper motor turning a lid;
  - commands sent through Azure IoT Hub cloud-to-device messages, which wait until the device receives them, so a busy feeder doesn't lose them.

### 4. Ahmad & Nawi (2025): IoT-Based Pet Care System
**Reference:** Ahmad, A. N. & Nawi, I. M. (2025). IoT-Based Pet Care System. *2025 IEEE International Conference on Automatic Control and Intelligent Systems (I2CACIS)*, Kuala Lumpur, 66–69. https://doi.org/10.1109/I2CACIS65476.2025.11100797

**Description:** The paper proposes a pet care system for cats that combines an automatic feeder, a smart water fountain and a litter box monitor. Each module has its own ESP32 and sensors, and all three are synchronized through Firebase and managed from one mobile app. The authors' main argument is that records of food intake, water intake and litter-box use can reveal irregularities that are early signs of disease. Each module identifies the cat with RFID; the feeder measures food with a load cell, while the fountain estimates water intake with a water level sensor. The evaluation is only qualitative, but it reports useful failures. The paper supports using intake as a health indicator and shows why water must be weighed.

- **Method / technology:**
  - three ESP32 modules, RFID readers, ultrasonic presence sensors;
  - DC-motor food dispenser with a load cell;
  - water level sensors, pump and solenoid valve;
  - litter box with moisture and weight sensors;
  - Firebase and one mobile app.
- **Setting and data:** prototype tested with cats; sensor readings compared with actual measurements; the number of trials is not reported.
- **Metrics / results:** no numerical results.
  - Food was released only to the registered RFID tag, but portions differed from the target (attributed to the DC motor).
  - The water level sensor missed intakes below 100 ml.
  - RFID worked only at about 2 cm.
  - Urination and defecation were sometimes misclassified.
- **What's missing:** no quantitative evaluation; the paper says servo in the design and DC motor in the results; the app only displays data, with no detection of deviations.
- **What I take:**
  - food and water intake as early indicators of disease;
  - independent modules synchronized through a cloud database, with one app;
  - identifying the animal before dispensing;
  - the water bowl on a load cell, as in the earlier designs they cite;
  - refill events recorded separately so they are not counted as negative intake.
- **How I improve:**
  - water intake measured with a load cell, with the smallest detectable intake tested;
  - camera identification instead of RFID, which their own future work proposes;
  - servo dispensing;
  - automatic deviation detection interpreted by an AI agent.

### 5. Saadoon & Koyuncu (2023): Intelligence feeder system for stray cats
**Reference:** Saadoon, A. M. & Koyuncu, H. (2023). Intelligence feeder system for stray cats. *Indonesian Journal of Electrical Engineering and Computer Science*, 31(3), 1507–1514. https://doi.org/10.11591/ijeecs.v31.i3.pp1507-1514

**Description:** The paper presents an automatic feeder for stray cats that provides food and water only after a camera recognizes a cat. A Jetson Nano runs SSD MobileNet V2, pretrained on COCO and used without extra training. The system confirms the detection 15 seconds later before an Arduino Mega opens the food valve or starts the water pump. Ultrasonic and water level sensors check whether the bowls and tanks are empty, and a GSM module sends an SMS with the GPS location when a tank needs refilling. It is the closest feeder to mine in using on-device camera detection to trigger dispensing. However, the reported "accuracy" values are confidence scores on four images, so its real detection performance is unknown.

- **Method / technology:**
  - Jetson Nano + webcam running SSD MobileNet V2 (COCO);
  - Arduino Mega with a servo food valve and a water pump;
  - ultrasonic and water level sensors;
  - SIM800L GSM + GPS;
  - GY-30 light sensor with a lamp for night operation.
- **Setting and data:** designed for outdoor stray cats; detection tested on 4 cat photos; GSM alerts and the light sensor tested separately.
- **Metrics / results:**
  - Confidence scores of 93.2–99.2% on the 4 images, reported as "accuracy".
  - Light: 1,263 lux in daylight vs 2.5 lux at twilight.
  - SMS alerts sent only when a tank is empty.
- **What's missing:**
  - No precision, recall, false detections or inference time.
  - No tests with real cats at the feeder.
  - Food is not weighed.
  - The Jetson Nano works against the low-cost goal.
- **What I take:**
  - dispensing only after on-device camera detection;
  - confirming the detection after a delay;
  - a pretrained COCO detector that already covers cats and dogs;
  - a powerful board for vision plus a microcontroller for actuators;
  - empty-tank alerts and lighting for night images;
  - the stray-animal welfare motivation.
- **How I improve:**
  - YOLO26n on a cheaper Raspberry Pi 5;
  - detection evaluated with precision, recall, false detections and inference time on images from the feeder's own camera, including empty and person-only frames;
  - food and water weighed;
  - dogs as well as cats;
  - each meal linked to a known animal's history.

### 6. Castillo-Arceo et al. (2024): Design and Development of a Smart Pet Feeder with IoT and Deep Learning
**Reference:** Castillo-Arceo, O. E., Renteria-Flores, R. U. & Santana-Mancilla, P. C. (2024). Design and Development of a Smart Pet Feeder with IoT and Deep Learning. *Engineering Proceedings*, 82(1), 63. https://www.mdpi.com/2673-4591/82/1/63

**Description:** The paper presents a dog feeder that combines a camera, a deep learning detector, a weight sensor and a servo, the combination closest to my own hardware. A YOLOv5 model trained on the Stanford Dogs dataset detects the dog, and an ultrasonic sensor makes sure food is dispensed only when a dog is close to the bowl. An HX711 weight sensor measures the food and the pet's weight, and a servo releases the portion. The results are described only qualitatively ("high precision", "accurately measured"), with no detection metrics or weight error. The paper does not state where YOLOv5 runs, although an Arduino Mega cannot run it. It shows that camera-based feeders exist but are rarely evaluated.

- **Method / technology:** Arduino Mega 2560, USB camera, YOLOv5, HX711 weight sensor, HC-SR04 ultrasonic sensor, servo motor.
- **Setting and data:** Stanford Dogs dataset (> 20,000 images, 120 breeds), annotated with labelImg; prototype tests with dogs.
- **Metrics / results:** no precision, recall, mAP, inference time or weight error reported.
- **What's missing:** no quantitative evaluation; the device that runs the model is not stated; no individual recognition; no water or medication module.
- **What I take:** camera + YOLO + weight sensor + servo + proximity trigger as a proven combination; training on a public dog dataset.
- **How I improve:**
  - YOLO26n on a Raspberry Pi 5, which can actually run the model, with heavier models in the cloud;
  - precision, recall, mAP@0.5, inference time and weight error measured;
  - water and medication modules;
  - emotion and pain recognition.

### Chapter summary
The reviewed feeders confirm that a low-cost controller with load cells, servos and a camera is a common, workable design, and that several systems already split the work between a Raspberry Pi and a microcontroller. Their limits recur across the papers:
- None measures how much water the animal drinks.
- Only one dispenses medication.
- Cameras are used only for live video or to detect presence.
- Data is shown to the owner but not interpreted.
- The evaluation is often qualitative, or uses confidence scores as accuracy.

My feeder keeps the proven hardware pattern and adds:
- food and water intake measured per meal;
- medication dispensing;
- face-based welfare monitoring;
- an AI agent that interprets the data;
- a measured evaluation with the reference values below.

**Metrics I will measure (from this chapter)**
- **Dispensing error** (g and %): 20 dispenses for each target portion (25, 50, 100 and 200 g), each weighed on a reference kitchen scale (0.1 g resolution). Reported as mean absolute error, mean % error and standard deviation. Reference: < 5% for portions ≥ 200 g and about 30–35% around 50 g (Aguilar Alvarez et al., 2021); 96.94–98% portion accuracy (S49, S59 in Pico-Valencia & Holgado-Terriza, 2025).
- **Load-cell error** (%) **and smallest detectable intake** (g / ml, 1 ml of water ≈ 1 g):
  - Error: about 20 known weights (5–500 g) placed on the food and water scales and compared with the reference scale.
  - Smallest detectable intake: known volumes (5, 10, 20, 50 ml) removed from the water bowl with a syringe, to find the smallest change the system detects.
  - Reference: 0.68–1.91% average error, rising to 4–7% below 15 g (Mahadiputra et al., 2024); a water level sensor missed intakes below 100 ml (Ahmad & Nawi, 2025).
- **Latency** (s), measured as timestamps over 20 repetitions and reported as mean, maximum and 90th percentile:
  - command → dispense: from the confirmed donation or scheduled command until the servo moves;
  - event → dashboard: from the feeder event until it appears in the web app.
  - Reference: 1.0–1.2 s device → app over MQTT (Mahadiputra et al., 2024); notifications called "instant" but not measured (Aguilar Alvarez et al., 2021).
- **Command success rate and jam rate** (%): executed commands out of all scheduled or triggered commands (food, medication and donations), and jams out of all dispenses. The test includes commands sent while the feeder is busy or offline. Reference: 9/10 = 90% (Mahadiputra et al., 2024); 306/306 = 100% with 7% jams (Aguilar Alvarez et al., 2021).
