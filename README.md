# Docker-IOT-Rpi 
![](https://github.com/sangam14/Docker-IOT-rpi/blob/master/DockerIOTrpi.gif)


# Install Docker on Raspberry pi 


```sudo apt-get update && sudo apt-get upgrade ```

```curl -sSL https://get.docker.com | sh```

```sudo usermod -aG docker your-username ```


# make Directory 
 mkdir Docker-ledblink


# jump into Directory 
 cd Docker-ledblink



nano / vi led_blinker.py 

```
import RPi.GPIO as GPIO
import time

# Configure the PIN # 8
GPIO.setmode(GPIO.BOARD)
GPIO.setup(8, GPIO.OUT)
GPIO.setwarnings(False)

# Blink Interval 
blink_interval = .5 #Time interval in Seconds

# Blinker Loop
while True:
GPIO.output(8, True)
time.sleep(blink_interval)
GPIO.output(8, False)
time.sleep(blink_interval)

# Release Resources
GPIO.cleanup()
```

nano / vi Dockerfile 
```
# Python Base Image from https://hub.docker.com/r/arm32v7/python/
FROM arm32v7/python:2.7.13-jessie

# Copy the Python Script to blink LED
COPY led_blinker.py ./

# Intall the rpi.gpio python module
RUN pip install --no-cache-dir rpi.gpio

# Trigger Python script
CMD ["python", "./led_blinker.py"]

```


# build dockerfile 
```
pi@raspberrypi:~/docker-ledblink $ ls
Dockerfile  led_blinker.py
pi@raspberrypi:~/docker-ledblink $ docker build -t "docker_blinker:v1" .Sending build context to Docker daemon  3.072kB
Step 1/4 : FROM arm32v7/python:2.7.13-jessie
 ---> fd232f7d5f5f
Step 2/4 : COPY led_blinker.py ./
 ---> Using cache
 ---> 2c20ac080696
Step 3/4 : RUN pip install --no-cache-dir rpi.gpio
 ---> Using cache
 ---> 1d7557012625
Step 4/4 : CMD ["python", "./led_blinker.py"]
 ---> Using cache
 ---> 856014f90903
Successfully built 856014f90903
Successfully tagged docker_blinker:v1


````

# Run container 
```
pi@raspberrypi:~/docker-ledblink $ docker container run --device /dev/gpiomem -d docker_blinker:v1
fbea78493630d0c5e623e3b25427931bd4e8e16d8f181c805f021f558822e355

```

OR 

```docker container run --privileged -d docker_blinker:v1```

# stop container 

```
 docker stop $(docker ps -a -q)
```
 
 
 
 


