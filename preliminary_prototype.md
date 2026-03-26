# Preliminary Prototype

[Go to Prototype Functionality section](#functionality)

[Go to TDS Measurement and Control section](#tds)

[Go to Circuit section](#circuit)

[Go to Code section](#code)

[Go to Carbon Filter section](#carbon)

[Go to Chemical Engineering Principles section](#chem)

[Go to Gantt Chart](#gantt_prelim)

<a id="functionality"></a>
## 1. Prototype Functionality

To date, we have developed a functional initial prototype capable of pumping water, measuring conductivity using a TDS sensor, and automatically shutting down once the water reaches a predefined concentration threshold.

### 1.1 Pump

The current prototype uses a peristaltic liquid pump powered by a 12V supply, with a maximum flow rate of approximately 100 mL/min. The pump successfully draws water from the "dirty water" reservoir, circulates it through tubing, and returns it to the reservoir.

In the final design, the pump is intended to drive water through a carbon filtration column. However, since the column has not yet been integrated, the system currently recirculates water back into the reservoir.

While the pump functions reliably, its flow rate appears to be too low for efficient operation. A low flow rate would significantly increase the time required for water to pass through the carbon filter and reach the desired concentration. To address this limitation, we have ordered a higher-capacity pump with a flow rate of up to 1000 mL/min, along with a compatible 12V power supply (0.8 A motor rating) to support the increased demand.

### 1.2 Next Steps: Estimation of Filter Permeability and Pump Power

1. Determine the pressure drop (ΔP) across the filter at different flow rates
   - Using a pressure sensor or finding literature values for similar carbon filters
2. Estimate filter permeability using Darcy's Law, solving for $k$ and comparing to literature:

$$\Delta P = \frac{\mu L Q}{k A}$$

3. Relate flow rate to system performance
   - Evaluate how flow rate affects filtration efficiency (TDS measurements) to determine an optimal flow rate that balances effective ion removal and processing time
4. Estimate pump power requirements using the calculated pressure drop:

$$P = \Delta P \cdot Q$$

$$P_{\text{actual}} = \frac{\Delta P \cdot Q}{\eta}$$

   - Adjust for pump efficiency $\eta$

5. Goals
   - Minimize energy consumption and optimize filter design parameters (filter length, packing density)

<a id="tds"></a>
## 2. TDS Measurement and Control

We integrated a TDS sensor into our circuit, positioning it in the clean water reservoir to monitor the conductivity of the filtered water in real time. The system is designed to automatically shut off once the water reaches a predefined threshold concentration.

The acceptable TDS level for drinking water is typically around 500 ppm, which we selected as our cutoff threshold for pump shutoff. The initial "dirty water" reservoir is prepared at approximately 2000 ppm TDS, a concentration considered unsuitable for consumption.

To validate this threshold, we prepared solutions with varying zinc (Zn) concentrations and measured their TDS values using the sensor. This allowed us to calibrate and confirm that the system shuts off at the desired concentration.

The integrated circuit that connects the pump and TDS sensor operates as a feedback system, continuously monitoring water quality and stopping filtration once the target TDS level is reached.

<a id="circuit"></a>
## 3. Circuit

The primary function of our circuit is to interface the pump and the TDS sensor with the Arduino board to collect data and enable automated control of the system.

The pump is powered by a 12V power supply and connected to the Arduino through a relay, which acts as a switch to turn the pump on and off. The TDS sensor measures the water's conductivity and outputs a signal to the Arduino. This data is then used to determine when the pump should continue operating or shut off once the desired threshold is reached.

Together, these components form a feedback-controlled system in which real-time conductivity measurements directly regulate the filtration process.

For future improvements, we aim to enhance the data interface by organizing the sensor output in a more user-friendly way. Specifically, we plan to implement a system that allows users to monitor conductivity (TDS) values in real time, improving usability and system transparency.

<a id="code"></a>
## 4. Code

This Arduino code reads a TDS sensor signal, averages it to reduce noise, and uses that value to control the pump automatically. It takes 20 readings from the sensor on pin A1, spaced 50 ms apart, and computes the average of the raw values. It compares this average to a threshold (111.1):

- If the value is **low** (cleaner water / fewer ions) → pump turns **OFF**
- If the value is **high** (more ions present) → pump stays **ON**

It also prints the average reading and pump status to the Serial Monitor.

<a id="carbon"></a>
## 5. Carbon Filter

Activated carbon releases fine particles ("fines") when water passes through it, making it necessary to rinse the filter prior to use. In addition to rinsing, we plan to incorporate a finer mesh within the column to better retain carbon particles and minimize their release into the water stream. We will compare two different mesh sizes to evaluate their effectiveness in reducing fines while maintaining adequate flow.

To improve consistency and usability, we aim to standardize the filter conditioning process in a way similar to commercial systems (e.g., requiring the user to run a set number of initial cycles before use). This will be determined experimentally using TDS measurements. Although carbon fines themselves are not detected by TDS sensors, activated carbon can release small amounts of dissolved impurities during initial use, which increase conductivity.

To establish a standard procedure, the filter will be conditioned by passing successive batches of deionized water through the system while monitoring TDS. The number of cycles required will be defined as the point at which the effluent TDS stabilizes between consecutive runs. This allows us to translate the conditioning step into a simple user instruction (e.g., "run the filter 2–3 times before use") while ensuring consistent performance.

<a id="chem"></a>
## 6. Chemical Engineering Principles

- Identify the permeability of our carbon filter, the length, and the flow rate in order to calculate the amount of power needed
- Calculate the efficiency of our filter, how long it takes for water to be cleaned, and evaluate whether flow rate can be manipulated to decrease processing time

<a id="gantt_prelim"></a>
## 6. Updated Gantt Chart
![Alt text](assets/gantt_prelimdesign.png)
