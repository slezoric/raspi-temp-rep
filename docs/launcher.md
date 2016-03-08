#Making the python script run at boot
Now to convert the pi as a stand alone embedded system i.e it will do the desired task if powered on.

This is achieved using the `crontab` function of the linux kernel and a shell script.

1. now go to the directory which contains the target files.

		$cd /ubidots

2. Create a pyhton script called `new.py` with the contents as

		import time
		time.sleep(10)
To induce a 10 sec delay of runnnig the ubi-test.py at boot up so all the required protocols and drivers will be up and running before the code is executed.

3. Create a new file called `launcher.sh` . And input the following data

		$cd /
		$cd /home/pi/ubidots
		$sudo python new.py
		$sudo python ubi-test.py &
		$cd /
		
4. Now make the `launcher.sh` as an executable file by

		$sudo chmod 755 launcher.sh
		
5. Now try running the `launcher .sh`

		$sudo sh launcher.sh
which will result in the script in running after  a time interwal of 10 secs.
  
6. Now edit the `crontab` using
		$sudo crontab -e
and add these lines at the bottom
		
		@reboot sh /home/pi/ubidots/launcher.sh > /home/pi/ubidots/logs/cronlog 2>&1
It will look as follows
![crontab-e](img/crontab-e.jpg)

7. now save this and reboot the system using the command
		
		$sudo reboot

8. Now the raspberry will reboot and the code will run as a background service.This need to be verified in two ways.
	1. Firstly use the following command to check wether the program is running or not.
			
			$ps aux | grep /home/pi/ubidots/ubi-test.py
		
	2. Secondly verify wether the value has beeen uploaded to the online [cloud](http://ubidots.com).
