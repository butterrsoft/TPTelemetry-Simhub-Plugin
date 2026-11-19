# TPTelemetry-Simhub-Plugin
A Simhub plugin that helps import telemetry for driving games that don't have it.

Made with Claude rather than sitting around waiting, sorry!

Works with Simhub v9.9.4 through v9.13.2 (newer versions untested but may work). To install you need the dotnet 10 SDK and possibly USBPcap too: https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/sdk-10.0.402-windows-x64-installer, and https://desowin.org/usbpcap/

Download the latest release, unpack, and follow the [setup tutorial](https://youtu.be/PGtzBJOsC7Q ) or the readme.md document in the TPTelemetry archive. 

The list of titles this plugin currently works with is:
### TP
- BG4JP - BG4 Tuned
- Grid
- GTI Club Supermini Festa
- Initial D 6
- Inital D Zero v2.3
- MKDX 118
- MKDX USA
- Outrun 2 SP SDX
- Sega Rally 3
- SWDC
- WMMT3DX+
- WMMT5DX+

### Flycast (v2.7-1-g44e4c7b50 from 19/8/26)
- Initial D 3	

### Supermodel (v0.4-20261116-git-0b82a98)
- Daytona 2 Power Edition
- SCUD Race

### M2emu (v1.1c)
- Daytona USA
- Sega Rally
- Sega Touring Car Championship

## Notes
Emulators and launchers change as they update, and this may break the plugin. If you get games you want running, save a working copy of your TP folder somewhere!

Some versions on Simhub had problems importing an effects profile. Try renaming any TPTelemetry .siprofile to something completely different before importing it, if so.

Without USBPcap, Outrun2 and BG4JP do not get some extra effects (and only Logitech G25 and G27 wheels are confirmed working with those particular effects in those games so far, other wheels are untested so let me know.) You need to go into the capture section of the plugin, tick the boxes and set your wheel - see the pic.

<img width="842" height="370" alt="Capture" src="https://github.com/user-attachments/assets/16409e75-224b-420d-adf7-8f07e5f80c98" />


## Adding Games
You can add custom games using the plugin, but it does require work. I will expand this section with a how-to or how-to-help soon!

## Thanks
Thanks to the TP team, Flyinghead, The Supermodel Team, Elsemi, SailorSat, MasterRaver, Boomslang, MightyMike, and all the other excellent devs out there without whose work this project would be pointless. Please consider supporting them.
