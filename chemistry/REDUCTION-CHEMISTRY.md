# Green Iron Reduction — Chemistry, Routes & Why H₂-DRI Wins the Decade

**Owner:** Oscar
**Created:** 2026-04-29
**Purpose:** Reference note. The "why" behind every reduction route VACSO might pursue, the chemistry problems each one faces, and where H₂-DRI sits in the landscape. Read before designing experiments, talking to investors, or sanity-checking lab results against the literature.

---

## 1. The overarching problem

Iron ore is iron stuck to oxygen, and oxygen really doesn't want to let go. Every green iron route is a different answer to the same question: **how do you pull the oxygen off without burning fossil fuel?**

Three families of answer:

1. **Use a different gas** (H₂ instead of CO) — the oxygen leaves as water instead of CO₂.
2. **Use electrons directly** (electrolysis) — oxygen leaves as O₂ gas.
3. **Use heat alone** (thermal/plasma) — get hot enough that oxygen falls off by itself.

---

## 2. Why H₂-DRI is hard (the chemistry)

### Core problem: the reaction sucks heat out of the bed

- CO route: `Fe₂O₃ + 3CO → 2Fe + 3CO₂`, ΔH ≈ **−24 kJ/mol** (exothermic — the furnace self-heats)
- H₂ route: `Fe₂O₃ + 3H₂ → 2Fe + 3H₂O`, ΔH ≈ **+99 kJ/mol Fe₂O₃** at 800 °C (endothermic — the furnace self-cools)

CO routes give heat back; H₂ routes take heat away. Single biggest reason H₂ shafts run hotter inlet temps and bigger gas flows than equivalent MIDREX-CO.

### The five optimisation headaches

**a) The "wet exhaust" problem (equilibrium ceiling)**
H₂ + ore → Fe + H₂O. Water builds up in the gas and re-oxidises the iron back to ore. At 800 °C equilibrium `K = pH₂O/pH₂ ≈ 0.45` — caps single-pass H₂ utilisation at ~30–40%. Rest must be dried, recompressed, recycled. Half the engineering complexity is water management.

**b) The "iron skin" problem (topochemical diffusion)**
Reduction proceeds outside-in. The outer Fe layer becomes a diffusion barrier. Reaction order shifts from chemical-controlled (early) to solid-state-diffusion-controlled (late). The last 10% of metallisation costs disproportionate residence time.

**c) The "sticky pellet" problem**
Above ~900 °C, freshly-reduced iron is soft and tacky. Iron whiskers form and pellets weld to each other and to walls. Caps practical operating temperature → caps reaction rate.

**d) The "fussy ore" problem (no melting)**
H₂-DRI never melts the ore → impurities aren't separated like in a blast furnace. Need DR-grade pellets (>67% Fe, low silica/alumina). DR-grade ore is ~3% of seaborne iron ore supply and already tight.

**e) The "cold dead zone" problem**
Below ~570 °C, wüstite (FeO) is thermodynamically unstable. Reduction path collapses from `Fe₂O₃ → Fe₃O₄ → FeO → Fe` to direct `Fe₃O₄ → Fe` — much slower kinetics. Can't drop temperature to save energy without falling off a kinetic cliff.

### Energy stack (honest numbers)

| Step | Energy |
|---|---|
| Thermodynamic minimum, ore → Fe | ~2.0 MWh/t-Fe |
| H₂-DRI shaft (heat + reduction, including recycle) | 2.5–3.5 MWh/t-Fe |
| H₂ production (electrolysis @ 70% LHV efficiency, ~55 kWh/kg-H₂, ~55 kg H₂/t-Fe) | **~3.0 MWh/t-Fe** |
| EAF melting of DRI | ~0.7 MWh/t-Fe |
| **Total green iron → liquid steel** | **~6–7 MWh/t** |

About half the total energy is making the hydrogen. Most of that is electrolyser overpotential, not the thermodynamic split itself. That's where the inefficiency really lives.

---

## 3. Why H₂-DRI wins the next decade

Not because it's the best physics — because it's the path of least resistance.

1. **The furnace already exists.** MIDREX/ENERGIRON shafts have made DRI since the 1970s on CO/H₂ mix. Switching to 100% H₂ is a tuning problem, not a new invention. ~120 plants worldwide.
2. **The supply chain is already there.** DR-grade pellets, refractory linings, EAFs, trained operators — all exist.
3. **The unsolved problems on alternatives are *materials* problems, not engineering ones.** MOE inert anode at 1600 °C: nobody has it. Aqueous current density: 10× too low. Materials problems take 10–20 years; gas-handling problems take 2–3.
4. **Capital is locked in now.** HYBRIT, H2 Green Steel/Stegra, ArcelorMittal, Thyssenkrupp, POSCO — multi-billion-euro plants with 2026–2030 startup. By the time MOE is ready, 30+ Mt/yr of H₂-DRI will already run.
5. **It's "good enough."** ~95% CO₂ reduction vs BF if H₂ is green. MOE might be 98%. 2030 climate targets won't wait for 98%.

