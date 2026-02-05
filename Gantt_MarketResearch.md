---
layout: default
title: Market Research and GANTT Timeline
---

# Market Research and GANTT Timeline

[Go to Market Research](#market)

[Go to GANTT Chart](#gantt)

[Go to References section](#references)


<a id="market"></a>

# 1. Market Research

### 1.1 Ideal product and target user
A countertop drinking-water purifier that combines activated-carbon adsorption with an adaptive control loop based on real-time conductivity and pH checks. Made for households that want drinking-water assurance without installation.

### 1.2 Market trends
The at-home drinking water filtration market is growing, driven by consumer concern about contaminants and demand for more convenient point-of-use (POU) systems.

- **U.S. POU:** $4.31B (2023) → $7.55B (2032), ~6.4% CAGR (Fortune Business Insights)
- **Global POU:** $31.90B (2024) → $53.56B (2030), ~9.2% CAGR (Grand View Research)

### 1.3 Competitive landscape

| Brand | Filter type | Target removal | Price |
|------|-------------|----------------|------:|
| Brita | Pitcher (activated carbon blend) | Lead, chlorine, cadmium, mercury, class I particulates, asbestos, benzene | $41.99 |
| PUR | Pitcher (carbon-based) | Lead, microplastics, chlorine, mercury, copper, zinc | $35.44 |
| ZeroWater | Pitcher (5-stage with ion exchange) | PFAS, lead, chromium, mercury | $37.99 |
| Aquasana | Countertop-powered filter | Microplastics, lead, cysts, chlorine, mercury, PFOA, PFOS, VOCs | $399.99 |
| APEC | Under-sink RO | Chlorine, fluoride, arsenic, lead, chromium | $230.99 |

**Notes:** Pitchers are low-cost but typically non-instrumented, relying on cartridge-lifetime assumptions. Higher-end countertop/RO systems can have stronger performance claims but cost more and may require installation. 

### 1.4 Pricing (project bill of materials)

#### Purchased items (assumed costs)

| Purchased Item | Cost |
|---|---:|
| TDS meter | $12 |
| Activated carbon | $10 |
| pH strips | $4 |
| Column | $96 |
| Peristaltic pump | $25 |
| Mesh filter | $12 |
| Female + male luer locks | $16 |
| Silicone tubing (2 mm ID) | $10 |
| 12V power adapter | $12 |
| **Total** | **$197** |

#### Additional items
| Additional item | Cost |
|---|---:|
| Arduino controller | $25 |
| 12V battery | $15 |
| 3D printer use | $10 |
| Hookup wires | $5 |
| **Total** | **$55** |

**Purchased items + additional items = $252** 

### 1.5 Value proposition and differentiation

#### Market gap
- Many low-cost filters rely on time-based assumptions rather than real-time verification.  
- Users can’t tell when a given run is “done,” especially if inlet water varies.    
- Filter replacement is expensive. 

#### Our differentiators
- **Adaptive:** stops when the measured conductivity reaches a calibrated threshold.  
- **Dual-check water quality:** primary conductivity/TDS trend + secondary pH checks.   
- **User-facing data:** readings can be logged and displayed.  
- **Low-cost filter replacement:** activated carbon media is inexpensive and replaceable.    

### 1.6 SWOT analysis

#### Strengths
- An adaptive endpoint improves trust compared with fixed-time filtration.
- Lower bill of materials cost than powered countertop units and RO.  

#### Weaknesses
- Conductivity is non-specific.  
- Activated carbon performance can vary (packing, leaching). 
- Results from ZnSO₄-in-DI may not directly transfer to real tap water without further validation. 

#### Opportunities
- Extend to broader use by adding metal-specific verification (colorimetric tests).
- Consumer interest in “smart” water products and transparency.   

#### Threats
- Established brands have strong distribution and certifications (NSF/ANSI), which are expensive for new products.   
- Mixed ions in tap water can make conductivity trends harder to interpret without additional sensing. 
- Ion exchange / RO may outperform carbon for some dissolved salts/metals. 

<a id="gantt"></a>
## 2.GANTT Chart

<a id="references"></a>
## 3.References
1. U.S. Point of Use Water Treatment Systems Market Size, Share & Industry Analysis, By Product Type (Under The Counter Filters, Counter Top Filters, Pitcher Filters, Faucet-mounted Filters, and Others), By Category (RO Filters, UV Filters, Gravity Filters, and Others), By Application (Residential and Light Commercial), and Country Forecast, 2024–2032.” Fortune Business Insights, 19 Jan. 2026, https://www.fortunebusinessinsights.com/u-s-point-of-use-water-treatment-systems-market-110045. Accessed 4 Feb. 2026.
2. “Water Purifier Market Size & Share Report, 2026–2035.” Global Market Insights Inc., https://www.gminsights.com/industry-analysis/water-purifier-market. Accessed 4 Feb. 2026.
3. “Water Filter Pitchers.” Brita, https://www.brita.com/water-pitchers/. Accessed 4 Feb. 2026.
4. “Under Sink Water Filters & Filtration Systems.” Aquasana, https://www.aquasana.com/under-sink-water-filters/. Accessed 4 Feb. 2026
5. “5-Stage Water Filter Pitchers & Dispensers – Pure Tasting Water.” Culligan ZeroWater, https://www.zerowater.com/collections/water-filter-pitchers-dispensers. Accessed 4 Feb. 2026.
