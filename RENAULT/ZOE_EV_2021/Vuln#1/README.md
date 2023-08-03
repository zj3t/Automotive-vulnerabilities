# ZOE EV (2021) Infotainment System Vulnerability Report #1

I reported a vulnerability to "https://www.renaultgroup.com/en/vulnerability-disclosure-policy"

## Time and date of discovery
2022.12.28 (Korea Standard Time) - Date I reported the vulnerability to Renault.

## Target
### Product Model

ZOE EV 2021

![image](https://github.com/zj3t/Automotive-vulnerabilities/assets/35731091/6fbbada2-025f-40c0-86b1-1796b41d24d8)

### Version

It was the latest version as of November 10, 2023.

![image](https://github.com/zj3t/Automotive-vulnerabilities/assets/35731091/7bad2676-7777-4ba0-b8a3-3fffe8424cd9)



## Technical Description

![image](https://github.com/zj3t/Automotive-vulnerabilities/assets/35731091/19501a4a-d42b-4af7-80aa-df7e4df46f96)

When you connect a USB storage device containing manipulated media files (MP3, WMA, OGG …) in a Renault ZOE EV(2021) vehicle, an error occurs during the process of loading the media files, causing the infotainment system to restart.

The cause of the issue that we analyzed is as follows:
MP3 files use a metadata format called ID3 in the header. Information related to the music file, such as the title of the music, the name of the musician, genre, etc. is included in the header. ID3 uses ID3v1 and ID3v2 versions.

The format of these ID3 tags starts with Tag Identifier (3byte) | Tag Version (2bytes) | Flags (1byte) | Size of Tags (4byte), followed by actual tag information in the form of variable fields.

The variable fields consist of Frame Identifier (4byte) | Frame Size (4byte) | Flags (2byte) | Data.

In the case of inputting ASCII data such as \xb9\xb9 outside the Unicode range in the Flags (2byte) area of the variable field. 

In addition to the flag (which consists of 2 bytes), the next 1 byte of data must also be modulated. This results in a total of 3 bytes being modulated to generate a crash: 2 bytes for the flag and 1 byte for the data. (e,g., 00 00 00 -> e1 a0 8e)

It is expected that unexpected error will occur during the parsing process in the Renault ZOE EV(2021) vehicle media player, resulting in a crash.

![image](https://github.com/zj3t/Automotive-vulnerabilities/assets/35731091/fe9fcb24-0a48-4dbe-8b5d-2d2f88e47501)



## Result
Renault's infotainment system has a USB Plug and Play feature, which means that media files stored on a USB drive will automatically play when inserted into the system.
I identified a media file through fuzzing that could trigger vulnerabilities in the infotainment system, and proved this by using a USB stick. 

### DEMO
The file size is too large. Replace with url. (https://github.com/zj3t/Automotive-vulnerabilities/tree/main/RENAULT/ZOE_EV_2021/Vuln%231/Demo)

![image](https://github.com/zj3t/Automotive-vulnerabilities/assets/35731091/cdaf431e-b9ee-46ff-8443-bac6a1e88062)



## Impact

When a USB is inserted into the port, the media file is automatically played and the Infotainment System is forcibly terminated. This can be a problem with availability. 
Furthermore, if the crash is caused by a memory-related bug (such as Overflow, OOB, Over Read/Write), it can lead to serious security issues such as Remote Code Execution. Therefore, if you can analyze the crash of the media player, you may be able to identify the cause of the vulnerability.


