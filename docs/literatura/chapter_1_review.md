## Chapter 1: IoT smart pet feeders

### 1. Pico-Valencia & Holgado-Terriza (2025): The Internet of Things Empowering the Internet of Pets
**Reference:** Pico-Valencia, P. & Holgado-Terriza, J. A. (2025). The Internet of Things Empowering the Internet of Pets—An Outlook from the Academic and Scientific Experience. *Applied Sciences*, 15(4), 1722. https://doi.org/10.3390/app15041722

**Description:** This systematic review maps how IoT has been used for pet care, based on 81 prototypes published between 2010 and 2024. Following the PRISMA and Kitchenham & Charters guidelines, the authors classified the systems by function, hardware, software, communication and place of use. Feeding is the most common function (79%), and most systems are built from low-cost boards such as ESP32, Arduino and Raspberry Pi, with load cells, PIR sensors and servos. Almost all target one pet at home, AI is used only to detect or recognize the pet, and many prototypes are never properly evaluated. The review shows where the field stands and which gaps my project addresses.

- **Method / technology:** systematic review (PRISMA + Kitchenham & Charters), 206 records from seven databases reduced to 81 studies.
- **Setting and data:** prototypes from 2010–2024; 15 target dogs, 14 cats, 43 pets in general.
- **Metrics / results:**
  - 93% of systems are used at home and none in shelters.
  - Only 2 of 81 dispense medication, and no system analyzes the animal's face.
  - Reported dispensing results: < 5% error for portions ≥ 200 g, 96.94–98% portion accuracy.
- **What's missing:** no shelter systems; no system interprets the data or recommends a vet visit; the quality of each study's metrics is not assessed.
- **What I take:**
  - the gaps that define my contribution (shelter setting, medication, face analysis, data interpretation);
  - confirmation of my hardware: Raspberry Pi + microcontroller, HX711 load cells, PIR, servos;
  - dispensing accuracy as a metric, with reference values.
- **How I improve:**
  - a feeder for a shelter with many animals;
  - water intake, medication and face-based welfare monitoring in one feeder;
  - an AI agent that turns the data into vet-visit recommendations.

### 2. Aguilar Alvarez et al. (2021): Pet feeder monitoring and remote control based on IoT
**Reference:** Aguilar Alvarez, S., Hinojosa Altamirano, R., Hidalgo Lascano, P. & Cruz Dávalos, P. (2021). Monitoreo y control remoto de un dispensador de alimento para mascotas basado en IoT. *Revista de Investigación en Tecnologías de la Información (RITI)*, 9(17), 77–88. https://doi.org/10.36825/RITI.09.17.008

**Description:** The paper presents a device that dispenses food and water in one unit, configured and monitored from an Android app over MQTT. An Arduino Mega controls all sensors and actuators, while a Raspberry Pi 3B handles communication, the schedule and the camera, linked over serial. Each feeder publishes on MQTT topics prefixed with its own ID, so many feeders can share one broker without changing the design. The evaluation includes about 300 dispensing tests across portion sizes and a 7-day test with a real dog. Dispensing was punctual, but accuracy depended strongly on portion size. It is the architecture closest to mine and the source of my dispensing reference values.

- **Method / technology:** Arduino Mega 2560 (sensors, actuators) + Raspberry Pi 3B (MQTT, camera, schedule); auger with DC motor and 5 kg load cell; Mosquitto broker and MySQL server; Android app.
- **Setting and data:** small, medium and large dog profiles, 5 tests each for 1–3 portions per day, checked on a commercial scale; 7 days with one dog.
- **Metrics / results:**
  - Dispensing error < 5% for portions ≥ 200 g, but about 30–35% around 50 g (R² = 0.43).
  - 306/306 portions on time; jams in 7% of tests.
- **What's missing:** food eaten and water drunk are not computed; small portions are inaccurate; notification delay is never measured.
- **What I take:**
  - the Raspberry Pi + microcontroller split;
  - one identity per feeder so many feeders share one cloud hub;
  - dispensing error measured over repeated portions of different sizes.
- **How I improve:**
  - food and water intake measured per meal;
  - dispensing accuracy tested on small portions;
  - notification latency measured.

### 3. Mahadiputra et al. (2024): Automatic Pet Feeder Rotational Model Using MQTT and Mobile Application
**Reference:** Mahadiputra, P. G. K., Suarjaya, I. M. A. D. & Wibawa, K. S. (2024). Automatic Pet Feeder Rotational Model Using MQTT and Mobile Application. *Jurnal Ilmiah Merpati*, 12(2), 113–124. e-ISSN 2685-2411.

