# H₂-DRI — Modelling Theorisation & Recent Studies

**Owner:** Oscar
**Created:** 2026-04-29
**Companion:** `REDUCTION-CHEMISTRY.md` (the chemistry context — read first)

This doc covers (1) the recent literature pushing back on each H₂-DRI chemistry headache, and (2) the modelling tracks we should run on the VACSO rig to test whether the limits are really limits.

---

## Part 1 — Recent studies tackling each challenge

### a) Wet exhaust / equilibrium ceiling

- **Spreitzer & Schenk (Montanuni Leoben, 2019–2024)** — H₂ utilisation vs T, gas velocity, pellet size in lab fluidised beds. 2023 paper: counter-current staging pushes utilisation to ~55%.
- **Heidari et al. (Aalto/SSAB, 2022)** — pressure-swing adsorption (zeolite 3A) for H₂O recycle, ~30% lower parasitic energy than condensation.
- **Wang et al. (Tsinghua, 2024)** — *membrane-assisted DRI*: Pd-Ag membrane separates H₂ from H₂O at 800 °C in-reactor. Continuously shifts equilibrium. Lab scale, single-pass utilisation >85%.

**Direction:** stop fighting equilibrium, remove water *during* reaction.

### b) Iron skin / diffusion

- **Zuo, Zou, Raabe et al. (MPIE Düsseldorf, 2021–2024)** — atom-probe tomography of partially-reduced pellets. 2023 *Acta Mater.* paper proposes "kinetic windows" where porosity self-sustains.
- **Kim et al. (POSCO/KAIST, 2023)** — *flash ironmaking*: submillimetre particles in free-fall reactor, reduction time hours → seconds, no skin to diffuse through.
- **Ma et al. (MPIE, 2022, *Nature*)** — H₂-plasma reduction of liquid Fe oxide, skips solid-state diffusion entirely.

**Direction:** make the particle small enough or hot enough that diffusion never becomes rate-limiting.

### c) Sticky pellet / whisker formation

- **Bahgat & Khedr (Cairo, 2007)** — original whisker morphology vs T, pH₂. Still cited.
- **Elzohiery & Sohn (Utah, 2017–2021)** — CaO/MgO surface coatings suppress whisker growth at 950 °C.
- **Cavaliere et al. (Lecce, 2023)** — pellet binder chemistry for H₂-only operation.
- **Stegra/H2GS pilot (2024–2025)** — real-world data: sticking onset ~70 °C *lower* than CO-route literature predicted.

**Direction:** surface chemistry tweaks + run cooler than CO instinct says.

### d) Fussy ore / impurities

- **HYBRIT (LKAB, 2021–2024)** — beneficiation supply-chain advance, not chemistry.
- **Bechara et al. (ArcelorMittal, 2023)** — DRI + electric smelting furnace (ESF) instead of EAF for low-grade ore (60% Fe). Slag handling moves to smelter. Likely the major-mill route.
- **Dai et al. (Northeastern Univ. Shenyang, 2024)** — selective H₂ reduction of titanomagnetite (Australia/NZ ironsands).

**Direction:** upgrade the ore or move impurity handling downstream.

### e) Cold dead zone (<570 °C)

- **Pineau et al. (Nancy, 2006–2020)** — canonical kinetic dataset showing the rate cliff.
- **Chen et al. (Northeastern, 2022)** — Ni or Cu doping shifts the cliff to ~480 °C. Lab scale.
- **Ranzani da Costa et al. (Mines Paris, 2013)** — multi-scale kinetic model used as the basis for most academic shaft-furnace simulations.

**Direction:** catalysts, or accept the floor and design around it.

### Cross-cutting / system-level

- **Patisson group (Mines Nancy, 2011–2024)** — REDUCTOR shaft-furnace model (open methodology, the academic reference).
- **Bhaskar et al. (KTH, 2020)** — first peer-reviewed techno-economic of HYBRIT-style 100% H₂ shaft.
- **Vogl, Åhman, Nilsson (Lund, 2018)** — seminal techno-economic; the baseline number for "how much does green steel cost."
- **Souza Filho et al. (MPIE, 2021–2024)** — multi-scale H₂-DRI from atomistic DFT to pellet-scale.

**Read these three first:** Spreitzer & Schenk (2019 review), Patisson REDUCTOR papers, Souza Filho et al. (2021).

---

## Part 2 — Modelling tracks for the VACSO rig

Six tracks, ranked by effort vs insight for our DN80 atmospheric flow-through rig.

### Track 1 — Equilibrium model (1 day, very high insight)

