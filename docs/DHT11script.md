#The Python Script

The following is the code that was used to upload the temperature and sensor data.
It uses the `dht11.py` extensive as teh support file which needs to be downloaded from [here](http://google.com)<br>

Now the following is the code used to push the data from DHT11 to ubidots.

	from ubidots import ApiClient
	import random
	import RPi.GPIO as GPIO
	import dht11
	import time
	import datetime

	#Create an "API" object

	api = ApiClient("18519feca06c452098f1cf2aae432f58ae088c37")

	#Create a "Variable" object

	temp = api.get_variable("56ba09f1762542160640c29a")
	humi = api.get_variable("56ba136d7625426c9967d2b5")
	#Here is where you usually put the code to capture the data, either through your GPIO pins or as a calculation. We'll simply put a random value here:

	GPIO.setwarnings(False)
	GPIO.setmode(GPIO.BCM)
	GPIO.cleanup()

	# read data using pin 4
	instance = dht11.DHT11(pin = 4)

	while True:
		result = instance.read()
		if result.is_valid():
			print("Last valid input: " + str(datetime.datetime.now()))
			print("Temperature: %d C" % result.temperature)
			print("Humidity: %d %%" % result.humidity)
			temperature = result.temperature
			humidity = result.humidity
			#Write the value to your variable in Ubidots
			temp.save_value({'value':temperature})
			humi.save_value({'value':humidity})
		time.sleep(5)

Now this is saved as the file `ubi-test.py`.
Make sure that the `dht11.py` is also in the same folder as the `ubi-test.py`.

Now to check wether the sensor is properly interfaced with the code try running it.

	$sudo python ubi-test.py

Which should give the output as

	Last valid input: 2016-03-02 21:20:12.238259
	Temperature: 30.8 C
	Humidity:    71.3 %		

This should cycle in an infinite loop until the process is killed.This automatically pushes the data to ubidots.


