# Hardware Setup Guide

This guide covers the physical setup of your IoT sensors and gateway for the H2-DRI lab telemetry system.

## Equipment List

| # | Device | Model | Type | Communication | Default Addr | Assigned Addr | Status |
|---|--------|-------|------|---------------|-------------|---------------|--------|
| 1 | CO Gas Sensor (ATO) | SN-3002-N01-V10-1000P-2 | Electrochemical | RS485 Modbus RTU | 0x01 | 0x01 (1) | VERIFIED WORKING (2026-03-23) |
| 2 | CO2/Temp/Humidity (NDIR) | SN-3002FL-CO2-N01 | NDIR | RS485 Modbus RTU | 0x01 | 0x02 (2) | VERIFIED WORKING (2026-03-27) |
| 3 | Pipeline CO (CERTEON) | IP65 duct-mount | Electrochemical | RS485 Modbus RTU | 0x01 | 0x03 (3) | Untested |
| 4 | BYD Display Controller | BYD/C (Wuxi Baiyida) | Controller | RS485 Modbus RTU | 0x01 | 0x04 (4) | Untested |
| 5 | BYD-2S IR Thermometer | BYD-2S140AR | IR Pyrometer | RS485 Modbus RTU | 0x01 | 0x05 (5) | Untested |
| 6 | Temp/Humidity (7-wire) | Unknown | Unknown | RS485? | ? | 0x06 (6) | Untested — 7 wires not yet identified |
| - | Waveshare RS485-TO-WIFI/ETH | RS485-TO-WIFI/ETH | Gateway | WiFi/Ethernet | N/A | N/A | Needs configuration |

## Detailed Sensor Specifications

### Sensor 1: CO Gas Sensor (ATO)

| Parameter | Value |
|-----------|-------|
| Model | SN-3002-N01-V10-1000P-2 |
| Type | Electrochemical CO sensor |
| Measurement Range | 0-1000 ppm |
| Power Supply | 10-30V DC |
| Baud Rate | 4800 (default) |
| Data Format | 8-N-1 |
| Function Codes | 0x03, 0x04 |
| Warm-up Time | >= 5 minutes |
| Wire Colors | Brown=VCC, Black=GND, Yellow=A(Data+), Blue=B(Data-) |
| **Note** | This is a CO-only variant — humidity and temperature registers always return 0 |

**Registers:**

| Register | Content | Scale | Normal Room Value |
|----------|---------|-------|-------------------|
| 0x0000 | Humidity | x 0.1 = %RH | 0 (CO-only variant) |
| 0x0001 | Temperature | x 0.1 = °C (signed) | 0 (CO-only variant) |
| 0x0002 | CO concentration | raw ppm | 0-5 ppm (clean air) |
| 0x07D0 | Device address | 1-254 | 1 (default) |
| 0x07D1 | Baud rate setting | see table | 1 (4800) |

**Verified:** 2026-03-23 on Windows laptop via CH340 USB-to-RS485 adapter (COM3). Detected 115 ppm CO from candle smoke test.

### Sensor 2: CO2/Temperature/Humidity Sensor (NDIR)

| Parameter | Value |
|-----------|-------|
| Model | SN-3002FL-CO2-N01 |
| Manufacturer | 普锐森 (PRSens) / 威盟士 (Weimengshi) |
| Type | NDIR infrared CO2 + temperature + humidity |
| CO2 Range | 0-5000 ppm |
| CO2 Accuracy | ±(50 ppm + 3% F.S.) @ 25°C, 400-5000 ppm |
| Temperature Range | -40°C to +80°C, ±0.5°C accuracy |
| Humidity Range | 0-100% RH, ±3% RH accuracy |
| Power Supply | 10-30V DC, 0.3W (24V DC), <85mA avg |
| Baud Rate | 4800 (default) |
| Data Format | 8-N-1 |
| Function Codes | 0x03, 0x04 (read), 0x06, 0x10 (write) |
| Data Update Interval | 2 seconds |
| Response Time | <90 seconds for 90% step change |
| Warm-up Time | 2 minutes (usable), 10 minutes (max accuracy) |
| Dimensions | 110 x 85 x 44 mm |
| Protection | IP65 waterproof housing |
| Wire Colors | Brown=VCC, Black=GND, Yellow=A(Data+), Blue=B(Data-) |

**Registers:**

