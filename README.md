# VACSO Research

**Public engineering and science notes from VACSO's hydrogen direct reduction of iron (H₂-DRI) program.**

Working documents from a working lab. The reactor is real, the hydrogen is real, the literature reviews are mine. Posted publicly because (a) science benefits from being reviewed in the open, and (b) anyone considering a meeting with VACSO can read what's actually under the hood before they take it.

## What's here

### Chemistry

- **[`chemistry/REDUCTION-CHEMISTRY.md`](chemistry/REDUCTION-CHEMISTRY.md)** — the full menu of green iron reduction routes (15 of them), the chemistry problems each one faces, and why H₂-DRI wins the next decade. **Read first.**
- **[`chemistry/MODELLING-AND-RECENT-STUDIES.md`](chemistry/MODELLING-AND-RECENT-STUDIES.md)** — recent literature pushing back on each H₂-DRI chemistry headache (Spreitzer, Patisson, MPIE, Stegra) and six modelling tracks ranked by effort vs insight for the VACSO rig.

### Hardware

- **[`hardware/HARDWARE-SETUP.md`](hardware/HARDWARE-SETUP.md)** — sensor specs for the lab telemetry stack: CO, CO₂, temperature, IR pyrometer over RS485 Modbus RTU. Modbus addresses, register maps, wire colours, verification dates, troubleshooting log.
- **[`hardware/THERMOCOUPLE-OPTIONS.md`](hardware/THERMOCOUPLE-OPTIONS.md)** — temperature sensor options for 800–1200°C operation: RS485 Modbus modules, MAX31856 ICs, Pi HATs, industrial 4–20mA, with prices and integration notes.

### Protocols

- **[`protocols/H2-DRI-FIRST-RUN.md`](protocols/H2-DRI-FIRST-RUN.md)** — full procedure for the first scientifically valid run on the DN80 atmospheric flow-through rig: pre-conditions, eight stages, instrument list, abort criteria, post-run passivation, pyrophoric storage. Written for a real lab, not a textbook.

## What's NOT here

The reactor design files, the AI agent code, the operator dashboard, the blockchain provenance contracts, the brand sites. Those are private repos. Walkthroughs available on request — every program above is running today, observable in real time.

## License

These docs are © 2026 Oscar Osborne / VACSO. Cite freely with attribution. They are working research notes, not peer-reviewed publications.

The first-run protocol is the internal SOP for VACSO's specific equipment and risk envelope. **It is NOT generic safety advice.** Do not adapt it to a different rig without your own engineering review and competent supervision. Hydrogen is no joke.

## About

Part of [VACSO](https://github.com/VACSO-ORG) — a sustainable materials company commercialising green steel through premium consumer products, blockchain-verified provenance, and AI-powered operations.

[vacso.com.au](https://vacso.com.au) · [oscar@vacso.com.au](mailto:oscar@vacso.com.au)
