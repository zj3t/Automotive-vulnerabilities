# Renault Group - ZOE Bug Report

•	Time and date of discovery
12.28.2022 (Korea Standard Time)
•	Product Model & number
2021, Renault ZOE EV
•	Technical Description
When you connect a USB storage device containing manipulated media files (MP3, WMA, OGG …) in a Renault ZOE EV(2021) vehicle, an error occurs during the process of loading the media files, causing the infotainment system to restart.
The cause of the issue that we analyzed is as follows:
MP3 files use a metadata format called ID3 in the header. Information related to the music file, such as the title of the music, the name of the musician, genre, etc. is included in the header. ID3 uses ID3v1 and ID3v2 versions.
The format of these ID3 tags starts with Tag Identifier (3byte) | Tag Version (2bytes) | Flags (1byte) | Size of Tags (4byte), followed by actual tag information in the form of variable fields.
The variable fields consist of Frame Identifier (4byte) | Frame Size (4byte) | Flags (2byte) | Data.
In the case of inputting ASCII data such as \xb9\xb9 outside the Unicode range in the Flags (2byte) area of the variable field. 
In addition to the flag (which consists of 2 bytes), the next 1 byte of data must also be modulated. This results in a total of 3 bytes being modulated to generate a crash: 2 bytes for the flag and 1 byte for the data. (e,g., 00 00 00  e1 a0 8e)
It is expected that unexpected error will occur during the parsing process in the Renault ZOE EV(2021) vehicle media player, resulting in a crash.
 
•	Sample Code
Our vulnerability is not something that operates through code, but manipulated media files.
I will attach these media files.
•	Reporting’s party Contact Information 
For review and inquiry. please contact us through the following contact information.
etjang@autocrypt.io
dhjeong@autocrypt.io
Thank you. 
•	Disclosure Plan(s)
Without your specific mention, it is likely to be used in the DEFCON presentation material.
•	Threat/Risk Assessment
CVSS 3.0 Score (4.6)
Since our vulnerability connects to the vehicle via a USB device, it is physical and the attack is not complex.
Just plugging a USB with malicious MP3 media files into the vehicle.
Additionally, the attack does not require permission and does not require user interaction(UI) controls.
Finally, we assigned the highest score to Availability, as the attack does not violate confidentiality or integrity,
This vulnerability completely shuts down and reboots the vehicle infotainment system.
 
 
•	Software Configuration
Software version. 283C35202R
Lasted Update. 11.10.2021
 

•	Relevant information about connected devices 
We have stored media files through a USB Storage device and connected it to the front USB connection of the Renault ZOE EV(2021) vehicles.
•	DEMO
We accept the form for this report via email. The email will contain attached sample files and a demo video.
![image](https://github.com/zj3t/Automotive-vulnerabilities/assets/35731091/496bf65f-fbdd-453a-a866-2039ac9884f6)
