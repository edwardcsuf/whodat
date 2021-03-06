# whodat [![Build Status](https://travis-ci.com/diddlyPop/whodat.svg?branch=master)](https://travis-ci.com/diddlyPop/whodat)
Get texts from a smart computer that notifies you who is at your door

Here's the gist: 
- A Raspberry Pi with a camera could be attached to the front of a door, the peephole on the back of the door, or maybe even inside something placed on your doorstep. 
- The Pi would attempt to scan the faces of those coming up to your door, and will notify you via text who's there. 
- We will need to train a facial-recognition model with pictures of people we'd like to recognize. 
  - This could be done at time of setup, with pictures of roommates and family members, or we could attempt to train iteratively with photos taken from the Pi. 
  - Hardware constraints may prevent us from training on-device. Due to scope of project we probably should avoid implementing  offloading the training to another device. 
- Send output from facial-recognition model to user via text-message using the Twilio Python library

<hr>


Table of contents
=================
<!--ts-->
   * [Table of contents](#table-of-contents)
   * [Installation](#installation)
   * [Setup](#setup)
   * [Dependencies](#dependencies)
   * [Pi Setup](#pi-setup)
   * [Docs](#docs)
      * [Pi Hardware](#pi-hardware)
      * [Pi Software](#pi-software)
      * [Image Classifier](#image-classifier)
      * [GUI](#gui)
      * [Continuous Integration](#continuous-integration)
<!--te-->


Installation
============
Linux 
```
TODO
```

macOS 
```
// face detection uses dlib 
// when we install dlib through `pip install -r requirements` we will need to have cmake on our path.

brew install cmake

git clone https://github.com/diddlypop/whodat.git
cd whodat

// download python3.7.6 or use pyenv (not available in pip / will need to compile from source)
//    to change global python version to python3.7.6

// activate virtualenv or use pipenv (`pip install pipenv` outside of virtual environment)

pip install -r requirements.txt
```

Windows
```
TODO

https://cmake.org/download/
```

Setup
============

Encodings
```
// place directories inside profiles/ that are named after the person you are looking to encode for
// place all photos of that person in their named directory

cd assets
mkdir profiles/

// place photos inside profiles

python encode_faces.py --dataset profiles/ --encodings encodings.pickle --detection-method hog

python pi_face_recognition.py -c haarcascade_frontalface_default.xml -e encodings.pickle
```

Flask
```
// to run flask test server run the WebApp/main.py

python main.py

// direct browser to 127.0.0.1:5000/
```

Testing
```
// call pytest to run test suite
pytest
```
Dependencies
============
* twilio [![Build Status](https://secure.travis-ci.org/twilio/twilio-python.png?branch=master)](https://travis-ci.org/twilio/twilio-python)
[![PyPI](https://img.shields.io/pypi/v/twilio.svg)](https://pypi.python.org/pypi/twilio)
[![PyPI](https://img.shields.io/pypi/pyversions/twilio.svg)](https://pypi.python.org/pypi/twilio)
  * Don't forget that you should never commit your API key for twilio. Use the environment varibles:
  `TWILIO_ACCOUNT_SID`
  `TWILIO_AUTH_TOKEN` 
* PySimpleGui [DOCS](https://pysimplegui.readthedocs.io/en/latest/)
* pytest [DOCS](https://pysimplegui.readthedocs.io/en/latest/)


Pi Setup
============
Pi Setup


Docs
============

Pi Hardware
------------
* [8MP Camera](https://www.amazon.com/Raspberry-Pi-Camera-Module-Megapixel/dp/B01ER2SKFS/ref=pd_sbs_60_t_2/131-0042561-3464047?_encoding=UTF8&pd_rd_i=B01ER2SKFS&pd_rd_r=d1922854-798c-4384-bfbb-ddb0f430979c&pd_rd_w=qAEG5&pd_rd_wg=NsYnu&pf_rd_p=5cfcfe89-300f-47d2-b1ad-a4e27203a02a&pf_rd_r=D13ERXMAK8XTRGEYWXS9&refRID=D13ERXMAK8XTRGEYWXS9&th=1)
* [RPi 4 Model B w/ 4GB RAM](https://www.microcenter.com/product/609038/4--model-b-4gb?src=raspberrypi)
  * Available in 1GB, 2GB, 4GB models
* [Pi SD Card](https://www.amazon.com/SanDisk-Ultra-microSDXC-Memory-Adapter/dp/B073JWXGNT/ref=sr_1_4?gclid=Cj0KCQiAyKrxBRDHARIsAKCzn8yOXbrsNP7zloY9gPx6NpudKe4LeQq666Z9RY8GpSEyjtg50SnVzZkaAjFWEALw_wcB&hvadid=329334403855&hvdev=c&hvlocphy=9031586&hvnetw=g&hvqmt=b&hvrand=3206827577863193800&hvtargid=kwd-295528487740&hydadcr=920_1012420322&keywords=sd+micro+sd+card&qid=1579902174&sr=8-4)
* [5MP Camera](https://www.amazon.com/Raspberry-Camera-Module-Megapixels-Sensor/dp/B07L82XBNM/ref=asc_df_B07L82XBNM/?tag=hyprod-20&linkCode=df0&hvadid=343234125040&hvpos=1o1&hvnetw=g&hvrand=15056261039921602735&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9031588&hvtargid=pla-717544328579&psc=1&tag=&ref=&adgrpid=68968886317&hvpone=&hvptwo=&hvadid=343234125040&hvpos=1o1&hvnetw=g&hvrand=15056261039921602735&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9031588&hvtargid=pla-717544328579)
* Need Case for Pi
* Battery Pack (200 IQ: Battery Case?)

Pi Software
------------
The raspberry pi will use a lightweight version of Raspbian. Our goal is to include the project within a docker image to eliminate the possibility of enviroment issues.

Image Classifier
------------
We are using OpenCV with the dlib library to recognize faces. The dlib library has been trained on over 3 million faces.

GUI
------------
PySimpleGui is an awesome and easy-to-use tkinter wrapper. It is great for Raspberry Pi's and simple applications.

Continuous Integration
------------
Using Travis CI for automatic testing
