
## Bypassing `USB Device Over Current Status Detected` on ASUS Prime A320M Motherboard because im too broke to buy a new AM4 Motherboard 💀

**⚠️ DISCLAIMER: Only attempt this if you know what you are doing!. You should not be ignoring the Over Current warning. I will not be responsible for any damage caused. Proceed at your own risk. ⚠️**

![Screenshot 2024-11-18 00-28-08](https://github.com/user-attachments/assets/a87112d4-5f1c-4ebe-a00c-9402e60d15f5)

**Required Tools:**
- UEFITool 0.26.0
- AMIBCP 5.02.0034
- IDA Professional 9.0
- CH341a Programmer + Neo Programmer V2.2.0.10

## Step 1: Obtain the Firmware
- You can get the capsule file from ASUS's Website
![image](https://github.com/user-attachments/assets/f09d91ea-d121-40a9-aae0-8acc8cecf23f)
- *Alternatively, you can dump your BIOS using your programmer.*

## Step 2: Extracting `ASUSPostMessage` Module
- Open the firmware in UEFITool.
- Use the search function *(CTRL + F)*, set it to text, and look for `USB Device`.
- Select the last occurrence and extract the body (in this case, at offset 63D0h).
![image](https://github.com/user-attachments/assets/e3ec1ebb-9e10-41e9-be47-86e56c3c6272)

## Step 3: Bypassing
- Open the extracted `ASUSPostMessage` file in IDA.
- Navigate to Text View.
- Scroll to the top and search *(CTRL + F)* for `USB Device` until you find the graph view for `USB Device Over Current Status Detected`.
- Locate the parent block connected to `loc_1EEE` (marked by green line from image).
![image](https://github.com/user-attachments/assets/b5d282f3-cc67-4509-b6ed-bfc641ead499)
- Highlight the `jz` instruction and go to `Edit > Patch Program > Assemble`.
- Replace the `jz xxx_xxxx` instruction with `nop`.
**The edited section should now look like this:**
![image](https://github.com/user-attachments/assets/b6415cc1-a828-4730-9cfe-5c6f43eb4861)
- Save the changes by selecting `Edit > Patch Program > Apply patches to input file`.

## Step 4: Building
- Return to UEFITool and replace the original `ASUSPostMessage` with the modified version from IDA.
![image](https://github.com/user-attachments/assets/83ca8d81-d635-47ea-baac-4015393849f2)
- Save the modified firmware file.

## Step 5: Disabling `Wait For 'F1' If Error`
- This step is essential if you are getting `Press F1 to enter SETUP` prompt.
- Note: This can also be done directly in the BIOS, so `AMIBCP` may not be necessary.
---
- Load the modified firmware file into `AMIBCP`.
- Navigate to: `1st Folder > Setup > Boot > Boot Configuration`.
- Set `Wait For 'F1' If Error` to `Disabled` for both `Failsafe` and `Optimal`.
- Save the file.
![image](https://github.com/user-attachments/assets/fd3dd66d-0f28-47ef-8e51-1421f503fc98)

# Flash the BIOS and Done!
**OverCurrent is now bypassed!**
Just hope it dont go🔥💀
