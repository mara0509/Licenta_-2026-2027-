## Chapter 5: Food and water intake as health indicators

### 1. Sonowal, Hall & Stellato (2025): Individual preferences for scented water bowls in dogs
**Reference:** Sonowal, R., Hall, N. J. & Stellato, A. C. (2025). Individual preferences for scented water bowls in dogs. *Frontiers in Veterinary Science*, 12, 1688084. https://doi.org/10.3389/fvets.2025.1688084

**Description:** The study tests whether dogs drink more from bowls with food-scented sleeves, but its value for this thesis is the measurement method. Water intake was recorded in owners' homes with custom scales, each made of a load cell, an HX711 amplifier and a microcontroller logging a timestamped weight every 5–30 seconds. An algorithm turned the readings into drinking events. It smoothed the signal, removed unstable readings and counted every weight loss above 2 g. Checked against manual inspection, it miscounted only 0.56% of the volume. The study also gives home reference values for dogs. It is the closest validated example of the load-cell bowl in my feeder.

- **Method / technology:** load cell + HX711 + microcontroller with SD card and clock; calibrated with a 2,000 g weight. Algorithm: 5-point moving average, readings more than 5 g from it removed, an event for every loss above 2 g, spikes shorter than about 10 s ignored.
- **Setting and data:** owners' homes; 20 healthy dogs for 4 days (1,122 drinking events), then 10 dogs for 14 days.
- **Metrics / results:**
  - Scale accuracy within 1 g; algorithm error 0.56% (5 of 95 events).
  - Mean intake 28.6 ± 4.70 mL/kg/day; 44.5 on dry food vs 23 on mixed food.
- **What's missing:** healthy dogs only, no baseline or threshold for abnormal change; evaporation not controlled; food not measured.
- **What I take:**
  - calibration with a known weight, with accuracy reported in grams;
  - the event detection algorithm and its thresholds;
  - validation against manual inspection, reported as % of volume.
- **How I improve:**
  - food and water measured in the same feeder, with readings sent to the cloud and compared with each animal's baseline.

### 2. Kosmal et al. (2026): Factors affecting water consumption in domestic cats (scoping review)
**Reference:** Kosmal, P. A. L., Shoveller, A. K., Gillies, G. & Armstrong, L. E. (2026). Diet format, protein, amino acids, salt, and osmolytes, as well as water viscosity, affect water consumption in domestic cats: a scoping review of 32 publications (published from 1975 to 2025) on water intake, hydration status, and related health outcomes. *Journal of Animal Science*, 104, skaf434. https://doi.org/10.1093/jas/skaf434

**Description:** This scoping review collects 50 years of studies on how much water cats drink, to establish what is normal. From 1,126 records, 32 mostly small laboratory studies were included. Healthy cats drank 22.8–51.2 mL/kg/day of total water, and intake varied strongly between and within cats. Diet format was the largest factor: cats on dry food drink more free water but take in less water overall than cats on wet food. No study included sick cats, and the minimum safe intake is still unknown.

- **Method / technology:** PRISMA-ScR scoping review of four databases, 1975–2025.
- **Setting and data:** 32 studies with 2–55 cats each; 81% in laboratories.
- **Metrics / results:**
  - Total water 22.8–51.2 mL/kg/day; free water 16.8–50.2 mL/kg/day.
  - Free-water drinking stopped within 1 day of a switch from dry to wet food.
- **What's missing:** no sick cats or individual changes over time; sensor error not reported; descriptive only; industry funding.
- **What I take:**
  - the normal range in mL/kg/day, used to check readings and to generate simulated data;
  - strong variation between cats, so each animal is compared with its own baseline, not a population value;
  - diet as the main confounder, so the diet type is recorded in the animal's profile.
- **How I improve:**
  - continuous, long-term intake data per animal with automatic deviation detection.

