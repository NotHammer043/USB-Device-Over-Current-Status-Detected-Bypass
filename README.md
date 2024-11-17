<h2>Bypassing <code>USB Device Over Current Status Detected</code> on ASUS Prime A320M Motherboard because I'm too broke to buy a new AM4 Motherboard 💀</h2>

<p><strong>⚠️ DISCLAIMER: Only attempt this if you know what you are doing! You should not be ignoring the Over Current warning. I will not be responsible for any damage caused. Proceed at your own risk. ⚠️</strong></p>

<p><img src="https://github.com/user-attachments/assets/a87112d4-5f1c-4ebe-a00c-9402e60d15f5" alt="Screenshot"></p>

<p><strong>Required Tools:</strong></p>
<ul>
    <li>UEFITool 0.26.0</li>
    <li>AMIBCP 5.02.0034</li>
    <li>IDA Professional 9.0</li>
    <li>CH341a Programmer + Neo Programmer V2.2.0.10</li>
</ul>

<h2>Step 1: Obtain the Firmware</h2>
<ul>
    <li>You can get the capsule file from ASUS's Website.</li>
</ul>
<p><img src="https://github.com/user-attachments/assets/f09d91ea-d121-40a9-aae0-8acc8cecf23f" alt="Firmware Screenshot"></p>
<p><em>Alternatively, you can dump your BIOS using your programmer.</em></p>

<h2>Step 2: Extracting <code>ASUSPostMessage</code> Module</h2>
<ul>
    <li>Open the firmware in UEFITool.</li>
    <li>Use the search function (<code>CTRL + F</code>), set it to text, and look for <code>USB Device</code>.</li>
    <li>Select the last occurrence and extract the body (in this case, at offset 63D0h).</li>
</ul>
<p><img src="https://github.com/user-attachments/assets/e3ec1ebb-9e10-41e9-be47-86e56c3c6272" alt="UEFITool Screenshot"></p>

<h2>Step 3: Bypassing</h2>
<ul>
    <li>Open the extracted <code>ASUSPostMessage</code> file in IDA.</li>
    <li>Navigate to Text View.</li>
    <li>Scroll to the top and search (<code>CTRL + F</code>) for <code>USB Device</code> until you find the graph view for <code>USB Device Over Current Status Detected</code>.</li>
    <li>Locate the parent block connected to <code>loc_1EEE</code> (marked by a green line from the image).</li>
</ul>
<p><img src="https://github.com/user-attachments/assets/b5d282f3-cc67-4509-b6ed-bfc641ead499" alt="IDA Screenshot"></p>
<ul>
    <li>Highlight the <code>jz</code> instruction and go to <code>Edit &gt; Patch Program &gt; Assemble</code>.</li>
    <li>Replace the <code>jz xxx_xxxx</code> instruction with <code>nop</code>.</li>
</ul>
<p><strong>The edited section should now look like this:</strong></p>
<p><img src="https://github.com/user-attachments/assets/b6415cc1-a828-4730-9cfe-5c6f43eb4861" alt="Patched IDA Screenshot"></p>
<ul>
    <li>Save the changes by selecting <code>Edit &gt; Patch Program &gt; Apply patches to input file</code>.</li>
</ul>

<h2>Step 4: Building</h2>
<ul>
    <li>Return to UEFITool and replace the original <code>ASUSPostMessage</code> with the modified version from IDA.</li>
</ul>
<p><img src="https://github.com/user-attachments/assets/83ca8d81-d635-47ea-baac-4015393849f2" alt="UEFITool Replace Screenshot"></p>
<ul>
    <li>Save the modified firmware file.</li>
</ul>

<h2>Step 5: Disabling <code>Wait For 'F1' If Error</code></h2>
<ul>
    <li>This step is essential if you are getting the <code>Press F1 to enter SETUP</code> prompt.</li>
    <li>Note: This can also be done directly in the BIOS, so <code>AMIBCP</code> may not be necessary.</li>
</ul>
<hr>
<ul>
    <li>Load the modified firmware file into <code>AMIBCP</code>.</li>
    <li>Navigate to: <code>1st Folder &gt; Setup &gt; Boot &gt; Boot Configuration</code>.</li>
    <li>Set <code>Wait For 'F1' If Error</code> to <code>Disabled</code> for both <code>Failsafe</code> and <code>Optimal</code>.</li>
    <li>Save the file.</li>
</ul>
<p><img src="https://github.com/user-attachments/assets/fd3dd66d-0f28-47ef-8e51-1421f503fc98" alt="AMIBCP Screenshot"></p>

<h1>Flash the BIOS and Done!</h1>
<p><strong>OverCurrent is now bypassed!</strong><br>
Just hope it dont go <span>🔥💀</span></p>
