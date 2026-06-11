# RetroPi TV

This is a miniature Raspberry Pi-powered desktop TV that continuously plays local video files on a small display, creating the effect of an always-on retro media appliance.

This project combines embedded Linux configuration, Raspberry Pi GPIO control, video playback automation, audio routing, 3D printing, and physical hardware assembly into a self-contained device.

---

## Preview

### Final Build


![Final RetroPi TV Build](assets/RetroTV_web.mp4)


## Project Overview

RetroPi TV is a compact Raspberry Pi-based media device designed to behave like a tiny always-on television. When powered on, the Raspberry Pi boots into a Linux environment, automatically starts the video player service, and continuously plays randomized local video files from storage.

The device includes:

* Raspberry Pi Zero running Raspberry Pi OS
* Waveshare 2.8-inch LCD display
* Local video playback from encoded media files
* Physical front power/display control
* Physical volume control
* Speaker output through an amplifier
* Custom 3D-printed enclosure
* Startup automation using `systemd`

The goal of this project was to build a functional embedded media appliance while learning how hardware, Linux services, GPIO, display configuration, and video processing work together in a real device.

---

## Key Features

* Continuous local video playback on boot
* Randomized media playback from a local video directory
* Headless Raspberry Pi setup over SSH
* Custom Waveshare LCD configuration
* GPIO-based physical button control
* PWM audio routed through GPIO pins
* Manual volume control using a potentiometer
* Speaker output through a mono amplifier
* Automated startup using Linux `systemd`
* 3D-printed retro TV enclosure
* Compact self-contained desktop form factor

---

## Technologies Used

### Hardware

* Raspberry Pi Zero with headers
* Waveshare 2.8-inch 640×480 LCD
* 2.5W mono amplifier
* 4-ohm speaker
* 1K potentiometer
* Push button
* Micro-USB breakout board
* 64GB microSD card
* 3D-printed enclosure
* Jumper wires and soldered connections

### Software

* Raspberry Pi OS Lite
* Linux
* Python
* GPIO
* `systemd`
* SSH
* H.264 video encoding
* `omxplayer`
* `raspi-gpio`
* `usbmount`

---

## System Architecture

```text
Encoded Video Files
        |
        v
~/simpsonstv/videos
        |
        v
Python Playback Script
        |
        v
omxplayer
        |
        +------------------> Waveshare LCD Display
        |
        +------------------> GPIO PWM Audio Output
                                |
                                v
                         Amplifier + Speaker
```

```text
Physical Button
        |
        v
GPIO Input
        |
        v
Python Button Script
        |
        v
Display / Playback Control
```

The Raspberry Pi handles video playback, display output, audio routing, and physical button input. Python scripts manage playback and button behavior, while `systemd` services ensure the device starts automatically whenever the Pi boots.

---

## How It Works

When the device powers on:

1. The Raspberry Pi boots into Raspberry Pi OS Lite.
2. Custom display settings route video output to the Waveshare LCD.
3. Audio is routed through GPIO using PWM audio configuration.
4. `systemd` starts the playback and button-control services.
5. The video player script selects local encoded videos and plays them continuously.
6. The physical button controls the screen behavior.
7. The physical volume knob adjusts speaker output.

This creates the illusion of a tiny TV that is always broadcasting content, even when the display is toggled off.

---

## Video Encoding Pipeline

The Raspberry Pi Zero has limited processing power and codec support, so videos are prepared before being copied to the device.

The media preparation workflow:

1. Collect source video files on a separate computer.
2. Convert videos into H.264 format with a 480-pixel height.
3. Store converted files in an `encoded` output folder.
4. Transfer encoded videos to a USB drive.
5. Copy the videos to the Raspberry Pi video directory.

Example project directory:

```text
simpsonstv/
├── videos/
│   ├── episode-1.mp4
│   ├── episode-2.mp4
│   └── episode-3.mp4
├── player.py
├── buttons.py
└── encode.py
```
---

## Challenges and What I Learned

### GPIO and Audio Routing

The Raspberry Pi Zero does not provide standard analog audio output, so audio had to be routed using PWM audio through GPIO. This required configuring the Raspberry Pi boot settings and wiring the GPIO audio output to an amplifier.

### Display Configuration

The Waveshare LCD required custom Raspberry Pi configuration so the Pi would output directly to the small screen instead of HDMI. This involved editing boot configuration settings, display overlays, DPI settings, and rotation values.

### Startup Automation

The device needed to behave like an appliance, not a normal computer. I used `systemd` services to automatically start the video playback and button-control scripts on boot.

### Resource Constraints

The Raspberry Pi Zero has limited processing power, so videos had to be encoded in a compatible format before being transferred to the device. This improved playback reliability and reduced processing load on the Pi.

### Hardware/Software Integration

This project required combining physical components, Linux configuration, Python scripts, wiring, and enclosure design into one working device.

---

## Future Improvements

Potential improvements for future versions:

* Add a custom startup animation
* Improve boot time with a lighter operating system
* Add near-gapless video playback
* Build a custom video selection interface
* Add TV-style channel graphics or overlays
* Add a shutdown-safe power button
* Add support for playlists or themed video folders
* Replace `omxplayer` with a more modern playback solution
* Add status LEDs for power or playback state
