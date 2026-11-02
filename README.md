# Assistive Device for Blind and Deaf Users

An Arduino-based wearable aid made of two devices:

1. **Obstacle alert device:** four ultrasonic sensors detect nearby obstacles. A voice message plays for blind users and a text message appears on an LCD for deaf users.
2. **Emergency safety device:** a button press sends an SMS with the wearer's GPS location through a GSM module.

## Features

| Device | What it does |
|---|---|
| Obstacle alert | Detects objects closer than 55 cm with 4 ultrasonic sensors (2 front, 2 back) |
| Voice output | DFPlayer Mini plays a pre-recorded message from an SD card (track 1 = front, track 2 = back) |
| Visual output | 16x2 LCD shows "Something at your frontside / backside" |
| Emergency SMS | GPS module gets the position; GSM module texts "I want Help" with latitude and longitude when the button is pressed |

## Circuit diagrams

**Obstacle alert device**

![Obstacle alert circuit](Circuit_Close_detection.png)

**Safety device**

![Safety device circuit](Circuit_Safety_Device.png)

The obstacle-alert circuit is also available as a Fritzing file: `Circuit_Close_Detection.fzz`.

## Hardware

**Obstacle alert device**
- Arduino board
- 4 x ultrasonic distance sensors (trigger/echo type)
- DFPlayer Mini MP3 module, SD card with audio tracks, and a speaker
- 16x2 LCD (parallel connection)

**Safety device**
- Arduino board
- GPS module
- GSM module (SMS)
- Push button and an indicator LED

## Software

- Arduino IDE
- Libraries: `DFRobotDFPlayerMini`, `LiquidCrystal`, `SoftwareSerial`, `TinyGPS++`

## How it works

**Obstacle alert** (`Code_close_detection.ino`)
1. Each sensor sends a 10 µs trigger pulse and measures the echo time to calculate the distance.
2. If an object is closer than 55 cm, the DFPlayer plays the matching voice track and the LCD shows the message.
3. The four sensors are read one after another in a loop.

**Safety device** (`Code_Safety_Device.ino`)
1. The GPS module is read continuously and the last position is stored.
2. When the button is pressed, the Arduino sends AT commands to the GSM module to text "I want Help" with the stored latitude and longitude.

## Setup

1. Wire the circuits as shown in the diagrams.
2. Copy the audio tracks to the DFPlayer SD card (`0001` = front warning, `0002` = back warning).
3. In `Code_Safety_Device.ino`, replace the placeholder phone number `+9194XXXXXX` with the number that should receive alerts.
4. Install the libraries in the Arduino IDE, then upload each sketch to its own board.

## Limitations

- Prototype only; not tested as a certified assistive device.
- Sensors are read one at a time with delays, so the response time is a few hundred milliseconds.
- If the GPS has no satellite fix, the SMS contains a
