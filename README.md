# Check Connected Devices
Check connected devices to your local network and send SMS to your phone via Twilio when a new one is detected

v0.1 - Tested in Kali Linux 2020

A simple bash script that works as follow:
- Checks the local network using nmap
- Parses the nmap output and creates a list of all the connected devices in the format <DeviceName,MAC>
- Compares the list with the device list that you have created (see below) and if there is a difference sends you an SMS via Twilio

The goal of this repo is to play around with the notion of sending alerts when changes like these occur to your local network.

Also the current code makes some assumptions that can be easily changed:
- Local network is at 192.168.1.0/24
- We do NOT wish to be able to reply to the SMS to trigger changes, to avoid any open ports on our end. This of course can be changed, but this is the approach of the script here.

Here are some instructions on how to run the program:
1. Add your devices to a device list file. For example:
```bash
myrouter,AA:BB:CC:DD:EE:FF
myIPHONE,AA:BB:CC:DD:EE:FF
```
To do the above you can simply run once nmap, similarly to how it is ran in the script
```bash
sudo nmap -sn <local_network>
```
2. Get the path of the device file and add it in the script replacing the DEVICE_LIST parameter

3. Add your Twilio parameters as requested to send the SMS. You need:
- Account Sid and Auth Token
- Twilio Number and Your Personal Number  
ALTERNATIVE: You can also install the twilio-cli, a very cool command line interface for all the twilio APIs, and replace the curl command in the script with the relevant twilio command
https://www.twilio.com/docs/twilio-cli/quickstart

You can add these directly or as env variables, for example on /etc/environment, but this will also depend on your distro, so this is left to you to add

4. Realistically, you would not like to run manually the script, but rather through a cron job. You can create/edit a crontab and add the relevant required params
```bash
sudo crontab -e
```
and then add something like
```bash
* * * * * /path/to/executable
```
The above is to run this every minute. If you want to check how to change this, just google about setting crontab params

[![New Devices SMS](http://i.imgur.com/LObdvz9.png)]()

# TODO
- Include cmd line params for local network IP and Twilio credentials
- Automate cron creation
