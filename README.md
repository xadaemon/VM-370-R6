# Preface

This is an updated for 2020 hardware and software version of the original package by `Andy Norrie and or Bob Abeles (not clear)` but to whoever it was thanks.
Updated by CardinalBytes \(cb\[AT\]bpmx.io\) on the 18th of january 2020
---
## Preflight
Before you begin, you will need to have the following:
* Hercules installed on your Linux system. I'm using 3.13 \(see bellow\).
* About 400 MB of free disk space. Once the installation is complete, the VM system occupies about 200 MB.
	* telnet to access the VM console
	* x3270 to access VM 3270 devices
	* VM/370 R6 documentation \(If you have some and want to share drop me an email\)
	* The DDR tape image files: VMREL6.ddr.aws and CPR6L0.ddr.aws (included in the images folder)


Here's what you should have after extracting the package:

- dmkddr.aws	`IPLable DDR tape`
- vm370r6.2020.cnf	`Hercules config file for VM/370`
- dmkrio.assemble	`The real I/O config for this VM system`
- dmksys.assemble	`The system config for this VM system`
- release6.direct	`The directory for this VM system`
- images/
    - CPR6L0.ddr.aws
    - VMREL6.ddr.aws
### Notes
My lab rig:
* i7 4770
* 16GB DDR3 RAM
* SSD
* Arch Linux

Hercules was compiled from sources with clang 9.0.1 and optimizations enabled

---
# VM/370 R6 Installation Instructions:
\(remember the to not type the `$`'s\)
1. run in a shell window
    ```sh
    $ dasdinit CPR6L0.3330-1 3330 CPR6L0 404 (those 0 are all zeroes)
    $ dasdinit VMREL6.3330-1 3330 VMREL6 404
    $ hercules -f vm370r6.2020.cnf
    ```

2. in another shell window:
	```SH
    $ telnet localhost 3270
    ```
3. back on Hercules:
	```
    ipl 580
    ```

4. in the telnet window you should see the prompt "ENTER CARD READER..."
   **This will error out in modern hardware with recent hercules versions however it is normal!**
    ```
	input 581 3420
	output 131 3330 VMREL6
	restore all
    ```
    now on the hercules terminal window please quit hercules and open it again. now repeat step 3, and back on the telnet type:
    ```
	input 582 3420
	output 132 3330 CPR6L0
	restore all
    ```
    > These on the original readme where stated to take up to 2 mins
    -- cb

5. on Hercules:
    ```	
    quit
    ```

6. a few more steps
    repeat step 2
    repeat step 3

7. on Hercules:
	```
    ipl 131
    ```

8. in telnet window you should see "CHANGE TOD CLOCK..."
	NO
    in telnet window you should see "START ((COLD WARM..."
	COLD
    up comes VM/370. Enable terminals like this:
	enable all

9. in another shell window:
    ```
	x3270 -model 3278-2
	```
    A 3270 window will appear, from its Connect menu select "Other...", then type
	`localhost 3270`
    and press the Connect button

You should now see the VM/370 logo. Further instructions are outside of the
scope of this document.

# NOTES
-----

There is a known problem in Hercules that causes intermittant DSP003 abends.
I'm looking into it.

Cheers to all,

Bob Abeles
