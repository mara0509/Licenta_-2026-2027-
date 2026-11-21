
## Chapter 5: Food and water intake as health indicators

### 1. Sonowal, Hall & Stellato (2025): Individual preferences for scented water bowls in dogs
**Reference:** Sonowal, R., Hall, N. J. & Stellato, A. C. (2025). Individual preferences for scented water bowls in dogs. *Frontiers in Veterinary Science*, 12, 1688084. https://doi.org/10.3389/fvets.2025.1688084

**Description:** The study tests whether dogs drink more from bowls fitted with food-scented sleeves than from an unscented bowl. Its value for this thesis is the measurement method rather than the scent question. Water intake was recorded at home with 16 custom scales, each built from a load cell, an HX711 amplifier and a microcontroller that logged a timestamped weight every 5–30 seconds. An algorithm turned the weight readings into drinking events: smoothing, removing unstable readings and counting weight losses above 2 g. Checked against manual inspection, it miscounted only 0.56% of the volume. The study also gives home reference values for dogs and shows that diet roughly doubles water intake. It is the closest validated example of the load-cell water bowl used in my feeder.

- **Method / technology:**
  - load cell + HX711 + Adafruit Feather 32u4 with SD card and real-time clock, battery powered;
  - calibrated with a 2,000 g weight before each session;
  - weight logged every 5 s (Experiment 1) or every 30 s (Experiment 2);
  - R algorithm: 5-point moving average, readings more than 5 g from the average removed, a drinking event counted for every loss above 2 g, spikes shorter than about 10 s ignored;
  - linear mixed models with the dog as a random effect.
- **Setting and data:** owners' homes, healthy dogs; Experiment 1: 20 dogs, 4 bowls, 4 days (1,920 h, 1,122 drinking events); Experiment 2: 10 dogs, 2 bowls, 14 days.
- **Metrics / results:**
  - Scale accuracy within 1 g.
  - Algorithm error 0.56% (5 of 95 events; 22.24 g of 3,941.44 g).
  - Mean intake 28.6 ± 4.70 mL/kg/day.
  - Diet: 44.5 mL/kg/day on dry food vs 23 on mixed wet and dry.
  - Evening 17.8 vs night 3.17 mL/kg.
  - Dogs drank 5.26 mL/kg/day more from their preferred scented bowl.
- **What's missing:**
  - healthy dogs only, so no link to illness, no individual baseline and no threshold for abnormal change;
  - evaporation not controlled; food intake not measured;
  - small sample and short periods; industry funding.
- **What I take:**
  - the load cell + HX711 setup with timestamped logging;
  - calibration with a known weight and accuracy reported in grams;
  - the event detection steps and thresholds;
  - validation against manual inspection, reported as % of volume;
  - intake in mL/kg/day;
  - comparing daily totals or the same time windows;
  - checking for a diet change before an alert.
- **How I improve:**
  - food and water measured in the same feeder;
  - evaporation measured and subtracted;
  - an individual baseline per animal with alert rules;
  - readings sent to the cloud instead of an SD card.

### 2. Kosmal et al. (2026): Factors affecting water consumption in domestic cats (scoping review)
**Reference:** Kosmal, P. A. L., Shoveller, A. K., Gillies, G. & Armstrong, L. E. (2026). Diet format, protein, amino acids, salt, and osmolytes, as well as water viscosity, affect water consumption in domestic cats: a scoping review of 32 publications (published from 1975 to 2025) on water intake, hydration status, and related health outcomes. *Journal of Animal Science*, 104, skaf434. https://doi.org/10.1093/jas/skaf434

**Description:** This scoping review collects 50 years of studies that measured how much water cats drink, to find out what is normal and whether a daily requirement exists. From 1,126 records, 32 studies were included, mostly small laboratory studies using bowls, fountains or metabolic cages. Healthy cats drank 22.8–51.2 mL/kg/day of total water, below the NRC recommendation of 50–60 mL/kg/day, which was derived from dogs. Intake varied strongly between and within studies. Diet format was the largest factor: cats on dry food drink more free water but take in less water in total than cats on wet food. No included study measured cats with kidney or urinary disease, and the minimum safe intake is still unknown. The review gives my reference range for cats and the confounders to check before an alert.

