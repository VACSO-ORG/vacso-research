# H₂-DRI First Run Protocol — Atmospheric Flow-Through Reactor

**Status**: Procedure for the first scientifically valid H₂ direct-reduction-of-iron run on the VACSO R&D rig.
**Reactor**: DN80 × 1000mm SS304 vertical tube in external LPG gas furnace.
**Atmosphere**: Open-vent flow-through. Never sealed, never pressurised above ~50 mbar over ambient.
**Feedstock**: 6–12mm hematite pellets, 250–500g per batch.
**Target**: ≥85% metallisation in 4–6h reduction at ~900°C bed centerline.
**Authoritative equipment list**: see [../hardware/HARDWARE-SETUP.md](../../hardware/HARDWARE-SETUP.md).

---

## ⚠️ Pre-conditions — DO NOT START until ALL TRUE

- [ ] Fixed H₂ detector mounted, armed, alarmed (test with H₂ leak puff before run)
- [ ] Portable H₂ detector clipped to operator
- [ ] Dry-powder fire extinguisher within 2m of reactor, charge gauge in green
- [ ] Static bond wires connected: reactor body → H₂ cylinder → cart frame → mains earth
- [ ] N₂ cylinder full enough for 3× purge × 5min @ 2 SLPM = ~30 L
- [ ] Shed roller door FULLY OPEN
- [ ] Second person physically present (mandatory for first 3 runs)
- [ ] Both rotameters reading zero (inlet and outlet)
- [ ] Bubbler oil topped up, no frozen / blocked outlet
- [ ] Cold trap empty, ice-bath topped up
- [ ] Furnace controller temperature alarm verified <1100°C cutout
- [ ] All SS-only tools laid out on red-tape "REACTOR ONLY" tray
- [ ] PPE on: Kevlar gauntlets, face shield over safety glasses, cotton/Nomex apron
- [ ] Lab notebook open, batch ID assigned, today's date and operator name written

If any item above fails: **STOP. Fix it. Do not proceed.**

---

## Stage 1 — Feedstock prep (24h before run, ~30 min)

