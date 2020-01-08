---
title: "A basic PIR-triggered, picture-taking, email-sending...alarm."
date: 2020-01-07
tags: [Raspberry Pi, python]
categories: [code]
header:
  image: "images/header_image2.png"
excerpt: "Raspberry Pi powered alarm."
---

Here is the basic code for a PIR-triggered, camera-based alarm system that emails a picture once triggered. The code works but will be made safer and more Pythonic.


```python
import os
import glob
import picamera
import RPi.GPIO as GPIO
import smtplib
from time import sleep
```

The following imports may seem unique, they're for the email library.


```python
from email.MIMEMultipart import MIMEMultipart
from email.MIMEText import MIMEText
from email.MIMEBase import MIMEBase
from email import encoders
```

Email and database variables


```python
sender = 'YourEmailHere@gmail.com'
password = 'YourGmailPasswordHere'
receiver = 'YourEmailHereAgain@gmail.com'
DIR = './Database/'
FILE_PREFIX = 'image'
```

GPIO settings for the PIR sensor


```python
GPIO.setwarnings(False)
GPIO.setmode(GPIO.BOARD)
GPIO.setup(11, GPIO.IN)  # Read output PIR 
```

The 'send_mail' function needs some try/exceptions but works.


```python
def send_mail():
    print 'Sending E-Mail'
    # Create the directory if not exists
    if not os.path.exists(DIR):
        os.makedirs(DIR)
    # Find the largest ID of existing images.
    # Start new images after this ID value.
    files = sorted(glob.glob(os.path.join(DIR, FILE_PREFIX + '[0-9][0-9][0-9].jpg')))
    count = 0
    
    if len(files) > 0:
        # Grab the count from the last filename.
        count = int(files[-1][-7:-4])+1

    # Save image to file
    filename = os.path.join(DIR, FILE_PREFIX + '%03d.jpg' % count)
    # Capture the face
    with picamera.PiCamera() as camera:
        pic = camera.capture(filename)
    # Sending mail
    msg = MIMEMultipart()
    msg['From'] = sender
    msg['To'] = receiver
    msg['Subject'] = 'Movement Detected'
    
    body = 'Picture is Attached.'
    msg.attach(MIMEText(body, 'plain'))
    attachment = open(filename, 'rb')
    part = MIMEBase('application', 'octet-stream')
    part.set_payload((attachment).read())
    encoders.encode_base64(part)
    part.add_header('Content-Disposition', 'attachment; filename= %s' % filename)
    msg.attach(part)
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.starttls()
    server.login(sender, password)
    text = msg.as_string()
    server.sendmail(sender, receiver, text)
    server.quit()
```

If the PIR sensor is HIGH then the 'send_mail' function is executed.The sleep method determines polling frequency.


```python
while True:
    i = GPIO.input(11)
    if i == 0:  # When output from motion sensor is LOW
        print "No intruders", i
        sleep(0.3)
    elif i == 1:  # When output from motion sensor is HIGH
        print "Intruder detected", i
        send_mail()
```
