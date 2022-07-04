# SMART-HELMETS
It is important for motorcyclists to understand the risks of riding without a helmet. Riders who do not wear helmets are at risk of suffering a traumatic brain injury if they are in an accident. Without protection, the head is vulnerable to a traumatic impact in an accident even when traveling at low speeds.      We all have seen someone losing life in a road accident, and people riding a two-wheeler without a helmet is seen so often, in fact from an article of India today says "4 people die every hour in India because they do not wear a helmet" So I tried to make a "helmet for a life" trying to stop people riding a two-wheeler without a helmet and alert them over SMS whenever they go on two riders to wear a helmet and mask for there safety.
#Components and Supplies
Bolt Iot Bolt Wifi module
 Arduino UNO
LED(generic)
Jumper wires(generic)
Transmitter+Receiver
5v Relay One channel module
9v Battery(generic)
Multitool, screwdriver
App and Online Services:
Bolt Iot Bolt Clouds
Arduino web Editor
#How It Works
So the main parts of setup consist of IR sensor, Realy, Transmitter and receiver, the Bolt Wi-Fi module, and 2 Arduino Uno.
one Arduino Uno is placed on the helmet and with the help of jumping wires bolt wifi module, transmitter and IR sensor are connected to it the IR sensor is inside the helmet which continuously checks whether there is an object inside its range or not.
Another Arduino Uno is placed near the battery of 2 wheeler vehicle and with the help of jumping wires relay module and receiver are connected to it, Realy module is connected to battery and Arduino.
When the person is not wearing a helmet the IR sensor gives value "LOW" and with the help of transmitter it will transmit this will value and receiver will catch thus relay will disconnect the circuit by which person will not be able to "Selfstart" the vehicle, whereas when the person wears the helmet also IR sensor detects it and the bolt wifi module knows it with the help of python code and send an SMS to the user to wear a helmet and mask for safety, the "HIGH" value is transmitted which is received by the receiver and the relay establish the connection and vehicle is started when self-start button is pressed.
The main purpose of sending SMS is to make the rider aware to wear helmet
Hardware setup
To know about the IR sensor working and connections you can follow this link also IR sensor with Arduino also I have attached the Fritzing breadboard diagram and my circuit also:
Software setup
The Arduino Code linked in below continuously detects the value from theIR sensor and sends it to the Bolt Wi-Fi module over serial communication.
A Python script running on a server or your PC queries the Bolt Cloud for the value using the Bolt Python Library, which in turn is based on the Bolt open APIs for Serial Read.
The Python script then checks if the value is "HIGH or LOW" if the IR sensor gives value 1 i.e. object in range so it sends SMS accordingly using the Twillio SMS service.
The Python script then checks if the value is "HIGH or LOW" if the IR sensor gives value 1 i.e. object in range so it sends SMS accordingly using the Twillio SMS service.
Code
import json, time, requests
from boltiot import Sms, Bolt

api_key = "Your api key"                                #Add your API key 
device_id  = "Your device ID"                           #Add your device ID
mybolt = Bolt(api_key, device_id)                       #Module ready

SID = 'Your SID'                                      #From your twilio dashboard
AUTH_TOKEN = 'Your authentication token'              #From your twilio dashboard
FROM_NUMBER = 'Phone number'                          #Twilio number
TO_NUMBER = 'Phone number'                            #To the number you want to send SMS

mybolt = Bolt(api_key, device_id)
sms = Sms(SID, AUTH_TOKEN, TO_NUMBER, FROM_NUMBER)
response = mybolt.serialRead('10')                  #Take response from arduino
while True:
  print ("\nReading")                               #reading value form arduino
  response = mybolt.serialRead('10')
  data = json.loads(response)
  val=data['value'].rstrip()                        #readings from arduino converted to string type
  time.sleep(5)
  
  #If arduino gives value 0 that is it sensor is in connected with object that means, person is wearing helmet we need to perform our task
  if val=="0":
    print("Wearing helmet\n")
    try:
      print("Making request to Twilio to send a SMS")
      response = sms.send_sms("It seems like you started your vehicle, do wear a helmet and mask. Have a safe ride!")
      print("Response received from Twilio is: " + str(response))
      time.sleep(600)
      print("Status of SMS at Twilio is :" + str(response.status) + "\n")
    except Exception as e:
      print ("Error occured: Below are the details")
      print (e)
      print("\n")
      time.sleep(5)
  else:
    None
Conclusion
The present situation in our country we are not using this type of two wheeler and this technology. To reduce the manual efforts and human errors, we need to have some kind of automated system monitoring all the parameters and functioning of the connections between the two wheeler personnel and the parents.
References:
http: www.en.Wikipedia.org
http:www.google.co.in
https://www.Arduino.cc






       








