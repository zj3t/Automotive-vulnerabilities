# Volkswagen Jetta (2021) Infotainment System Vulnerability Report

I reported a vulnerability to "https://www.volkswagen.de/de/mehr/rechtliches/kontakt-cyber-security.html"

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

Discover Media(infotainment system of VW) software : 0876 </br>
Media codec : 1.2.0

## Technical Description

![image](https://user-images.githubusercontent.com/35731091/229762099-36991d9d-1487-41ae-b9d9-b15e1065be14.png)

A vulnerability exists in Volkswagen's Infotainment System(Discover Media).
I attempted media file fuzzing to find vulnerabilities in Volkswagen's infotainment system.

To automate the fuzzing process(Because transferring files to a USB stick is time consuming), I connected my laptop to Volkswagen's USB port and generated numerous media files with a fuzzer. I then continuously performed real-time media fuzzing by mounting and unmounting the files.
I conducted fuzzing on various types of media files such as WAV, MP3, WMA, and OGG, and discovered that the vulnerability existed in a malicious (mutated) OGG file.
Since Volkswagen's media player was more robust than expected, I created a separate media file fuzzer specifically for Volkswagen's infotainment system.
I fuzzed more than 20,000 media files per day, and discovered the vulnerability in the OGG file after one day of fuzzing.

## Result
Volkswagen's infotainment system has a USB Plug and Play feature, which means that media files stored on a USB drive will automatically play when inserted into the system.
I identified a media file through fuzzing that could trigger vulnerabilities in the infotainment system, and proved this by using a USB stick. 
As a result, Volkswagen's infotainment system did not turn on again after being turned off. 

**The issue persisted even after turning off and on the engine(Even removing the USB drive did not resolve the issue with the infotainment system not turning on again.), and manual reboot of the infotainment system was required to resolve the issue.**

### DEMO #1
<img width="80%" src="https://user-images.githubusercontent.com/35731091/229770054-6dea74c0-08d7-40e9-bec4-037a053eb421.mp4"/>

### DEMO #2
<img width="80%" src="https://user-images.githubusercontent.com/35731091/229767110-531f5a83-3133-4d06-8bde-09e6246434ee.mp4"/>

## Impact

When a USB is inserted into the port, the media file is automatically played and the Infotainment System is forcibly terminated. This can be a problem with availability. 
Furthermore, if the crash is caused by a memory-related bug (such as Overflow, OOB, Over Read/Write), it can lead to serious security issues such as Remote Code Execution. Therefore, if you can analyze the crash of the media player, you may be able to identify the cause of the vulnerability.

## Volkswagen's response
Volkswagen acknowledged the report as a vulnerability

![image](https://user-images.githubusercontent.com/35731091/229770832-46b0aed6-74e0-4616-a7a9-0fed197bdff1.png)

