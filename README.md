# Features
* BambuVideoSteam automatically adds the required Sources (text labels and dynamic sensor values) to OBS via WebSockets. This is a double-edged sword. It requires you have a scene called BambuStream with no Sources configured inside it. Only then can you run this script and generate the Sources.
* BambuVideoSteam automatically stops your OBS broadcast as soon as the print ends. I learned this through playing with it and realizing I no longer had to stop my broadcasts. Some might find this annoying, I actually thought it was kind of neat, but ideally this is something that would be baked into an options menu in future versions.

# Prerequisites
If you are missing any of the following pieces of software, none of this will work:
 * [Orca Slicer](https://github.com/SoftFever/OrcaSlicer/releases) or [Bambu Studio](https://bambulab.com/en/download/studio) (In the below instructions, `pathToSDP` changes depending on which of these you choose)
 * [Open Broadcaster Software | OBS](https://obsproject.com/download)
 * [Visual Studio Code (VSCode)](https://code.visualstudio.com/download)
 * [.NET 6.x SDK](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)

# Configure:
1. In OBS, create a Scene called `BambuStream`. You don't need to populate it with Sources. This will happen when you run this application, as the application sends all data including Source values/icons via WebSockets (built into OBS and configured below).
1. Download this repository (extract the contents if you downloaded a .zip of the repository)
1. Open VSCode, click <kbd>File</kbd> > <kbd>Open Folder</kbd>.
1. Select the folder called `BambuVideoStream-master` (the one that contains the `.vscode`, BambuVideoStream, and Images folders within it)
1. Click <kbd>Select Folder</kbd>.
1. In VSCode, you'll see the contents of this repository. Find the file called `appSettings.Development.json`. Inside, set the following values:

| Variable | Value |
| -------- | ------------- |
| ipAddress  | Set this to the `IP` of your Bambu X1C 3D Printer. You can get this by tapping the hex nut (⬢) icon from the front panel on your printer. |
| password   | Set this to the `Access Code` displayed on your Bambu X1C 3D Printer. You can get this by tapping the hex nut (⬢) icon from the front panel on your printer. This is also the same access code used to connect from the slicer software of your choosing. |
| serial     | Set this to the `Device Info` value on your Bambu X1C 3D Printer. |
| pathToSDP | There are two things to check here. First, ensure you replace `XXXXX` with your Windows username. Second, ensure it points to your slicer's libraries. For OrcaSlicer this is different. (e.g. `C:/Users/<yourusername>/AppData/Roaming/OrcaSlicer/cameratools/ffmpeg.sdp` Again, depending on the slicer software you use, this will be different. Note I change the default `BambuStudio` to `OrcaSlicer` in the path. If you're like me and never installed BambuStudio, you will need to do this, otherwise this tool will print to a non-existent ffmpeg.sdp. |
| ObsWsConnection | You don't need to change anything here, but be warned, if you don't enable this in OBS under <kbd>Tools</kbd> > <kbd>WebSocket Server Settings</kbd>, it won't work. It currently only works on your local network and without password authentication. |

# Bugs
1. You have to manually delete and recreate the BambuStream scene in OBS every time you run the application as it doesn't gracefully handle the presence of an existing one with Source values inside it. If someone wants to take a crack at that it would make this a lot more convenient to use.
2. Images/preview.png is using a placeholder until the application is updated to pull an actual preview image of the print.

# Run
1. In VSCode hold <kbd>Ctrl</kbd> + <kbd>F5</kbd> to run it. Pay attention to the terminal and output in VSCode to identify any errors that may come up.


# OLD README CONTENT
# BambuVideoStream
.Net app to push MQTT sensor data from Bambu Lab 3D printer to OBS for video streaming.


You will need OBS Studio with all the InputText sources defined. 

Instructions for streaming the Bambu webcam with OBS are here: https://wiki.bambulab.com/en/software/bambu-studio/virtual-camera.

Those will get updated when a message is received from MQTT. 

I'm sure there is a better way to do this, but it works for now.

The utility is a .Net app and connects to both MQTT running on the X1 and OBS Studio's websocket connection. 

When a message is received from MQTT, it is parsed and the text input values are updated via the websocket connection to OBS.

Here is a sample recorded stream: https://www.youtube.com/watch?v=MW3osyXAUTI

You will need your printer's local IP address, password and serial number to connect to MQTT. The password is the access code found on the LCD in the WiFi settings.

Much of this work is derived from this thread: https://community.home-assistant.io/t/bambu-lab-x1-x1c-mqtt/489510.
