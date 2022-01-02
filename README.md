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