- **Method / technology:** PRISMA-ScR scoping review of Google Scholar, Ovid Medline, PubMed and Web of Science, covering English papers from 1975–2025 that report intake as a number.
- **Setting and data:** 32 studies with 2–55 cats each; 81% in laboratories, a few in homes, one in a shelter. Intake reported as free water (drinking only, 22 studies) or total water (drinking + food moisture, 15 studies).
- **Metrics / results:**
  - Total water 22.8–51.2 mL/kg/day; free water 16.8–50.2 mL/kg/day.
  - Urine specific gravity below 1.035 reached at 42.0–52.1 mL/kg/day total water.
  - Intake raised by high protein, high salt, nutrient-enriched water and thicker water; not affected by fat, fibre, or still vs flowing water.
  - Free water drinking stopped within 1 day of a switch from dry to wet food.
  - Dry diets linked to 2.64× (vs mixed) and 6.90–15.87× (vs canned) higher risk of lower urinary tract disease.
- **What's missing:**
  - no diseased cats, individual baselines or changes over time;
  - methods not compared and sensor error not reported;
  - descriptive only (no meta-analysis); industry funding.
- **What I take:**
  - the normal range as a plausibility check on load-cell readings;
  - an individual baseline instead of one population threshold;
  - diet as the main confounder, recorded in the health journal;
  - free water measured at the bowl, with total water estimated from the diet's moisture;
  - intake in mL/kg/day;
  - excessive drinking linked to a later kidney disease diagnosis.
- **How I improve:**
  - continuous, long-term intake data per animal in a shelter, which this literature lacks;
  - deviations detected automatically and combined with the diet record.

### 3. van Diggelen et al. (2026): Water intake of colony and privately owned cats on dry, mixed and wet diets
**Reference:** van Diggelen, R. A., Gillies, G. J. J., Shoveller, A. K., Banton, S. & Kosmal, P. A. L. (2026). Water intake and consumption behavior of colony and privately-owned healthy domestic cats fed 100% dry, 50% dry and 50% wet, and 100% wet diets: a comparison of research and home environments. *Journal of Animal Science*, 104, skag201. https://doi.org/10.1093/jas/skag201

**Description:** The study measures how food moisture changes how much and when cats drink, and whether research colony results hold at home. Twelve colony cats and ten privately owned cats each ate a dry, a mixed and a wet diet for 14 days each. Drinking was recorded continuously by smart fountains that identify each cat by RFID and log the volume, duration and time of every drink. Free water intake fell from about 98 mL/day on dry food to 14.5–36.5 mL/day on wet food. Colony and home results were strongly correlated. Most drinking happened within 2 hours after a meal, and intake rose briefly for about 24 hours after every diet change. The authors conclude that intake must be judged over several days, not one.

- **Method / technology:**
  - RFID smart fountains (Felaqua Connect, Petlibro Dockstream) logging volume, duration and timestamp per drink;
  - free water, total water, drinking time and drinks per day;
  - mixed models with the cat as a random effect.
- **Setting and data:**
  - Study 1: 12 neutered male colony cats in one room.
  - Study 2: 10 owned cats at home.
  - Three diets × 14 days after a 6-day transition (42 days).
- **Metrics / results:**
  - Free water, colony: 98.0 / 53.0 / 14.5 mL/day (dry / mixed / wet). Home: 98.2 / 71.6 / 36.5 mL/day.
  - Colony total water 103 → 155 mL/day (dry → wet).
  - Drinks 5.8 → 1.6 per day; drinking time 588 → 88.5 s/day.
  - On dry food, 69.0 mL drunk in the 4 h after meals vs 29.1 mL in the other 20 h.
  - Colony vs home: r = 0.872 (free water), 0.788 (total).
  - Transient rise of about 24 h after each diet change.
- **What's missing:**
  - fountain accuracy and evaporation not measured;
  - shared drinking split volumes equally;
  - fixed diet order; healthy cats only; industry funding.