1. **Sieve pellets** through 12mm mesh first (reject anything that doesn't pass), then 6mm mesh (reject anything that DOES pass — that's fines/dust).
2. **Weigh sieved batch** on precision balance: target 250–500g. Record `m_pellets_initial` to 0.1g in notebook.
3. **Photograph the bag + balance reading** with phone. Filename will be linked to batch ID.
4. **Print + apply QR label** on bag with batch ID encoding `{batch_id, mass_g, date, operator}`.
5. **Calculate theoretical maximum O loss** assuming pure Fe₂O₃: `m_O_max = m_pellets_initial × 0.30` (30% of mass is oxygen in pure hematite, less if there's gangue). Record in notebook.

## Stage 2 — Reactor loading (60 min before heating)

6. **Cool reactor confirmed**: tube body <40°C touch-test on outer skin (gloved). If still warm, wait.
7. **Open top end-cap**, withdraw thermocouple, set aside on red tray.
8. **Insert empty alumina boat** through top, lower with stiff wire to rest on the SS wire mesh ~200mm above the bottom inlet.
9. **Pour pellets into boat** through top via a long funnel — confirm bed depth 30–80mm, level surface.
10. **Re-insert thermocouple**: through top end-cap gland (Teflon ferrule), lower until tip is mid-depth in the pellet bed (NOT touching alumina, NOT in headspace gas). Confirm by gentle resistance + temperature drift if you hold it in a warm hand briefly.
11. **Re-seal top end-cap**: tighten gradually (cross-pattern if multi-bolt), confirm thermocouple gland is tight enough that probe doesn't slide but loose enough to slide if grabbed (Teflon ferrule).
12. **Soap-test all gas joints** at the reactor: top end-cap seal, thermocouple gland, inlet fitting at bottom, outlet fitting at top, regulator-to-rotameter, rotameter-to-needle-valve, every elbow. Any bubble = re-tighten or re-tape, retest. **No bubbles ANYWHERE before proceeding.**

## Stage 3 — N₂ purge (30 min)

13. **Set 3-way purge/reduce/vent valve** to N₂ inlet.
14. **Open N₂ cylinder regulator** to 1 bar, then needle valve until inlet rotameter reads 2.0 SLPM.
15. **Confirm bubbler is bubbling** continuously at outlet — this is your proof gas is flowing through, not stuck.
16. **Hold N₂ purge for 5 min**, then close N₂ for 30 sec, then re-open for another 5 min. Repeat once more (3 cycles total).
17. **Verify outlet rotameter** reads roughly the same as inlet (small offset from cold trap pressure drop is OK).
18. **Photograph rotameter readings + thermocouple display** for batch record.

## Stage 4 — Heat-up under N₂ (60–90 min)

19. **Reduce N₂ flow** to 0.5 SLPM (purge maintenance — keeps air out during heat-up).
20. **Set LPG gas furnace** to ramp to wall temp 950°C at ~5°C/min ramp rate.
21. **Log bed thermocouple every 30 sec** to telemetry (Modbus poll).
22. **Wait** until bed thermocouple reads 880–900°C. Wall temp will be higher (~950°C) — that's expected; the reaction is endothermic and the bed lags.
23. **DO NOT proceed until bed temp is stable** for 5 consecutive minutes within ±5°C of 900°C target.

## Stage 5 — Reduction with H₂ (4–6 hours)

24. **Photograph + record** bed temp, wall temp, both rotameter readings, dewpoint, time. This is `t = 0` for the run.
25. **Switch 3-way valve** from N₂ to H₂. (Keep N₂ supply standing by — if anything goes wrong, switch back to N₂ first, THEN react.)
26. **Open H₂ source** (cylinder regulator OR QL-500). For first run use **cylinder** — known flow, known purity.
27. **Adjust H₂ inlet rotameter** to 2.0 SLPM via needle valve. Watch outlet rotameter — should rise to nearly 2.0 SLPM within 30 sec as gas displaces N₂ in the line.
28. **Monitor every 15 min**:
    - Inlet flow (rotameter) — should hold at 2.0 SLPM
    - Outlet flow (rotameter) — initially equal to inlet, drops as H₂ is consumed by reduction
    - Bed temperature — should DROP 30–80°C below set-point initially (endothermic reaction kicks in), then recover toward set-point as reaction slows
    - Dewpoint at outlet — should HOLD near 0°C while reduction is active (~30% H₂O in outlet)
    - Bubbler — must remain bubbling. **If bubbling stops, STOP run immediately — outlet is blocked.**
29. **End-point detection**: reduction is complete when:
    - **Dewpoint drops** sharply (from ~0°C to <-20°C) and stays low for 15 min
    - **Outlet flow ≈ inlet flow** (no more H₂ being consumed)
    - **Bed temperature** rises back to wall temp (no more endothermic load)
    - All three indicators must agree before declaring endpoint.
30. **Record run-time** when endpoint reached. Typical: 3–5h for 250g, 5–7h for 500g.

## Stage 6 — Cool-down under N₂ + passivation (overnight + 30 min)

31. **Switch 3-way valve back to N₂** at 0.5 SLPM.
32. **Close H₂ source** at the regulator/QL-500.
33. **Wait 5 min** for any residual H₂ in line to flush, confirm both rotameters reading N₂ flow only.
34. **Set furnace controller to OFF** (passive cool-down).
35. **Maintain N₂ purge at 0.5 SLPM** until bed temp <100°C — typically overnight, 8–12h.
36. **At end-of-shift**: confirm N₂ flow holding, bubbler bubbling, all detectors armed. Door can be closed for overnight cooldown only if fixed H₂ detector + automatic ventilation are operational. Otherwise leave door cracked.
37. **PASSIVATION (next morning, before opening reactor)**: once bed temp <100°C, deliberately oxidise just the OUTER pellet surface to form a protective film. Crack the N₂ regulator down so a small amount of shop air leaks in alongside N₂ at 0.5 SLPM total. Hold for 30 minutes. Monitor bed temp — should rise no more than +20°C; if it spikes higher, too much air at once, throttle back. **Without passivation, opening the reactor exposes raw sponge iron to air → spontaneous re-oxidation flash possible.**
38. **Switch back to pure N₂** at 0.2 SLPM after passivation, cool to <40°C before opening (typically another 1–2 h).

## Stage 7 — Unload + measure + STORE (next morning, 90 min)

37. **Confirm reactor body cool** (<40°C skin temp) before opening.
38. **Close N₂ supply** at regulator. Bubbler stops bubbling — that's expected now.
39. **Open top end-cap**, withdraw thermocouple, lift out alumina boat with long SS tongs (KEVLAR GAUNTLETS).
40. **Photograph product** in boat — should be metallic grey/dark (not red/brown — that's unreduced).
41. **Weigh boat + product** on precision balance: record `m_combined_final` to 0.1g.
42. **Calculate by difference**:
    - `m_pellets_final = m_combined_final − m_boat_empty` (weighed at start)
    - `m_O_lost_actual = m_pellets_initial − m_pellets_final`
    - `metallisation_% = (m_O_lost_actual / m_O_max) × 100`
43. **Cross-check via cold trap water mass**:
    - Drain cold trap to a tared beaker, weigh water mass
    - `m_O_via_water = m_water × (16/18)` (water is 16/18 oxygen by mass)
    - Compare: `m_O_lost_actual` and `m_O_via_water` should agree within 10% — if not, investigate (boat spillage? trap leak? scale calibration?)
44. **Record everything** in lab notebook: batch ID, all masses, calculated metallisation, runtime, any anomalies.
45. **Anchor on blockchain** via telemetry service: hash of {batch ID, all sensor logs, both photos, mass values, calculated metallisation} → EAS attestation on Base.

## Stage 8 — Store the DRI (within 1 hour of unload)

DRI is **pyrophoric** — fresh sponge iron has enormous surface area and will re-oxidise (and potentially self-heat to autoignition) if left exposed. Don't dawdle between weighing and sealing.

46. **Lay out a Mylar pouch** on a clean surface. Open one O₂ absorber sachet (it activates on air contact — work fast).
47. **Pour DRI** from boat into the pouch. Drop in the O₂ absorber alongside.
48. **Squeeze out air** from the pouch (Mylar collapses easy with a flat hand sweep) and immediately **heat-seal** with the impulse sealer at the open end. Aim for a 5mm continuous seal.
49. **Label the pouch** with the QR code printed earlier — batch ID, date, mass, metallisation %.
50. **Photograph** the sealed labelled pouch. Add to batch record.
51. **Place in steel storage cabinet** with a silica gel desiccant pack on the same shelf (NOT inside the pouch — pouch already has the O₂ absorber). Cabinet stays locked, on a non-combustible surface, away from any moisture source.
52. **FIFO discipline**: oldest pouches at the front. When you eventually melt this DRI in the induction furnace, oldest goes first. Storage shelf life is ~12 months before metallisation drift becomes significant.

**Storage failure modes**:
- Pouch seal failure → DRI exposed to air → can self-heat. Inspect pouches monthly for puffiness (CO₂/H₂ generation = seal fail).
- Wet conditions inside cabinet → DRI + H₂O → Fe(OH)₃ + H₂. Yes, wet DRI generates more hydrogen. Keep desiccant fresh, regenerate quarterly at 120°C/2h.
- Mixing batches in one pouch → can't trace provenance. ONE batch per pouch, period.

---

## Failure modes — what to do

| Symptom | Likely cause | Action |
|---|---|---|
| **Bubbler stops bubbling during run** | Outlet blocked (frozen cold trap, bubbler oil overcooled, kinked line) | **EMERGENCY**: switch 3-way valve to N₂ at 0.5 SLPM. Close H₂. Investigate before re-opening H₂. |
| **H₂ alarm goes off** | Leak somewhere | EMERGENCY: close H₂ at source. Switch to N₂. Evacuate shed. Don't re-enter for 15 min minimum. |
| **Bed temperature climbs above 1000°C** | Furnace runaway, reaction generating heat (rare in DRI), TC fault | Cut furnace power. Maintain N₂ purge. Investigate after cool-down. |
| **Inlet rotameter drops to zero mid-run** | Empty cylinder / electrolyser water out / regulator failed | Switch to N₂ purge. Cool reactor under N₂. Investigate. Don't try to "fix and continue" — restart batch. |
| **Outlet flow > inlet flow** | LEAK INWARD (air entering downstream) — very dangerous, air + hot H₂ | EMERGENCY: switch to N₂, close H₂. Find leak before re-running. |
| **Dewpoint never rises after H₂ start** | Pellets already reduced (oxidation occurred during loading?) or gas not actually flowing through bed (channelling) | Stop run. Inspect product. Re-evaluate procedure. |
| **Pellets sintered into a single mass at end** | Bed temp exceeded ~1050°C (sintering temp) or run too long with high H₂ | Reduce wall temp setpoint or reduce runtime. Sintered DRI is still usable but breaks the kinetic data. |

---

## Run quality scoring (for blockchain attestation)

A run is "scientifically valid" only if ALL of these are true:

- [ ] Pre-run safety checklist 100% complete (signed, photographed)
- [ ] All 3 N₂ purge cycles logged
- [ ] Bed thermocouple held within ±20°C of target for ≥80% of reduction phase
- [ ] H₂ inlet rotameter held within ±10% of setpoint for ≥80% of reduction
- [ ] Bubbler activity continuous throughout (no gaps >30s)
- [ ] Mass balance check (cold trap water vs pellet mass loss) agrees within 10%
- [ ] No emergency interventions during run
- [ ] Photographs at start, midpoint, and end
- [ ] Lab notebook entry signed and dated

If ANY box unchecked: run is logged but NOT anchored to blockchain. Re-run with corrective action.

---
