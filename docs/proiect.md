# Project description

## 1. Context and problem

**Problem.** Shelters and sanctuaries care for many animals with limited staff. Early signs of illness or distress (eating or drinking less, drinking much more than usual, pain, a change in mood) are easy to miss when no one can watch each animal continuously. Medical records are often on paper or incomplete, potential adopters see little reliable information about an animal's health, and shelters depend on donations.

**For whom.** Shelter staff and veterinarians (earlier detection of health problems, accurate records), potential adopters and donors (transparent, verified information), and the animals themselves (faster care).

**Why it matters.** Changes in eating and drinking are recognised clinical signs: reduced intake below 80% of energy needs for 3 days is a clinical alert threshold in cats (ISFM, 2022), and increased drinking occurs in 85% of diabetic cats (iCatCare, 2025). The feeder observes each animal at the moment it is most visible, at the bowl, and turns this into information staff can act on. In the literature, ~93% of smart pet feeders are designed for homes and **none for shelters** (Pico-Valencia & Holgado-Terriza, 2025).

**Setting of this project.** The shelter is **simulated**. Its residents are represented by **photographs of real cats and dogs** (from public datasets), each assigned to a kennel and given an online profile. These photos are the images the system must recognise: they are shown to the physical feeder's camera or sent by the simulated feeders, and the vision models detect the animal, check that it is the animal assigned to that kennel, and estimate its emotional state and pain.

## 2. Objectives and results

*To be completed after the literature review.*

## 3. Solution and architecture

The system connects a smart feeder in each kennel to a cloud backend and a web application. The feeder collects data at every meal, the cloud turns it into welfare information and recommendations, and the web application presents it to staff and visitors.

### Components

**Smart feeder**
* One physical feeder is built; the other kennels are represented by simulated feeders that send the same kind of data, so the platform can be demonstrated with several animals.
* A food dispenser and a medication dispenser, each driven by its own servo motor.
* Two load cells measure the food bowl and the water bowl.
* A motion sensor wakes the camera only when an animal approaches.
* A Raspberry Pi 5 coordinates the components and communicates with the cloud.

**Computer vision (hybrid edge–cloud)**
* On the Raspberry Pi, a lightweight YOLO26n model checks whether an animal is present. Images without an animal, such as people or empty frames, never leave the device.
* In the cloud, models trained on public datasets estimate the emotional state of dogs and pain indicators in cats, based on the Feline Grimace Scale.
* The results are combined into a welfare score (positive / neutral / negative) and a pain indicator, recorded at each meal.
* Each kennel is assigned to one animal, so the animal at the feeder is the one expected for that kennel. Individual recognition is future work.

**Food and water intake monitoring**
* For each visit, the system records the time and the amounts eaten, drunk and dispensed.
* The data is aggregated per day and compared with the animal's usual intake, built from several days of data, and with its estimated energy requirement.
* Large deviations, such as eating much less or drinking much more, are treated as health indicators.

**Digital health journal**
* Each animal has an online journal with vaccines, health conditions, allergies and medication.
* Only veterinarians can add or modify medical data; other users can only read it.
* Each entry records who added it and when, and whether it has been verified by a veterinarian.
* A vision-language model extracts vaccines and dates from photos of the vaccination booklet. The data is saved only after a veterinarian confirms it, and the system then reminds staff of upcoming vaccinations.

**Medication**
* The journal holds each animal's medication schedule.
* At the scheduled time, the feeder's medication dispenser releases the dose, the caretaker confirms in the web application that it was given, and the event is recorded.

**AI agent**
* An LLM-based agent analyses each animal's data by calling tools: food intake, water intake, mood and pain trend, and health journal.
* When it detects a worrying pattern, for example eating less, drinking more and negative mood for several days, it searches a veterinary knowledge base built from open-access sources (RAG).
* It writes a summary for the staff and, when appropriate, recommends a vet visit, citing the sources used.
* The agent supports decisions and does not make diagnoses.

**Web platform**
* Public part: all animals with their profiles, photos, welfare status and adoption information, and food donations for a chosen animal.
* Staff part: health journals, intake and welfare charts, medication schedules, alerts and the AI agent's recommendations.
* The web application is responsive and can also be used on phones, including for photographing health booklets.

