### Intro


Using TensorFlow Lite with Python is great for embedded devices based on Linux, such as Raspberry Pi.  

This is the guide for installing TensorFlow Lite on the Raspberry Pi and running pre-trained object detection models on it.

## Step 1. Setting up Rasperry Pi

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

**Prepare apt-get Sources**

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

**Do the Upgrade**

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

**2. Making sure camera interface is enabled in the Raspberry Pi Configuration menu**

Click the Pi icon in the top left corner of the screen, select Preferences -> Raspberry Pi Configuration, and go to the Interfaces tab and verify Camera is set to Enabled. If it isn't, enable it now, and reboot the Raspberry Pi.

Converting Tensorflow to Tensorflow Lite

Using TensorFlow Lite converter. It takes TensorFlow model and generates a TensorFlow Lite model (an optimized FlatBuffer format identified by the .tflite file extension).

## Step 2. Install TF Lite dependecies and set up virtual environment

clone this repo

```cpp
git clone https://github.com/yanovsk/Raspberry-Pi-TF-Lite-Object-Detection
```

rename the folder to "tfliteod"

```cpp
mv Raspberry-Pi-TF-Lite-Object-Detection tfliteod
cd tfliteod
```

run shell script to install get_pi_requirements

```cpp
bash get_pi_req.sh
```

Note: shell script will auto install the lastest version of Tensorflow. To install specific version of TF, run pip3 install tensorflow==x.xx (where x.xx stands for the version you want to install)

**Set up virtual environment**

Install vitrtualenv

```cpp
pip3 install virtualenv 
```

Then, create the "tfliteod-env" virtual environment by issuing:

```cpp
python3 -m venv tfliteod-env
```

This will create a folder called tfliteod-env inside the tflite1 directory. The tfliteod-env folder will hold all the package libraries for this environment. Next, activate the environment by issuing:

```cpp
source tfliteod-env/bin/activate
```

## Step 3. Set up TensorFlow Lite detection model

Once, tensorflow is install we can proceed to seting up the object detection model. 

We can use either pre-trained model or train it on our end. For the simplicity sake let's use pre-trained [sample model by google](https://www.tensorflow.org/lite/examples/object_detection/overview)

Download the sample model (also could be done thru direct link [here](https://tfhub.dev/tensorflow/lite-model/ssd_mobilenet_v1/1/metadata/1?lite-format=tflite))

```cpp
wget https://storage.googleapis.com/download.tensorflow.org/models/tflite/coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.zip
```

upzip it 

```cpp
unzip coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.zip -d Sample_model
```

## Step 4. Run the model

Note: the model should work on either [Picamera module](https://projects.raspberrypi.org/en/projects/getting-started-with-picamera) or any other webcam plugged in to the Raspberry Pi as a usb device. 

From ```home/pi/tfliteod``` run the following command:

```cpp
python3 TFL_object_detection.py --modeldir=Sample_model
```

After initializing  webcam window should pop-up on your Raspebbery Pi and object detection should work.

Note: this model can recongnize only 80 common objects (check labels.txt for more info on metadata)

However, you can custom train the model [using this guide.](https://github.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi#part-1---how-to-train-convert-and-run-custom-tensorflow-lite-object-detection-models-on-windows-10)

Happy hacking!
