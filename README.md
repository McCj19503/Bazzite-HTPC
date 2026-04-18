# Bazzite HTPC Documentation

Personal documentation for any and all tools implemented to optimize a Bazzite HTPC to have the most console-like experience!

⠀
## Main
* [Hardware](readme.md#Hardware)
* Initial system/software setup
* HDMI CEC compatability
* HDMI 2.1 workaround
* USB wake
* Minor optimizations

⠀
## Hardware

<sub>Facebook marketplace is not overated! I saved like 60-70% on low-risk components.</sub>

⠀
<ins>***Chassis:***</ins>
*Silverstone GD05-B*
Best thing I could find for the performance I wanted due to size constraints in a TV cabinet. Has space for a sata DVD drive as well.

⠀
<ins>***Motherboard:***</ins>
*MSI MAG B550 TOMAHAWK MAX WIFI*
The only factor was finding a motherboard with USB-Wake. Unfortunately most motherboard manufacturers don't include that information, even in the manual, so it's either a gamble, or find something that has already been discussed online before.

⠀
<ins>***GPU:***</ins>
*AMD 7900XT.*
Each GPU has it's own quirks but it seems AMD will consistently be the most stable.

⠀
<ins>***Others:***</ins>
These are generally going to be preference based.
* AM4 used 5800x3d and TeamForce 32gb RAM. (stability/ram prices)
* 1TB WD Black m.2 SSD ()
* 





⠀








<ins>***Motherboard:***</ins>SeaGate internal 4TB HDD.
* Generic 5.25" optical drive.


## unf
- USB-CEC adapter
  - Pulse-Eight USB-CEC Adapter recommended
- HDMI cable routed through the adapter
- TV with HDMI-CEC support
  - On Samsung TVs, enable Anynet+

## Software Requirements

- Bazzite
- `libcec`
- `cec-client`
- `systemd`

## Setup

### 1. Verify the CEC adapter is detected

```bash
ls /dev/ttyACM*


- Power on TV when the HTPC boots
- Power off TV when the HTPC sleeps or shuts down
- Works in Gamescope (Game Mode) and Desktop Mode
- Compatible with Samsung TVs using Anynet+
- Uses native Linux tools: `libcec`, `cec-client`, and `systemd`
