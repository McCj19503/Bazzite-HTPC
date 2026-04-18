## Hardware & Info
Utilized the generic amazon Pulse-Eight USB to HDMI CEC adapter. The adapter works by bridging CEC commands. Most GPUs don't actually provide HDMI CEC so this, in theory, is a very good utility. The adapter reads and injects signals into the HDMI signal by the TV, via from the USB port.  
Specifically, for Bazzite, the adapter utilizes Pulse-Eight's LibCEC library which reads the CEC signals.   
In short, the TV sends or recieves a CEC value (this is where manufacturers can differ between how they define these values afaik). The adapter then recieves that message and decodes it into a value you can reproduce. The issues, to my limited knowledge of outside examples (feel free to leave input), are that you will likely have to initialize your own values on a per-TV basis. It's not a ones-size-fits-all and can get quickly get confusing and difficult.



## Software Tools 
<ins>**libCEC**</ins>  
Pulse-Eight's libCEC decodes CEC messages from the TV and encodes commands you send. It is immensely vital to use this library.  
<ins>**Cec-Client**</ins>  
Comes with libCEC. Primary testing interface for viewing the CEC signal the TV is sending. Values seen similarly as `<< 01:44:01`  
* ```/etc/systemd/system/``` will host service files. These can execute alongside your system services (eg sleep & wake), calling your .sh files that run your primary scripts.
* ```/usr/local/bin/``` hosts your shell files, which will actually send the signal  
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