- **What I take:**
  - per-animal, per-event logging (volume, duration, time);
  - drinks per day and drinking time as variables;
  - normal values for cats;
  - ignoring about 24 h after a diet change;
  - a multi-day baseline;
  - comparing water around meal times;
  - one animal per bowl.
- **How I improve:**
  - load-cell accuracy and evaporation measured;
  - one animal per kennel, so no shared volumes;
  - the diet record used automatically to suppress false alerts.

### 4. Taylor et al. (2022): ISFM Consensus Guidelines on Management of the Inappetent Hospitalised Cat
**Reference:** Taylor, S., Chan, D. L., Villaverde, C., Ryan, L., Peron, F., Quimby, J., O'Brien, C. & Chalhoub, S. (2022). 2022 ISFM Consensus Guidelines on Management of the Inappetent Hospitalised Cat. *Journal of Feline Medicine and Surgery*, 24(7), 614–640. https://doi.org/10.1177/1098612X221106353

**Description:** These international consensus guidelines describe why cats stop eating, when reduced intake becomes dangerous and how to respond. Inappetence is the third most common reason cats are brought to a vet. Its causes include almost every disease group, as well as pain, nausea, medication and stress. Normal intake is defined through the resting energy requirement (RER = 70 × body weight^0.75). A significant drop is defined by amount and duration: below 80% of RER, with risk rising from low (under 3 days) to moderate (3–5 days) and high (over 5 days). Feeding support should start no later than 3 days after a cat stops eating. The guidelines give my clinically accepted rule for when low food intake should trigger an alert, and they recommend the Feline Grimace Scale because pain reduces appetite.

- **Method / technology:** expert consensus (ISFM panel, endorsed by the AAFP) based on the literature and clinical experience; no new data.
- **Setting and data:** hospitalised cats; strategies also apply outside the hospital.
- **Metrics / results:**
  - RER = 70 × BW(kg)^0.75.
  - Intake below 80% of RER: under 3 days low risk, 3–5 days moderate, over 5 days high.
  - Feeding support no later than 3 days after eating stops.
  - Weight loss, BCS below 4/9, muscle loss, vomiting/diarrhoea and low albumin raise the risk; two or more high-risk factors mean immediate action.
  - Normal pattern of up to 20 small meals per 24 h; food and water kept apart.
- **What's missing:**
  - thresholds based on expert opinion (adapted from Perea, 2012);
  - cats only, in a hospital setting;
  - no automatic monitoring or detection performance; industry involvement.
- **What I take:**
  - RER as each animal's minimum daily energy target;
  - below 80% of RER as the threshold;
  - three alert levels by duration (watch, warn, recommend a vet);
  - 3 days as the limit for a vet recommendation;
  - combining intake with weight and health-journal risk factors;
  - checking pain, stress, medication and diet changes before alerting;
  - the link between the Feline Grimace Scale and appetite;
  - small, frequent meals and separate food and water bowls.
- **How I improve:**
  - the rule computed automatically from load-cell data and the food's calorie density;
  - applied continuously in a shelter rather than judged by staff in a hospital.

### 5. Taylor et al. (2025): iCatCare consensus guidelines on diabetes mellitus in cats
**Reference:** Taylor, S., Cannon, M., Church, D., Fleeman, L., Fracassi, F., Gilor, C., Mott, J., Niessen, S. et al. (2025). 2025 iCatCare consensus guidelines on the diagnosis and management of diabetes mellitus in cats. *Journal of Feline Medicine and Surgery*, 27, 1–37. https://doi.org/10.1177/1098612X251399103

**Description:** These guidelines cover the diagnosis, treatment and monitoring of diabetes, one of the most common hormonal diseases in cats, whose prevalence is thought to be rising. The classic signs are changes in eating and drinking: increased drinking in 85% of diabetic cats, increased urination in 75%, weight loss in 60% and increased appetite in 55%. Because diabetic cats usually eat well, reduced appetite points to another disease or to ketoacidosis, a dangerous complication. For monitoring, the guidelines recommend the ALIVE Diabetic Clinical Score, which combines weight loss, drinking, appetite and activity. They state that measured intake is better than owner reports, since owners get used to increased drinking. They also explicitly name smart feeders, water fountains and litter trays as helpful monitoring tools.

