# Twang 2D

A 2D take on [Twang](https://github.com/Critters/TWANG), the one-dimensional LED strip dungeon game played with a door-stop spring. This version targets a 64 × 64 LED matrix.

This repository holds a browser playtest demo. It simulates the matrix, the spring joystick and the game, so levels and tuning can be tested before the hardware exists.

## Run the demo

Open `index.html` in a browser. It has no build step and no dependencies.

## Controls

| Input | Tilt | Twang (attack) |
|---|---|---|
| Keyboard | Arrows or WASD | Space or Enter |
| Virtual spring | Drag the brass knob | Flick the knob fast, or press **Twang** |
| Gamepad | Left stick | Any button |

As on the original, tilt sets the player's speed directly, and the player cannot move while a twang attack rings out. A death restarts the level. A run has 3 lives.

## Levels

| # | Name | Teaches |
|---|---|---|
| 1 | First tilt | Moving around walls |
| 2 | Twang | Attacking enemies that stand still in a narrow corridor |
| 3 | Patrol ring | Enemies that swing back and forth on tracks |
| 4 | Spawners | Spawners that fire enemies across the room |
| 5 | Lava fields | Lava that switches between hot and cool |
| 6 | Conveyors | Belts that push the player |
| 7 | The boss | A 3 × 3 boss that shoots, takes 3 hits, and locks the exit |

The **Playtest** panel has a level select, sliders for player speed, twang radius and tilt dead zone, god mode, and a run log of deaths and best clear times per level.

## Bill of materials

The recommended build uses a HUB75 panel, not WS2812 LEDs (see the comparison below).

| # | Part | Qty | Example / spec | Approx. cost (USD) |
|---|---|---|---|---|
| 1 | 64 × 64 RGB LED panel, HUB75 | 1 | P3 (192 × 192 mm) or P2.5 (160 × 160 mm). Must have the **E** address line (1/32 scan). Usually comes with a ribbon cable and a power cable. | 25–45 |
| 2 | Controller | 1 | Adafruit Matrix Portal S3: ESP32-S3, plugs directly into the HUB75 connector, has a STEMMA QT (I2C) port. | 20 |
| 3 | IMU (accelerometer + gyro) | 1 | LSM6DSOX or LSM6DS3TR-C breakout with STEMMA QT. The original Twang used an MPU-6050 (GY-521); it also works but is an old part. | 5–12 |
| 4 | Door-stop spring | 1 | Spring door stop, about 75–90 mm long, rubber tip removed. | 3 |
| 5 | Spring-tip cap | 1 | 3D-printed cap that holds the IMU on the spring tip. | — |
| 6 | IMU wire | 1 m | 4-core flexible silicone wire, 26 AWG: 3.3 V, GND, SDA, SCL. | 3 |
| 7 | I2S audio amplifier | 1 | MAX98357A breakout. | 6 |
| 8 | Speaker | 1 | 4 Ω or 8 Ω, 2–3 W, 40 mm. | 3 |
| 9 | Power supply | 1 | 5 V 3 A USB-C supply into the Matrix Portal, which feeds the panel through its screw terminals. | 10 |
| 10 | Base | 1 | Wooden or 3D-printed base, about 150 × 150 mm, heavy enough not to move when the spring is twanged. The door stop screws into it. | 5 |
| 11 | Panel frame | 1 | 3D-printed or wooden. Optional smoked acrylic front for contrast. | 5–15 |
| 12 | Small parts | — | M3 screws, heat-shrink, cable ties, spiral wrap for the wire along the spring. | 5 |
| | **Total** | | | **about 90–130** |

Prices are rough estimates. Check current prices and stock with your supplier.

### Assembly notes

- **E address line:** close the address-E solder jumper on the Matrix Portal S3. Without it, a 64 × 64 panel shows only its top half correctly.
- **Spring wire:** run the 4 wires along the outside of the spring in spiral wrap. A standard STEMMA QT cable is stiff enough to change how the spring moves, so solder silicone wire to the IMU instead.
- **I2C length:** keep the IMU wire under about 0.5 m. Longer I2C runs can give read errors.
- **Power:** a 64 × 64 panel can draw about 4 A at full white. Game frames are mostly dark, so typical current is much lower. Cap the brightness in firmware so the panel stays within the 3 A supply.
- **Firmware libraries:**
  - Panel: Adafruit Protomatter or ESP32-HUB75-MatrixPanel-DMA.
  - IMU: Adafruit LSM6DS (or an MPU6050 library).
  - Sound: ESP32 I2S.

### Alternative: WS2812 build

| | HUB75 panel | WS2812 (16 × 16 panels × 16) |
|---|---|---|
| LEDs | 4,096 on one board | 4,096 on 16 boards |
| Maximum current at 5 V | about 4 A | up to about 245 A at full white (60 mA per LED) |
| Power supply | 5 V 3–4 A | Several large 5 V supplies, with power injection on every panel |
| Size | 160–192 mm | about 640 × 640 mm at 10 mm pitch |
| Matrix cost | 25–45 | 200–300 |
| Wiring | One ribbon cable | 16 data chains and thick power wires |

Use the WS2812 option only for a large wall installation.

The demo's **Est. draw @ 5 V** readout uses the WS2812 power model (about 20 mA per colour channel plus 1 mA idle per LED). It does not apply to a HUB75 panel, which lights only some rows at a time.
