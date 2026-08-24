
Will be trying to get this up on a GitHub or something and not this PDF lol– just need to figure it out.

A Guide of some things that I have learned when using CachyOS. I'll be putting together some more things asap like:
- Installing Davinci Resolve
- Windows Hello
- Adding Refresh Rate Scripts for Laptop Power Modes
- Fixing the Brightness in this Hybrid Intel x Nvidia GPU Setup
- Modding Risk of Rain 2 (using R2 Modman)
- and generally adding more pictures and such of some of the steps

So check back on this guide as I can get to updating it! Feel free to use this info, but keep in mind this was on:
- CachyOS
- Kernal: ~7.2.0-1-cachyos (64bit)
- KDE Plasma: ~6.7.4
- The Laptop I have: https://rog.asus.com/us/laptops/rog-zephyrus/rog-zephyrus-g16-2025-gu605/ (285H, 32GB RAM, RTX 5070 Ti (mobile), 1TB)


___
<div style="page-break-after: always;"></div>

# To allow more display refresh rates and configurations you can run these commands
#display #linux #cachy 

```
kscreen-doctor output.<OUTPUT>.addCustomMode.2560.1600.120000.full
```
- Obviously replace 2560 as horizontal resolution, 1600 as vertical and 120000 as the refresh rate (in millihertz)
	- "full" should pretty much remain the same
- you get the ``<OUTPUT>`` by entering this command: 
```
	kscreen-doctor --outputs
```
- This will provide a result that looks like this:
![[KDE showing the display options 1.png]]
- As you can see at the top it shows the display output. In this case the number would be "1" and that is all you have to do. Replace ``<OUTPUT>`` with 1 or whatever display number the target is



___
<div style="page-break-after: always;"></div>

# Launching Firebot in X11 Mode
- Super useful to bypass the requirements for approving hotkeys, then them not working anyways. There are also probably other reasons for doing this

- You need to right click Firebot in the app launch and edit it
- In this field replace what was there (%U) and add this command:
	``Exec=firebot --ozone-platform=x11 %U``
- Should look like this:
	![[Pasted image 20260820025942.png|397]]



___
<div style="page-break-after: always;"></div>

# Steam Launch Options! (esp. Proton options)
- Very Important for gaming on Linux! A lot of these options do not have interface options like they do in Windows 
	- I.E. this is a command that will give the DLSS debug menu (indicator), upgrade DLSS to the latest version, pick what letter preset, and display mangoHUD for stats
```
		PROTON_DLSS_INDICATOR=1 PROTON_DLSS_UPGRADE=1 DXVK_NVAPI_DRS_SETTINGS="NGX_DLSS_SR_OVERRIDE_RENDER_PRESET_SELECTION=RENDER_PRESET_L" mangohud %command%
```
- Granted the upscaler stuff only really works with the CachyOS Proton, ProtonGE, or sometimes the Proton Experiemental at the moment
- ***How it's formatted***
	- You separate them with spaces
	- The `=1` is usually binary for enabling or `=0` for disabling
	- EVERYTHING with these modifiers end with the end with the `%command%`
		- This signifies it takes place before running the game

| ``PROTON_DLSS_INDICATOR=1``                                                                                                   | The DLSS debug. Very similar to the registry teak you can use on Windows                                                                      |
| ----------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| ``PROTON_DLSS_UPGRADE=1``                                                                                                     | Upgrade to latest DLSS version fetched by the Proton                                                                                          |
| <small>``DXVK_NVAPI_DRS_SETTINGS=``<br>followed by ``"NGX_DLSS_SR_OVERRIDE_RENDER_PRESET_SELECTION=RENDER_PRESET_L"``</small> | Picks the letter preset. The last letter is all you need to change (make sure the version of DLSS you are running supports the letter preset) |
| ``mangohud``                                                                                                                  | Enables the FPS overlay configurable by Goverlay. Doesn't need the `=1`                                                                       |

- The specific setup I used when testing was:
	`PROTON_DLSS_INDICATOR=1 mangohud %command%`