- **Method / technology:** expert consensus (iCatCare Veterinary Society panel, endorsed by the FelineVMA); no new data or sensor evaluation.
- **Setting and data:** cats in clinical practice; risk factors are age over 8, obesity, neutered males, indoor confinement and low activity.
- **Metrics / results:**
  - Ordinary diabetes: increased drinking 85%, increased urination 75%, weight loss 60%, increased appetite 55%.
  - Ketoacidosis in 34–41% of newly diagnosed cats.
  - ALIVE score: 4 signs × 0–3 (total 0–12); weight loss mild < 5%, moderate 5–10%, severe > 10%.
  - 24 h water intake correlates "to a degree" with average glucose in treated cats.
  - Safe weight loss in obese diabetic cats 0.5–1% per week.
- **What's missing:**
  - the water–glucose link is not quantified;
  - smart devices mentioned but not evaluated;
  - no detection performance; cats only.
- **What I take:**
  - a sustained rise in water intake as a meaningful alert;
  - the pattern "drinks more + eats more + loses weight" for the agent to look for;
  - a drop in appetite in a diabetic cat as a warning;
  - measured intake instead of staff impressions;
  - clinical support for smart feeders;
  - the ALIVE score as a model for combining signals;
  - weight-loss cut-offs for health-journal entries.
- **How I improve:**
  - water and food measured automatically per animal;
  - the combined pattern checked by the AI agent across intake, weight entries and the journal.

### 6. Peralta, Nichelason & Trepanier (2025): Nutrient-enriched water in clinically dehydrated cats
**Reference:** Peralta, M., Nichelason, A. & Trepanier, L. (2025). Voluntary acceptance of nutrient-enriched water supplement and promotion of water intake in clinically dehydrated cats. *Journal of Feline Medicine and Surgery*, 1–8. https://doi.org/10.1177/1098612X251334279

**Description:** This prospective study tested whether mildly dehydrated pet cats would voluntarily drink a nutrient-enriched water supplement at home instead of receiving fluids by injection. The authors explain that dehydration is common in cats with reduced appetite, vomiting, diarrhoea or increased urine losses, and that cats' weak thirst drive means they may not drink enough to correct it. Thirteen cats, 5–8% dehydrated, were followed for 48 hours, with owners weighing the bowls on a calibrated scale. The cats drank a median of only 13.7 mL/kg/day of plain water, but 38.9 mL/kg/day of the supplement. Hydration scores and owner-rated quality of life improved, although blood markers did not change. The study shows why low water intake matters in cats and that weighing bowls is a valid way to measure it.

- **Method / technology:** prospective observational study; bowls weighed by owners on a calibrated scale (1 g = 1 mL confirmed); hydration scored by two clinicians; blood tests, urine concentration, weight and an owner quality-of-life survey compared before and after 48 h.
- **Setting and data:** 13 pet cats from two university clinics, treated at home for 48 h; 10 with gastrointestinal signs; mixed diets.
- **Metrics / results:**
  - Plain water 13.7 mL/kg/day (range 2.37–56) vs supplement 38.9 mL/kg/day (P = 0.005).
  - Total fluid 47.2 mL/kg/day.
  - Fluid deficit 75 → 51 mL/kg (P = 0.006).
  - Quality of life 27 → 32 / 35 (P = 0.001).
  - No significant change in blood markers or weight.
- **What's missing:**
  - small sample, 48 h, no control group or blinding;
  - manual weighing, evaporation not measured, food water not counted;
  - a treatment study, not detection; industry funding.
- **What I take:**
  - dehydration causes that show up as intake changes;
  - cats' weak thirst drive, so low intake must be flagged by the system;
  - 13.7 mL/kg/day as a supporting figure, not a threshold;
  - the diet needed to interpret water readings;
  - the welfare effect of dehydration;
  - bowl weighing with 1 g = 1 mL.