H₂-DRI is the shaft furnace your grandfather built, running on different gas. MOE is a science project that might be brilliant in 2040.

---

## 4. The full menu of reductants

### Chemical reductants (gas/solid)

| # | Route | TRL | Notes |
|---|---|---|---|
| 1 | **Carbon monoxide (CO)** | 9 | Incumbent. 95% of iron ever made. "Green" version: CO from biomass gasification or RWGS on captured CO₂. |
| 2 | **Natural gas (CH₄)** | 9 | Reformed in-situ to syngas. ~40% lower CO₂ than BF. Bridge fuel; pairs with CCS. |
| 3 | **Hydrogen (H₂)** | 7–9 | The headline. Endothermic, clean if green. |
| 4 | **Syngas blends (CO + H₂)** | 9 | Pragmatic middle. New plants take 0–100% H₂ for gradual transition. |
| 5 | **Ammonia (NH₃)** | 4–5 | Cracks → N₂ + H₂. Solves H₂ transport. ~15% energy penalty for cracking. POSCO + Japanese pilots. |
| 6 | **Biomass / biochar** | 8 (Brazil) | Same chemistry as coke, recently-absorbed CO₂. ~10% of Brazilian pig iron. Land-area limited globally. |
| 7 | **Methane pyrolysis (turquoise)** | 5 | Split CH₄ → H₂ + solid C, no CO₂. Monolith, Hazer. Pilot. |

### Direct electrochemical (no chemical reductant)

| # | Route | TRL | Notes |
|---|---|---|---|
| 8 | **Molten oxide electrolysis (MOE)** | 5 | Boston Metal. 1600 °C. Anode lifetime is the open problem. |
| 9 | **Aqueous (alkaline) electrolysis** | 4–5 | Electra, ArcelorMittal Siderwin. 60–110 °C. Plates iron like electroplating. Current density 10× too low. |
| 10 | **Molten salt electrolysis (FFC-style)** | 3 | Cambridge. Iron variant research-only. |

### Exotic / lab-scale

| # | Route | TRL | Notes |
|---|---|---|---|
| 11 | **H₂ plasma smelting (HPSR)** | 4 | voestalpine. Atomic H more reactive than H₂. Single-step ore → liquid Fe. |
| 12 | **Solar thermochemical** | 3–4 | ETH/DLR/Synhelion. Concentrated sunlight runs redox cycle or directly dissociates ore at 1500 °C+. Capacity factor ~25%. |
| 13 | **Microwave-assisted** | 3 | Heats ore particle directly, not gas. MIT, Hokkaido. **Most promising directed-energy route at scale.** |
| 14 | **Laser-induced thermal dissociation** | 2 | NASA/JAXA — relevant for lunar regolith, not Earth-scale. Lasers deliver heat precisely but don't help with the chemistry. |
| 15 | **Bioreduction (microbial)** | 1 | Geobacter/Shewanella. Never tonnes of iron, but interesting for tailings. |

---

## 5. Chemistry problems by route

### 5A. Hydrogen DRI
*See section 2 above.*

### 5B. Molten Oxide Electrolysis (MOE)

- **The "dissolving anode" problem** — *the* unsolved problem. Positive electrode in molten oxide at 1600 °C with pure O₂ off it. Carbon anodes get eaten (and emit CO₂). Metal anodes dissolve. Ceramic anodes crack. Boston Metal claims a Cr-Fe alloy works; nobody has run one for years yet.
- **The "leaky bath" problem.** Molten slag at 1600 °C eats refractory linings.
- **The "wrong product" problem.** Si, P co-reduce with Fe → composition control is hard.

### 5C. Aqueous electrolysis

- **The "slow plating" problem** — *the* unsolved problem. Current density ~10× too low to be economic.
- **The "dirty ore" problem.** Need finely ground, fairly pure ore.
- **The "hydrogen waste" problem.** At higher voltages, water splits (H₂ side reaction) instead of plating Fe.

### 5D. Ammonia