| Register | PLC Address | Content | Function Codes | Range |
|----------|-------------|---------|----------------|-------|
| 0x0000 | 40001 | Humidity value | 0x03/0x04 | 0-1000 (x0.1 = %RH) |
| 0x0001 | 40002 | Temperature value | 0x03/0x04 | -400 to 1000 (x0.1 = °C, signed) |
| 0x0002 | 40003 | CO2 concentration | 0x03/0x04 | 0-5000 ppm |
| 0x0030 | 40049 | Temperature upper limit | 0x03/0x04/0x06/0x10 | -400 to 1000 |
| 0x0031 | 40050 | Temperature lower limit | 0x03/0x04/0x06/0x10 | -400 to 1000 |
| 0x0032 | 40051 | Temperature calibration | 0x03/0x04/0x06/0x10 | -400 to 1000 |
| 0x0033 | 40052 | Humidity upper limit | 0x03/0x04/0x06/0x10 | 0-1000 |
| 0x0034 | 40053 | Humidity lower limit | 0x03/0x04/0x06/0x10 | 0-1000 |
| 0x0035 | 40054 | Humidity calibration | 0x03/0x04/0x06/0x10 | -400 to 1000 |
| 0x0036 | 40055 | CO2 upper limit | 0x03/0x04/0x06/0x10 | 0-5000 |
| 0x0037 | 40056 | CO2 lower limit | 0x03/0x04/0x06/0x10 | 0-5000 |
| 0x0038 | 40057 | CO2 calibration | 0x03/0x04/0x06/0x10 | -2000 to 2000 |
| 0x07D0 | 42001 | Device address | 0x03/0x04/0x06/0x10 | 1-254 (default: 1) |
| 0x07D1 | 42002 | Baud rate setting | 0x03/0x04/0x06/0x10 | see baud table |

**Baud Rate Table (register 0x07D1):**

| Value | Baud Rate |
|-------|-----------|
| 0 | 2400 |
| 1 | 4800 (default) |
| 2 | 9600 |
| 3 | 19200 |
| 4 | 38400 |
| 5 | 57600 |
| 6 | 115200 |
| 7 | 1200 |

**Precautions:**
- Do not expose to corrosive gases for extended periods
- Avoid prolonged exposure to high concentrations of organic gases (causes zero-point drift)
- Do not store in environments with high concentrations of alkaline gases
- Avoid strong airflow around the sensor
- No contact with organic solvents, paints, chemicals, oils, or high-concentration gases

**Verified:** 2026-03-27 on Windows laptop via CH340 USB-to-RS485 adapter (COM4). Address changed from 1 to 2 via Modbus command. Readings: 47.3% RH, 473 ppm CO2 (indoor room).

### Sensor 3: Indoor Temp/Humidity Sensor (LCD Display)

| Parameter | Value |
|-----------|-------|
| Model | SN-3005MG-FL-WS-N01 |
| Manufacturer | PRSens (普锐森) |
| Type | Temperature + Humidity with LCD display |
| Power Supply | 10-30V DC |
| Baud Rate | 4800 (default) |
| Data Format | 8-N-1 |
| Function Codes | 0x03, 0x04 |
| Wire Colors | Brown=VCC, Black=GND, Yellow=A(Data+), Blue=B(Data-) |

**Registers:**

| Register | Content | Scale |
|----------|---------|-------|
| 0x0000 | Humidity | x 0.1 = %RH |
| 0x0001 | Temperature | x 0.1 = °C (signed) |
| 0x07D0 | Device address | 1-254 (default: 1) |
| 0x07D1 | Baud rate setting | see baud table |

**Verified:** 2026-03-27 on Windows laptop via CH340 USB-to-RS485 adapter (COM4). Address changed from 1 to 3. Readings: 67.0% RH, 19.0°C (indoor lab).

### Baud Rate Conflict Note

Sensors 1-3 use **4800 baud** (default). Sensors 4-5 (BYD) use **9600 baud** (default). All devices on a single RS485 bus must share the same baud rate. Options:
- Change BYD sensors to 4800, OR
- Use two separate RS485 buses (one per baud rate group)

---

## Step 1: Configure Modbus Addresses

**Important:** All sensors default to address 0x01. You must assign unique addresses before connecting them to the same RS485 bus.

### Recommended Address Assignments

