# Getting started

## Setting up your Bolt
The Curious Bolt exposes a serial port over USB that can be used to interface with its glitcher, scope, and programmable I/O.

A python library is available to talk to your Bolt. To use it:

- Install _python_ (3.x) and _pip_
- Install _pyserial_ by running `pip install pyserial`
- Copy [scope.py](../lib/scope.py) to your working directory
- Check if you can connect by running `python3 -c "from scope import Scope;s=Scope()"`. If you get a warning that the Bolt could not be found, check its USB connection.
- If you run Linux and can't connect, check [your udev rules](linux_udev.md).

## Logic analysis in PulseView
![A screenshot of PulseView being used with Curious Bolt](images/pulseview.png)
See [Logic Analyzer](logic_analyzer.md) for instructions.

## Fault injection with the voltage glitcher
![](images/scope.jpg)
See [Voltage Glitching](voltage_glitching.md) for instructions.

## Programmable I/O
You can get the Bolt to send programmable output patterns on its output GPIO, based on triggers. See [Programmable I/O](programmable_io.md).

## Differential power analysis with the oscilloscope
Documentation for this is in progress.

⚠️ **NOTE FOR ECSC23 PARTICIPANTS**: the scope of your Bolt is not necessary for your challenges, and it will not help you either. For the competition you can ignore this feature.

## Build from sources

If you would like to make changes to the Curious Bolt firmware, you can follow these build instructions. [Clone](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) the [Bolt repository](https://github.com/tjclement/bolt).

### Build with Docker (Windows/Linux/MacOS)

To build on any operating system, we've provided a Dockerfile. Use as follows:
1. Install Docker: [Windows](https://docs.docker.com/desktop/setup/install/windows-install/), [Linux](https://docs.docker.com/desktop/setup/install/linux/), or [MacOS](https://docs.docker.com/desktop/setup/install/mac-install/)
    - On Linux, you can also install the lighter-but-no-GUI [Docker Engine](https://docs.docker.com/engine/) 
2. In a terminal, go to the repo folder `bolt/firmware/bolt`
3. Build the bolt-builder `sudo docker build -t bolt-builder .`
    - This will build the docker image from `Dockerfile` in the current folder. This docker file will contain all neccessary packages for building the Bolt firmware.
4. Use the bolt-builder `sudo docker run --rm -it -v "$(pwd)":/project bolt-builder /build.sh`
    - This will build the UF2 image that we can install on the Curious Bolt
    - The built image can be found in `bolt/firmware/bolt/build/glitcher.uf2`
5. Go to the directory where you put `scope.py` see [Setting up your Bolt](#setting-up-your-bolt).
    - Alternatively, `scope.py` is also in this repo `bolt/lib/scope.py`
6. Put the Bolt in bootloader mode `python3 -c "from scope import Scope; s=Scope(); s.update()"`
    - If successful, the bootloader LEDs will turn off, the USB device disconnects, and reconnects as a storage device.
7. Put the `glitcher.uf2` we built earlier on the newly connected storage device. After some time, the device should disconnect again and boot into the updated firmware. 

### Unbricking the Bolt
If you modified the code in such a way that it can't be put into bootloader mode anymore using `scope.py`, then find the two test points next to the USB connector (TP1 and TP2 on the [schematic](https://github.com/tjclement/bolt/blob/main/hardware/bolt/schematics.pdf)). Unplug the Bolt, short the two testpoints, and connect the Bolt to your computer. It should now connect in bootloader mode, allowing you to flash your firmware again.