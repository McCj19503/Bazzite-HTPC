## Hardware & Info
Utilized the generic amazon Pulse-Eight USB to HDMI CEC adapter. The adapter works by bridging CEC commands. Most GPUs don't actually provide HDMI CEC so this, in theory, is a very good utility. The adapter reads and injects signals into the HDMI signal by the TV, via from the USB port.  
Specifically, for Bazzite, the adapter utilizes Pulse-Eight's LibCEC library which reads the CEC signals.   
In short, the TV sends or recieves a CEC value (this is where manufacturers can differ between how they define these values afaik). The adapter then recieves that message and decodes it into a value you can reproduce. The issues, to my limited knowledge of outside examples (feel free to leave input), are that you will likely have to initialize your own values on a per-TV basis. It's not a ones-size-fits-all and can get quickly get confusing and difficult.


## Software Tools 
<ins>**libCEC**</ins>  
Pulse-Eight's libCEC decodes CEC messages from the TV and encodes commands you send. It is immensely vital to use this library.  
<ins>**Cec-Client**</ins>  
Comes with libCEC. Primary testing interface for viewing the CEC signal the TV is sending
