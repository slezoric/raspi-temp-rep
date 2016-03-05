# Check up

###To check up the DHT11 sensor.
In the terminal window:

	$cd 
	$cd ubidots/
	$sudo python ubi-test.py

The output will be as follows:

	Last valid input: 2016-03-02 21:20:12.238259
	Temperature: 30.8 C
	Humidity:    71.3 %

And it will continue to run as it is a infinte loop.

To break from the infinite loop just press `cntrl + c`

Also verify whether the value has been update in the [ubidots](http://ubidots.com) account.
The data present there should show values being updates as per the timing mentioned in the program.