**Description:** The authors built a cat feeder with three compartments, each with its own load cell and HX711, controlled by an ESP32 that opens the next compartment at times set in an Android app. Feeder and app communicate over secure MQTT (TLS, port 8883) with per-client credentials. Commands to the feeder and data from the feeder use separate topics. The evaluation is unusually transparent: load cells tested with 20 known weights against a kitchen scale, transmission delay measured 20 times, and scheduled feedings counted. Their one failed feeding was a command that arrived while the motor was still turning. It is my main reference for load-cell accuracy, MQTT delay and command handling.

- **Method / technology:** ESP32, 3 load cells + HX711, stepper motor, RTC; HiveMQ broker, MQTT over TLS, 8 topics split into control and monitoring; Android app with schedules stored on the phone.
- **Setting and data:** lab tests: 20 objects (8.5–169.9 g) per compartment, 20 delay measurements, 20 motor rotations, 10 scheduled feedings.
- **Metrics / results:**
  - Load-cell error 0.68–1.91% on average, rising to 4–7% for objects under 15 g.
  - Device → app delay 1.0–1.2 s.
  - Motor reached exactly 90° in only 2 of 20 rotations.
  - Scheduled feedings 9/10, with the failure caused by a command arriving while the feeder was busy.
- **What's missing:** no intake calculation, no water measurement; data stored on one phone.
- **What I take:**
  - the load-cell test protocol: about 20 known weights vs a reference scale, with focus on small weights;
  - commands down and telemetry up over MQTT with TLS and per-device credentials;
  - delay measured over repeated trials.
- **How I improve:**
  - a cloud database shared by admin, veterinarian and visitors;
  - commands queued by Azure IoT Hub until the feeder receives them, so a busy feeder doesn't lose them;
  - servo dispensers instead of a stepper motor.

### 4. Ahmad & Nawi (2025): IoT-Based Pet Care System
**Reference:** Ahmad, A. N. & Nawi, I. M. (2025). IoT-Based Pet Care System. *2025 IEEE International Conference on Automatic Control and Intelligent Systems (I2CACIS)*, Kuala Lumpur, 66–69. https://doi.org/10.1109/I2CACIS65476.2025.11100797

**Description:** The paper proposes a cat care system that combines a feeder, a water fountain and a litter box monitor, each with its own ESP32, synchronized through Firebase and managed from one app. The authors argue that records of food and water intake can reveal irregularities that are early signs of disease. The feeder measures food with a load cell, the fountain estimates water intake with a water level sensor, and each module identifies the cat by RFID. The evaluation is only qualitative, but it reports useful failures. The paper supports intake as a health indicator and shows why water must be weighed.

- **Method / technology:** three ESP32 modules, RFID readers, DC-motor food dispenser with a load cell, water level sensors, Firebase and one mobile app.
- **Setting and data:** prototype tested with cats; the number of trials is not reported.
- **Metrics / results:** no numbers. Portions differed from the target with the DC motor; the water level sensor missed intakes below 100 ml; RFID worked only at about 2 cm.
- **What's missing:** no quantitative evaluation; the app only displays the data.
- **What I take:**
  - food and water intake as early signs of disease;
  - a load cell for water instead of a level sensor;
  - servo dispensing instead of a DC motor.
- **How I improve:**
  - camera recognition instead of RFID tags;
  - measured accuracy and smallest detectable intake;
  - automatic detection of intake changes, interpreted by the AI agent.

### 5. Saadoon & Koyuncu (2023): Intelligence feeder system for stray cats
**Reference:** Saadoon, A. M. & Koyuncu, H. (2023). Intelligence feeder system for stray cats. *Indonesian Journal of Electrical Engineering and Computer Science*, 31(3), 1507–1514. https://doi.org/10.11591/ijeecs.v31.i3.pp1507-1514

**Description:** The paper presents a feeder that provides food and water only after a camera recognizes a cat. A Jetson Nano runs SSD MobileNet V2, pretrained on COCO and used without extra training, while an Arduino Mega controls the servo, pump and sensors. Ultrasonic and water level sensors only check whether the bowls are empty, so the system never knows how much the cat ate. It is the closest feeder to mine in running camera detection on the device itself. However, the reported "accuracy" values are confidence scores on four photos, so its real detection performance is unknown.

- **Method / technology:** Jetson Nano + webcam with SSD MobileNet V2 (COCO); Arduino Mega with servo, pump, ultrasonic and water level sensors.
- **Setting and data:** designed for outdoor stray cats; detection tested on 4 cat photos.
- **Metrics / results:** confidence scores of 93.2–99.2% on 4 images, reported as accuracy; no speed, false detections or dispensing amounts.
- **What's missing:** no real detection evaluation; food and water not weighed; a costlier board than a Raspberry Pi.
- **What I take:**
  - camera detection running on the feeder itself;
  - a COCO-pretrained detector, which already covers cats, dogs and people;
  - a vision board plus a microcontroller for the actuators.