- **How I improve:**
  - automatic, continuous weighing instead of owner weighing;
  - evaporation measured;
  - low water intake flagged against each animal's baseline.

### 7. Quinn & Quain (2026): Overweight and Obesity in Dogs and Cats
**Reference:** Quinn, R. & Quain, A. (2026). Overweight and Obesity in Dogs and Cats: An Exploration of Animal Welfare and Behaviour Impacts, and Recommendations for Management in Veterinary Primary Care. *Animals*, 16(8), 1204. https://doi.org/10.3390/ani16081204

**Description:** This narrative review examines overweight and obesity in dogs and cats through the Five Domains model of animal welfare, and gives practical recommendations for veterinary care. About half of adult dogs and cats are overweight. Obesity is linked to diabetes, osteoarthritis and other diseases, and shortens life by up to 2.5 years in dogs. The main cause is a chronic positive energy balance, with ad libitum feeding, dry food and treats among the risk factors. The authors also stress that too little food is harmful, for example causing fatty liver disease in cats, and that stress can lower or raise appetite. They recommend calculated daily rations, treats at no more than 10% of daily calories, small frequent meals and regular weighing, ideally with scales built into feeders.

- **Method / technology:** narrative review (Web of Science, PubMed, Google Scholar, 2025) structured by the Five Domains model; no new data.
- **Setting and data:** owned dogs and cats in homes and primary care; the largest cited study covers 4.9 million dogs and 1.3 million cats.
- **Metrics / results:**
  - Overweight / obese: adult dogs 44.5% / 8.4%, adult cats 47.2% / 13.9%.
  - Obesity defined as ≥ 30% above ideal weight (BCS 8/9).
  - Labradors fed 25% less lived longer; overweight dogs lived up to 2.5 years less.
  - Safe weight loss 1–2% per week.
  - Treats ≤ 10% of daily calories.
  - Natural meals: dogs 4–8, cats 8–12 per day.
- **What's missing:**
  - no systematic protocol or quality rating;
  - owned pets, not shelters;
  - no method for detecting illness from intake.
- **What I take:**
  - overfeeding as a welfare risk, which justifies a strict daily limit;
  - donated portions counted inside the daily ration, with extras capped at 10% of calories;
  - an exact daily ration from energy needs;
  - underfeeding alerts as well as overfeeding;
  - intake read together with the emotion results, since stress changes appetite;
  - body weight recorded alongside intake;
  - medications that suppress appetite checked before alerting;
  - small, frequent portions.
- **How I improve:**
  - the daily limit enforced automatically, including donations;
  - intake, weight and welfare data combined in one profile per animal in a shelter setting.

### Chapter summary
The reviewed studies agree that changes in eating and drinking are early, measurable signs of common diseases. Increased drinking appears in 85% of diabetic cats, and intake below 80% of the energy requirement for 3 days is a clinical risk. Low water intake matters because cats rarely correct dehydration themselves, and both overfeeding and underfeeding harm welfare.

They also show that a load cell under the bowl measures water accurately, but that intake must be judged against each animal's own multi-day baseline. Diet, recent diet changes, stress, pain and medication can all change intake without disease. None of the studies measures evaporation, follows sick animals or turns intake into automatic alerts.

My feeder adds:
- food and water measured per meal;
- evaporation correction;
- an individual baseline;
- alert rules based on clinical guidelines;
- an AI agent that checks diet, medication and welfare results before recommending a vet visit.

