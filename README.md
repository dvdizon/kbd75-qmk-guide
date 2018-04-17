# KBD75 R3+ QMK How-to Guide

This guide was originally posted on [reddit](https://www.reddit.com/r/MechanicalKeyboards/comments/6hl837/kbd75_r3_qmk_howto_guide/), but it was archived and I wanted to keep it alive somewhere else just incase it got deleted.

If this is your first time reading this, I've had to add more information because people have gotten good information from the comments of this thread.

**DISCLAIMER**: I am personally not a qmk_firmware expert, other members of the community are more experienced than I am and I have not encountered all the errors or issues. If you are encounting issues I suggest consulting with the provider of your hardware and/or the #kb-help channel on the /r/mechanicalkeyboards discord. 

There two sections:

- Original Post with configuringkd!!?!j?f your KBD75 with QMK
- Some information if you think you have a PS2AVR pcb (R1 and R2) instead

If you have any suggestions or corrects, please add a comment below and I will add/edit this post :)

Updates
-----
- **6/19/17 UPDATE**: I have opened a pull-request with the qmk_firmware master repo, and I have actually successfully used the latest version of QMK Flasher to flash my KBD75. I will update this guide once I have anything substantial to change.
- **6/20/17 UPDATE**: My pull-request for the kbd75 port for the qmk_firmware on the master repo was merged! I will plan to add KBD75 support on kbfirmware.com next!
- **7/5/17 UPDATE**: My pull-request to qmk firmware builder is still open, qmk.sized.io moved http://kbfirmware.com/ ([see ruiqimao's reddit post](https://www.reddit.com/r/MechanicalKeyboards/comments/6jpvai/news_httpqmksizedio_is_now_httpkbrmaoio/)), and I updated the guide below for using the latest QMK Flasher (v0.5.2).
- **4/16/18 UPDATE**: Moving the guide to github, since my reddit post was archived.

Edits
-----

- **edit 1** Added a note up top clarifying that this is for KBD75 pcbs that support QMK.
- **edit 2** Added ISO key mapping notes from /u/Distq
- **edit 3** Added a section for PS2AVR incase people mistakenly flash their PS2AVR with QMK.
- **edit 4** Updated installing QMK Flasher software and links to qmk.sized.io to kbfirmware.com
- **edit 5** Added default lighting control keymapping
- **edit 6** Updated hotkeys with RGB Underglow Brightness Increase and Decrease, thanks to /u/OneNightFriend
- **edit 7** Made Warnings more prominent about binding a `FN` key before uploading a new .hex file when using http://qmkeyboard.cn
- **edit 8** Update mentions of hex files to not have spaces in path, thanks to /u/OneNightFriend and /u/OleDaneBoy



# Original Post with configuring your KBD75 with QMK

----

I'll be honest, figuring out how to change some key mapping for [my new KBD75](http://imgur.com/a/zf7bL) was not straight forward to a keyboard newbie like me who doesn't use QMK. Bootmapper client was a tad easier to use (I use it for my WKL B.mini EX X2).

I want to fill in some gaps in the documentation. Thanks to /u/kbdfans for answering my questions and other users :)

**Disclaimer**: Some users have reported that their R3 PCBs did not come with QMK, but with ps2avr. These instructions are for QMK.

------
### Context

Between http://qmkeyboard.cn/, the piece of paper that came with the keyboard, I also went to the "buy" page of the KBD75 R3 (where I ordered it), and found "How do I update the program?" which links to this [Google Doc](https://docs.google.com/document/d/1ohFor5nKtKH3-1orGzN4uCCkNO87u2r8AvzMicUHl04/edit). The screenshots are in Chinese so I wrote it step by step below.

-----

### Changing key mapping on http://qmkeyboard.cn

If your keyboard came pre-assembled as mine, the second key on the top right should actually be your FN (function) key. This key: http://imgur.com/SjdRgNf

So when you first go to http://qmkeyboard.cn, and you have not edited your layout before, you will want to choose the `KBD75` layout preset. By default, the key which came in my keyboard as the "FN" key is actually the Scroll Lock key or `SLCK`.

**Note:** The "FN" key is actually just a modifier key to let you access `Layer 1`. Notice that the main layer is actually `Layer 0` because we programmers start numbering things starting with 0 ;)

##### Remapping a key
You can re-create this "FN" key by: 

1. Going to the `KEYMAP 键位` section of the layout
1. Picking a key to replace by clicking on it.
1. Make sure `选择层进行修改 Select a layer to modify.` is set to `0`
1. Under `配置选中的按键 Configure the selected key.`, Click on the box where the current key assignment is, in this case `KC_SLCK`.
1. Choose the `FN` tab and click on `MO()`
1. Then select `Layer 1`

It should look a little like this: http://imgur.com/xP5LvJD

This will make the formerly default Scroll Lock (`SLCK`) key, to become a Function (`MO(1)`) key.