- **How I improve:**
  - YOLO26n on a cheaper Raspberry Pi 5;
  - detection evaluated with precision, recall and inference time on feeder images;
  - food and water weighed.

### 6. Castillo-Arceo et al. (2024): Design and Development of a Smart Pet Feeder with IoT and Deep Learning
**Reference:** Castillo-Arceo, O. E., Renteria-Flores, R. U. & Santana-Mancilla, P. C. (2024). Design and Development of a Smart Pet Feeder with IoT and Deep Learning. *Engineering Proceedings*, 82(1), 63. https://www.mdpi.com/2673-4591/82/1/63

**Description:** The paper presents a dog feeder that combines a camera, a YOLOv5 detector, a weight sensor and a servo, the combination closest to my hardware. YOLOv5 was trained on the Stanford Dogs dataset, and an ultrasonic sensor ensures the system reacts only when a dog is near the bowl. An HX711 weight sensor measures the food, and a servo releases the portion. The results are only qualitative, and the paper does not say where YOLOv5 runs, although the Arduino Mega it uses cannot run it. It shows that camera feeders exist but are rarely evaluated.

- **Method / technology:** Arduino Mega 2560, camera, YOLOv5, HX711 weight sensor, HC-SR04 ultrasonic sensor, servo.
- **Setting and data:** Stanford Dogs dataset (> 20,000 images, 120 breeds); prototype tests with dogs.
- **Metrics / results:** no precision, recall, mAP, inference time or weight error reported.
- **What's missing:** no quantitative evaluation; the device running the model is not stated; no water or medication.
- **What I take:**
  - camera + YOLO + weight sensor + servo as a workable combination;
  - a proximity sensor that wakes the vision part (a PIR sensor in my feeder).
- **How I improve:**
  - YOLO on a board that can actually run it, with measured accuracy and speed;
  - water and medication modules.

### Chapter summary
The reviewed feeders show that a low-cost controller with load cells, servos and a camera is a common, workable design, and that splitting the work between a Raspberry Pi and a microcontroller is already established. Their recurring limits are:
- water drinking is not measured;
- medication is almost never dispensed;
- cameras are used only for live video or to detect presence;
- data is shown but not interpreted;
- evaluation is often qualitative.

My feeder keeps the proven hardware pattern and adds measured food and water intake, medication dispensing, face-based welfare monitoring and an AI agent, all evaluated with numbers.

**Design decisions from this chapter**
- Raspberry Pi 5 for vision and communication, ESP32 for the servos → the Pi + microcontroller split is established (Aguilar Alvarez et al., 2021; Saadoon & Koyuncu, 2023; Pico-Valencia & Holgado-Terriza, 2025)
- Load cells with HX711 under both the food and the water bowl → a water level sensor missed intakes below 100 ml (Ahmad & Nawi, 2025)
- Servo dispensers for food and medication → a DC motor gave inaccurate portions (Ahmad & Nawi, 2025), a stepper missed its target in 18 of 20 rotations (Mahadiputra et al., 2024), and an auger jammed in 7% of tests (Aguilar Alvarez et al., 2021)
- A separate medication compartment → only 2 of 81 reviewed systems dispense medication (Pico-Valencia & Holgado-Terriza, 2025)
- PIR sensor wakes the camera; YOLO runs on the feeder → proximity-triggered vision (Castillo-Arceo et al., 2024) and on-device detection (Saadoon & Koyuncu, 2023)
- Azure IoT Hub with MQTT over TLS, one identity per feeder, commands down and telemetry up → secure MQTT with per-device credentials (Mahadiputra et al., 2024); per-feeder addressing scales to many feeders (Aguilar Alvarez et al., 2021)
- Commands queued until the feeder receives them → a command sent while the feeder was busy was lost (Mahadiputra et al., 2024)
- A cloud database shared by all roles → data stored on one phone limits the system to one user (Mahadiputra et al., 2024)

**Metrics I will measure (from this chapter)**
- **Dispensing error** (g and %): 20 dispenses for each target portion (25, 50, 100 and 200 g) weighed on a reference scale; mean absolute error, % error and standard deviation. Medication doses counted as released correctly or not. Reference: < 5% for ≥ 200 g, about 30–35% around 50 g (Aguilar Alvarez et al., 2021).
- **Load-cell error** (%) **and smallest detectable intake** (g / ml): about 20 known weights (5–500 g) on each scale vs the reference scale; known volumes (5, 10, 20, 50 ml) removed from the water bowl with a syringe. Reference: 0.68–1.91% average, 4–7% below 15 g (Mahadiputra et al., 2024); intakes below 100 ml missed by a level sensor (Ahmad & Nawi, 2025).
- **Latency** (s): command → servo moving, and feeder event → dashboard, over 20 repetitions (mean, maximum, 90th percentile). Reference: 1.0–1.2 s device → app over MQTT (Mahadiputra et al., 2024).