**Design decisions from this chapter**
- Load cell + HX711 under the water bowl, calibrated with a known weight, with timestamped readings every few seconds → measured water within 1 g at home (Sonowal et al., 2025); bowl weighing is a valid intake measure, 1 g = 1 mL (Peralta et al., 2025)
- Drinking and eating events detected by a moving average, unstable readings (> 5 g from the average) removed, an event counted for each loss > 2 g, spikes shorter than about 10 s ignored → algorithm error of 0.56% vs manual inspection (Sonowal et al., 2025)
- Evaporation measured with an inaccessible control bowl and subtracted → no reviewed study controlled evaporation (Sonowal et al., 2025; van Diggelen et al., 2026; Peralta et al., 2025), and cats on wet food may drink only 14.5 mL/day (van Diggelen et al., 2026)
- Intake reported in mL/kg/day (water) and as % of RER (food) → comparable across animals of different sizes (Sonowal et al., 2025; Kosmal et al., 2026); RER = 70 × BW^0.75 (Taylor et al., 2022)
- Each animal compared with its own baseline built over several days, not with one population value → intake varies strongly between and within cats and the minimum safe intake is unknown (Kosmal et al., 2026); single-day values are unreliable (van Diggelen et al., 2026)
- Food alert when intake is below 80% of RER: watch (< 3 days), warn (3–5 days), vet recommendation (≥ 3 days with another risk factor, or > 5 days) → clinical risk levels for inappetent cats (Taylor et al., 2022)
- Alert on a sustained rise in water intake, and the agent looks for "drinks more + eats more + loses weight" → increased drinking in 85% of diabetic cats; classic diabetes pattern (Taylor et al., 2025)
- Alert on low water intake as well as high → cats' weak thirst drive means they don't correct dehydration (Peralta et al., 2025)
- Diet recorded in the health journal, and intake alerts suppressed for about 24 h after a diet change → diet roughly doubles or cuts intake (Sonowal et al., 2025; Kosmal et al., 2026); transient change after every switch (van Diggelen et al., 2026)
- Before alerting on low intake, the agent checks recent medication, pain and emotion results → pain, stress, nausea and medication cause inappetence, and the Feline Grimace Scale is recommended (Taylor et al., 2022); appetite-suppressing drugs confound intake (Quinn & Quain, 2026)
- Daily ration calculated from energy needs, no free feeding, served as several small portions → ad libitum feeding is a major obesity risk; cats eat 8–12 small meals a day (Quinn & Quain, 2026); up to 20 meals per 24 h (Taylor et al., 2022)
- Donated food counted inside the daily ration, with extra portions capped at about 10% of daily calories and blocked once the limit is reached → overfeeding affects about half of dogs and cats; treats ≤ 10% of daily calories (Quinn & Quain, 2026)
- Body weight recorded by the veterinarian in the health journal, with weight-loss levels < 5% / 5–10% / > 10% → weight change is part of both the inappetence and diabetes risk assessments (Taylor et al., 2022; Taylor et al., 2025)
- One animal per kennel and bowl; food and water bowls separate → shared fountains made individual volumes uncertain (van Diggelen et al., 2026); food and water should be kept apart (Taylor et al., 2022)
- Water compared over daily totals or the same time windows, especially after meals → intake peaks in the evening (Sonowal et al., 2025) and within 2 h after meals (van Diggelen et al., 2026)
- Simulated feeders generate intake from literature values (normal ranges, drinks per day, meal-linked drinking, diet effects) → realistic data for kennels without a physical feeder (Kosmal et al., 2026; van Diggelen et al., 2026; Sonowal et al., 2025)

**Metrics I will measure (from this chapter)**
- **Event detection error** (% of volume and number of events): known amounts removed from the food and water bowls (spoon and syringe) in sequences that mimic meals and drinks, including bowl nudges and refills. Detected totals and events are compared with the actual ones. Reference: 0.56% volume error, 5 of 95 events miscounted (Sonowal et al., 2025).
- **Evaporation** (g/day): an identical, inaccessible water bowl placed next to the feeder for 7 days, with daily loss logged and then subtracted from intake. Reference: not measured in any reviewed study; compare with the lowest normal intake of 14.5 mL/day for cats on wet food (van Diggelen et al., 2026).
- **Alert performance on simulated scenarios:** detection rate (%), false alerts per animal per 30 days, and detection delay (days from onset). Scenarios: food below 80% of RER for 3+ days, a sustained rise in water, low water, a diet change (should not alert) and normal variation. Reference: 3-day rule (Taylor et al., 2022); about 24 h transient after a diet change (van Diggelen et al., 2026). No reviewed study reports detection performance, so this is new evidence.