### 3. van Diggelen et al. (2026): Water intake of colony and privately owned cats on dry, mixed and wet diets
**Reference:** van Diggelen, R. A., Gillies, G. J. J., Shoveller, A. K., Banton, S. & Kosmal, P. A. L. (2026). Water intake and consumption behavior of colony and privately-owned healthy domestic cats fed 100% dry, 50% dry and 50% wet, and 100% wet diets: a comparison of research and home environments. *Journal of Animal Science*, 104, skag201. https://doi.org/10.1093/jas/skag201

**Description:** The study measures how food moisture changes cats' drinking, in a research colony and in homes. Twenty-two cats each ate a dry, a mixed and a wet diet for 14 days, while RFID smart fountains logged the volume and time of every drink. Free water intake fell from about 98 mL/day on dry food to 14.5–36.5 mL/day on wet food. After every diet change, intake rose briefly for about 24 hours before settling. The authors conclude that intake must be judged over several days, not one.

- **Method / technology:** RFID smart fountains (Felaqua Connect, Petlibro Dockstream) logging volume, duration and time per drink.
- **Setting and data:** 12 colony cats and 10 owned cats; three diets × 14 days.
- **Metrics / results:**
  - Free water 98.0 / 53.0 / 14.5 mL/day (dry / mixed / wet) in the colony.
  - Drinks per day 5.8 → 1.6 from dry to wet food.
  - Transient change of about 24 h after each diet switch.
- **What's missing:** fountain accuracy and evaporation not measured; shared drinking split volumes equally; healthy cats only; industry funding.
- **What I take:**
  - a baseline built over several days, since single days are unreliable;
  - intake alerts paused for about 24 h after a diet change.
- **How I improve:**
  - measurement accuracy tested against reference weights, which the fountains never were.

### 4. Taylor et al. (2022): ISFM Consensus Guidelines on Management of the Inappetent Hospitalised Cat
**Reference:** Taylor, S., Chan, D. L., Villaverde, C., Ryan, L., Peron, F., Quimby, J., O'Brien, C. & Chalhoub, S. (2022). 2022 ISFM Consensus Guidelines on Management of the Inappetent Hospitalised Cat. *Journal of Feline Medicine and Surgery*, 24(7), 614–640. https://doi.org/10.1177/1098612X221106353

**Description:** These international consensus guidelines describe when reduced food intake in cats becomes dangerous and how to respond. Inappetence is the third most common reason cats are brought to a vet, and it can be caused by almost any disease, but also by pain, nausea, medication or stress. Normal intake is defined through the resting energy requirement (RER = 70 × body weight^0.75). A significant drop is intake below 80% of RER, with risk rising from low (under 3 days) to moderate (3–5 days) and high (over 5 days). They give the clinically accepted rule behind my food-intake alerts.

- **Method / technology:** expert consensus (ISFM panel, endorsed by the AAFP); no new data.
- **Setting and data:** hospitalised cats; strategies also apply outside the hospital.
- **Metrics / results:**
  - RER = 70 × BW(kg)^0.75.
  - Below 80% of RER: under 3 days low risk, 3–5 days moderate, over 5 days high.
  - Feeding support no later than 3 days after eating stops.
- **What's missing:** thresholds based on expert opinion; cats only, in a hospital; no automatic monitoring.
- **What I take:**
  - RER as each animal's daily energy target;
  - below 80% of RER as the threshold, with three alert levels by duration (watch, warn, recommend a vet);
  - pain, stress, medication and diet changes checked before an alert.
- **How I improve:**
  - the rule computed automatically from load-cell data and the food's calorie density.

### 5. Taylor et al. (2025): iCatCare consensus guidelines on diabetes mellitus in cats
**Reference:** Taylor, S., Cannon, M., Church, D., Fleeman, L., Fracassi, F., Gilor, C., Mott, J., Niessen, S. et al. (2025). 2025 iCatCare consensus guidelines on the diagnosis and management of diabetes mellitus in cats. *Journal of Feline Medicine and Surgery*, 27, 1–37. https://doi.org/10.1177/1098612X251399103

