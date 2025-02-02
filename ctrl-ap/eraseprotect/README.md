# ERASEPROTECT SAMPLE
**DISCLAIMER: If you enable both APPROTECT and ERASEPROTECT at the same time without setting the internal ERASEPROTECT.DISABLE register in firmware, you will no longer be able to program the nRF5340! It will be Bricked!**

# Error
This sample do only work for the application core.
The eraseprotect in this sample can be undone by recovering the network core.
I have not added a fix for this yet.
If you want me to speed up on the fix, create an issue# Error
This sample do only work for the application core.
The eraseprotect in this sample can be undone by recovering the network core.
I have not added a fix for this yet.
If you want me to speed up on the fix, create an issue.

## Versions
- nRF Connect SDK: v2.0.1
- nRF5340DK
- Python 3
- pynrfjprog 10.16.0

## Documentation
[CTRL-AP - Control access port](https://infocenter.nordicsemi.com/topic/ps_nrf5340/ctrl-ap.html?cp=3_0_0_7_9)

## Preparation
Change the snr numbers in the scripts to match the ID printed on the sticker on your DK, or found via:
```
nrfjprog --ids
```

## Building and Running
First, build and flash the code to a nRF5340DK.
Add --recover option to undo a previous ERASEPROTECT. 
```
west build -p -b nrf5340dk_nrf5340_cpuapp
west flash --recover
```

The firmware will enable ERASEPROTECT on startup.
Check the status from the [ERASEPROTECT.STATUS](https://infocenter.nordicsemi.com/topic/ps_nrf5340/ctrl-ap.html?cp=3_0_0_7_9_5_0_5#register.ERASEPROTECT.STATUS) register with a script:
```
python3 scripts/read_eraseprotect_status.py
```

To disable eraseprotect, the firmware has written a key to an internal [ERASEPROTECT.DISABLE](https://infocenter.nordicsemi.com/topic/ps_nrf5340/ctrl-ap.html?cp=3_0_0_7_9_6_5#unique_1767310017). Write the same key to another, different [ERASEPROTECT.DISABLE](https://infocenter.nordicsemi.com/topic/ps_nrf5340/ctrl-ap.html?cp=3_0_0_7_9_6_7#unique_1545884140) with the script:
```
python3 scripts/disable_eraseprotecy.py
```

## Alternatives
As long as APPROTECT is not enabled, you can disable the ERASEPROTECT by flashing with the --recover option, which will use the debugger to reset the chip.
ERASEPROTECT can also be enabled by the debugger by running the script:
```
python3 scripts/enable_eraseprotect.py
```

## Trusted Firmware-M
This sample does not work with TF-M included (nrf5340dk_nrf5340_ns), since the registers used are secure, and the application is non-secure.
To make it work with TF-M, you will have to make a secure service for setup of ERASEPROTECT.
For hints on how this can be done, see [TF-M secure peripheral partion](https://developer.nordicsemi.com/nRF_Connect_SDK/doc/2.0.1/nrf/samples/tfm/tfm_secure_peripheral/README.html)