**Donations**
* Visitors donate food for a chosen animal with real payments processed by Stripe, so card data never reaches the platform.
* After the payment is confirmed through a verified Stripe webhook, the backend sends a dispense command to the animal's feeder.
* A daily limit prevents overfeeding: donations are blocked when the limit is reached, and a payment that arrives after the limit is refunded.
* <How the donated money is used: e.g. transferred to a partner shelter / controlled demo with refunds.>

### Communication and data flow

1. **Detection at the feeder.** When an animal approaches, the motion sensor wakes the camera and YOLO26n checks whether an animal is present. Only images containing an animal are uploaded over HTTPS.
2. **Sending data to the cloud.** Load-cell readings and visit events are sent to Azure IoT Hub over MQTT with TLS. A local SQLite buffer keeps the data if the internet connection drops.
3. **Processing.** Background workers analyse the images (emotion, pain) and store the intake and welfare data in the database.
4. **Analysis and alerts.** The AI agent reads the data through its tools, retrieves veterinary sources and creates alerts that are shown in the web application.
5. **Donation.** The visitor pays through Stripe; the backend verifies the payment and checks the daily limit, then sends a dispense command to the feeder through IoT Hub, and the ESP32 activates the food servo.
6. **Medication.** At the scheduled time the feeder dispenses the dose, the caretaker confirms it in the web application, and the dose is recorded as given or missed.

### User roles

* **Visitor (`USER`):** browses the animals and their health journals (read-only), donates food and manages their profile.
* **Veterinarian (`VETERINARIAN`):** sees the animals with alerts, uses the AI agent, uploads health booklets, creates and verifies journal entries and manages medication schedules. This is the only role that can modify the health journal.
* **Administrator (`ADMIN`):** manages animals, feeders, users and roles, uses the AI agent and can view, but not modify, the health journals.

Access to protected operations is enforced by the backend and by the database. Public registration creates visitor accounts only.

### Automatic statuses

* **Medication doses:** `SCHEDULED` (the time has not arrived), `DISPENSED` (released by the feeder, waiting for confirmation), `GIVEN` (confirmed by the caretaker), `MISSED` (not confirmed within the allowed time).
* **Donations:** `PENDING` (payment not yet confirmed), `PAID` (payment verified), `DISPENSED` (food released), `REFUNDED` (the daily limit was reached after payment).
* **Alerts:** `OPEN` (detected by the agent), `ACKNOWLEDGED` (seen by a veterinarian), `RESOLVED` (closed by a veterinarian).
* **Feeders:** `ONLINE` (data received recently), `OFFLINE` (no data within the expected interval).

## 4. Hardware and software

### Hardware

The physical feeder is built from the following components:
* **Raspberry Pi 5 (2 GB):** the main board; coordinates all components, runs the animal-presence model and communicates with the cloud. It uses an active cooler and the official power supply, to avoid overheating and unexpected shutdowns.
* **ESP32:** a microcontroller that controls the two servo motors.
* **Two servo motors:** one releases food, the other releases the scheduled medication dose from a separate compartment.
* **Two load cells with HX711 amplifiers:** weigh the food bowl and the water bowl to measure intake.
* **PIR motion sensor (HC-SR501):** detects when an animal approaches and wakes the camera.
* **USB webcam:** captures images of the animal at the bowl.

### Software

**Edge (Raspberry Pi)**
* Python 3 on Raspberry Pi OS.
* OpenCV, to read frames from the camera.
* Ultralytics YOLO26n, exported to NCNN/ONNX with INT8 quantization, to detect the animal.
* The Azure IoT device SDK, to communicate with the cloud.
* SQLite, as a local buffer.

**Firmware (ESP32)**
* Arduino / C++, to drive the servo motors on commands from the Raspberry Pi.

**Communication**
* Azure IoT Hub with MQTT over TLS and X.509 certificates, for events from the feeders and commands to them.
* Device twins, for each feeder's settings; the simulated feeders act as digital twins.
* HTTPS, for uploading animal images.