There are a HUGE list of commands the ProtonGE GitHUB:
	https://github.com/gloriouseggroll/proton-ge-custom#options

This is another useful list that may apply (similar to the ProtonGE stuff)
	https://github.com/BananaWorks07/Proton/blob/em-10/docs/EM-ADDITIONS.md
	- mostly the stuff for Wayland specifically


- **We have also had some luck with some AMD stuff:**

![[Pasted image 20260820033558.png]]
	``PROTON_FSR4_RDNA3_UPGRADE=1 PROTON_FSR4_INDICATOR=1 %command%`` -- this was using ProtonGE. CachyOS-Proton seems to upgrade automatically

| ``PROTON_FSR4_RDNA3_UPGRADE=1`` | Upgrades to FSR 4 on RNDA 3 (diff than RDNA 4)                                         |
| ------------------------------- | -------------------------------------------------------------------------------------- |
| ``PROTON_FSR4_UPGRADE=1``       | Upgrade to FSR 4 on RDNA 4                                                             |
| ``PROTON_FSR4_INDICATOR=1``     | Gives the little overlay in the top left. Only seems to run if FSR 4 is being upgraded |


___
<div style="page-break-after: always;"></div>

# Fixing the Download  Speeds in Steam

- At least in my experience on btrfs, KDE, with Limine. I get VERY reduced download speeds on Linux. 
	- Same experience on SteamOS (did try this though)
		- It would never go over the 1Gbps even though the adaptors are capable of 2.5Gbps+

- `chattr +C ~/.steam/steam/steamapps`
	- This command removes the "Copy on Write" for the Steam directory
	- Which means it takes away the ability to have snapshots here. I don't think it's *too* big of a deal for a Steam Library, but up to you



___
<div style="page-break-after: always;"></div>

# Using Bottles to Run Overwatch (WIP)
- *This allows you to run a Windows launcher on Linux*

- Use the Bottles Flatpak install (actually important)
	- And give it some extra permissions using another app called "Flatseal". Things like access to file directories and USB devices for mics
- Go to the top right and install the CachyOS Proton or ProtonGE
- Then add a bottle in the top left and pick the "Game" Option
	- Going through the process will pull up a bunch of presets, just pick Battle.net
	- Choose the Proton you want
	- Run the install
	- And even it will prompt to add a desktop shortcut
		- It might ask for a command permission and prompt the command that gives the Bottles Flatpak the ability to create shortcuts
		- Enter that command in the terminal
		- This should also be it: ``flatpak override com.usebottles.bottles --user --filesystem=xdg-data/applications``
- You have to toggle the DLSS option
	- This is even required to run Reflex
- Make sure you give time for the app to launch and shutdown
	- If it doesn't shutdown properly it most likely won't save settings in the game and might have to recompile shaders




___
<div style="page-break-after: always;"></div>

# Getting Davinci Resolve Running (WIP)


- In Resolve at least on Cachy, it does not recognize the fonts by default
	- Resolve wants fonts in /usr/share/fonts
	- im not sure where the font manager installs fonts
		- possible that installing fonts with pacman or shelly does put them in the right place
	- you basically have to manually install the fonts if you download them off the internet into that folder (which does need admin permission)
	- Chris Titus has a great vid on this: https://youtu.be/oHsboGBxUuc?si=77_JkFtBdfMuj8zh 



___
<div style="page-break-after: always;"></div>

# Modding ROR2 (WIP)

-  i won in ROR2
	- modding was normal, but i couldnt get the launch command working in steam, so i had to use the mod launcher (not a bit deal)
	- there was a LONG load screen on the entry to the moon which was worrying
	- i hit a fortnite defaulty on mythrix


___
<div style="page-break-after: always;"></div>

# Getting Windows Hello (Howdy) (WIP)
for Howdy
- i edited the `/etc/pam.d/system-auth` and `/etc/pam.d/system-local-login`
- **NEED TO COME BACK TO THIS LATER!**



___
<div style="page-break-after: always;"></div>

# Guide for Fixing Brightness (WIP)
![[Pasted image 20260820210250.png]]

