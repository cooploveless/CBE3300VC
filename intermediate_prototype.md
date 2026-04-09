# Intermediate Prototype

[Go to What Has Changed section](#changes)

[Go to Chemical Engineering Considerations section](#chem)

[Go to Mass Balance section](#mass)

[Go to Flow Rate Calculation section](#flowrate)

[Go to Schematics section](#schematics)

[Go to Column Rinse Analysis section](#rinse)

[Go to Full Trial Run Data section](#trial)

[Go to Updated GANTT Chart section](#gantt)

[Go to What's Next section](#next)


<a id="changes"></a>
## 1. What Has Changed Since Initial Prototype

Since our initial prototype, we have completed basic construction of our design. We have optimized column packing and rinsing, verified our design with mass balances, and completed a purification run and refined our circuit system and housing. We completed a procedure for how to prep the carbon in the column to mitigate contamination and buildup. We also completed mass balance and conservation calculations to find how much contaminated water we can purify and how often the column needs to be replaced. We conducted a full run of our purification system from column packing to pump failure (dictated by temperature). Lastly, we constructed a preliminary housing for our circuitry.

<a id="chem"></a>
## 2. Chemical Engineering Considerations

It is important to clarify that the 2000 ppm concentration refers to zinc (Zn²⁺) in the zinc sulfate heptahydrate solution, not the total salt concentration. Although Zn²⁺ ions are adsorbed onto the activated carbon, the system must maintain charge balance. Due to electroneutrality, Zn²⁺ removal cannot occur independently of other ions. While zinc is adsorbed onto the carbon, sulfate and other ions remain or redistribute to maintain charge balance, meaning the filter alters ionic equilibrium rather than removing ZnSO₄ as a neutral species.

<a id="mass"></a>
## 3. Mass Balance

**Parameters:**
- Adsorption capacity: $q = 20 \text{ mg Zn/g carbon}$ (value obtained from literature)
- Mass of activated carbon: $m_{\text{carbon}} = 50 \text{ g}$
- Influent zinc concentration: $C_{\text{Zn,in}} = 2000 \text{ mg Zn/L}$

$$m_{\text{Zn,cap}} = q \cdot m_{\text{carbon}}$$

$$m_{\text{Zn,cap}} = 20 \cdot 50 = 1000 \text{ mg Zn}$$

$$V_{\text{sat}} = \frac{m_{\text{Zn,cap}}}{C_{\text{Zn,in}}} = \frac{1000}{2000} = 0.5 \text{ L}$$

This calculation estimates the maximum amount of zinc the activated carbon filter can remove before reaching saturation. Using a literature adsorption capacity (mg Zn per g carbon) and the total mass of carbon in the filter, the total zinc removal capacity is determined. This value is then divided by the influent zinc concentration to estimate the theoretical volume of water that can be treated before the filter is fully saturated, assuming ideal conditions and complete utilization of adsorption sites.

### 3.1 Limitations of Theoretical Capacity Estimate

Based on the theoretical calculation, the activated carbon filter is expected to reach saturation after treating approximately 0.5 L of a 2000 ppm Zn solution. However, experimental results show that the TDS readings plateau after only ~200 mL, indicating that the filter stops effectively removing ions much earlier than predicted.

Some potential reasons for this discrepancy:

- **Non-ideal flow distribution:** The solution may flow through certain preferential paths (e.g., the center of the column), reducing contact with carbon near the walls and leaving a portion of adsorption sites unused.
- **Poor packing of the carbon bed:** If the carbon is not tightly packed, void spaces can form, allowing the solution to bypass the adsorbent and decrease contact efficiency.
- **Uncertainty in adsorption capacity:** The value of 20 mg Zn/g carbon was taken from literature, but adsorption capacity varies significantly depending on carbon type, surface chemistry, and operating conditions.
- **Mass transfer limitations:** The theoretical model assumes instantaneous adsorption, but diffusion of Zn²⁺ into the pores of the carbon can be slow. At higher flow rates, there may not be enough contact time for equilibrium adsorption to occur.
- **Breakthrough vs. full saturation:** The calculation assumes full utilization of adsorption sites, but in practice, breakthrough occurs before full saturation, meaning the filter becomes ineffective while still containing unused capacity.

<a id="flowrate"></a>
## 4. Flow Rate Calculation

To calculate the flow rate of the column, we filled the column with water and purged it with air. We then ran the pump connected to the filter for one minute and collected the flow-through. After one minute, we weighed the flow-through. We found that our pump with the column attached has a flow rate of 140 mL/min.

<a id="schematics"></a>
## 5. Schematics

### 5.1 Circuit Schematic

![Circuit Schematic](/CBE3300VC/assets/circuit_schematic.png)

*Figure 1: Circuit schematic showing Arduino, relay, 12V pump, and TDS sensor connections.*

### 5.2 Overall System Flow Diagram

![System Flow Diagram](/CBE3300VC/assets/system_flow_diagram.png)

*Figure 2: Overall system flow diagram.*

### 5.3 Column Cross Section

![Column Cross Section](/CBE3300VC/assets/column_cross_section.png)

*Figure 3: Column cross section showing luer lock, 0.22 μm filters, and activated carbon pellets.*

<a id="rinse"></a>
## 6. Column Rinse Analysis and Recommendation

We conducted a study to analyze how many times the column needs to be rinsed prior to use to eliminate carbon contamination in the circulated water. 30 mL of DI water was passed through the column, the eluent collected, and TDS measured. Note that a TDS digital reading of 470 corresponds to 2000 ppm Zn²⁺ and a value of 240 corresponds to 500 ppm.

| Total Volume Rinsed (mL) | Digital TDS Output |
|:---:|:---:|
| 30 | 375 |
| 60 | 115 |
| 90 | 38.2 |
| 120 | 32 |
| 150 | 29 |

We concluded that the column should be rinsed with 120 mL of water prior to filtering. Despite this, we ran a few preliminary runs and were having significant pump resistance. We were also having poor filtration, so we disassembled the column and found a thick buildup of carbon slurry on the outlet filter. Photos of the slurry and filter are shown below. We believe this is because the column terminated with a 0.22 μm filter, which only filters out very small carbon particles — larger small pieces accumulate at the filter and form a thick layer.

<img src="/CBE3300VC/assets/carbon_slurry.png" alt="Carbon Slurry" width="350">

*Figure 4: Carbon slurry buildup on glove.*

<img src="/CBE3300VC/assets/filter_contamination.png" alt="Filter Contamination" width="350">

*Figure 5: Outlet 0.22 μm filter contamination.*

To mitigate this, we are planning on passing approximately 400 mL of DI water through a 150 mesh filter to remove larger carbon particles during manufacturing. The column is then loaded and the user rinses it with 120 mL of water prior to use.

<a id="trial"></a>
## 7. Full Trial Run Data

We completed a trial run using a new filter following our refined preparation specifications. The system was loaded with 200 mL of 2000 ppm Zn²⁺ solution. Our TDS shutoff threshold (digital value of 240) corresponds to a Zn²⁺ concentration of 500 ppm. The pump was turned off prematurely as it overheated. The data collected is graphed below as a function of time.

![TDS Trial Run Graph](/CBE3300VC/assets/tds_trial_run.png)

*Figure 6: TDS digital reading over time with 2000 ppm Zn²⁺ load.*

There is an obvious drop-off in purification as time elapses. It appears that purification stalls near a digital output of 370, despite our mass balance indicating we should be able to purify over five times this amount.

<a id="gantt"></a>
## 8. Updated GANTT Chart

[![Updated Gantt Chart](/CBE3300VC/assets/gantt_updated.png)](https://penno365-my.sharepoint.com/:x:/g/personal/clov_upenn_edu/IQDdv_JKPAcoRI9zU8REIblJAVjyT01BERbfhOXnsotJhqw?e=s16CbP)

*Figure 7: Updated GANTT Chart (Click to See Live Updates)*

<a id="next"></a>
## 9. What's Next

Our primary goal by the next presentation is to fix our column so that we can reach a pump shutoff specification. We are going to remake our solution to ensure our concentration is correct and rerun a full purification until the spec is reached or the pump overheats. If we are still not able to reach the shutoff specification, we plan on lowering our load concentration, as 2000 ppm is an extreme case and tap water has a lower total dissolved solids level.
