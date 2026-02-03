# Preliminary Design Repor

[Go to Filter section](#filter)

## 1. Introduction and Project Relevance
Access to safe drinking water remains a persistent public health and infrastructure challenge. Dissolved heavy metals are especially concerning because they can enter water supplies through industrial discharges, mining runoff, and pipe corrosion. Even at low concentrations, metal ions can accumulate over time and pose long-term health and environmental risks, making low-cost, reliable purification strategies important.

In this project, we are designing a small-scale “adaptive” water purifier that removes dissolved zinc ions from water and uses real-time sensing to determine when purification is complete. We chose zinc sulfate in deionized water as a model system since it isolates zinc removal from the confounding background ions present in tap water, enabling clearer performance evaluation. We selected an adsorption-based approach using activated carbon because it is inexpensive, scalable, and compatible with a recirculating design: water can pass through the filter multiple times until readings indicate improvement. Rather than purchasing a commercial cartridge, we will fabricate our own activated-carbon filter media and housing to optimize adsorption performance and better understand the design tradeoffs. For continuous feedback and threshold-based control, we will monitor purification progress with a TDS (conductivity) meter. In addition, we will perform periodic secondary pH checks with indicator strips to verify that the solution's acidity remains within the expected range. If pH variability proves to be a significant driver of performance or measurement noise, we may integrate an in-line pH probe to enable continuous pH logging. 


## 2. Introduction to Activated Carbon Filter and Zinc Analyte {:#filter}
### 2.1 Design Objectives and Performance Targets

In this project, we will remove dissolved zinc from water using an adsorption-based activated-carbon filter operated in a recirculating loop, while monitoring purification progress using conductivity (TDS) measurements.

- **Model system:** Zinc sulfate in deionized (DI) water as a controlled, low-background ionic system  
- **Removal mechanism:** Adsorption of dissolved zinc species onto activated carbon  
- **Adaptive endpoint:** Use a conductivity threshold to determine when purification is complete  

**Performance targets:**
- **Initial concentration:** 30 ppm ZnSO₄ *(high enough to produce a clear, measurable conductivity signal above DI baseline).*  
- **Target concentration:** ≤ 5 ppm ZnSO₄ *("effectively cleaned" benchmark that represents a large reduction, while still being measurable with a conductivity-based proxy).*  
- **Stop threshold:** Stop recirculation when the conductivity reading corresponds *(via a calibration curve)* to ≤ 5 ppm ZnSO₄, and the reading is stable over a short time window  

### 2.2 Model Contaminant Chemistry

To evaluate zinc removal under controlled conditions, we will prepare an ionic contaminant by dissolving zinc sulfate (ZnSO₄) in DI water. This isolates zinc's behavior from the background ions in tap water, making conductivity-based monitoring more interpretable.

When ZnSO₄ is added to water, it dissolves and dissociates to form dissolved ions:

$$
\mathrm{ZnSO_4(aq) \rightarrow Zn^{2+}(aq) + SO_4^{2-}(aq)}
$$

Here, **Zn²⁺** is the target contaminant ion that we aim to remove.

Sulfate can participate in an acid–base equilibrium that can shift solution acidity:

$$
\mathrm{SO_4^{2-} + H_2O \rightleftharpoons HSO_4^- + OH^-}
$$

While the contaminant is defined by the dissolved ionic species (Zn²⁺ and sulfate), this equilibrium introduces a secondary effect: if pH drifts during filtration, it can influence (i) adsorption behavior via carbon surface charge and (ii) conductivity readings via changes in ionic speciation and surface chemistry. For this reason, we will perform periodic pH checks to confirm the solution remains within the expected range.
### 2.3 Point of Zero Charge

- **PZC:** the pH where activated carbon has no net surface charge  
- **pH < PZC** → surface tends to be positively charged; **pH > PZC** → negatively charged  
- **Zn²⁺** is a cation, so pH can change how strongly zinc adsorbs  
- **Measurement impact:** pH shifts can also alter surface ion exchange, causing conductivity changes that are not solely due to zinc removal. So, we track pH to interpret TDS trends.

### 2.4 Adsorption-Based Filtration Mechanism

Activated carbon is well-suited for a small-scale adsorption purifier because it is:

- Inexpensive and widely available  
- High surface area, enabling adsorption sites for dissolved species  
- Compatible with recirculation, allowing multiple passes  

The main limitation is that conductivity/TDS is not zinc-specific: it responds to the *total* ionic content of the water. Two practical consequences follow:

1. Both Zn²⁺ and sulfate contribute to conductivity, so the sensor tracks the overall ZnSO₄ remaining, not zinc alone. This is acceptable in DI water because the contaminant composition is known and stable.  
2. Activated carbon can introduce ions or fines, artificially raising conductivity and masking true removal. Changes in pH can alter the carbon surface charge and ion release/uptake, creating measurement noise even if zinc removal is progressing.

**Carbon pre-treatment**  
To reduce leaching, we will pre-rinse the activated carbon with DI water prior to running adsorption tests.

**Media specification and rationale**  
We will use Marineland Black Diamond Premium Activated Carbon (22 oz) as the adsorption media. This is an appropriate choice for our prototype because it is:

- Commercially consistent and easy to source  
- Designed for aqueous applications, indicating suitability for continuous water contact
- High-surface-area activated carbon makes it a practical, low-cost adsorption medium for dissolved species in a recirculating purifier


## 3. Sensor System: TDS (Conductivity) Meter

### 3.1 Purpose and role

The TDS sensor provides continuous feedback on dissolved ions in the loop. In this system, the conductivity trend is a practical proxy for remaining contaminants, allowing for an adaptive stop once readings indicate the solution has reached the target quality.

### 3.2 Measurement principle and Arduino interface

A TDS meter estimates dissolved solids by measuring electrical conductivity (EC), since dissolved ions carry current. Because “TDS” is a derived value, we will treat the sensor primarily as a conductivity/analog signal source and interpret it through calibration.

We will use the **CQRobot TDS Meter Sensor**:

- **Supply:** 3.3–5.5 V  
- **Output:** 0–2.3 V analog signal (compatible with 3.3 V or 5 V controllers)

**Arduino integration:** the sensor board outputs an analog voltage, which the Arduino reads using an analog-to-digital converter (ADC). We will log the raw ADC/voltage and convert it to estimated ppm ZnSO₄ using our calibration curve.

### 3.3 Calibration and mapping to concentration

Conductivity depends on ion identity and temperature, so we will calibrate the sensor specifically for ZnSO₄ in DI water. We will prepare standards at 0, 5, 10, 20, and 30 ppm ZnSO₄, record stabilized readings, and fit a calibration curve:

- **sensor output → ppm ZnSO₄**

This curve provides an estimate of concentration and a stop threshold.

### 3.4 Main sources of error and noise

- **Temperature drift:** EC increases with temperature; we will keep temperature conditions as consistent as possible during runs  
- **Carbon leaching/fines:** activated carbon can temporarily increase EC; pre-rinsing the media reduces baseline drift and improves interpretability  
- **Bubbles:** bubbles near the probe can cause spikes; we will place the probe in a consistently wetted, well-mixed location  

### 3.5 Limitations and future extensions

- **Non-specific measurement:** EC/TDS tracks total ionic content, not Zn²⁺ specifically. In our model system this is acceptable because ZnSO₄ is the dominant added salt, but the signal can still be influenced by carbon leaching or pH-driven effects.

## 4. System Overview
### 4.1 Design

### 4.2 Filter Design

As mentioned, our filter will be made up of activated carbon, a common compound used for industrial water purification. Our filter will be self-made, taking design inspiration from column chromatography packing strategies. Our filter casing is a plastic 30mL fast protein liquid chromatography column. This was chosen because the inlet allows for a luer lock connection, enabling pressurization of the system by the parastalic pump. The activated carbon will be grinded up using a mortar and pestle to increase surface area and homogenize the size of the solid. To avoid the compound contaminating the water, a filter will be added at the bottom of the column. We will be testing both a larger mesh and low micron membrane to optimize purification while mitigating the need for high pressure or low flow rate. 

The column will be pre wet before each run with DI water. The outlet will run into a reservoir to enable larger volumes of water to be continuously filtered. 

### 4.3 Continuous Purification Monitoring
The TDS meter will be placed at the bottom of the filter outlet’s reservoir to ensure it is always submerged. Experimentation will be done to determine the need for mixing and placement of the meter. Additional monitoring instruments may be implemented if needed outlined in section 6.

### 4.4 Pump Housing
A custom housing for the pump, circuit, and battery will be designed in AutoCad and 3D printed. The pump will be controlled using an Arduino and powered by a 12V battery. The Arduino will be tuned to turn off the pump at a specific TDS reading. All circuitry will be in this housing and designed in a way that is accessible for maintenance and potential additions to the design.

### 4.5 Bubble Detection and Mitigation
Introduction of bubbles into the filter is a major risk to the integrity of the design. A bubble would introduce increased pressure to the filter and may result in fouling or bursting of the secondary filter. This bubble would most likely come from either a hole in the tubing or from a reservoir if the flow rate is too fast or there is not enough water in the system. We plan to reduce the risk of a bubble forming by these methods through our design and operating procedures. However, an additional IR LED bubble detector will be placed in-line with the pump. If the detector, designed in a similar way as a colorimeter, notes the presence of the bubble, a visible LED will activate and the pump will stop. Additional safety mitigation measures will be discussed as we have a better idea of the operating pressures, flow rates, and design constraints. 

### 4.6 Control Mechanisms
100 TDS measurements will be taken every second and averaged. This average will be sent to the Arduino and compared to a predetermined threshold of 5mg/mL, in line with EPA’s recommendation. Measurements are averaged to reduce noise and additional measurements may be added if needed. 
Similarly, the bubble detector uses a predetermined threshold IR absorbance value to indicate when a bubble is present. This is established during control runs. It will have to be calculated if the concentration of zinc sulfate has any effect on this absorbance. However, we hypothesize that any impact will not be significant. The bubble detector takes 1000 measurements every second and the Arduino uses its average. 

## 5. Parts List
### 5.1 Circuitry
- [ ] Arduino Microcontroller  
- [ ] 12V battery  
- [ ] 2 × 220Ω resistor  
- [ ] 1 × 10KΩ resistor  
- [ ] 1 × 1N4001 diode  
- [ ] Hook-up wire  
- [ ] Battery to Arduino connector  
- [ ] Plastic cups  
- [ ] Silicone tubing *(sizing should be similar to the project last year)*  
- [ ] TDS meter  

### 5.2 Filter
- [ ] Activated carbon  
- [ ] Small column with filters *(around 30 mL)*  
- [ ] pH strips  
- [ ] Female and male luer connections with barb  
- [ ] Peristaltic pump  
- [ ] Mesh filter  
- [ ] Zinc sulfate  

### 5.3 Tools and Equipment
- [ ] 100 mL volumetric flask  
- [ ] Mortar and pestle  

### 5.4 To Purchase
- [ ] [TDS meter](#)  
- [ ] [Activated carbon](#)  
- [ ] [pH strips](#)  
- [ ] Column: [option 1](#) and [option 2](#)  
- [ ] [Peristaltic pump](#)  
- [ ] [Mesh filter](#)  
- [ ] [Female and male luer locks](#)  
- [ ] [Silicone tubing (2 mm ID)](#)  
- [ ] [12V power adapter](#)

## 6. Potential Design Additions
### 6.1 pH Monitoring
As aforementioned, the analyte, zinc sulfate, undergoes an acid-base reaction in solution. This results in the production of a strong acid which the filter should be able to eliminate. Therefore, monitoring the presence of this acid may be another way to analyze filter purification. If time permits and the TDS sensor is functioning, we plan to look into this additional monitoring technique.
We will begin by using pH paper to intermediably check for the solution’s pH. This is a cost effective way to check our hypothesis that the acid will bind to the carbon filter. If this is true, we plan to add an Arduino compatible pH meter to our design in addition to the TDS sensor. This would require additional testing and code development.

### 6.2 Analyte
We plan on using zinc sulfate because of its low price and previous success for a similar project. However, we are also interested if our design will work with other heavy metals and dissolved solids. We would like to expand on our design using copper, manganese, and salt solutions. We would also be interested if our design purifies and detects water contaminated with multiple times of solids (salt and metal mixtures). 
## 7. References
