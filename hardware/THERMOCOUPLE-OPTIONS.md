# Thermocouple Integration Options for H2-DRI Lab Furnace

**Context:** H2-DRI process runs at 800-1200C. Type K thermocouples (rated to 1260C) are the minimum viable option. Type N (1300C) or Type S/R (1600C+) provide more headroom for peak temperatures. Existing system: Raspberry Pi 5, RS485 Modbus RTU bus via Waveshare gateway, Python poller, PostgreSQL + InfluxDB.

**Existing alternative:** BYD-2S IR Thermometer on RS485 address 0x06 (untested) -- non-contact surface temperature. Worth testing first as it is already wired into the bus.

---

## 1. RS485 Modbus Thermocouple Transmitters (Best Fit -- Uses Existing Bus)

These plug directly into the existing RS485 Modbus RTU bus. Minimal code changes -- just add new device address and register map to the Python poller.

### Single-Channel Modules

| Product | Range | Accuracy | TC Types | Power | Price (approx) |
|---------|-------|----------|----------|-------|----------------|
| **Eletechsup N4KTA01** (B version) | -200 to +1350C | +/-2C (-200 to 700C) | K | DC 5-24V, 5-7mA | ~US$10-15 (AliExpress) |
| **Generic "K-Type Thermocouple RS485 Transmitter"** | 0 to 1300C | +/-0.5% FS | K | DC 12-24V | ~US$8-20 (AliExpress) |
| **Datexel DAT3018** (industrial) | per TC type | +/-0.1% FS | K,J,T,E,N,R,S,B | DC 10-30V | ~US$80-150 (industrial supplier) |

### Dual-Channel Modules

| Product | Range | Accuracy | TC Types | Power | Price (approx) |
|---------|-------|----------|----------|-------|----------------|
| **Cytron Dual-Channel K-Type TC to RS485** | -200 to +1350C | +/-2C | K | DC 5-36V | ~US$25-35 (Cytron.io) |
| **Generic dual-channel AliExpress** | -200 to +1350C | +/-1C | K,J,T,E,N | DC 12-24V | ~US$15-25 |

### Multi-Channel Modules (4-16 channels)

| Product | Channels | Range | Accuracy | TC Types | Price (approx) |
|---------|----------|-------|----------|----------|----------------|
| **ComWinTop 8-ch RS485** | 8 | -200 to +1350C | +/-1C | K | ~US$47-76 (AliExpress) |
| **Generic 4/8/16-ch RS485** | 4/8/16 | -200 to +1350C | +/-1C | K,J,T,E,N | ~US$30-60/47-76/60-100 |
| **ComWinTop 16-ch RS485** | 16 | -200 to +1350C | +/-1C | K,J,T,E,N | ~US$80-120 (Amazon) |
| **Mechanivis 4/6/8/10-ch with display** | 4-10 | -200 to +1300C | K | DC 24V | ~US$40-80 (Amazon) |
| **Datexel DAT10018** (industrial) | 8 | per TC type | +/-0.05% | K,J,T,E,N,R,S,B | ~US$200+ |

**Pros:**
- Drops straight into existing RS485 Modbus bus -- no new wiring infrastructure
- Same poller code pattern (just new address + register map)
- Same Waveshare gateway connection
- Industrial-grade isolation and ESD protection on most modules
- DIN rail mounting available on multi-channel units
- Configurable baud rate (default 9600, existing bus is 4800 -- must match)

**Cons:**
- Must configure baud rate to 4800 to match existing sensors (or reconfigure bus to 9600)
- Cheap modules may have poor cold-junction compensation at high ambient temps near furnace
- Single-channel modules need one RS485 address per thermocouple

**Integration effort:** LOW -- add device config to poller, write register map, assign Modbus address. Same pattern as existing CO/CO2 sensors.

**Recommendation:** Start with an **Eletechsup N4KTA01** (~$12) for single-zone testing, then move to a **ComWinTop 8-ch module** (~$50-75) for multi-zone monitoring. The N4KTA01 covers -200 to 1350C which gives headroom above the 1200C DRI process max.

---

## 2. SPI/I2C Thermocouple Breakout Boards (Direct to Pi GPIO)

These connect directly to the Pi's SPI bus. Require new Python code (separate from Modbus poller) but well-supported libraries exist.

### Chip Comparison