Following the instructions above, you can also remap the `Pause` key into something like the `Del` (Delete) key.

##### Saving and Loading your key mapping on the website
*Saving your changes into a file*: If you want to be able to import your layout when you go to http://qmkeyboard.cn, go to the `SETTINGS 设置` section, and click `Save Configuration` under `保存你的布局 Save your layout.`. This will ask you to save a `json` file on your computer. 

The main reason for this is you can actually use this generate file to load your keyboard layout/key mapping on the QMK project's QMK Firmware Builder at http://kbfirmware.com/.

*Loading your changes into the website* : The next time you go to http://qmkeyboard.cn/, just click on `Upload` under `Upload QMK Firmware Builder configuration 上传自己的配置[.json]`

#####  Saving a .hex file for flashing
Once you're satisfied with your changes, you will want to download the `.hex` file from the website so that you can flash your keyboard.

You can do this by going to the `COMPILE 下载固件` section and then click on `Download .hex` under `下载.hex固件 Download the .hex file to flash to your keyboard.`.

**=====================================================================**

- **WARNING: Make sure you have a key bound for `FN` before flashing your keyboard WARNING** 
  - **Not binding a `FN` key will result in you NOT having a FN key to put your keyboard into "Bootloader" mode.**
  - If you accidentally do this, you will have to open up the case and press the physical RESET button. See [this comment thread](https://www.reddit.com/r/MechanicalKeyboards/comments/6hl837/kbd75_r3_qmk_howto_guide/djnu4ep/).

- **WARNING: The path to the hex file cannot have spaces in it. WARNING** thanks to /u/OneNightFriend and /u/OleDaneBoy
  - If it does you'll get an erased board without firmware uploaded. If you move the file somewhere where there aren't spaces in the path you will be able to successfully reflash.
See this thread, [[QMK][HELP] New KBD75 can't flash](https://www.reddit.com/r/olkb/comments/7bw1kn/qmkhelp_new_kbd75_cant_flash/), posted by /u/OleDaneBoy

**=====================================================================**

That part of the page looks like this: http://imgur.com/Ma2Ei5t

-------

### Key-mapping for non-US ISO keys

Thanks to /u/Distq for figuring this out:

The [documentation](https://docs.qmk.fm/keycodes.html) for key codes shows a couple of "non-US" codes. KC_NUBS supposedly maps to backslash/pipe but in reality works as </>/| (for me, at least). 

So if anyone has the same problem and finds this in the future, KC_NUBS (listed under the alphabetic letters in the "Primary" keys maps on the firmware builder) maps to the usual ISO key for lt/gt/bar.


-------

### Installing the QMK Flasher software
When you got your keyboard, you got a piece of paper that said, "Program web link http://qmkeyboard.cn/".
At the bottom of the page and the piece of paper you saw `QMK Firmware flasher download 百度云链接，github链接`, with an arrow to the github link. **Ignore this and see instructions below**

~~The first link, I couldn't get to work/download, the second one was a link to the releases page of the official QMK project.~~

~~However when I went to the github page I mistakenly just downloaded the latest version. Installing the newest version (as of 6/16/17, its v.0.5.2) did not match the instructions for this keyboard that I found in the Google Doc.~~

~~**Install an older version of qmk flasher**, the version where it was still called qmk firmware flasher: https://github.com/qmk/qmk_flasher/releases/tag/v0.5.0~~

##### 7/5/17 Update: 

**Install the latest version of [QMK Flasher 0.5.2](https://github.com/qmk/qmk_flasher/releases/tag/0.5.2)**, `QMK Firmware  Flasher` was been renamed as `QMK Flasher`. After you install this, when you open it you may get this error in the app ([screenshot](http://imgur.com/kSijL61)): `Could not run dfu-programmer! Have you installed the driver? Try using qmk_driver_installer to fix it.`

If you see that error message, just continue to the **"Bootloader" Mode and Installing drivers** section below.

----------

### "Bootloader" Mode and Installing drivers

Once you've installed the `QMK Flasher` software and downloaded a `.hex` file with your keymap changes, you will need to flash your keyboard.

You will be able to set your keyboard to "Bootloader" mode with `FN + backspace`. Doing this, your keyboard will reconnect as a new device called `ATmega32u4` which Windows 10 will **not** automatically find drivers for. 

This is when the [Google Doc](https://docs.google.com/document/d/1ohFor5nKtKH3-1orGzN4uCCkNO87u2r8AvzMicUHl04/edit) came in handy.

To install the drivers:

1. go to `Device Manager` in Windows
1. right click the `ATmega32u4` device (it will have a warning icon next to it), then `Update Drivers`
1. Click on `Browse my computer for driver software`, then find the path where you installed QMK Firmware Flasher, and in that path find `$path\resources\app.asar.unpacked\dfu\dfu-prog-usb-1.2.2` or ie. `C:\Program Files (x86)\QMK Flasher\resources\app.asar.unpacked\dfu\dfu-prog-usb-1.2.2`

If you've sat there waiting for your keyboard to be ready to flash (in QMK Flasher v0.5.0) or see the error `Could not run dfu-programmer! Have you installed the driver? Try using qmk_driver_installer to fix it.` (in QMK Flasher v0.5.2), installing drivers should fix these behaviors and make the keyboard immediately flashable.

### My keyboard won't work when I set it to "Bootloader" mode
If you set your keyboard on "Bootloader" mode it becomes unusable (you can't type on it), you can always unplug and replug the keyboard so it becomes usable to type anything. 

**=====================================================================**

**WARNING**: **DO NOT UNPLUG IT WHILE IT'S BEING FLASHED**.

**IF YOU DO THIS, YOU MAY BRICK YOUR KEYBOARD'S PCB**

**=====================================================================**

--------

### Default Keymapping for controlling lights

You can find this on Layer `1` if you upload my [kbd75.json](https://pastebin.com/RQzUQsvf) (I pasted this on pastebin, not sure where else to put it) file on https://kbfirmware.com/. ([See a screenshot](http://imgur.com/3swbJ5n))

Key combo | Effect | Key code |
--- | --- | ---- |
`FN` + `Q` | Toggle RGB Underglow On/Off |  `RGB_TOG` |
`FN` + `W` | Toggle RGB Underglow Modes | `RGB_MOD`  |
`FN` + `E` |  RGB Underglow Hue Increase | `RGB_HUI` |
`FN` + `R` |  RGB Underglow Hue Decrease | `RGB_HUD` |
`FN` + `T` |  RGB Underglow Saturation Increase | `RGB_SAI` |
`FN` + `Y` |  RGB Underglow Saturation Descrease | `RGB_SAD` |
`FN` + `U` |  RGB Underglow Brightness Increase | `RGB_VAI` |
`FN` + `I` |  RGB Underglow Brightness Decrease | `RGB_VAD` |
`FN` + `C` |  In-switch back light decrease | `BL_DEC` |
`FN` + `V` |  In-switch back light toggle on/off | `BL_TOGG` |
`FN` + `B` |  In-switch back light increase | `BL_INC` |
`FN` + `N` |  In-switch back light step through | `BL_STEP` |

---------

~~~~~~~~~~~~~~~~~~~~

# If you think you have a PS2AVR pcb (R1 and R2), read below

Comments by /u/mattizmyname, re-ordered/modified for context by /u/blackhawkpanda

~~~~~~~~~~~~~~~~~~~~

----------

# Bootmapper Client vs. QMK

I've only had awful experiences with QMK and given all the posts about it, I am obviously not alone. Hopefully you have the PS2AVR version because if so it should be pretty straightforward and not require nearly as many hoops as this post lays out for QMK.

**Note**: Since KBDfans is from China and relies on google translate- I think there have been some miscommunication with people and many "QMK" people probably actually have PS2AVR and vice versa.

### Bootmapper Client vs QMK?

The first round of KBD75 had a black PCB and used PS2AVR, the second round was a white PCB w/ PS2AVR, and my understanding is the 3rd round forward is a white PCB w/ a reset button that uses QMK.

- **Round 1**: Black PCB - PS2avrGB_firmware
- **Round 2**: White PCB - PS2avrGB_firmware  **<--- This is the round where it gets confusing**
- **Round 3+**: White PCB with a physical reset button - qmk_firmware **<---- Some users have reported that their R3 KBD75 only worked with Bootmapper Client**

----
### PS2AVR Version

If you're confident you have the PS2AVR version of the KBD75 PCB, you ***should*** be able to use bootmapper client to change anything on the board.

I don't really know for sure how to differentiate which one you have, but my understanding is the obvious difference is having a reset button on the PCB or not.

### Flashing it / upgrading the firmware:
With the PS2AVR PCB, you should be able to use **PS2AVRGB_Firmware** w/ Bootmapper Client. You can read [livingspeedbump's guide on configuring your KBD75](https://www.massdrop.com/talk/1392/programming-kbd-keyboards-via-bootmapper-client?mode=guest_open) via Bootmapper Client.

You should know you have the PS2AVR one if Bootmapper **will successfully let you connect to the PCB**.

If it doesn't work initially, sometimes you have to re-plug it in or change USB slots.

If it still doesn't work, you likely have QMK, or something is wrong with your PCB (worst case.)

### Debugging PS2AVR PCB

You're ***not*** supposed to use ps2avrGB4U firmware.

I have used more than one KBD75 w/ PS2AVR and the firmware you're supposed to use is **PS2avrGB_firmware**, probably why you're having issues.

Don't fear though, I made the same mistake initially as well. You should be fine once you flash it properly.

-----------------------------------
