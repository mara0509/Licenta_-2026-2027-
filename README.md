# Licenta_2026-2027

#Description of project

Animal shelters often have to care for a large number of animals with a very small team of staff and volunteers. Because caregivers don't have the time to closely watch every single animal, early signs of illness—like eating or drinking less, drinking way too much, signs of pain, or sudden mood changes—are easy to miss until the issue becomes serious. On top of that, caring for so many animals makes it hard to keep medical records organized, which can lead to missed treatments and animals not getting the care they need. Tight budgets also make it difficult to buy enough quality food or give potential adopters clear, accurate information about the animals they want to bring home. Solving these connected problems requires a practical system that makes it easier to track animal health, organize medical data, and share reliable information with adopters.

This project builds an IoT system for a simulated shelter of cats and dogs. Each kennel has a smart feeder that dispenses food and medication, weighs food and water intake, and uses a camera to recognise the animal and estimate its emotional state and pain at every meal. The data is stored with a digital health journal in each animal's online profile. An LLM agent with RAG over veterinary sources recommends a vet visit when the indicators suggest a problem. A public platform shows the animals for adoption and lets visitors donate food, which the feeder dispenses within a daily limit.

## Architecture

The system is organised in seven layers, from the feeder in the kennel to the applications used by staff and visitors.

**1. Hardware layer (smart feeder)**
The physical feeder in each kennel. Only one is built; the other kennels use simulated feeders that send the same data.
- **Raspberry Pi 5 (2 GB):** the main board; coordinates all components and communicates with the cloud.
- **ESP32:** controls the two servo motors.
- **Servo motor 1:** food dispenser.
- **Servo motor 2:** medication dispenser, in a separate compartment.
- **2 load cells + 2 HX711 amplifiers:** weigh the food bowl and the water bowl.
- **PIR sensor (HC-SR501):** detects when an animal approaches and wakes the camera.
- **USB webcam:** captures images of the animal at the bowl.

**2. Edge software layer (on the Raspberry Pi)**
Processes data locally so that only useful information leaves the feeder.
- **Python 3 and Raspberry Pi OS:** main language and operating system.
- **OpenCV:** reads frames from the camera.
- **YOLO26n, exported to NCNN/ONNX with INT8 quantization:** checks whether an animal is present. Images without an animal (people, empty frames) are discarded and never leave the device.
- **SQLite:** local buffer, so no data is lost when the internet connection drops.

**3. Communication layer**
Connects the feeders to the cloud securely.
- **Azure IoT Hub (MQTT over TLS, X.509 certificates):** sends events from the feeder to the cloud (intake, visits) and commands from the cloud to the feeder (dispense food or medication).
- **Device twins:** store each feeder's settings in the cloud; the simulated feeders act as digital twins.
- **HTTPS:** uploads animal images from the Raspberry Pi to cloud storage.

**4. Backend layer (cloud, in Docker containers)**
Receives the data, runs the processing and serves the applications.
- **FastAPI (Python):** the main REST API for the web dashboard and the mobile app.
- **Background workers + Redis queue:** process feeder events and analyse images asynchronously, so the API stays fast.
- **Docker / Docker Compose, hosted on Azure Container Apps:** package and run all services the same way locally and in the cloud.

**5. Data layer**
Stores all information about the animals, the feeders and the users.
- **PostgreSQL (via Supabase):** animals, feeders, intake, welfare scores, health journal, vaccines, medication, users and donations.
- **Row Level Security:** enforces the user roles in the database (only the veterinarian can modify the health journal).
- **pgvector:** stores the veterinary texts used by the AI agent (RAG).
- **Supabase Storage and Auth:** stores images and scanned health booklets; handles login for all roles.

**6. Artificial intelligence layer**
Turns the raw data into welfare information and recommendations.
- **Emotion and pain models** (trained with PyTorch in Google Colab, run with ONNX Runtime): estimate the emotional state of dogs and pain indicators in cats (Feline Grimace Scale) from facial images; **Grad-CAM** shows which part of the face influenced each prediction.
- **Animal identification:** Phase 1, one animal per kennel; Phase 2, individual recognition with DINOv2/v3.
- **LLM agent (Azure OpenAI, tool calling, MCP):** reads intake, welfare and health journal data, detects worrying patterns and recommends a vet visit, citing veterinary sources retrieved with **RAG**. It supports decisions and does not diagnose.
- **Vision-language model:** extracts vaccines and dates from photos of the health booklet; data is saved only after a person confirms it.

**7. Client layer**
The applications used by people.
- **Web dashboard (React + TypeScript):** for the admin and the veterinarian; manage animals and feeders, view intake and welfare charts, use the AI agent, edit the health journal.
- **Mobile app (Expo React Native + TypeScript):** for visitors and the veterinarian; browse animals, donate food, scan health booklets, receive push notifications.
- **Payments (Stripe, test mode):** donations are confirmed through a verified Stripe webhook before the feeder dispenses food, within a daily limit.


