# Zinc-ing: Adaptive Water Purifier

> An adaptive, low-cost point-of-use water purifier that responds to real-time water quality using conductivity-based TDS monitoring and activated carbon adsorption.

---

## Overview

Zinc-ing is a point-of-use (POU) water purification system designed to remove dissolved zinc (Zn²⁺) from contaminated water. Unlike conventional filters that operate on fixed time schedules, Zinc-ing uses a real-time conductivity sensor to adaptively control a peristaltic pump — shutting off filtration only when the water has reached a target purity threshold. The system integrates activated carbon adsorption, a custom-packed column, and an Arduino-based control circuit into a compact, affordable package totaling under $250 in parts.

---

## Market Context

Point-of-use water treatment is one of the fastest-growing segments in the global water industry.

| Market | 2023/2024 | 2030/2032 |
|--------|-----------|-----------|
| U.S. POU | $4.31B | $7.55B |
| Global POU | $31.90B | $53.56B |

This growth is driven by increasing awareness of heavy metal contamination, aging municipal infrastructure, and demand for decentralized, affordable filtration.

---

## Competitive Landscape

Existing consumer POU filters address a range of contaminants at widely varying price points:

| Brand | Filter Type | Target Contaminants | Price |
|-------|-------------|---------------------|-------|
| Brita | Pitcher (activated carbon blend) | Lead, chlorine, cadmium, mercury, particulates, asbestos, benzene | $41.99 |
| PUR | Pitcher (carbon-based) | Lead, microplastics, chlorine, mercury, copper, zinc | $35.44 |
| ZeroWater | Pitcher (5-stage ion exchange) | PFAS, lead, chromium, mercury | $37.99 |
| Aquasana | Countertop powered filter | Microplastics, lead, cysts, chlorine, PFOA/PFOS, VOCs | $399.99 |
| APEC | Under-sink RO | Chlorine, fluoride, arsenic, lead, chromium | $230.99 |

**What makes our project special:** Zinc-ing targets zinc specifically using a custom affinity chromatography-style packed column with adaptive shut-off — eliminating the over-filtering and unnecessary filter replacements that plague time-based conventional systems.

---

## How It Works

### Core Filtration: Activated Carbon Adsorption

Zinc ions (Zn²⁺) are removed by binding to oxygen-containing surface functional groups on activated carbon pellets, primarily carboxylate groups (R-COOH). The adsorption efficiency is pH-dependent:

- **Low pH:** H⁺ ions compete with Zn²⁺ for adsorption sites, reducing removal efficiency.
- **Higher pH:** The carbon surface becomes more favorable for Zn²⁺ adsorption.

Our zinc source — zinc sulfate heptahydrate (ZnSO₄·7H₂O) — dissociates into Zn²⁺ and SO₄²⁻ in solution. Because sulfate is not as strongly adsorbed onto carbon, it can persist in solution and maintain a higher background TDS reading even as zinc is removed. This is an important consideration when interpreting conductivity measurements.

**Key activated carbon properties used:**
- Low cost and widely available
- High surface area with abundant adsorption sites
- Compatible with recirculating flow systems

**Pre-treatment:** Activated carbon can leach fine particles and ions when first packed, artificially elevating measured conductivity. A DI water pre-rinse was used to stabilize baseline conductivity before any zinc-contaminated water was introduced.

### Adaptive Control: Conductivity-Based TDS Measurement

Rather than replacing filters on a fixed schedule, Zinc-ing monitors water quality in real time using a TDS (Total Dissolved Solids) sensor that estimates contamination via electrical conductivity.

![Circuit Schematic](/CBE3300VC/assets/circuit_schematic.png)

*Figure 1: Circuit schematic.*
The sensing and control architecture:

1. The TDS sensor outputs an analog voltage proportional to the solution's conductivity.
2. The Arduino reads this voltage through its ADC (analog-to-digital converter).
3. 100 raw ADC values are averaged, logged, and compared to the setpoint.
4. When the measured TDS drops below the target setpoint \ (385 digital output units), the Arduino cuts power to the peristaltic pump via the relay.

This approach eliminates unnecessary filter cycling and extends usable filter life compared to systems that operate on fixed timers.

---

## Device Design

![Column Cross Section](/CBE3300VC/assets/column_cross_section.png)

*Figure 2: Column cross section.*

### Flow System

- Water is drawn from a hold container by a peristaltic pump and driven upward through the packed column.
- Effluent exits through a 0.22 µm mesh filter at the column outlet, preventing carbon fines from reaching the treated water.
- A TDS sensor is positioned in-line at the outlet to continuously monitor effluent quality.
- Flow is recirculated until the target TDS is achieved, then the pump stops.

### Column Design

The column uses an affinity chromatography-inspired packing strategy, allowing flexibility in bed volume, porosity, and applied pressure. Inlet flow is connected directly to the column; higher inlet pressurization allows fewer recirculation loops before reaching target TDS. Male and female Luer locks provide leak-free connections at the column inlet and outlet.

**Packed-bed tradeoff — carbon mass vs. hydraulic performance:**

| More Carbon | Too Much Carbon |
|-------------|-----------------|
| More adsorption sites | Tighter packing |
| Greater surface area | High pressure drop |
| Higher theoretical Zn²⁺ capacity | Slower flow rate |
| | Risk of clogging |