**Description:** These guidelines cover the diagnosis and monitoring of diabetes, one of the most common hormonal diseases in cats. Its classic signs are changes in eating and drinking: increased drinking in 85% of diabetic cats, weight loss in 60% and increased appetite in 55%. The guidelines state that measured intake is better than owner reports, because people get used to increased drinking. They explicitly name smart feeders and water fountains as helpful monitoring tools.

- **Method / technology:** expert consensus (iCatCare Veterinary Society, endorsed by the FelineVMA); no new data.
- **Setting and data:** cats in clinical practice; risk factors include age over 8, obesity and indoor confinement.
- **Metrics / results:**
  - Increased drinking 85%, increased urination 75%, weight loss 60%, increased appetite 55%.
  - 24 h water intake correlates "to a degree" with average blood glucose.
- **What's missing:** the water–glucose link is not quantified; smart devices are not evaluated; cats only.
- **What I take:**
  - a sustained rise in water intake as an alert;
  - "drinks more + eats more" as a pattern for the AI agent;
  - clinical support for measuring intake automatically instead of relying on staff impressions.
- **How I improve:**
  - the combined pattern checked automatically across food, water and the health journal.

### 6. Peralta, Nichelason & Trepanier (2025): Nutrient-enriched water in clinically dehydrated cats
**Reference:** Peralta, M., Nichelason, A. & Trepanier, L. (2025). Voluntary acceptance of nutrient-enriched water supplement and promotion of water intake in clinically dehydrated cats. *Journal of Feline Medicine and Surgery*, 1–8. https://doi.org/10.1177/1098612X251334279

**Description:** This prospective study tested whether mildly dehydrated pet cats would voluntarily drink a nutrient-enriched water supplement at home. The authors explain that dehydration is common in cats with reduced appetite, vomiting or diarrhoea, and that cats' weak thirst drive means they may not drink enough to correct it. Thirteen cats were followed for 48 hours, with owners weighing the bowls on a calibrated scale. The cats drank a median of only 13.7 mL/kg/day of plain water, and hydration and quality of life improved once their intake rose. The study shows why low water intake matters in cats.

- **Method / technology:** prospective observational study; bowls weighed by owners (1 g = 1 mL confirmed); hydration, blood tests and quality of life compared before and after 48 h.
- **Setting and data:** 13 mildly dehydrated pet cats (5–8%), treated at home for 48 h.
- **Metrics / results:**
  - Plain water 13.7 mL/kg/day vs supplement 38.9 mL/kg/day.
  - Quality of life 27 → 32 / 35; no change in blood markers.
- **What's missing:** 13 cats, 48 h, no control group; manual weighing; a treatment study, not detection.
- **What I take:**
  - cats' weak thirst drive, so low water intake is flagged, not only high intake;
  - bowl weighing as a valid measure of drinking (1 g = 1 mL).
- **How I improve:**
  - continuous automatic weighing instead of manual weighing.

### 7. Quinn & Quain (2026): Overweight and Obesity in Dogs and Cats
**Reference:** Quinn, R. & Quain, A. (2026). Overweight and Obesity in Dogs and Cats: An Exploration of Animal Welfare and Behaviour Impacts, and Recommendations for Management in Veterinary Primary Care. *Animals*, 16(8), 1204. https://doi.org/10.3390/ani16081204

**Description:** This narrative review examines overweight and obesity in dogs and cats as a welfare problem and gives practical feeding recommendations. About half of adult dogs and cats are overweight, and obesity is linked to diabetes and osteoarthritis and shortens life by up to 2.5 years in dogs. Free feeding, dry food and treats are among the main causes. The authors recommend calculated daily rations, small frequent meals, and treats at no more than 10% of daily calories.

- **Method / technology:** narrative review structured by the Five Domains welfare model; no new data.
- **Setting and data:** owned dogs and cats; the largest cited study covers 4.9 million dogs and 1.3 million cats.
- **Metrics / results:**
  - Overweight / obese: dogs 44.5% / 8.4%, cats 47.2% / 13.9%.
  - Treats ≤ 10% of daily calories.
  - Natural meals: dogs 4–8, cats 8–12 per day.
