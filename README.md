# tensorflow
Install TensorFlow on Raspberry Pi 4
Thank you Evan @ https://github.com/EdjeElectronics
```
git clone https://github.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi.git
```
Make the name shorter
```
mv TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi tflite1
cd tflite1
```
Install virtualenv
```
sudo pip3 install virtualenv
```
Create the "tflite1-env"
```
python3 -m venv tflite1-env
```
This will create a folder called tflite1-env inside the tflite1 directory. The tflite1-env folder will hold all the package libraries for this environment. Next, activate the environment by issuing:
```
source tflite1-env/bin/activate
```
To make things easier, I wrote a shell script that will automatically download and install all the packages and dependencies. Run it by issuing:
```
bash get_pi_requirements.sh
```

```
wget https://storage.googleapis.com/download.tensorflow.org/models/tflite/coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.zip
```
Unzip it to a folder called "Sample_TFLite_model" by issuing (this command automatically creates the folder):
```
unzip coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.zip -d Sample_TFLite_model
```
```
python3 TFLite_detection_webcam.py --modeldir=Sample_TFLite_model
```
Open a command terminal and move into the /home/pi/tflite1 directory and activate the tflite1-env virtual environment by issuing:
```
cd /home/pi/tflite1
source tflite1-env/bin/activate
```
Add the Coral package repository to your apt-get distribution list by issuing the following commands:
```
echo "deb https://packages.cloud.google.com/apt coral-edgetpu-stable main" | sudo tee /etc/apt/sources.list.d/coral-edgetpu.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update
```
Install the libedgetpu library by issuing:
```
sudo apt-get install libedgetpu1-std
```
```
wget https://dl.google.com/coral/canned_models/mobilenet_ssd_v2_coco_quant_postprocess_edgetpu.tflite
mv mobilenet_ssd_v2_coco_quant_postprocess_edgetpu.tflite Sample_TFLite_model/edgetpu.tflite
```
Insert picture of Coral USB Accelerator plugged into Raspberry Pi here!

Make sure the tflite1-env environment is activate by checking that (tflite1-env) appears in front of the command prompt in your terminal. Then, run the real-time webcam detection script with the --edgetpu argument:
```
python3 TFLite_detection_webcam.py --modeldir=Sample_TFLite_model --edgetpu
```

The --edgetpu argument tells the script to use the Coral USB Accelerator and the EdgeTPU-compiled .tflite file. If your model folder has a different name than "Sample_TFLite_model", use that name instead.

After a brief initialization period, a window will appear showing the webcam feed with detections drawn on each from. The detection will run SIGNIFICANTLY faster with the Coral USB Accelerator.

If you'd like to run the video or image detection scripts with the Accelerator, use these commands:
```
python3 TFLite_detection_video.py --modeldir=Sample_TFLite_model --edgetpu
python3 TFLite_detection_image.py --modeldir=Sample_TFLite_model --edgetpu
```