| Chip | Resolution | TC Types | Range | Status | Notes |
|------|-----------|----------|-------|--------|-------|
| **MAX6675** | 12-bit (0.25C) | K only | 0 to +1024C | **DISCONTINUED** | Too low range for DRI |
| **MAX31855** | 14-bit (0.25C) | K only* | -200 to +1350C | Active | *Variants exist for J,T,N etc |
| **MAX31856** | 19-bit (0.0078C) | K,J,N,R,S,T,E,B | -210 to +1800C | Active | Best option, 50/60Hz filtering |

*MAX6675 is unsuitable -- discontinued and only reads to 1024C.*

### Breakout Boards

| Product | Chip | Channels | Price (approx) |
|---------|------|----------|----------------|
| **Adafruit MAX31855 Breakout** (PID 269) | MAX31855 | 1 | ~US$15 |
| **Adafruit MAX31856 Breakout** (PID 3263) | MAX31856 | 1 | ~US$18 |
| **SparkFun MAX31855 Breakout** | MAX31855 | 1 | ~US$15 |
| **Playing With Fusion 2-ch MAX31856** | MAX31856 | 2 | ~US$40 |
| **Playing With Fusion 4-ch MAX31856** | MAX31856 | 4 | ~US$65 |
| **Generic MAX31855 module** (AliExpress) | MAX31855 | 1 | ~US$3-5 |
| **Generic MAX31856 module** (AliExpress) | MAX31856 | 1 | ~US$5-8 |

**Pros:**
- Very low cost (especially generic modules)
- High resolution (MAX31856 = 19-bit)
- Excellent Python libraries (adafruit-circuitpython-max31856)
- Direct connection, no gateway needed
- Open thermocouple detection (broken wire alarm)
- MAX31856 supports all thermocouple types -- future-proof for Type S/R if needed

**Cons:**
- Uses Pi GPIO SPI pins -- limited to 2 SPI buses on Pi 5 (CE0/CE1 per bus = 4 channels without multiplexing)
- Each channel needs its own CS (chip select) pin
- Separate code path from Modbus poller -- needs its own service/thread
- No galvanic isolation on cheap boards -- noise near furnace could be an issue
- Wiring runs from furnace to Pi must be kept short or use shielded cable

**Integration effort:** MEDIUM -- new Python service using adafruit-circuitpython libraries, new SPI wiring, but code is straightforward. Data still goes to same PostgreSQL/InfluxDB.

**Recommendation:** Good for prototyping. A **generic MAX31856 module** (~$6) is the cheapest way to get a thermocouple reading into the Pi. Use the Adafruit board (~$18) if you want reliability and proper level shifting.

---

## 3. USB Thermocouple Readers

Connect to Pi via USB. Self-contained with their own ADC and cold-junction compensation.

| Product | Channels | TC Types | Range | Resolution | Price (approx) |
|---------|----------|----------|-------|------------|----------------|
| **Pico Technology TC-08** | 8 (+1 CJC) | K,J,T,E,R,S,N,B | -270 to +1820C | 20-bit, 0.025C (Type K) | ~US$300-350 |
| **Measurement Computing USB-TC** | 8 | K,J,T,E,R,S,N,B | per TC type | 24-bit | ~US$400+ |
| **Lascar EL-USB-TC** | 1 | K,J,T | per TC type | N/A | ~US$80-100 |

**Pros:**
- Plug and play USB -- no GPIO wiring
- Professional-grade accuracy and calibration
- Linux/Raspberry Pi drivers available (TC-08 has PicoLog for Linux + Python SDK)
- Self-powered from USB
- TC-08 supports 10 readings/second

**Cons:**
- Expensive compared to other options
- Separate software ecosystem -- need to integrate readings into existing pipeline
- USB connection less robust in industrial environment
- Lascar EL-USB-TC is really a standalone logger, not easily scriptable

**Integration effort:** MEDIUM -- TC-08 has Python SDK (picosdk), needs wrapper to push data to PostgreSQL/InfluxDB. Separate from Modbus pipeline.

**Recommendation:** The **Pico TC-08** is excellent if budget allows (~$300). Professional accuracy, 8 channels, good Linux support. Overkill for a small lab but very reliable.

---

## 4. Industrial 4-20mA Thermocouple Transmitters

Traditional industrial approach: thermocouple transmitter outputs 4-20mA current loop, read by an ADC on the Pi.