- **Tool:** Python + Cantera (already in our science sidecar) or hand-rolled with NIST-JANAF tables.
- **What:** Baur–Glaessner diagram for our envelope. Plot equilibrium `pH₂O/pH₂` vs T from 400–1100 °C for each step (Fe₂O₃→Fe₃O₄, Fe₃O₄→FeO, FeO→Fe). Overlay achievable ratios.
- **Output:** theoretical utilisation ceiling for any inlet condition. Identifies which temperatures even allow >50% utilisation.
- **Status:** half-day exercise. Should ship before run #1.

### Track 2 — Pellet-scale shrinking-core model (1–2 days)

- **Tool:** Python ODE solver. Three-interface unreacted-core (Levenspiel).
- **What:** time to metallisation X% for a pellet of diameter d at T with H₂ fraction y. Couple chemical kinetics (Arrhenius) + gas-film diffusion + ash-layer (iron-skin) diffusion + solid-state diffusion in series.
- **Output:** which resistance dominates at each stage. Tells us if iron-skin is the bottleneck for *our* pellet size, or whether it's gas-film or chemical kinetics.
- **Calibration:** Mines Nancy (Patisson) papers for rate constants. Compare against TGA data once we have it.

### Track 3 — 1D shaft / packed-bed model (3–5 days, very high insight)

- **Tool:** Python or MATLAB. Coupled mass + energy + momentum balances along bed axis. Pellet model from Track 2 as sub-model at each cell.
- **What:** axial T, gas composition, metallisation profile. Vary inlet T, H₂ flow, bed length.
- **Output:** is our DN80 tube length sufficient for full reduction at planned flow rates? Where does temperature sag? Is atmospheric (no-recycle) operation thermodynamically capable of >X% metallisation?
- **Publishable.** Patisson's REDUCTOR papers are the template.

### Track 4 — CFD with reactive porous media (2–4 weeks, high insight, expensive)

- **Tool:** OpenFOAM, ANSYS Fluent, or COMSOL.
- **What:** 2D/3D flow + reaction + heat transfer in actual tube geometry. Resolves channelling, dead zones, hot/cold spots.
- **Output:** whether sensor placement captures representative gas. Whether wall heat-loss masks bed-core behaviour.
- **Honest take:** probably overkill unless we want a CFD chapter. Track 3 captures 80% of what matters.

### Track 5 — Multi-scale (atomistic → pellet → reactor)

- **Tool:** DFT (VASP/Quantum ESPRESSO) → kinetic Monte Carlo → continuum.
- **What:** electron-level Fe-O bond breaking, surface H₂ adsorption, vacancy diffusion through wüstite.
- **Output:** *why* the rate cliff exists at 570 °C. *Why* certain catalysts (Ni, Cu) shift it.
- **Honest take:** PhD-thesis-deep. MPIE Düsseldorf already does this; we won't beat them. Skip unless supervisor demands the chapter.

### Track 6 — Techno-economic / system model

- **Tool:** Python or Excel. Mass + energy balance from electricity in → liquid steel.
- **What:** $/t-Fe and kWh/t-Fe vs (electricity price, electrolyser efficiency, H₂ utilisation, capacity factor). Apples-to-apples vs MOE, charcoal-BF.
- **Output:** which engineering improvements *actually move the needle* economically (often not the ones papers focus on).
- **For VACSO/YC:** investors will care about this. Build before they ask.

### Recommended order

1. Track 1 (equilibrium) — 1 day. Foundation for everything else.
2. Track 2 (pellet model) — 2 days. Calibrates against TGA, justifies pellet-size choice.
3. Track 3 (1D shaft) — 1 week. Publishable. Validate against DN80 runs.
4. Track 6 (TEA) — 3 days, parallel to above. Investor materials.
5. Track 4 (CFD) — only if reviewers/supervisor demand. Skip Track 5.

---

## Part 3 — The honest limits

The equilibrium ceiling and iron-skin problem cannot be overcome by chemistry — only by engineering. Recent literature (Wang membranes, Kim flash ironmaking, MPIE plasma) confirms: every "breakthrough" is an engineering rearrangement that *avoids* the constraint, not a chemistry change that defeats it.

Our modelling work should identify which engineering knobs — pellet size, gas velocity, recycle ratio, temperature staging, in-bed sorbent — move *our* rig into the operating window. Not wait for a chemistry miracle.

---

## Cross-references

- Chemistry context: `REDUCTION-CHEMISTRY.md`
- Hardware: `docs/HARDWARE-SETUP.md`
- First-run safety: `docs/../protocols/H2-DRI-FIRST-RUN.md`
- Thermocouple selection: `docs/THERMOCOUPLE-OPTIONS.md`
- Science service: `science/` (Cantera + ML surrogates running on port 8086)
- Existing UI: `frontend/src/components/science/ModelsView.tsx`, `SimulationView.tsx`, `DigitalTwinView.tsx`
