# Wireless Networks and Security

## Introduction to Python, Scapy and MAC Security

__Please work in teams of 3 students__

__Korean/Swiss hybrid teams are highly encouraged__

### For this first part, you will need to:

*  Learn the basics of WiFI sniffing using Scapy
*	Detect if a particular WiFi client can be found in the premises
*	Gather a list of SSIDs announced by the WiFi clients in proximity

You will need to search the Internet in order to learn details about Scapy and the aircrack-ng suite of tools. __It is highly recommended to use a Kali Linux Distribution. If you use a VM, you will need a WiFi USB adapter. Please see the instructor for details.__

You will find very good information about Scapy [here](https://scapy.readthedocs.io/en/latest/usage.html).

### Just Some Info to get you Started

Basic sniffing with Scapy is very easy:

```python
sniff(iface="wlan0mon", prn=handler_function)
```

This will start sniffing on interface wlan0mon (it needs to be in monitor mode!!!). Every time a frame is sniffed, it will call function ```handler_function``` and pass the frame as an argument:

```
def handler_function(packet):
	do_some_stuff_with_your_packet
```

Reading a pcap file from Scapy is also pretty easy:

```
a = rdpcap(pcap_filename)
```

will read ```pcap_filename``` contents into variable ```a```.


Writing a pcap file is also easy:

```
wrpcap(filename, a)
```

will write the contents of variable ```a``` to file ```filename```.


### Configuring Monitor Mode

For most of your work, you will need to configure your WiFi interface in monitor mode.

If you are using Kali Linux, most interfaces can be configured with just a single command.

To configure a __Alfa AWUS036H, AWUS036NH, AWUS051NH (look at the back of your Alfa) and most probably, the interface of your own laptop (if you're working with a native Linux Install__ into monitor mode, you will need to use the following command (please verify using ```ifconfig``` that your interface is called ```wlan0```. Otherwise, simply use the right name):

```bash
sudo airmon-ng start wlan0
```

You should now have a new interface called ```wlan0mon``` working in monitor mode.

If you are using an __Alfa AWUS036ACH__ (the black cool looking interfaces with two antennae), __you must activate USB 3.0 compatibility for your VM__. For every other interface, you will need to use USB 2.0.

When using those __Alfa AWUS036ACH__ interfaces, things get a little more complex when configuring monitor mode. __WARNING, only use the following commands if your are using the AWUS036ACH interfaces__ :

### Install The Driver (available with Kali. For other distributions, you may probably need to compile from sources) :

```bash
sudo apt-get install realtek-rtl88xxau-dkms
```

Now, to go to monitor mode:

### Put your interface “down”

```bash
sudo ip link set wlan0 down
```

### Change mode to montor

```bash
sudo iwconfig wlan0 mode monitor
```

### In case you need to compile de driver (you don't need this if you use Kali and/or you were able to install the realtek driver using your paquet manager:

```bash
git clone https://github.com/astsam/rtl8812au.git
cd rtl8812au
make
sudo make install
```

### Avoiding channel hopping

__WARNING:__ For most of your work, it might be important to avoid channel hopping (i.e., fix your sniffing and/or injecting channel). This is the safest option to do it. Open a terminal and open ```airodump-ng``` with the ```--channel``` option:

```airodump_ng --channel channel_number name_of_interface_in_monitor_mode```

Keep that terminal window open and ```airodump-ng``` running while your own scripts execute.


## Some important hints (come back to them often... you will probably need them):


- If you need to snif or inject trafic, your 802.11 interface needs to be configured into monitor mode
- Python has an interactive prompt, very useful for development. Simply invoque it with the ```python``` command from your terminal. From there, you can import any module (including scapy) and run any command. Actually, you may execute your whole script from the interactive prompt!
- Scapy also features an interactive prompt. You invoque it by running the ```scapy``` command from your terminal
- In interactive mode, "variable_name + \<enter\>" will return the contents of the variable
- You may visualise the details of a frame in interactive Scapy mode by using the ```show()``` method. For example, if you have a variable called ```arp``` that contains a frame, you can see all the fields and values they contain by using this command: ```arp.show()```. 


## Your Work

### 1. Detect if one specific 802.11 client is present in the premises

It may be useful to detect if a certain user is close to the premises. Think, for example, about a fire in a building. We might gather a list of devices and contrast them with the ones that have already left.

Client detection can also be used for marketing research. In the USA, for example, the ailes of shopping malls are sniffed in order to detect which showcases attract most visitors and which brand of phone they use. This service is usually interconnected with other shopping malls so that a wider tracking of shoppers habits can be observed.

The police may also use this method to follow a suspect.


__ATTENTION__: Tracking an iPhone is no longer possible in most conditions since version 8 of iOS.

* Develop a script in Python/Scapy capable of sniffing the frames necessary for detecting 802.11 clients. The script runs in command line and takes a MAC address as argument. It will run and lets you know when the client (MAC address) has been detected.

### 2. WiFi Clients Talk Too Much (optional... if you're fast enough!!!)

a) Using the script you just wrote as a basis, modify it in order to capture the names of the networks announced by different clients being detected in the vicinity of your scanner.


You may show the network names (SSID) with the corresponding MAC adresses as they are captured. But you will need to keep track of which names correspond to which clients!

b) Use an online ressource to automatically determine the WiFi interface vendor for each detected MAC address. Show this information with every line you print.

So, every time your scanner prints results, they will look like this:

```
00:1B:63:21:10:33 (Apple Inc.) – HEIG-VD, SU19, Seoul, MyWiFi
00:09:18:10:23:01 (Samsung) – HEIG-VD, Marathon, europa, eduroam
```


## Questions

__Q1__ : What type of frames are used to detect clients in a passive way?

__Q2__ : why is it no longer possible to detect iPhones since version 8 of iOS?

__Q3__ : what is the purpose of those frames?

__Q4__ : why are these frames not secured?

__Q5__ : what other information can you obtain besides the MAC address?



## Deliverables

Fork of the original repo. Then, a Pull Request containing :

- The names of the students. You can add this to the ```README.md```

- Script for detection of 802.11 targets __extensively commented/documented__

- Script for detection and printing of SSID __extensively commented/documented__

- Answers to the questions. You may answer directly on the ```README.md```