### Transmitters

| Product | Range | Accuracy | TC Types | Price (approx) |
|---------|-------|----------|----------|----------------|
| **Generic K-Type 4-20mA transmitter** | 0-1000C | +/-0.5% FS | K | ~US$10-20 (AliExpress) |
| **DOPZNJWF K-Type 4-20mA** | 0-500C or 0-1000C | +/-0.5% FS | K | ~US$15-25 (Amazon) |
| **Dwyer Series 651** | configurable | +/-0.2% FS | K, PT100 | ~US$80-120 |
| **ATO K-Type with transmitter** | 0-1300C | +/-0.5% | K | ~US$30-50 |

### ADC to Read 4-20mA on Pi

| Product | Channels | Resolution | Interface | Price (approx) |
|---------|----------|------------|-----------|----------------|
| **ADS1115 breakout** (Adafruit/generic) | 4 | 16-bit | I2C | ~US$3-15 |
| **NCD 4-ch 4-20mA receiver (ADS1115)** | 4 | 16-bit | I2C | ~US$30 |
| **MCP3008** | 8 | 10-bit | SPI | ~US$3-5 |

**4-20mA reading method:** Place a 250 ohm precision resistor across the ADC input. 4mA = 1.0V, 20mA = 5.0V. Scale linearly to temperature range.

**Pros:**
- Industry-standard signal -- immune to electrical noise over long cable runs
- Can run hundreds of meters of cable
- Galvanic isolation inherent in current loop
- Good for separating hot furnace zone from electronics

**Cons:**
- Two components needed (transmitter + ADC) -- more complexity
- Requires 24V power supply for transmitter loop
- Most cheap transmitters max at 1000C -- need to verify range for DRI
- ADS1115 is 16-bit but accuracy depends on resistor precision
- Separate code path from Modbus poller

**Integration effort:** HIGH -- need transmitter + ADC + precision resistor + 24V supply + new Python I2C code. More wiring and calibration.

**Recommendation:** Only worthwhile if thermocouple cables need to run long distances (>10m) from furnace to Pi. For a small lab, the RS485 Modbus option is simpler and achieves the same result.

---

## 5. Raspberry Pi Thermocouple HATs (Multi-Channel, Purpose-Built)

Dedicated Pi HATs designed specifically for thermocouple measurement.

| Product | Channels | TC Types | Resolution | Accuracy | Stackable | Price (approx) |
|---------|----------|----------|------------|----------|-----------|----------------|
| **Sequent Microsystems 8-TC HAT** | 8 | K,J,T,N,E,B,R,S | 24-bit | 0.1% (0.01% with cal) | Yes (8 layers = 64 TC) | ~US$80-100 |
| **MCC 134 DAQ HAT** (Digilent) | 4 | K,J,T,N,E,B,R,S | 24-bit | Professional grade | Yes (8 HATs = 32 TC) | ~US$150-200 |

**Pros:**
- Purpose-built for thermocouple measurement on Pi
- Excellent accuracy (Sequent: 0.1%, MCC 134: professional grade)
- Open thermocouple detection
- Python libraries included (both have pip-installable packages)
- Stackable for scaling to many channels
- Sequent HAT also includes RS485 port on the board
- MCC 134 has open-source C/C++ and Python libraries

**Cons:**
- More expensive than generic modules
- HAT sits on top of Pi -- physical space consideration
- Separate code from Modbus poller
- MCC 134 limited to 1 reading/second per channel
- Sequent HAT uses I2C (address configurable for stacking)

**Integration effort:** MEDIUM -- pip install library, write Python service to read and push to database. Well-documented APIs.

**Recommendation:** The **Sequent Microsystems 8-TC HAT** (~$90) is the best multi-channel Pi-native option. 8 thermocouples, stackable, includes RS485 port, 0.1% accuracy. The MCC 134 is more expensive but professional-grade if accuracy is critical for research data.

---

## 6. Wireless Thermocouple Options

WiFi or BLE modules for cable-free monitoring.

