# Overview

Project intends to construct a basic DBC file and tools compatible with the Bosch eBike drive system. DBC file is being created using SavvyCAN and remains compatible with commercial applications such as CANio/CANdb++.


# How to access the CAN?

**Option 1:** Charge port 

Be careful as 38v is live when the CAN BUS is live. I recommended making a custom adapter although it’s not required.
![image](https://github.com/jewwy/BoschCX-DBC/assets/12469803/1ae0c6c8-6409-4b9f-a821-3a170b5d7003)

**Option 2:** Accessible on the display or around the drive unit / battery. 
Green = CAN H / Yellow = CAN L.

**Option 3:** USB (Under investigation)

USB exists via HID interface using native OS drivers. Communication protocol uses CAN like frames however the messages are independent of the actual CAN BUS. 

# What data is available?

**Data streams:** speed, cadence, torque, system runtime, odometer, range, BMS sensors, and other miscellaneous items such as light control and walk mode. 

**Requestable data:** It’s possible to request additional data using diagnostic message ID’s. Serial number, software versions, diagnostic info / trouble codes, and likely a lot more. It will take more investigation to see what’s accessible. 

# How to wake up the bike?
**Option 1:** Turn on the display unit

**Option 2:** Send ID 0x06612097 Data 00 00
(Works without the display connected)

Startup sequence.
1. Display sends a wake up packet after hitting the power button. This uses the internal display battery. 
2. BMS starts initialization, 38v becomes live, and the drive unit starts outputting 12v (for the display).
3. Handshake is completed within a couple seconds. 
4. The drive unit starts sending keep alive packets to the BMS (0x09B).
5. System runs until it hits an inactivity timer, stops receiving keep alive packets, or until it receives a sleep command. 

# Further Study

The Bosch Diagnostic application is available for public download however it requires a provisioned UniToken Pro dongle to authorize the user level (IBD, OEM, or Bosch). It’s basically just a set of jar files that’s easily readable by Procyon. Someone who understands Java and ODX better than me can presumably research this to re-establish some of the diagnostic methods.

https://bosch-ebike-updates.com/data/DiagnosisSoftware/Update/Setup.exe

A better approach would be to avoid the abstraction and focus on reverse engineering the ECU and BMS firmware via JTAG. 

# Risks

Use this data at your own risk! It's unknown what will happen if you spoof the speed reading or try to force race mode on a non-supported motor for example. The ECU and BMS do not contain any "fuses" to permanently set the bike in tamper mode. That's done via software registers however methods to control the registers have not been determined short of having a full CFF3 from Bosch.



