## Hardware & Info
Utilized the generic amazon Pulse-Eight USB to HDMI CEC adapter. The adapter works by bridging CEC commands. Most GPUs don't actually provide HDMI CEC so this, in theory, is a very good utility. The adapter reads and injects signals into the HDMI signal by the TV, via from the USB port.  
Specifically, for Bazzite, the adapter utilizes Pulse-Eight's LibCEC library which reads the CEC signals.   
In short, the TV sends or recieves a CEC value (this is where manufacturers can differ between how they define these values afaik). The adapter then recieves that message and decodes it into a value you can reproduce. The issues, to my limited knowledge of outside examples (feel free to leave input), are that you will likely have to initialize your own values on a per-TV basis. It's not a ones-size-fits-all and can get quickly confusing.



## Software Tools 
<ins>**libCEC**</ins>  
Pulse-Eight's libCEC decodes CEC messages from the TV and encodes commands you send.  

<ins>**Cec-Client**</ins>  
Comes with libCEC. Primary testing interface for viewing the CEC signal the TV is sending. Values seen similarly as `<< 01:44:01`  
* ```/etc/systemd/system/``` will host service files. These can execute alongside your system services (eg sleep & wake), calling your .sh files that run your primary scripts. The default installed services are in ```/usr/lib/systemd/system```.
* ```/usr/local/bin/``` hosts your shell files, which will actually send the signal.
## Setup
<ins>**Orientations**</ins>  
There are two primary ways to orientate the adapter.:  
***1***
```
      [ PC HDMI OUT ][PC USB OUT]
              │           │
              ▼           ▼
        [ Pulse-Eight Adapter ]
                   │
                   ▼
          [        TV        ]
```
***2***
```
[ PC HDMI OUT ]    [PC USB OUT]
        │           │
        │           ▼
        │    [ Pulse-Eight Adapter ]
        │            │(PE HDMI OUT)
        │            │
        │            │
        ▼            ▼
        [        TV        ]
```
Orientation 1 generally is the "cleaner" setup(or if you have limited intputs), though the problem is that the CEC adapter limits the signal to 4k60, so if you want better performance than that, you will likely prefer orientation 2, as you'll have a direction connection to the TV from your HTPC and will register the secondary input as a control system.

<ins>**How does Automation work?**</ins>  
* First thing to do is confirm that your adapter is properly detected, running ```sudo cec-client -l```. (you may have to stop a default service using the adapter by running ```lsof /dev/ttyACM0``` then ```sudo systemctl stop servicename.service```) 
* Once you can confirm it's detected, confirm you can connect to the adapter using ```sudo cec-client -p /dev/ttyACM0 -t p -d 1``` 
* The ```on``` and ```standby``` commands are easily the most important when it comes to CEC. You can test them via ```echo "on 0" | sudo cec-client -s -d 1 -p /dev/ttyACM0 -t p```.
```echo "standby 0" | sudo cec-client -s -d 1 -p /dev/ttyACM0 -t p``` If these both work, then you should already be golden. If these  don't, then that's what most of the doc is for:

**Confirm that the CEC commands are being read:**
* using ```sudo cec-client -p /dev/ttyACM0 -t p -d 1```, you should be able to see values such as ```<< 01:44:01```,```<< 4f:82:10:00``` when doing things such as powering on the TV, changing inputs, pressing remote buttons, etc. These are the raw CEC messages that are being sent across the BUS. I have yet to come across an instance with a television where the issue was these signals not existing. Good news: if they exist, you can use them!

**Creating new CEC commands:**

Primary goal here is to be able to find the CEC traffic and to set those values to return useful commands. CEC values generally follow the structure: ```[source][destination]:[operation][:data]```.  
Before, we used echo to trigger a default cec-client command that is set as a service by libCEC to output traffic to the adapter. This is pre-made and generally doesn't often send the correct signal (hence issues with many users turning off their devices).  
*So how do we make our own (automated) commands?*  
We create our own service that is set to run after system services (suspend.target, etc). These will then trigger .sh scripts, which we will use to transmit our cec commands.
```
system event
        ↓
systemd service triggers
        ↓
custom service runs
        ↓
script runs cec-client command
        ↓
TV responds
```

<ins>**Automation Setup**</ins>  

Confirming with the initial setup that your device is both recognized and that the CEC signals are being recognized-  

1. **Identify your working CEC commands**
Using ```sudo cec-client -p /dev/ttyACM0 -t p -d 1``` Confirm the consistent values that occur on an action(power on/off). Use these values returned into ```echo "tx <value>" | cec-client ...``` to double check that you can execute them yourself.
2. **Creating a script to automate the command.**
In the folder we will store our .sh files, ```/usr/local/bin/```, create a file that will transmit our value. For instances, ```/usr/local/bin/tv-on.sh```. Enter your script with the CEC value it coinsides with: ```#!/bin/bash
echo "on 0" | /usr/bin/cec-client -s -d 1 -p /dev/ttyACM0 -t p```, then make sure it can be executed with ```sudo chmod +x /usr/local/bin/tv-on.sh```
2. **Create systemd service files**
Our service files will be stored in ```/etc/systemd/system/```. Typically it will read along the lines of:
```
[Unit]
Description=Power On TV After Resume
After=suspend.target

[Service]
Type=oneshot
ExecStart=/bin/bash -c 'sleep 2 && /usr/local/bin/tv-on.sh'

[Install]
WantedBy=suspend.target
```