| Product | Channels | TC Types | Range | Connectivity | Battery | Price (approx) |
|---------|----------|----------|-------|-------------|---------|----------------|
| **Comark Diligence 600 WiFi** | 1-2 | K | per probe | 2.4GHz WiFi | Yes | ~US$200-300 |
| **ThermoWorks ThermaData WiFi** | 2 | K | per probe | WiFi | 1 year | ~US$150-250 |
| **Monnit ALTA Wireless TC** | 1 | K | up to 400C | Proprietary RF | Yes | ~US$100-150 + gateway |
| **NCD Long-Range Wireless TC** | 1 | K | up to 260C | 900MHz mesh | Yes | ~US$80-100 + gateway |
| **Phase IV Industrial Wireless** | 2 | K | industrial | Proprietary | Yes | ~US$200+ |
| **Imagine Industrial 2.4GHz TC** | 1 | K | per probe | 2.4GHz + 4-20mA receiver | Yes | ~US$150-200 |

**Pros:**
- No cables near furnace -- reduces fire/heat damage risk to wiring
- Easy to relocate sensors
- Some units have cloud dashboards

**Cons:**
- Most consumer wireless TC sensors max at 260-400C -- **NOT suitable for 800-1200C DRI process** (the probe itself is K-type and rated higher, but the wireless transmitter electronics cannot survive near the furnace)
- Expensive per channel
- Battery replacement needed
- Proprietary gateways and protocols -- hard to integrate with existing Modbus pipeline
- WiFi reliability in industrial environments is poor
- Latency and data rate limitations

**Integration effort:** HIGH -- proprietary APIs, separate gateways, custom integration code.

**Recommendation:** **Not recommended for DRI furnace monitoring.** The transmitter electronics cannot survive near 800-1200C. Wireless TC sensors are designed for food processing, HVAC, and cold chain -- not metallurgical furnaces. If you need wireless, you would need to run a traditional thermocouple cable out of the hot zone and place the wireless transmitter in a cool area, which defeats the purpose.

---

## Summary Comparison

| Option | Cost (1 ch) | Cost (8 ch) | Integration | Accuracy | Best For |
|--------|-------------|-------------|-------------|----------|----------|
| **RS485 Modbus transmitter** | ~$12 | ~$50-75 | LOW (existing bus) | +/-1-2C | Production monitoring |
| **SPI breakout (MAX31856)** | ~$6-18 | ~$50-65 | MEDIUM (new SPI code) | +/-2C | Prototyping |
| **USB reader (TC-08)** | ~$300 | ~$300 (built-in 8ch) | MEDIUM (Python SDK) | 0.025C | Research-grade data |
| **4-20mA transmitter + ADC** | ~$25 | ~$150+ | HIGH (2 components) | +/-0.5% FS | Long cable runs |
| **Pi HAT (Sequent)** | ~$90 | ~$90 (built-in 8ch) | MEDIUM (pip library) | 0.1% | Multi-zone, clean setup |
| **Wireless** | ~$150+ | ~$1200+ | HIGH (proprietary) | +/-2C | NOT suitable for DRI |

---

## Recommended Approach for DRI Lab

### Phase 1: Quick validation (~$15 total)
1. **Test the BYD-2S IR Thermometer** already on the bus (address 0x06) -- free, already wired
2. Buy one **Eletechsup N4KTA01** RS485 module (~$12) + a Type K thermocouple probe rated to 1300C (~$5-10)
3. Add to existing Modbus bus, configure address, add to poller

### Phase 2: Multi-zone monitoring (~$60-100 total)
1. Buy a **ComWinTop 8-channel RS485 module** (~$50-75)
2. Buy 4-6 Type K thermocouple probes for different furnace zones
3. Wire all to single module, single RS485 address, multiple registers

### Phase 3: Research-grade (if needed for PhD data)
1. Add a **Sequent Microsystems 8-TC HAT** (~$90) or **Pico TC-08** (~$300) for high-resolution logging
2. Consider Type S thermocouples (1600C) if approaching Type K limits

### Thermocouple Probe Selection Notes
- For 800-1200C DRI, use **mineral-insulated (MI) Type K probes** with Inconel 600 sheath
- Standard fibreglass-insulated K probes are only good to ~500C
- MI probes with 6mm OD Inconel sheath: ~$15-30 on AliExpress, ~$50-100 from Omega/RS
- For furnace atmosphere (H2 + CO), ensure sheath material is compatible -- Inconel 600 is suitable
- Probe length: choose based on furnace insertion depth needed (typically 150-500mm)
- Consider **Type N thermocouples** as alternative to Type K -- better stability at 800-1200C, less drift over time, similar price
