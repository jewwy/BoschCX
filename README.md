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

# ODX Files

ODX exports included. These are essentially raw XML translations of the deserialised OdxDocument objects. Some conversion is required. For example, bPdu was encoded in base64 during export. 

# Risks

Use this data at your own risk! It's unknown what will happen if you spoof the speed reading or try to force race mode on a non-supported motor for example. The ECU and BMS do not contain any "fuses" to permanently set the bike in tamper mode. That's done via software registers however methods to control the registers have not been determined short of having a full CFF3 from Bosch.


