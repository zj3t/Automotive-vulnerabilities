# Chevrolet Equinox Media Player's DoS vulnerability
## Summary
I made an effort to find vulnerabilities in the infotainment system of a 2021 Chevrolet Equinox vehicle.
I created a testcase for Chevrolet Equinox's media player and performed fuzzing.
The Chevrolet Equinox's system was up to date and was built on 2021.03.26.

![image](https://user-images.githubusercontent.com/35731091/227777360-f2f6a3e9-c573-46af-9f70-844834422674.png)
![image](https://user-images.githubusercontent.com/35731091/227777373-d389877f-a26d-4fd4-b0f6-973039f53e42.png)

Since the Chevrolet Equinox's infotainment system was based on Android, it was difficult to play malicious media files created by the fuzzer (most media files are not recognized by the system).

Therefore, I developed a bit-flipping fuzzer and efficiently fuzzed the media player by minimizing file damage.
As a result, the Chevrolet Equinox's media player crashed.

### DEMO #1
<img width="80%" src="https://user-images.githubusercontent.com/35731091/227781653-8701a1a0-989d-485d-b48f-38b0ec37128a.mp4"/>


### DEMO #2
<img width="80%" src="https://user-images.githubusercontent.com/35731091/227781670-6e1ca2a1-7239-4bfc-9ac4-3bb556e978ec.mp4"/>

It seemed difficult to use the media player without removing the USB.

## Impact
When a USB is inserted into the port, the media file is automatically played and the Chevrolet Equinox's media player is forcibly terminated. This can be a problem with availability. Furthermore, if the crash is caused by a memory-related bug (such as Overflow, OOB, Over Read/Write), it can lead to serious security issues such as Remote Code Execution. Therefore, if you can analyze the controller of Chevrolet Equinox and dump the crash of the media player, you may be able to identify the cause of the vulnerability.

## Response to GM

1. On February 22, 2023, I reported a vulnerability in the media player to GM.
2. The response from GM was as follows.
![image](https://user-images.githubusercontent.com/35731091/227782051-9045eaab-9094-451b-b4d6-3ca0fbe0f2e1.png)
