# SimpleSerial
This is a lightweight library that handles serial communication between any arduinos or raspberry pis.

# How to Wire
To use this library between two arduinos, first connect the ground wires. Then attach your wire to any digital input pin on the slave device, and to any pwm pin on your master (The pwm pins are marked with a tilda). It's as simple as that!

# How to Use the Library
To use this library, first drag the file to your arduino libraries folder. In the beginning of your program, type, #include <Serial.h>.
Then declare a global serialPin structure at the top of your program (serialPin nameofserialPin;). In the setup loop of your master device, initialize the serialPin with the function beginPin(). The function takes 5 parameters, a pointer to your declared serialPin, the minimum values you would like to send, the maximum value you would like to send, the pin number, and whether the pin is an input ('I') or an output ('0'). Ex: beginPin(&nameofserialPin, -90, 90, 10, 'O'). For your slave device, declare a similar funcition, but instead of 'O', write 'I'. To send a value accross the bus from the master, use the function writePin(), where the parameters are a pointer to the serialPin struct, and an integer value you would like to send. Ex: writePin(&nameofserialPin, 35); On your slave device write serialDecode(), where the only parameter is the pointer to the data structure, and it returns an integer value. 

# How the Library Works
This library works by sending pwm pulse from the master to the slave. The master encodes the value that is being sent where it maps the integer range that you are sending to 0 to 255; The slave decodes this by measuring the pwm pulse and converting your data back to the range that you specified. 

# Possible Changes
In your program, there are two values that you may need to change to get this to work. One is analogAccuracy, which is the resolution of your pwm output (On the raspberry pi and most arduinos, it is 255). You may also need to change analogReadAccuracy, which is the total time in microseconds between each pwm pulse. This is used to by the slave to decode this data. I have found that 1260 works well if your slave is an arduino. 

# Accuracy of this Library
As this library is extremely simple, the accuracy of your data that you are sending is mediocre. It is very consistent with what it recieves, but the recieved data may be 1 number off. 
