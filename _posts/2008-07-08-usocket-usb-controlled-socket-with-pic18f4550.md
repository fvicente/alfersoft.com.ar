---
id: 9
title: 'USocket &#8211; USB controlled Socket with PIC18F4550'
date: 2008-07-08T01:06:25+00:00
author: fvicente
layout: post
guid: http://www.alfersoft.com.ar/blog/?p=9
permalink: /2008/07/08/usocket-usb-controlled-socket-with-pic18f4550/
categories:
  - Electronic Projects
tags:
  - CDC
  - circuit
  - electronic
  - firmware
  - PIC
  - PIC16F4550
  - project
  - socket
  - USB
comments: true
---
## Introduction

The idea of this project is to control (switch off/on) two power sockets with a computer by using its USB port. I&#8217;ve chosen USB in first place because I wanted to experiment with the PIC18F4550 microchip&#8217;s microcontroller, and secondly because the power supplied by this port (500mA) is enough to activate a relay without any additional power supply.

<img src="http://www.alfersoft.com.ar/files/usocket/pictures/proto04.jpg" alt="Prototype" width="450" height="338" />

<!--more-->

The firmware is based in [SIXCA USBDAQ](http://www.sixca.com/eng/articles/usbdaq/ "SIXCA USBDAQ") which is in turn based on [microchip&#8217;s CDC sample](http://www.microchip.com/stellent/idcplg?IdcService=SS_GET_PAGE&nodeId=2124&param=en532204&page=wwwFullSpeedUSB "Microchip's CDC sample"). USBDAQ is extremely easy to use. It implements a very simple set of ASCII commands to turn on/off the digital outputs, that I use to control the two relays. I just needed to adjust the bMaxPower in order to negotiate the 500mA with the host, and I also changed the vendor ID and the name of the device to USocket.

I&#8217;ve also made the schematic and board design of the circuit with [Eagle](http://www.cadsoftusa.com/ "CadSoft Eagle"), but I never used it since I made it on a strip board :D, so if you plan to use it please double check everything before.

The whole project including PIC firmware and Eagle files is available for download. You are free to use or modify it as you like, but please verify Microchip&#8217;s CDC sample license if you plan to use it for commercial applications.

<span class="BODYTEXT"><span class="BODYTEXT"><strong>Disclaimer:</strong> This project involves high voltage electricity. You should not attempt this project unless you are comfortable with basic concepts of AC and DC electricity, induction, and reading circuit schematics. You and your adult supervisor are responsible for your safety when doing this project!</span></span>

**Advice:** Despites the high voltage part of the circuit is virtually isolated from the controller circuit by relays, these devices always represents a risk of mechanical failure and they can possibly damage your equipment or even cause physical injuries, so we (the author or the webmasters) are not responsible of any lose or damage.

## The Circuit

<p style="text-align: center;">
  <a title="Schematic" href="http://www.alfersoft.com.ar/files/usocket/pictures/full.png" target="_blank"><img class="aligncenter" style="border: 1px solid black;" src="http://www.alfersoft.com.ar/files/usocket/pictures/full.png" alt="Figure 1 - Schematic" width="450" height="393" /></a>
</p>

<p style="text-align: center;">
  Figure 1 &#8211; Schematic
</p>

Pretty much the same as USBDAQ but two (identical) sub-circuits were added to control the relays. A Darlington-transistor is used to protect the hardware. Once again, the idea was obtained from the web (thanks to google) and [here is the link to the original article](http://b-l-w.de/serialrelay_en.php "Relay control").

The PCB is divided in 3 little boards, for commodity and socket case space issues. (I&#8217;ve used only 2 strip boards in the actual prototype).

<p style="text-align: center;">
  <a title="Board" href="http://www.alfersoft.com.ar/files/usocket/pictures/full_board.png" target="_blank"><img class="aligncenter" style="border: 1px solid black;" src="http://www.alfersoft.com.ar/files/usocket/pictures/full_board.png" alt="Figure 2 - Board" width="450" height="271" /></a>
</p>

<p style="text-align: center;">
  Figure 2 &#8211; Board
</p>

List of materials:

  * IC1 &#8211; PIC16F4550 Microchip&#8217;s micro-controller ([datasheet](http://www.alfersoft.com.ar/files/usocket/datasheets/pic18f4550-microchip.pdf "PIC18F4550 by Microchip"))
  * Q2 &#8211; Crystal 20Mhz
  * R1 &#8211; Resistor 4.7K
  * R2 &#8211; Resistor 1M
  * R3, R5 &#8211; Resistors 150
  * R4, R6 &#8211; Resistors 100K
  * K1, K3 &#8211; 5v Relays. I&#8217;ve used FBR211 by Fujitsu ([datasheet](http://www.alfersoft.com.ar/files/usocket/datasheets/fbr211-relay-fujitsu.pdf "FBR211 Relay by Fujitsu"))
  * D1, D2, D3. D4 &#8211; Diodes 1N4004
  * Q1, Q3 &#8211; BC517 Darlington-transistors ([datasheet](http://www.alfersoft.com.ar/files/usocket/datasheets/bc517.pdf "BC517 Darlington Transistor"))
  * LED1, LED2 &#8211; Regular leds
  * C1, C2 &#8211; Capacitors 22pF
  * C3 &#8211; Capacitor 470pF
  * X1 &#8211; Mini-USB Connector Type B ([datasheet](http://www.alfersoft.com.ar/files/usocket/datasheets/2411-02-lumberg-usb-connector-type-b.pdf "USB connector type B"))



## PIC Firmware

Here is what I needed to modify from the original <a title="SIXCA USBDAQ" href="http://www.sixca.com/eng/articles/usbdaq/" target="_blank">SIXCA USBDAQ</a>:

I set bMaxPower to 250, which means 500mA. The power provided by hosts and hubs is twice the value of bMaxPower field, but in reality they are likely to allocate either 100 or 500 milliamperes rather than the specified amount.

The file modified is the one that contains the USB descriptor: fw/cdc/autofiles/usbdsc.c

<pre class="brush: cpp; title: ; notranslate" title="">/* Configuration 1 Descriptor */
CFG01=
{
/* Configuration Descriptor */
sizeof(USB_CFG_DSC),    // Size of this descriptor in bytes
DSC_CFG,                // CONFIGURATION descriptor type
sizeof(cfg01),          // Total length of data for this cfg
2,                      // Number of interfaces in this cfg
1,                      // Index value of this configuration
0,                      // Configuration string index
_DEFAULT,               // Attributes, see usbdefs_std_dsc.h
250,                    // Max power consumption (2X mA) 250 = 500mA

...

</pre>

In the same file I modified the Vendor ID, Product ID and corresponding strings

<pre class="brush: cpp; title: ; notranslate" title="">/* Device Descriptor */
rom USB_DEV_DSC device_dsc=
{
sizeof(USB_DEV_DSC),    // Size of this descriptor in bytes
DSC_DEV,                // DEVICE descriptor type
0x0200,                 // USB Spec Release Number in BCD format
CDC_DEVICE,             // Class Code
0x00,                   // Subclass code
0x00,                   // Protocol code
EP0_BUFF_SIZE,          // Max packet size for EP0, see usbcfg.h
0xAF01,                 // Vendor ID
0xAF0A,                 // Product ID: CDC RS-232 Emulation Demo
0x0000,                 // Device release number in BCD format
0x01,                   // Manufacturer string index
0x02,                   // Product string index
0x00,                   // Device serial number string index
0x01                    // Number of possible configurations
};

...

rom struct{byte bLength;byte bDscType;word string[16];}sd001={
sizeof(sd001),DSC_STR,
'a','l','f','e','r','s','o','f','t','.',
'c','o','m','.','a','r'};

rom struct{byte bLength;byte bDscType;word string[21];}sd002={
sizeof(sd002),DSC_STR,
'A','l','f','e','r','S','o','f','t',' ',
'U','S','o','c','k','e','t',' ','1','.','0'};

</pre>

And the file driver/win2k_winxp/mchpcdc.inf  Windows, to match the new vendor, product ID and description

<pre class="brush: cpp; title: ; notranslate" title="">[DeviceList]
%DESCRIPTION%=DriverInstall, USB\VID_AF01&PID_AF0A

...

;------------------------------------------------------------------------------
;  String Definitions
;------------------------------------------------------------------------------

[Strings]
MCHP="alfersoft.com.ar"
MFGNAME="alfersoft.com.ar"
DESCRIPTION="Communications Port"
SERVICE="AlferSoft USocket 1.0"

</pre>

## Programming the PIC

I&#8217;ve used WinPic800 to program the PIC. Here is a screenshot of the parameters I used to program it.

<img style="border: 1px solid black;" src="http://www.alfersoft.com.ar/files/usocket/pictures/usbparams.png" alt="Parameters to program the PIC" width="450" height="304" />

## Commands

I didn&#8217;t modified any of the USBDAQ commands. They are all available but I only need to use these 4 commands:

  * *A01 (activate relay 1)
  * *A00 (deactivate relay 1)
  * *A11 (activate relay 2)
  * *A10 (deactivate relay 2)

All the commands are followed by an enter (chr(13) or &#8216;\n&#8217;).

## Installing (Windows)

  1. Plug the device into the USB port, the following message will appear in the tray bar.
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket01_inst_win.png" alt="Installation Windows step 1" width="440" height="180" />
</p>

  2. In the first page of the &#8220;Found New Hardware Wizard&#8221; select &#8220;No, not this time&#8221; and click Next button.
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket02_inst_win.png" alt="Installation Windows step 2" width="450" height="351" />
</p>

  3. Select the &#8220;Install from a list or specific location (Advanced)&#8221; option and click Next button.
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket03_inst_win.png" alt="Installation Windows step 3" width="450" height="351" />
</p>

  4. Mark the &#8220;Include this location in the search:&#8221; and browse for the directory where the .inf file is located (driver\win2k_winxp)
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket04_inst_win.png" alt="Installation Windows step 4" width="450" height="351" />
</p>

  5. This message will appear:
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket05_inst_win.png" alt="Installation Windows step 5" width="450" height="351" />
</p>

  6. And then this one:
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket06_inst_win.png" alt="Installation Windows step 6" width="450" height="351" />
</p>

  7. When the installation is done press &#8220;Finish&#8221;
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket07_inst_win.png" alt="Installation Windows step 7" width="450" height="351" />
</p>

  8. After that the hardware is ready to use.
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket08_inst_win.png" alt="Installation Windows step 8" width="440" height="180" />
</p>

## Testing (Windows)

  1. Go to the device manager and locate the new COM port added, in my example COM10 was added
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket09_test_win.png" alt="Installation Windows step 9" width="450" height="317" />
</p>

  2. Open Hyperterminal and choose a name for the new conection
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket10_test_win.png" alt="Installation Windows step 10" width="332" height="299" />
</p>

  3. Select the new COM port
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket11_test_win.png" alt="Installation Windows step 11" width="296" height="300" />
</p>

  4. Set the Bit rate to 115200
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket12_test_win.png" alt="Installation Windows step 12" width="344" height="407" />
</p>

  5. This step is optional to see what we are typing on the screen, choose ASCII Setup
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket13_test_win.png" alt="Installation Windows step 13" width="344" height="423" />
</p>

  6. Then mark &#8220;Echo typed characters locally&#8221;
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket14_test_win.png" alt="Installation Windows step 14" width="263" height="316" />
</p>

  7. Finally connect and type one of the commands
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket15_test_win.png" alt="Installation Windows step 15" width="450" height="302" />
</p>

## Installing (Linux &#8211; Ubuntu)

  1. Plug it. That&#8217;s it! a new device probably called /dev/ttyACM0 will be added by the OS.



## Testing (Linux &#8211; Ubuntu)

  1. Open gtkterm if you don&#8217;t have it installed type &#8220;sudo apt-get install gtkterm&#8221; from a terminal
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket01_linux.png" alt="Testing Linux step 1" width="450" height="294" />
</p>

  2. Go to Configuration -> Port, set the port to /dev/ttyACM0, the speed to 115200 and click OK
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket02_linux.png" alt="Testing Linux step 2" width="450" height="234" />
</p>

  3. Select Configuration -> Local echo
  4. Type the command and press enter
<p style="text-align: center;">
  <img class="aligncenter" src="http://www.alfersoft.com.ar/files/usocket/pictures/screenshots/usocket03_linux.png" alt="Testing Linux step 3" width="450" height="294" />
</p>

<p style="text-align: center;">
  </ol> 
  
  <h2>
    Problem
  </h2>
  
  <p>
    After I mounted everything in the case a problem appeared, if I plug the USB cable to the computer and then the socket to the wall, the PIC hangs. But it works well the other way round (first plug to the wall and the to the computer). That is probably because the high voltage cables are too close to the PIC and unfortunatelly I&#8217;m not an expert in that matter and I don&#8217;t know how to fix it except by putting the circuit in a separated box. Experts needed! If you know another way to fix it please let me know!
  </p>
  
  <h2>
    Pictures
  </h2>
  
  <p>
    <img src="http://www.alfersoft.com.ar/files/usocket/pictures/proto01.jpg" alt="Prototype" width="450" height="338" />
  </p>
  
  <p>
    <img src="http://www.alfersoft.com.ar/files/usocket/pictures/proto05.jpg" alt="Prototype" width="450" height="338" />
  </p>
  
  <p>
    <img src="http://www.alfersoft.com.ar/files/usocket/pictures/board02.jpg" alt="Board" width="450" height="338" />
  </p>
  
  <p>
    <img src="http://www.alfersoft.com.ar/files/usocket/pictures/board04.jpg" alt="Board" width="450" height="338" />
  </p>
  
  <p>
    <img src="http://www.alfersoft.com.ar/files/usocket/pictures/case01.jpg" alt="Case" width="450" height="338" />
  </p>
  
  <p>
    <img src="http://www.alfersoft.com.ar/files/usocket/pictures/case02.jpg" alt="Case" width="450" height="338" />
  </p>
  
  <h2>
    Download
  </h2>
  
  <p>
    <a title="USocket Project" href="http://www.alfersoft.com.ar/files/usocket/usocket.zip">Link to download USocket project</a>, enjoy it!
  </p>
  
  <p>
  </p>
