# Infotainment System of Volkswagen Jetta(2021) Vulnerability

## Time and date of discovery
2023.02.28 (Korea Standard Time)

## Target
### Product Model

Volkswagen Jetta 2021

![image](https://user-images.githubusercontent.com/35731091/229760465-de19eacf-6f1c-499b-affd-fdefdc7a43b4.png)

### Version

It was the latest version as of February 28, 2023.

![image](https://user-images.githubusercontent.com/35731091/229760756-310af850-921a-4dbf-8da1-9e9c13bd9506.png)
![image](https://user-images.githubusercontent.com/35731091/229760781-ca76aa93-0cdc-47b6-a4fe-5dbcbd9d36db.png)

Discover Media(infotainment system of VW) software : 0876 
Media codec : 1.2.0

### Technical Description

![image](https://user-images.githubusercontent.com/35731091/229762099-36991d9d-1487-41ae-b9d9-b15e1065be14.png)

A vulnerability exists in Volkswagen's Infotainment System(Discover Media).
I attempted media file fuzzing to find vulnerabilities in Volkswagen's infotainment system.

To automate the fuzzing process(Because transferring files to a USB stick is time consuming), I connected my laptop to Volkswagen's USB port and generated numerous media files with a fuzzer. I then continuously performed real-time media fuzzing by mounting and unmounting the files.
I conducted fuzzing on various types of media files such as WAV, MP3, WMA, and OGG, and discovered that the vulnerability existed in a malicious (mutated) OGG file.
Since Volkswagen's media player was more robust than expected, I created a separate media file fuzzer specifically for Volkswagen's infotainment system.
I fuzzed more than 20,000 media files per day, and discovered the vulnerability in the OGG file after one day of fuzzing.

### Result
Volkswagen's infotainment system has a USB Plug and Play feature, which means that media files stored on a USB drive will automatically play when inserted into the system.
I identified a media file through fuzzing that could trigger vulnerabilities in the infotainment system, and proved this by using a USB stick. 
As a result, Volkswagen's infotainment system did not turn on again after being turned off. 

<u> The issue persisted even after turning off and on the engine(Even removing the USB drive did not resolve the issue with the infotainment system not turning on again.), and manual reboot of the infotainment system was required to resolve the issue.</u>  
