# Introduction
Repo containing examples with flask, opencv, crow, ffmpeg, etc as well as 
mockups of an RTSP based IP camera stream application. The project was intended
to serve as a solution to view the Discover CCRI test site however the amount 
of issues that arised with python threading, codec + buffering issues, and more
issues related to performance, lead to the final solution to be a pre-existing 
open source CCTV application zoneminder. Check it out [here](https://github.com/DiscoverCCRI/site-surveillance)

---

# Dependencies
---
### minimal dependencies
**fetch updates**
```
$ sudo apt-get update
```

**install Docker**
```
$ curl -sSL https://get.docker.com | sh
```
- add user to docker grp and fix perms on docker sock <br>
        ```
        $ sudo usermod -aG docker pi
        $ sudo chmod 666 /var/run/docker.sock 
        ``` <br>
- confirm installation by checking version and running hello world container <br>
        ```
        $ docker --version
        $ docker run hello-world
        ``` <br>
        
**py 2&3 support**
```
$ sudo apt-get install python-dev python-numpy &&
sudo apt-get install python3-dev python3-numpy
```

**pip3 for easy installation of python modules**
```
$ sudo apt-get -y install python3-pip
```
**install ngrok for proxying, making localhost visible to outside**
```
$ sudo apt install ngrok &&
pip3 install ngrok-api
```
visit here for more directions: https://github.com/ngrok/ngrok-api-python
- You'll want to [create an ngrok account](https://dashboard.ngrok.com/get-started/setup) and add your authtoken to the 
ngrok agent as well as add your API-key to /src/test_stream_v1.py <br>
        ```
        $ ngrok config add-authtoken <authtoken>
        ``` <br>
- after setting up ngrok open up port 5000 to forward publicly <br>
        ```
        $ ngrok http 5000
        ``` <br>
this will open an ngrok session and provide you with the generated public facing
URL of your project.


**For GTK GUI features**
```
$ sudo apt-get install libgtk-3-dev
```
bulk install our python dependencies using pip
**datetime, openCV, Flask, imutils**
```
$ pip3 install datetime opencv-python flask imutils
```
python3 test_stream_v1.py proxy_stream.py
```
Enter the IP address of the machine you are hosting this project from in a browser
and your feed should be present. 

# Issues
---
Below I highlight some issues I ran into when installing some dependencies for 
the examples in this directory. Besides that the biggest issue was performance.
Properly threading multiple streams to allow for the best quality stream proved
difficult in python with opencv and flask. A proper solution could include the use
of a low level language utilizing some sort of memory manipulation. 

Issues I ran into when installing on Raspian on RPI 4
  - When installing imutils there might be some errors, to fix I installed
  these packages and fixed some various issues with numpy
    ```
    pip3 install opencv-python
    sudo apt-get install libcblas-dev
    sudo apt-get install libhdf5-dev
    sudo apt-get install libhdf5-serial-dev
    sudo apt-get install libatlas-base-dev
    sudo apt-get install libjasper-dev 
    sudo apt-get install libqtgui4 
    ```
  - Issues with numpy on my local machine with bad versions. To fix:
    ```
    $ pip3 install -U numpy
    ```

# Examples
---
***Using openCV to view basic feed*** <br>
see: ```ip_cam/src/tests/1-cam-feed.py```
![example](https://github.com/DiscoverCCRI/ip_cam/blob/main/imgs/basic_feed.png)

***Using openCV to view multiple cameras*** <br>
see: ```ip_cam/src/tests/multi-feed_v3.py```
![example2](https://github.com/DiscoverCCRI/ip_cam/blob/main/imgs/multi-stream.png)

***Using flask to host IP camera's feed on localhost port 5000*** <br>
see: ```ip_cam/src/test_stream_v1.py```
![example3](https://github.com/DiscoverCCRI/ip_cam/blob/main/imgs/flask_localhost.png)

***Using ngrok to proxy localhost port 5000 to generated URL*** <br>
*see: ip_cam/src/proxy_stream.py* <br>
![example4](https://github.com/DiscoverCCRI/ip_cam/blob/main/imgs/localhost-to-ngrok_stream.png)

***ngrok interface*** <br>
*using the ngrok cmd* <br>
    ```
    ngrok http 5000 
    ``` <br>
    
![example4](https://github.com/DiscoverCCRI/ip_cam/blob/main/imgs/ngrok_session.png)