50 g of activated carbon was selected to balance adsorption capacity with acceptable flow rate.

---

## Cost of Device

**Primary components:**

| Item | Cost |
|------|------|
| TDS meter | $12 |
| Activated carbon | $10 |
| Column | $96 |
| Peristaltic pump | $25 |
| Mesh filter | $12 |
| Female + male Luer locks | $16 |
| Silicone tubing (2 mm ID) | $10 |
| 12V power adapter | $12 |
| **Subtotal** | **$194** |

**Additional components:**

| Item | Cost |
|------|------|
| Arduino controller | $25 |
| 12V battery | $15 |
| 3D printer use | $10 |
| Hookup wires | $5 |
| **Subtotal** | **$55** |

**Total build cost: $248**

---

## Column Rinse Analysis

Before processing contaminated water, the packed carbon column is rinsed with DI water to remove the likehood of leached carbon.

![Column Rinse Procuedre](/CBE3300VC/assets/rinse.png)
*Figure 6: Technique used to pre-rinse column.*

We then looked at how carbon after pre-rinse may leach into our water sample.

**Method:** After packing, 200 mL of DI water was recirculated through the column while TDS was continuously measured in the hold container.

![Water Contamination](/CBE3300VC/assets/water_contamination.png)
*Figure 7: Contaminated water run.*

**Findings:**
- Carbon leaching does not significantly elevate TDS measurements under normal operating conditions.
- A short rinse (~100 mL) before use is sufficient to stabilize baseline conductivity.
- Target shut-off digital output: **385**

---

## Saturation Volume Analysis

A theoretical estimate of the filter's lifetime capacity was compared against experimental results.

**Given parameters:**
- Adsorption capacity: *q* = 20 mg Zn / g carbon
- Carbon mass: *m*_carbon = 50 g
- Influent zinc concentration: *C*_Zn,in = 2000 mg Zn / L

**Theoretical saturation volume:**

$$m_{Zn,cap} = q \times m_{carbon} = 20 \times 50 = 1000 \text{ mg Zn}$$

$$V_{sat} = \frac{m_{Zn,cap}}{C_{Zn,in}} = \frac{1000}{2000} = 0.5 \text{ L}$$

| | Value |
|--|-------|
| Theoretical saturation volume | 0.5 L |
| Experimental TDS plateau | ~1000 mL |

The filter became ineffective earlier than the theoretical prediction. Several mechanisms contribute to this discrepancy:

- **Non-ideal flow / channeling** — water preferentially flows through low-resistance paths, bypassing portions of the carbon bed.
- **Poor carbon packing** — void spaces reduce effective contact between water and adsorbent.
- **Literature *q* mismatch** — the reported adsorption capacity may not match the specific carbon used.
- **Mass transfer / contact time limits** — insufficient residence time prevents equilibrium adsorption.
- **Early breakthrough** — Zn²⁺ breaks through before the full bed is saturated.

---

## Experimental Results
Here are a few of the results from the purification runs we completed.

![Trial 1](/CBE3300VC/assets/trial1.png)
*Figure 3: TDS digital output vs. time — Trial 1.*

![Trial 2](/CBE3300VC/assets/trial2.png)
*Figure 4: TDS digital output vs. time — Trial 2.*

![Trial 3](/CBE3300VC/assets/trial3.png)
*Figure 5: TDS digital output vs. time — Trial 3.*

The TDS digital output shows two distinct regimes:

1. **Rapid initial drop (0–2 min):** The activated carbon quickly adsorbs a large fraction of Zn²⁺ as fresh, unsaturated sites are readily available.
2. **Gradual decline (2–12.5 min):** As the column approaches saturation, adsorption slows and TDS decreases more gradually toward the shut-off threshold of 385.

The adaptive shut-off successfully terminates the pump at the target output, demonstrating the system's ability to self-regulate based on real-time water quality.

---

## Arduino Housing

![Arduino Housing](/CBE3300VC/assets/housing.png)
*Figure 9: Arduino and circuit housing.*

The Arduino controller is housed in a custom enclosure designed to protect the electronics from water exposure. Foam padding was added to mitigate pump vibration and reduce noise during operation.

---

## Safety Considerations

**Pump:**
- The peristaltic pump can overheat if run continuously for more than 20 minutes. Current operating cycles remain well below this threshold.
- Pump vibration is dampened using a custom foam-fitted holder in the housing.

**Tubing and connections:**
- Fluid is under pressure throughout the recirculating loop. Luer-lock fittings are used at all connection points to ensure secure, leak-free joints.

---

## Conclusion and Next Steps

Zinc-ing demonstrates initial promise as an adaptive, low-cost point-of-use zinc removal system. The conductivity-based adaptive shut-off successfully responds to real-time filtration performance, and the activated carbon column provides measurable Zn²⁺ removal over multiple recirculation cycles.

**Planned improvements:**
- Evaluate new column packing techniques to reduce channeling and improve breakthrough performance.
- Integrate a **pH sensor** to optimize adsorption conditions in real time (leveraging the pH-dependence of Zn²⁺–carbon binding).
- Add a **bubble detector** to protect the column from fouling caused by trapped air.
- Add an **in-line TDS display** to show current filtration status to the user.
- Install a **second TDS sensor** at the column inlet for differential concentration tracking.

---

*CBE 3300: Water Purification — University of Pennsylvania, School of Engineering and Applied Science*
