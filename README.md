# Linux power optimization tutorial for Nvidia and Ubuntu based distribuitions

Optimizing power consumption for Linux/Unix system is not an easy tasks. We had very few contents about this theme, and few people available to discuss that. This tutorial is intended for users of nvidia/ubuntu based laptops.

1. **Know your hardware and powertop** - First step we need to do for start optimizing power in our system is know what hardware we are using. Intel based devices are easier to tune, because many of them have opensource implementations. Most devices have a power save mode, and unlike windows, in Linux/Unix system thei aren't configured to save power by default. We need to get information about all devices and their power operations modes. Best tool I know to acquire this is use powertop(by intel) tool.

* **Powertop** - Powertop is a monitoring tool designed by intel to list all devices thats have power operation modes, list, and tweak them. We can install powertop with the command line bolow:
```
sudo apt install powertop
```

First step using powertop is to run the calibration the readings on battery power with this command:
``` 
sudo powertop -c
```

It will take about 10 minutes to run the callibration. The system will turn the display on and off some times, and is not possible to do anything else during the process.
Powertop can be used by itself to see what is using resources on your system. It needs to be left open for a little amount of time to gather statistics, and beaccurate.
```
powertop
```
We can also generate reports with powertop
```
sudo powertop --html=report.html
```
It's usefull see what running processes or applications are taking more power. You can uninstall them or change some settings to reduce power usage.

![Powertop](images/report.png)

We can also tune the devices to save power mode. In report, there is a tab **Tuning**. You will find many suggestions to increase battery life.

![Powertop](images/report_tuning.png)


In image below, in section **Tunables**, we can see all devices thats have power modes available for tweak. The **Bad** label show us what devices are operating in performance mode. We need to setup most of them to **Good** label, to acquire power save mode.

![Powertop](images/powertop.png)

Some devices like usb controllers for mouses don't need to be put in save power mode, or we can experience some behaviors like mouse stops to work for some time, because the device stoped to save power for a moment. We can enable or disable power save mode just hiting space key.

We can enable all suggested tunings running the command line:
```
sudo powertop --auto-tune
``` 

2. **Overheating, CPU/GPU monitoring, Fine Tuning** - Biggest vilains of pc overheating are cpus and gpus. If thei are working in maximum performance every time, thei will get at the threshold temperature faster, and the performance issues will begin happen. Overheating also leads to hardware damage, and its very important that our devices are operating in proper temperature.

* **Sensors tool** - We can use sensors tool to monitore our cpus temperature.
To install sensors:

```
sudo apt install lm-sensors
```
```
$ sensors
coretemp-isa-0000
Adapter: ISA adapter
Package id 0:  +47.0°C  (high = +100.0°C, crit = +100.0°C)
Core 0:        +45.0°C  (high = +100.0°C, crit = +100.0°C)
Core 1:        +46.0°C  (high = +100.0°C, crit = +100.0°C)
Core 2:        +47.0°C  (high = +100.0°C, crit = +100.0°C)
Core 3:        +44.0°C  (high = +100.0°C, crit = +100.0°C)
Core 4:        +45.0°C  (high = +100.0°C, crit = +100.0°C)
Core 5:        +44.0°C  (high = +100.0°C, crit = +100.0°C)

BAT0-acpi-0
Adapter: ACPI interface
in0:          17.28 V  
curr1:       1000.00 uA 

pch_cannonlake-virtual-0
Adapter: Virtual device
temp1:        +44.0°C  

acpitz-acpi-0
Adapter: ACPI interface
temp1:        +25.0°C  (crit = +107.0°C)
```

Package Id 0 is a mean for the cores cpu temperature.

Psensors is a tool that works using sensors. Its a gui with charts:

![Psensors](images/psensors.png)

**GPU/CPU temperatures higher than 85c leads to hardware damage**, and for years I searched for a way to reduce gpu work, but without success because the lack of documentation. Our eyes don't notice frame transitions at a speed higher than 24fps. Until 60fps, we can perceive a smooth change of quality in vídeo. Above 60fps there are no perception in quality overall. **For Nvidia users, we can config the card to use on demand mode, and setup Xorg to sync with monitor(60hz)**.

![Nvidia](images/nvidia_on_demand.png)

* **Nvidia Profile Configurarion for 60fps**

![Nvidia](images/nvidia_profile.png)

* **Nvidia Rule Configuration for 60ps** - Not Xorg, but all opengl/vulkan applications can run at 60fps using this rules

![Nvidia](images/nvidia_rules.png)

* **MangoHud and Games** - As 60fps cap for games and Xorg may be a good start, some games will require more tweak. 60fps may be a lot of work for some games that aren't very well optimized, and, for Linux/Unix systems, I couldn't find a Nvidia key instruction to cap fps lower than 60. [MangoHud](https://github.com/flightlessmango/MangoHud) is a software that will helpus to do this fine tuning.

For ubuntu base distros, we can use MangoHud ppa
```
sudo add-apt-repository ppa:flexiondotorg/mangohud
sudo apt update
sudo apt install mangohud
```

We can cap fps using the command line
```
MANGOHUD_CONFIG=fps,fps_limit=30 mangohud --dlsym glxgears
```

![Glxgears](images/glxgears.png)