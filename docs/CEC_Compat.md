## Hardware
Utilized the generic amazon Pulse-Eight USB to HDMI CEC adapter. The adapter works by bridging CEC commands. Most GPUs don't actually provide HDMI CEC so this, in theory, is a very good utility. The adapter reads and injects signals into the HDMI signal by the TV, via from the USB port.  
Specifically, for Bazzite, the adapter utilizes Pulse-Eight's LibCEC library which reads the CEC signals 