**Backend**
* Python with FastAPI, as the REST API for the web application.
* Background workers with a Redis queue, for processing events and images.
* Docker and Docker Compose, to package and run all services.

**Data**
* PostgreSQL through Supabase, for animals, feeders, intake, welfare scores, health journals, medication, users and donations.
* Row Level Security, to enforce the user roles in the database.
* pgvector, to store the veterinary texts used by the AI agent.
* Supabase Auth and Storage, for login and for storing images and scanned booklets.
* Supabase CLI migrations, for versioned changes to the database schema.

**Artificial intelligence**
* PyTorch and Google Colab, to train the emotion and pain models with transfer learning on public datasets.
* ONNX Runtime, to run the trained models in the backend.
* Grad-CAM, to show which part of the face influenced each prediction.
* Azure OpenAI, for the LLM agent (tool calling, structured outputs) and for booklet extraction (vision-language model).
* LangGraph or Pydantic AI, to manage the agent's reasoning and tool calls.
* RAGAS, to evaluate the quality of the RAG answers.

**Web application**
* React with TypeScript (Vite), as a responsive web application for all roles.

**Payments and security**
* Stripe with webhook signature verification, for real donation payments.
* HTTPS/TLS for all communication.
* Environment variables and Azure Key Vault for secrets, so no keys are stored in the code.
* GitHub for version control.

### Licenses

* Ultralytics YOLO26 is licensed under AGPL-3.0, which requires the project's source code to be open; this repository is public.
* The other libraries and frameworks are open source.
* The licenses of the public datasets and pretrained models are checked before use.

## 5. Feasibility and scope

**Included**
- One physical feeder (food, water, medication, camera) and several simulated feeders.
- A simulated shelter whose residents are photos of real cats and dogs from public datasets.
- Edge animal filter (YOLO26n) and cloud emotion and pain models trained on public datasets.
- One animal per kennel for identification.
- Intake monitoring with baseline-deviation alerts; health journal with booklet extraction.
- LLM agent with RAG over open-access veterinary sources.
- A responsive web application for all roles.
- Food donations with real payments through Stripe, with a daily limit and refunds.

**Excluded**
- Real shelter animals and real clinical validation; the agent gives decision support, **not diagnoses**.
- Multiple physical feeders.
- Individual animal recognition (future work).
- A separate mobile app; the web application works on phones.

**Resources**
- The physical feeder extends the smart pet feeder I developed in a semester project, with the new components described in Section 4.
- Google Colab for training; free tiers of Azure and Supabase; a Stripe account.
- Public datasets of cat and dog faces (emotion, Feline Grimace Scale pain).
- Open-access veterinary literature for the RAG knowledge base.

**Technical risks and mitigation**

| Risk | Mitigation |
|---|---|
| 2 GB RAM and limited CPU on the Raspberry Pi 5 | Nano model, NCNN/ONNX export, INT8 quantization; measure RAM and inference time |
| Thermal throttling and power-related shutdowns on the Pi 5 | Active cooler, official 5 V / 5 A supply, log temperature during tests |
| Accuracy loss after INT8 quantization | Compare FP32 vs INT8 (mAP, precision, recall) and keep FP32/FP16 if the drop is too large |
| Small or biased emotion and pain datasets; photos differ from feeder-camera images | Transfer learning, augmentation, evaluation on a separate test set; report limitations |
| Load-cell noise, drift and water evaporation | Calibration with known weights, filtering, a control bowl for evaporation |
| Diet changes and other confounders mistaken for illness | Multi-day baseline, ignore the first ~24 h after a diet change, check the journal |
| LLM hallucinations | RAG with cited sources, structured outputs, evaluation on scenarios with known answers |
| Real payments: account verification, refunds and transparency towards donors | Complete Stripe account verification early, state clearly how donations are used, refund payments that exceed the daily limit, verify every webhook |
| Limited web development experience | Supabase for login, roles and storage; React + TypeScript with widely documented patterns; build the web application in small steps |
| Cloud costs or free-tier limits | Local Docker setup, limits on API calls |
| Time (many modules) | Clear priorities: core feeder, vision and agent first; booklet extraction and extras last |