| Device | Address (Hex) | Address (Dec) | Baud Rate |
|--------|--------------|---------------|-----------|
| CO Gas Sensor | 0x01 | 1 | 4800 |
| CO2/Temp/Humidity | 0x02 | 2 | 4800 |
| Pipeline CO Sensor | 0x03 | 3 | 4800 |
| BYD Display Controller | 0x04 | 4 | 9600 |
| BYD-2S IR Thermometer | 0x05 | 5 | 9600 |

### Changing Address on CO/CO2 Sensors

Use a USB-to-RS485 adapter to configure each sensor individually. **Only connect one sensor at a time** when changing addresses.

**Option A: Python script (recommended — works reliably with CH340 adapters):**

```python
import serial, struct, time

PORT = "COM4"       # Check Device Manager for correct port
BAUD = 4800
CURRENT_ADDR = 1    # Current sensor address
NEW_ADDR = 2        # Desired new address

def crc16_modbus(data):
    crc = 0xFFFF
    for byte in data:
        crc ^= byte
        for _ in range(8):
            crc = (crc >> 1) ^ 0xA001 if crc & 1 else crc >> 1
    return crc

ser = serial.Serial(PORT, BAUD, timeout=2, bytesize=8, parity="N", stopbits=1)
time.sleep(0.5)

# Write new address to register 0x07D0 using function code 0x06
req = struct.pack(">BBHH", CURRENT_ADDR, 0x06, 0x07D0, NEW_ADDR)
req += struct.pack("<H", crc16_modbus(req))
ser.reset_input_buffer()
ser.write(req)
time.sleep(1)
resp = ser.read(100)
print(f"Response: {resp.hex()}" if resp else "No response — power cycle and re-test")
ser.close()
```

