import cv2
import numpy as np
import imutils
import datetime

gun_cascade =cv2.CascadeClassifier('cascade.xml')
camera  = cv2.VideoCapture(0)

firstFrame = None
gun_exist = None

while True:

    ret, frame = camera.read()
  
    frame = imutils.resize(frame,width=500,height=500)
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    gun = gun_cascade.detectMultiScale(gray,1.3,5,minSize=(100,100))
   
    if len(gun)>0:
        gun_exist = True
    for(x,y,w,h) in gun:
       
        frame = cv2.rectangle(frame,(x,y),(x+w,y+h),(255,0,0),2)
       
        roi_gray = gray[y:y + h, x: x + w]
        roi_color = frame[y:y+h, x: x + w ]
    if firstFrame is None:
        firstFrame = gray
        continue
    
    cv2.imshow('gray',frame)
    if cv2.waitKey(20) & 0xFF==ord('d'):
       break


    
if gun_exist:
        print('gun exist')
else:
    print('gun didnot detected')
    


camera.release()
cv2.destroyAllWindows()






