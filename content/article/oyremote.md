---
title: OyRemote
author: michael
layout: page
date: 2015-03-28

---

![text](/images/2015/03/oyremote0171.png)

Please note that there will be no more development on this application. I don't even own the receiver any more. It's still available as is though, for those who'd like to give it a go. Also, since people were asking for the source code, I've made what I could still find available on GitHub (it's version 0.11 only unfortunately): [https://github.com/flipswitchingmonkey/oyremote](https://github.com/flipswitchingmonkey/oyremote)

**OyRemote** is a small application written in C# that connects to your (network enabled) Onkyo AV Receiver and lets you control virtually every aspect of it. As a matter of fact, you can control more than you would normally be able to with your remote. For example in certain modes the receiver may not let you set up a sleep mode with the remote – while through the network protocol you can!

OyRemote came to life after I recently purchased an Onkyo TX-NR1007, which offers both RS-232 and Network control through the ISCP protocol. As I had no RS-232 connection available at the moment, I decided to play around with the network controls (which are using basically the same protocol as the serial one). After initially being more of a proof of concept, the project has now grown to include most of the available functionality that the protocol offers.

OyRemote has grown quite a bit from where I started (a tiny program to change the volume). While it originally was more of a proof of concept, it has become a more feature rich application to control many aspects of your Onkyo Receiver through the network.  
It was developed for Windows in C#, however, even though it is neither officially tested or supported, OyRemote appears to work fine on OSX as well through Mono, and I assume the same is true for Linux.

## Features

- custom buttons (now with colour)
- batch commands
- autorun batch commands at start
- all configuration stored in a human readable XML file
- a Command Builder to build complex command strings from a selection of hundreds of commands (almost the entire ISCP command set)
- (new with 0.17a) remote control via URL
- *Note:* If you have been using a version before version 0.13a and the new one crashes at start, delete the keys at HKEY_CURRENT_USER\Software\OyRemote

## Download

*(requires .NET 3.5)*

[OyRemote 0.17a](/files/software/oyremote/oyremote.0.17a.exe) (14/11/2010)  
[OyRemote 0.16e](/files/software/oyremote/oyremote.0.16e.exe) (17/03/2010)  
[OyRemoteCl 0.2 (command line version)](/files/software/oyremote/oyremotecl.0.2.exe) (14/02/2010)


**Warning:** keep in mind that you are sending low level commands directly to your receiver – I will take NO responsibility for what happens to it! (nor does Onkyo, for that matter)

This application is absolutely free for personal use! 

## Changelog

14/11/2010 – version 0.17a

- add a tiny web server listening on port 60127 by default (in preparation for future Windows Phone 7 interface)
- added configuration options for listening port

17/03/2010 – version 0.16e
  
- small update regarding last.fm integration – added an option to turn last.fm off (for those without internet connection) and made the last.fm request its own thread, so the application does not pause anymore during requests (which was annoying to say the least)

24/02/2010 – version 0.16d
  
- this is a beta release before 0.17 to test the new last.fm features
- added a last.fm tab that display artist information and the album cover of the currently played song
- added mini cover in trackinfo
- added setting to hide trackinfo and statusbar (change it for the current session in the “view” menu, or save the setting in your Settings tab)
- Note: the last.fm info that is returned from the receiver is somewhat unreliable and often incomplete, so you may need to refresh the info with the refresh button in the trackinfo – this can cause the application to seemingly hang while it’s grabbing the last.fm data.

21/02/2010 – version 0.16b

- Live Log did not open – fixed
- batch command Edit button caused exception – caught and handled

21/02/2010 – version 0.16a

- moved title/track etc. info at the bottom of the window to be shown all the time (disadvantage: app requires a 1024×768 or higher resolution now)
- fixed preset selector when Internet Radio is chosen as input

21/02/2010 – version 0.16

- reworked most of the UI
- added a third row of custom buttons (a total of 54 now)
- fixed some minor bugs
- custom button names are auto-set now if “-/-” or blank
- added a colour parameter to the custom buttons – set it in the Command Builder dialog
- removed some buttons from the UI – those functions are in the menus

14/02/2010 – version 0.15

- PAUSE (in ms) command added
- Added Title/Artist/Album info to network controls
- Some minor layout changes to fit new labels

07/02/2010 – version 0.14b – Custom button name is only automatically set if name is still -/- (the default blank name).

07/02/2010 – version 0.14a – Fixed small bug where the setting of input device ids did not work. Also, when you add a command to your selection in the command builder, it sets the name automatically from -/- to the name of the command.

06/02/2010 – version 0.14 – Decided to get rid of the Listening Mode selectors and go for more (and larger) custom buttons. Also – and this is kind of a major change, so expect potential bugs – I went away from storing the config data in the registry and instead use an XML file now. As an added extra, in that same XML file you can now change the name and ID of the input devices.

03/02/2010 – version 0.13a – Autoconnect setting was not saved correctly.

03/02/2010 – version 0.13 – Loads of news. Menus added. “Amplifier IP” link now works. Added a Command Builder and Custom Buttons to go with it. Commands can now be built out of a command list of roughly 500 commands and then stored either as a batch or button. Added center level control. Note that both the center and subwoofer level are temporary only.

29/01/2010 – version 0.11 – Because it would have been way too easy if things would have just worked… fixed the Listening Mode preset buttons I hope. Added some new stuff under the hood and also added a small text box next to the cursor keys. Click in it and it will capture the up/down/left/right/return/backspace keyboard keys and send them as remote cursor keys. Much easier to navigate the setup menu that way. Theres also a live stream window that lets you see the incomign and outgoing traffic on the connection to the receiver.

28/01/2010 – version 0.10 – Some substantial changes this time. See feature list. Rewrote most of the net and UI code. Made the interface more configurable and also squashed a number of bugs.

25/01/2010 – version 0.06 – Quite a few changes in this one. Reworked the volume control, it’s now a slider and it only sets the volume when it’s set via the keyboard or when the mousebutton is lifted, thus making for much smoother operation. Added cursor controls (up/down/left/right). Added more inputs. Haven’t gotten around to add more Listening Modes – but soon. Batch commands can now be checked for Autorun, meaning they are run whenever the application connects to the receiver.

25/01/2010 – version 0.05 – Added some Listening Modes (not complete yet), fixed (I hope) the return channel from the receiver (there was a problem where basically none were evaluated), made sure the return channel is flushed after volume change (slows down volume control once again a bit, but can’t be helped for now). Four batch commands now.

24/01/2010 – version 0.04 – Found another tiny bug, volume control should now be quicker. Also added a Thread.Sleep delay to batch commands, which can be configured in milliseconds. Simple way to try it out would be to run “MVL10,MVL15,MVL20,MVL25″ with e.g. 500ms delay and watch the volume go up step by step.

24/01/2010 – version 0.03 – Added About window and some minor fixes to batch commands.

24/01/2010 – version 0.02 – I’ve added the possibilty to run batch commands now. To test it out, there are two batch command presets that also will be stored in the registry. The format is rather simple, just a comma separated list of commands. For example, “SLI24,PRS06″ should select the radio for input and radio preset 6 for the channel.

24/01/2010 – version 0.01 – This is more of a proof of concept than anything. So far you can set the volume, input selection and radio preset. If you select an input your receiver does not support, it should jump pack the last valid one. Both the currently selected input on the receiver side, its volume and radio preset should also be detected at startup (once connected). Connection details (and in the future some other configuration details) will be stored under HKEY_CURRENT_USERSoftwareOyRemoteConnectionDetails. The “custom command” field lets you send Onkyo commands directly, minus the !1 part. So, for the volume this would be “MVLxx” where xx is Hex. The full list of commands are available on the net, I’m not sure whether these are public or not, so until I’m sure I won’t publish them myself. Future versions are planned to include the full feature set of functions as well as presets and maybe even batch commands. But that’s in the far future…