- **Cracking penalty:** ~15% of NH₃ energy content lost to break it apart.
- **Nitriding risk:** incomplete cracking → iron nitrides (brittle).
- Inherits all H₂-DRI problems downstream.

### 5E. Biomass / charcoal

- **Land area limit:** to replace global coke ≈ 30% of arable land in eucalyptus. Doesn't scale.
- **Density problem:** charcoal is fluffy; furnaces redesigned smaller.
- **Quality variability:** batch-to-batch composition variation.

### 5F. Hydrogen plasma (HPSR)

- **Electrode erosion:** plasma torches eat their own electrodes.
- **Energy waste:** most plasma energy heats gas that isn't reducing anything.
- **Single-pass conversion:** low utilisation per pass.

### 5G. Solar thermal (>3000 °C, no reductant)

- **Recombination problem:** Fe + O₂ recombine to Fe₂O₃ on cooling unless quenched fast.
- **Capacity factor:** daylight only. Furnaces hate being switched off.

### 5H. Microwave-assisted

- **Coupling cliff:** once ore reduces to metallic Fe, it stops absorbing microwaves and starts reflecting. Helps with first part of reduction, not last.
- **Hot-spot problem:** uneven heating → local melting/sticking before bulk reduction completes.

### 5I. Lasers

- Lasers are *energy delivery*, not reductants. Still need H₂/C/electrons to bond with the oxygen.
- Useful only for thermal dissociation at >3700 °C — energetically vastly worse than 800 °C H₂-DRI on Earth.
- Relevant for lunar/space ISRU, not industrial green steel.

---

## 6. Pattern across all routes

Every route hits one of three walls:

| Wall | Hits |
|---|---|
| **The oxygen has to go somewhere, and where it goes blocks more reaction** | H₂-DRI (water re-oxidises), electrolysis (O₂ attacks anode), plasma (radicals recombine) |
| **The reduced iron itself gets in the way** | H₂-DRI (iron skin, sticky pellets), aqueous (low current density), microwave (reflects) |
| **The ore is too dirty / the product carries impurities** | All of them; worst for MOE and aqueous |

H₂-DRI's three walls are all *engineering* problems we know how to manage (recycle the gas, control the temperature, beneficiate the ore). MOE's walls are *materials* problems we don't yet know how to solve. That's why H₂-DRI ships first.

---

## 7. Thermodynamic best-case

Cleanest framing: **minimise the number of energy conversions between electricity and Fe–O bond cleavage.**

- H₂-DRI: `electricity → electrolysis → H₂ → heat+reduction → Fe`. Three lossy stages.
- Direct electrolytic reduction (MOE / aqueous): `electricity → Fe`. One stage.

Thermodynamic floor for ore → Fe is **~2.0 MWh/t-Fe** (ΔG of Fe₂O₃ formation, ~6.6 MJ/kg-Fe). MOE has demonstrated ~4 MWh/t-Fe at lab scale; theoretical limit is the floor. H₂-DRI can't get there because electrolyser overpotential and H₂→heat conversion losses are baked into the path.

**Thermodynamic answer:** direct electrolysis wins.
**Engineering answer:** H₂-DRI wins for the next decade.

---

## 8. Implications for the VACSO rig (DN80, atmospheric, flow-through)

Binding constraints we'll actually feel before chasing high metallisation:

1. **Sticking above ~900 °C** with fines/small pellets. Plan for whisker formation.
2. **30–40% utilisation ceiling** forcing a recycle loop or large H₂ excess. Single-pass at lab scale is fine for proving chemistry but masks real industrial energy economics — instrument inlet/outlet H₂O so we can compute utilisation rather than guess it.
3. **Heat loss through tube wall** dominating mass-balance error at small batch sizes. Insulate heavily; consider guard heaters for cleaner energy balance.
4. **Pyrophoricity of fresh DRI** — already covered in `../protocols/H2-DRI-FIRST-RUN.md`. Passivation + Mylar+O₂-absorber storage non-negotiable.

For PhD publishability, **H₂-only** is the right call over syngas because:
- Cleanest chemistry to characterise (no carbon balance).
- Most transferable to the route the rest of the world is converging on.
- QL-500 electrolyser already on order — no fossil-feedstock dependency.

---

## 9. Cross-references

- DRI rig setup: `../hardware/HARDWARE-SETUP.md`
- First-run safety: `../protocols/H2-DRI-FIRST-RUN.md`
- Electrolyser specs: memory `project_dri_electrolyser_ql500.md`
- Reactor setup: memory `project_dri_reactor_actual_setup.md`
- Thermocouple selection: `../hardware/THERMOCOUPLE-OPTIONS.md`
