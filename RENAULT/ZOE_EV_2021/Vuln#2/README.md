# ZOE EV (2021) Infotainment System Vulnerability Report #2

I reported a vulnerability to "https://www.renaultgroup.com/en/vulnerability-disclosure-policy"

## Time and date of discovery
2023.02.23 (Korea Standard Time) - Date I reported the vulnerability to Renault.

## Target
### Product Model

ZOE EV 2021

![image](https://github.com/zj3t/Automotive-vulnerabilities/assets/35731091/6fbbada2-025f-40c0-86b1-1796b41d24d8)

### Version

It was the latest version as of January 16, 2023.
SW version : 283C35519R

![image](https://github.com/zj3t/Automotive-vulnerabilities/assets/35731091/7d0f793d-032c-4379-938d-e071b546e4e8)

## Technical Description

![image](https://user-images.githubusercontent.com/35731091/229762099-36991d9d-1487-41ae-b9d9-b15e1065be14.png)

A vulnerability exists in Renault's Infotainment System(Discover Media). I attempted media file fuzzing to find vulnerabilities in Renault's infotainment system.

To automate the fuzzing process(Because transferring files to a USB stick is time consuming), I connected my RPI4 to Renault's USB port and generated numerous media files with a fuzzer. I then continuously performed real-time media fuzzing by mounting and unmounting the files. I conducted fuzzing on various types of media files such as WAV, MP3, WMA and discovered that the vulnerability existed in a malicious (mutated) WMA file.

If you create a malicious *.WMA file and play it on ZOE, it reboots.
The vulnerability can be reproduced by inserting the WMA media file into a USB device and connecting it to the vehicle.
The causes of the crash are as follows.
![image](https://github.com/zj3t/Automotive-vulnerabilities/assets/35731091/0df353dc-7ba5-47cf-9148-8c78fd9cade5)

In the WMA file,  Activation of the corresponding 0x14th byte causes a crash.

## Result
Renault's infotainment system has a USB Plug and Play feature, which means that media files stored on a USB drive will automatically play when inserted into the system.
I identified a media file through fuzzing that could trigger vulnerabilities in the infotainment system, and proved this by using a USB stick. 

### DEMO
The file size is too large. Replace with url. (https://github.com/zj3t/Automotive-vulnerabilities/tree/main/RENAULT/ZOE_EV_2021/Vuln%232/Demo)

![image](https://github.com/zj3t/Automotive-vulnerabilities/assets/35731091/8b972e52-0b0e-480c-b035-79e9355a3b7b)


## Impact

When a USB is inserted into the port, the media file is automatically played and the Infotainment System is forcibly terminated. This can be a problem with availability. 
Furthermore, if the crash is caused by a memory-related bug (such as Overflow, OOB, Over Read/Write), it can lead to serious security issues such as Remote Code Execution. Therefore, if you can analyze the crash of the media player, you may be able to identify the cause of the vulnerability.
