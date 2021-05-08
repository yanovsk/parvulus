# Guide
Run object detection model on the Raspberry Pi
**Intro**

---

Using TensorFlow Lite with Python is great for embedded devices based on Linux, such as Raspberry Pi and Coral devices with Edge TPU, among many others. 

This is the guide for installing TensorFlow Lite on the Raspberry Pi and running object detection models on it.

**Step 1. Setting up Rasperry Pi**

---

Upgrade Raspbian Stretch to Buster

(If you on Buster, skip this step and simply run sudo apt-get update and sudo apt-get dist-upgrade)

```cpp
$ sudo apt-get update && sudo apt-get upgrade -y
```

Verify nothing is wrong. Verify no errors are reported after each command. Fix as required (you’re on your own here!).

```cpp
$ dpkg -C
$ apt-mark showhold
```

### Prepare apt-get Sources

Update the sources to apt-get. This replaces “stretch” with “buster” in the repository locations giving apt-get access to the new version’s binaries.

```cpp
$ sudo sed -i 's/stretch/buster/g' /etc/apt/sources.list    
$ sudo sed -i 's/stretch/buster/g' /etc/apt/sources.list.d/raspi.list
```

Verify this caught them all by running the following, expecting no output. If the command returns anything having previously run the sed commands above, it means more files may need tweaking. Run the sed command for each. The aim is to replace all instances of “stretch”.

```cpp
$ grep -lnr stretch /etc/apt
```

Speed up subsequent steps by removing the list change package.

```cpp
$ sudo apt-get remove apt-listchanges
```

### Do the Upgrade

To update existing packages without updating kernel modules or removing packages, run the following.

```cpp
$ sudo apt-get update && sudo apt-get upgrade -y
```

Alternatively, to include kernel modules and removing packages if required, run the following 

```cpp
$ sudo apt-get update && sudo apt-get full-upgrade -y
```

Cleanup old outdated packages.

```cpp
$ sudo apt-get autoremove -y && sudo apt-get autoclean
```

Verify with

```cpp
 cat /etc/os-release.
```

Update Firmware

```cpp
$ sudo rpi-update
```

and

```cpp
sudo apt-get install -y python3-pip
```

and

```cpp
pip3 install --upgrade setuptools
```

---

2. Making sure camera interface is enabled in the Raspberry Pi Configuration menu

Click the Pi icon in the top left corner of the screen, select Preferences -> Raspberry Pi Configuration, and go to the Interfaces tab and verify Camera is set to Enabled. If it isn't, enable it now, and reboot the Raspberry Pi.

Converting Tensorflow to Tensorflow Lite

Using TensorFlow Lite converter. It takes TensorFlow model and generates a TensorFlow Lite model (an optimized FlatBuffer format identified by the .tflite file extension).

**Step 2. TensorFlow Lite Download and Setup**

---

Install dependencies 

```cpp
sudo apt-get install -y libatlas-base-dev libhdf5-dev 
libc-ares-dev libeigen3-dev build-essential 
libsdl-ttf2.0-0 python-pygame festival python3-h5py
```

If this is too complicated to type in your shell, clone this repo

```cpp
git clone https://github.com/yanovsk
```

rename the folder to "tfliteod"

```cpp
mv TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi tfliteod
cd tfliteod
```

run shell script to install get_pi_requirements

```cpp
bash get_pi_req.sh
```