**Option B: Manufacturer config software (RS485ControlV2.exe):**
1. Connect sensor to USB-RS485 adapter
2. Open RS485ControlV2.exe (included with sensor, or use [SensorOneSet](https://www.infwin.com/sensoroneset-configuration-utility-for-rs485-sensors/))
3. Select correct COM port
4. Click "Test Baud Rate" to detect current settings
5. Change device address in the Address field
6. Click "Apply"

**Raw Modbus command reference:**
```
Set address 1 to address 2:
Request:  01 06 07 D0 00 02 08 86
Response: 01 06 07 D0 00 02 08 86
```

### Changing Address on BYD Instruments

1. Press the Setting button (⚙) to enter settings mode
2. Enter password: 2000
3. Navigate to "Id" parameter using the Shift button
4. Use Up/Down buttons to set new address (1-255)
5. Press Setting to confirm
6. Restart the instrument

**Baud Rate Settings (BAUD parameter):**
- 0 = 1200
- 1 = 2400
- 2 = 4800
- 3 = 9600 (recommended for BYD instruments)

---

## Step 2: RS485 Bus Wiring

### Wiring Diagram

```
[Power Supply 24VDC]
       |
       +----[CO Sensor]----+
       |         |         |
       |        A B        |
       |         | |       |
       +----[CO2 Sensor]---+
       |         |         |
       |        A B        |
       |         | |       |
       +--[Pipeline CO]----+
       |         |         |
       |        A B        |
       |         | |       |
       +--[BYD Display]----+
       |         |         |
       |        A B        |
       |         | |       |
       +--[BYD-2S IR]------+
       |         |         |
       |        A B        |
       |         | |       |
       +----[Gateway]------+
                 |
            [120Ω Resistor]
```

### Wire Colors (Standard)

| Wire Color | Function |
|------------|----------|
| Yellow | RS485-A (Data+) |
| Blue | RS485-B (Data-) |
| Brown/Red | Power + (10-30VDC) |
| Black | Power - (GND) |

### Wiring Steps

1. **Power Off** all devices before wiring

2. **Connect RS485 Data Lines:**
   - Connect all A terminals together (Yellow wires)
   - Connect all B terminals together (Blue wires)
   - Do NOT swap A and B - this will cause communication failure

3. **Connect Power:**
   - Each sensor needs 10-30VDC (24V recommended)
   - Use appropriately rated power supply (calculate total current draw)
   - Gateway needs 5-36VDC

4. **Add Termination Resistor:**
   - Add 120Ω resistor between A and B at the end of the bus
   - Required if total cable length > 100m
   - Helps prevent signal reflections

5. **Verify Connections:**
   - Check for continuity on A and B lines
   - Verify no short between A and B
   - Verify power polarity

### Cable Recommendations

- Use twisted pair shielded cable for RS485
- Maximum bus length: 1200m (at 9600 baud)
- Keep RS485 cables away from high-voltage/high-frequency equipment
- Ground the shield at one end only

---

## Step 3: Gateway Configuration

### Access Gateway Web Interface

1. Power on the Waveshare gateway
2. Connect your computer to gateway WiFi: `RS485_TO_WIFI_xxxx`
3. Open browser: `http://10.10.100.254`
4. Login: admin / admin (default)

### Configure WiFi (STA Mode)

1. Go to **Mode Selection**
2. Select **Station** mode
3. Go to **STA Interface Setting**
4. Enter your WiFi credentials:
   - SSID: Your network name
   - Security: WPA2PSK/AES
   - Password: Your WiFi password
5. Set IP: DHCP or Static (e.g., 192.168.1.100)
6. Click **Apply** then **Restart**

### Configure Serial Port

1. Go to **Application Setting**
2. Set UART parameters:
   - Baud Rate: **9600** (use highest common rate)
   - Data Bits: 8
   - Stop Bits: 1
   - Parity: None
3. Work Mode: **Modbus TCP ⟷ Modbus RTU**

### Configure Network Socket

1. Socket A settings:
   - Protocol: TCP
   - Role: Server
   - Local Port: 502 (Modbus TCP standard port)
2. Click **Apply**

### Verify Connection

After restarting:
1. Reconnect to your main WiFi network
2. Find gateway's IP address (check router or use `RS485_TO_WIFI_xxxx` AP)
3. Ping the gateway: `ping 192.168.1.100`
4. Gateway is ready when you get a response

---

## Step 4: Test Individual Sensors via Direct USB-to-RS485

Before connecting sensors to the Waveshare gateway, test each sensor individually using a USB-to-RS485 adapter. This isolates the sensor from gateway/network issues.

### Requirements

- USB-to-RS485 adapter (CH340/CH341 chipset, appears as COM port on Windows or /dev/ttyUSB0 on Linux)
- Python 3 with `pyserial` and `minimalmodbus` installed: `pip install minimalmodbus`
- 10-30V DC power supply for the sensor (24V recommended)

### Wiring for Direct USB Test

```
[Power Supply 10-30VDC]
    (+) ──── Brown wire (VCC)
    (-) ──── Black wire (GND)

[USB-to-RS485 Adapter]
     A  ──── Yellow wire (Data+)
     B  ──── Blue wire (Data-)
```

### Known Issue: CH340 Adapters and minimalmodbus

CH340/CH341 USB-to-RS485 adapters may not work correctly with `minimalmodbus` out of the box. The adapter can echo TX bytes or produce framing that confuses the library. Use raw `pyserial` instead:

```python
import serial
import struct
import time

PORT = "COM3"       # Windows — check Device Manager
# PORT = "/dev/ttyUSB0"  # Linux (Raspberry Pi)
BAUD = 4800
ADDR = 1

def crc16_modbus(data):
    crc = 0xFFFF
    for byte in data:
        crc ^= byte
        for _ in range(8):
            crc = (crc >> 1) ^ 0xA001 if crc & 1 else crc >> 1
    return crc

ser = serial.Serial(PORT, BAUD, timeout=2, bytesize=8, parity="N", stopbits=1)
time.sleep(0.5)

# Build Modbus RTU request: read 3 holding registers starting at 0x0000
req = struct.pack(">BBHH", ADDR, 0x03, 0x0000, 3)
req += struct.pack("<H", crc16_modbus(req))

ser.reset_input_buffer()
ser.write(req)
time.sleep(0.5)
resp = ser.read(100)

print(f"Raw response ({len(resp)} bytes): {resp.hex()}")

if len(resp) >= 11:
    humidity = struct.unpack(">H", resp[3:5])[0] / 10
    temperature = struct.unpack(">h", resp[5:7])[0] / 10
    gas = struct.unpack(">H", resp[7:9])[0]
    print(f"Humidity: {humidity}%RH  Temp: {temperature}C  Gas: {gas} ppm")
else:
    print("No valid response — check wiring and power")

ser.close()
```

### Warm-up Time

Sensors require a warm-up period after power-on before readings are valid:

| Sensor Type | Warm-up Time | Notes |
|-------------|-------------|-------|
| CO (ATO, electrochemical) | >= 5 minutes | Registers read 0 during warm-up |
| CO2 (NDIR) | 2-3 minutes | NDIR lamp must stabilize |

**Registers will return all zeros during warm-up — this is normal.** Wait the full warm-up time before concluding the sensor is faulty.

### Expected Register Values (Direct USB Test)

**CO Sensor (ATO) — Address 1, Baud 4800:**

| Register | Content | Scale | Normal Room Value |
|----------|---------|-------|-------------------|
| 0x0000 | Humidity | x 0.1 = %RH | 30-60% (0 if CO-only variant) |
| 0x0001 | Temperature | x 0.1 = °C (two's complement) | 20-25°C (0 if CO-only variant) |
| 0x0002 | CO concentration | raw ppm | 0-5 ppm (normal air) |

**CO2 Sensor (SN-3002FL-CO2-N01) — Address 1 (default), Baud 4800:**

| Register | Content | Scale | Normal Room Value |
|----------|---------|-------|-------------------|
| 0x0000 | Humidity | x 0.1 = %RH | 30-60% |
| 0x0001 | Temperature | x 0.1 = °C (two's complement) | 20-25°C |
| 0x0002 | CO2 concentration | raw ppm | 400-600 ppm (indoor air) |

**Config registers (all sensors):**

| Register | Content |
|----------|---------|
| 0x07D0 | Device address (read/write) |
| 0x07D1 | Baud rate setting (read/write) |

**Note:** CO at 0 ppm in a normal room is a valid reading — there is no carbon monoxide in clean indoor air. To verify the CO sensor is working, you would need a controlled CO source.

### Troubleshooting Direct USB Test

| Symptom | Cause | Fix |
|---------|-------|-----|
| No response at all | Sensor not powered | Check 10-30V DC on Brown(+)/Black(GND) |
| No response at all | A/B wires swapped on adapter | Swap Yellow and Blue on the adapter (see CRITICAL note below) |
| No response at all | No common ground | Add jumper wire from power supply GND to USB adapter GND |
| No response at all | Wrong baud rate | Try 4800, 9600, 19200 |
| minimalmodbus NoResponseError | CH340 echo/framing issue | Use raw pyserial script above |
| All registers read 0 | Sensor warming up | Wait >= 5 minutes after power-on |
| All registers read 0 (after warm-up) | CO-only variant (no temp/humidity probes) | Register 2 (CO) at 0 ppm may be correct in clean air |
| Humidity/Temp = 0 but gas > 0 | CO-only or CO2-only variant | Normal — not all units include temp/humidity |

### Verified Working Configuration (2026-03-23)

Tested on Windows laptop via CH340 USB-to-RS485 adapter (COM3):
- **CO Sensor (ATO):** Address 1, Baud 4800, 8-N-1, Function Code 0x03
- Modbus communication confirmed working
- Sensor MCU responds with valid framing and CRC
- Registers 0-2 readable, config registers 0x07D0-0x07D1 readable

---

## Step 5: Test Communication via Gateway

### Quick Test with Modbus Tool

Use a Modbus TCP client to test communication through the Waveshare gateway:

1. Download: Modbus Poll, QModMaster, or similar
2. Connect to gateway IP:502
3. Read holding registers from address 1:
   - Slave ID: 1
   - Function: 03 (Read Holding Registers)
   - Start Address: 0
   - Quantity: 3

If successful, you'll see sensor values!

### Expected Register Values

**CO Sensor (Address 1):**
- Register 0: Humidity (x 0.1 = %RH)
- Register 1: Temperature (x 0.1 = °C)
- Register 2: CO concentration (ppm)

**CO2 Sensor (Address 2):**
- Register 0: Humidity (x 0.1 = %RH)
- Register 1: Temperature (x 0.1 = °C)
- Register 2: CO2 concentration (ppm)

**BYD-2S IR Thermometer (Address 5):**
- Register 0: Temperature (°C)

---

## Troubleshooting

### No Communication

1. **Check wiring:** A to A, B to B (not swapped)
2. **Check addresses:** Each sensor must have unique address
3. **Check baud rate:** All devices must use same baud rate
4. **Check termination:** Add 120Ω resistor if cable > 100m
5. **Check power:** Each sensor must be powered

### Intermittent Communication

1. **Cable issues:** Replace or shorten cables
2. **Interference:** Keep RS485 away from motors/VFDs
3. **Multiple terminations:** Only one 120Ω at end of bus
4. **Grounding:** Ground shield at one end only

### Gateway Not Connecting to WiFi

1. **Check credentials:** SSID and password are case-sensitive
2. **Check signal:** Move gateway closer to router
3. **Check channel:** Try static channel on router
4. **Reset gateway:** Hold Reload button 5+ seconds

---

## Safety Notes

### BYD-2S IR Thermometer + Protective Shield

- **Water cooling required** when ambient temperature > 60°C
- Water temperature must be ≤ 35°C
- Water pressure: 0.2-0.4 MPa
- Water flow: 0.15-0.2 m³/min

### Induction Furnace Environment

- Keep sensors away from magnetic fields
- Use shielded cables
- Ensure proper grounding
- IR thermometer must have clear line of sight to target
- Clean protective shield lens regularly

---

## Proposed Future Equipment

### Quartz Crucible Cover for Induction Furnace

A fused quartz lid/dome over the induction furnace crucible enables atmosphere control and integrates with the sensor chain.

**Why needed:**
- Molten steel oxidizes rapidly in open air, forming slag and degrading quality
- Controlled atmosphere (argon gas blanket) prevents oxidation
- Exhaust port feeds off-gas to the sensor chain for real-time monitoring
- Quartz is transparent to near-IR (0.75-1.1μm) — the BYD-2S IR thermometer can measure melt temperature through it

**Design:**

```
         [BYD-2S IR Thermometer]
          Addr: 5, looks down through quartz
                  │
                  ▼
        ┌─────────────────────────┐
        │   Fused Quartz Lid      │  ← IR transparent at 0.75-1.1μm
        │                         │
        │  ┌───┐          ┌───┐   │
        │  │Gas│          │Gas│   │
        │  │In │          │Out│──────→ [Pipeline CO] → [CO2] → [Temp/Hum] → Vent
        │  │(Ar)          │   │   │         Addr:3      Addr:2    Addr:6
        └──┴───┴──────────┴───┴───┘
        ┌─────────────────────────┐
        │                         │
        │       CRUCIBLE          │
        │     (molten steel)      │
        │                         │
        └─────────────────────────┘
        ┌─────────────────────────┐
        │   Induction Coil        │
        │   (water-cooled Cu)     │
        └─────────────────────────┘
```

**Specification for supplier:**
- Fused quartz (NOT soda-lime glass) — rated to 1000°C+
- Sized to fit crucible diameter (measure and specify)
- Flat or domed viewing port on top for IR pyrometer (0.75-1.1μm wavelength)
- Gas inlet port (6-8mm OD tube fitting) for argon
- Gas exhaust outlet port (6-8mm OD tube fitting) for sensor chain
- BYD-2S water-cooled shield (BYD-FJ-BHZ2S) mounts above the quartz, protects IR thermometer when ambient >60°C

**Suppliers to investigate:**
- AliExpress: search "quartz glass crucible cover custom" or "quartz bell jar"
- Australia: Thermoline Scientific, Technical Glass Products
- CDOCAST (complete lab vacuum induction systems for reference)

### Exhaust Sensor Chain

Once the quartz lid is installed, the exhaust port connects to the sensor chain:

```
Crucible exhaust → Pipeline CO Sensor (Addr 3, IP65 duct-mount)
                 → CO2 Sensor (Addr 2)
                 → Temp/Humidity (Addr 6)
                 → Safe vent (outside or fume hood)
```

This gives real-time off-gas analysis during melting — CO/CO2 levels indicate reduction progress and combustion state.

### Argon Gas Supply

- Standard argon bottle with regulator
- Flow rate: 5-10 L/min (enough to blanket crucible surface)
- Connect to quartz lid gas inlet port via silicone or PTFE tubing

---

---

## Setup Log

### 2026-03-23: CO Sensor Verified

- **Machine:** Laptop (LAPTOP-5VQ88K3R, Tailscale 100.83.125.126)
- **Adapter:** CH340 USB-to-RS485 on COM3
- **Result:** CO sensor (address 1, 4800 baud) working. Detected 115 ppm CO from candle smoke test.
- **Discovery:** minimalmodbus library doesn't work reliably with CH340 adapters — raw pyserial with manual CRC calculation works perfectly.
- **Test scripts saved on laptop:** `modbus_debug.py`, `modbus_read.py`, `modbus_live.py`, `modbus_scan.py`

### 2026-03-27: CO2 Sensor Setup

- **Machine:** Laptop (LAPTOP-5VQ88K3R, Tailscale 100.83.125.126)
- **Adapter:** Same CH340 USB-to-RS485, now on COM4

**Timeline of troubleshooting:**

1. Connected CO2 sensor with Yellow=A, Blue=B per manual. Powered with 24V (red light on). **No response.**
2. Scanned all addresses (1-5, 80, 254) at all baud rates (1200-115200). **No response.**
3. Added common ground (jumper from 24V PSU GND to USB adapter GND). **No response.**
4. Swapped Yellow and Blue wires on USB adapter (A↔B). **No response** (but note: first test after swap failed because swapping the Black/GND wire killed power — red light went off).
5. Restored correct power wiring (Black back to PSU GND). Red light back on.
6. Plugged CO sensor back in to verify adapter works — **CO sensor responded immediately** at address 1. Adapter confirmed working.
7. Reconnected CO2 sensor (with A/B still swapped from step 4). **SUCCESS!** Sensor responded at address 1, baud 4800.
8. Changed CO2 sensor address from 1 to 2 via Modbus write to register 0x07D0. **Confirmed working at address 2.**
9. Final readings: 47.3% RH, 0.0°C (warming up), 473 ppm CO2 (indoor room — realistic).

**CRITICAL LESSON — A/B Polarity:**

Both sensors use Yellow=A, Blue=B as per the manual. The issue was likely a loose connection rather than a true polarity swap. After physically reconnecting the wires (unplugging and re-seating), the CO2 sensor started responding. **If a sensor doesn't respond, try re-seating the wire connections before assuming A/B are swapped.**

**Configuration software:**
- Manufacturer provides RS485ControlV2.exe (Windows GUI) but it was not needed
- Generic alternative: [SensorOneSet v3.3.0](https://www.infwin.com/sensoroneset-configuration-utility-for-rs485-sensors/) — downloaded to laptop at `C:\Users\oscar\SensorOneSet\`
- All configuration was done successfully via Python scripts over SSH

### Current Sensor Status (2026-03-27)

| Sensor | Address | Baud | Verified | A/B Polarity on CH340 |
|--------|---------|------|----------|-----------------------|
| CO (ATO) | 1 | 4800 | YES | Yellow=A, Blue=B |
| CO2 (SN-3002FL) | 2 | 4800 | YES | Yellow=A, Blue=B |
| Temp/Humidity (SN-3005MG) | 3 | 4800 | YES | Yellow=A, Blue=B |
| Pipeline CO | 4 | 4800 | NO | Yellow=A, Blue=B (expected) |
| BYD Display | 5 | 9600 | NO | TBD |
| BYD-2S IR | 6 | 9600 | NO | TBD |

### Remaining Setup Steps

1. **Configure Waveshare gateway** — connect to its WiFi hotspot, set baud 4800, Modbus TCP mode, port 502, join lab WiFi at 192.168.1.100
2. **Wire sensors to gateway** (instead of USB adapter) — daisy-chain on RS485 bus
3. **Switch Pi .env** to `MODBUS_MODE=tcp`, `MODBUS_GATEWAY_HOST=192.168.1.100`, port 502
4. **Register devices** — run `npx tsx scripts/register-devices.ts` on Pi
5. **Start services** — API, Modbus poller, frontend on Pi
6. **Verify** — Monitor tab should show live readings

### Infrastructure Status (2026-03-27)

**Raspberry Pi 5** (100.126.209.39):
- PostgreSQL: running (h2dri_telemetry)
- InfluxDB: running (localhost:8086)
- API: running on port 8085
- Frontend: running on port 5181 (access via http://100.126.209.39:5181)
- Poller: not yet started (waiting for gateway setup)
- No USB-RS485 adapter connected to Pi

**Waveshare Gateway** (192.168.1.100):
- Pingable from Pi and laptop
- No TCP ports open — needs configuration via WiFi hotspot
- Required settings: Baud 4800, 8-N-1, Modbus TCP↔RTU, port 502, Station mode

---

## Next Steps

After completing hardware setup:

1. Run database setup: `npx tsx scripts/setup-db.ts`
2. Start the telemetry service: `npm run dev`
3. Register devices: `npx tsx scripts/register-devices.ts`
4. Test readings: `npx tsx scripts/test-readings.ts`
5. Open dashboard: `http://localhost:5180`