- **What's missing:** no systematic protocol; owned pets, not shelters.
- **What I take:**
  - a daily ration calculated from energy needs and served as several small portions;
  - donated food counted inside the daily ration, with extra portions capped at about 10% of daily calories.
- **How I improve:**
  - the daily limit enforced automatically by the feeder, including donations.

### Chapter summary
The reviewed studies agree that changes in eating and drinking are early, measurable signs of disease. Intake below 80% of the energy requirement for 3 days is a clinical risk, increased drinking appears in 85% of diabetic cats, and cats rarely correct low water intake themselves. They also show that a load cell measures intake accurately, but intake must be judged against each animal's own multi-day baseline, because diet changes, pain, stress and medication change it without disease. None of the studies measures evaporation, follows sick animals or turns intake into automatic alerts. My feeder measures food and water per meal, keeps an individual baseline, applies guideline-based alert rules, and lets the AI agent check confounders before recommending a vet visit.

**Design decisions from this chapter**
- Load cells calibrated with a known weight; events detected with a moving average, unstable readings removed, an event for each loss > 2 g, spikes < 10 s ignored → accuracy within 1 g and 0.56% algorithm error (Sonowal et al., 2025); bowl weighing is valid, 1 g = 1 mL (Peralta et al., 2025)
- Evaporation rate measured and subtracted from water intake → no reviewed study controlled it (Sonowal et al., 2025; van Diggelen et al., 2026; Peralta et al., 2025), and cats on wet food may drink only 14.5 mL/day (van Diggelen et al., 2026)
- Intake normalised (water in mL/kg/day, food as % of RER, using the weight in the animal's profile) and compared with the animal's own baseline over several days → strong variation between cats (Kosmal et al., 2026); single days are unreliable (van Diggelen et al., 2026); RER = 70 × BW^0.75 (Taylor et al., 2022)
- Food alerts when intake is below 80% of RER: watch (< 3 days), warn (3–5 days), vet recommendation (> 5 days, or ≥ 3 days with another warning sign); the same rule assumed for dogs → clinical risk levels (Taylor et al., 2022)
- Water alerts in both directions: a sustained rise and a sustained drop → increased drinking in 85% of diabetic cats (Taylor et al., 2025); weak thirst drive in cats (Peralta et al., 2025)
- Before recommending a vet visit, the agent checks diet changes (pausing alerts for about 24 h after one), medication and pain / emotion results, and looks for "drinks more + eats more" → confounders of inappetence (Taylor et al., 2022); 24 h transient after diet changes (van Diggelen et al., 2026); diabetes pattern (Taylor et al., 2025)
- Daily ration from energy needs in several small portions; donations inside the ration, extras capped at about 10% of daily calories and blocked at the limit → free feeding and treats drive obesity (Quinn & Quain, 2026)
- Simulated feeders generate intake from literature values (normal ranges, drinks per day, diet effects) → realistic data for kennels without a physical feeder (Kosmal et al., 2026; van Diggelen et al., 2026; Sonowal et al., 2025)

**Metrics I will measure (from this chapter)**
- **Event detection error** (% of volume and number of events): known amounts removed from the food and water bowls (spoon, syringe) in sequences that mimic meals and drinks, including bowl nudges and refills, compared with the detected totals and events. Reference: 0.56% volume error, 5 of 95 events miscounted (Sonowal et al., 2025).
- **Evaporation** (g/day): the feeder's water bowl left untouched for 7 days, with the daily loss logged and used as the correction. Reference: not measured in any reviewed study; compared with the lowest normal intake of 14.5 mL/day (van Diggelen et al., 2026).
- **Alert performance on simulated scenarios:** detection rate (%), false alerts per animal per 30 days and detection delay (days), for food below 80% of RER, a rise and a drop in water, a diet change (must not alert) and normal variation. Reference: 3-day rule (Taylor et al., 2022). No reviewed study reports detection performance.
