This lab is focused on analyzing a potentially suspicious file using basic Linux investigation commands. I practiced verifying file types, checking metadata and timestamps, extracting readable strings, and identifying recent file activity. These skills help SOC analysts safely inspect unknown files without executing anything that could be harmful.

Terminal Entry 1 — Confirm Location and List Directory
Command You Typed  
pwd && ls -la

Terminal Output

/home/Tyree-academy
total 20
drwxr-xr-x  2 Tyree-academy Tyree-academy   75 Sep  5 20:04 .
drwxr-xr-x  3 root          root            55 Sep  5 20:04 ..
-rw-r--r--  1 Tyree-academy Tyree-academy  220 Sep  5 20:04 .bash_logout
-rw-r--r--  1 Tyree-academy Tyree-academy 3771 Sep  5 20:04 .bashrc
-rw-r--r--  1 Tyree-academy Tyree-academy  807 Sep  5 20:04 .profile
-rw-r--r--  1 root          root          4510 Sep 11 05:01 README.txt
Analyst Observation  
The output shows I am in my home directory /home/Tyree-academy, which contains standard configuration files and a readable README.txt. None of the files appear suspicious, and README.txt is safe to inspect because it is a plain text file without execute permissions.

Terminal Entry 2 — Determine True File Type
Command You Typed  
file README.txt

Terminal Output

README.txt: ASCII text, with escape sequences
Analyst Observation  
The file command confirms that README.txt is an ASCII text file, matching its filename and extension. Analysts verify file type first because filenames can be misleading, and confirming the true type prevents accidental execution of malicious files.

Terminal Entry 3 — Examine File Metadata and Timestamps
Command You Typed  
stat README.txt

Terminal Output

  File: README.txt
  Size: 4510       Blocks: 16      IO Block: 4096   regular file
Device: 103h/66305d   Inode: 222490101   Links: 1
Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
Access: 2026-09-05 20:04:09.297437022 +0000
Modify: 2026-09-11 16:32:44.271653579 +0000
Change: 2026-09-11 16:32:44.271653579 +0000
 Birth: 2026-09-05 20:04:09.297437022 +0000
Analyst Observation  
The Access, Modify, Change, and Birth timestamps show when the file was opened, edited, metadata‑updated, and created. Modify and Change times match, meaning the file’s contents and metadata were updated at the same moment. Differences between these values can indicate metadata manipulation, but timestamps alone cannot prove malicious activity because normal system processes also update metadata.

Terminal Entry 4 — Extract Readable Strings
Command You Typed  
strings README.txt | head -20

Terminal Output

[1mWelcome to the CyLab Security Academy webshell!
[1mUse the arrow keys or spacebar to scroll, or type
[1;30mq
[1;37m to exit.
This is a browser-accessible Linux shell that can be used for solving CyLab Security Academy challenges.
Note that using the webshell is not necessary for solving challenges.
All files and programs are either available for download or accessible via remote ports. It is intended primarily for users who do not have access to their own local shell environment, such as students using school-provided hardware.
The webshell has many common tools for solving CTF challenges available.
Due to restrictions in the webshell, it is generally not possible to install additional software. However, if there is a specific tool that you feel would be helpful to include, send a note on Discord or to support@cylabacademy.org and we will consider adding it.
There are various restrictions in place to prevent abuse of the webshell.
Some resource limits can be checked by typing
[0;36musage
[0m. Note that all
Analyst Observation  
The strings output shows readable documentation text describing the CyLab webshell environment. This confirms the file contains informational content rather than executable logic. Strings analysis is useful because it reveals human‑readable clues about a file’s purpose without executing it.

Terminal Entry 5 — Search for Recently Modified Files
Command You Typed  
find . -mtime -1 -type f

Terminal Output
./README.txt
Analyst Observation  
The command shows that README.txt was modified within the last day, which is expected for a file included in the webshell environment. Time‑based searches help analysts identify recent activity that may be related to an incident. If unexpected files appeared here, I would investigate their timestamps, permissions, and contents.

Key Takeaway
This lab reinforced safe file‑analysis techniques that SOC analysts use to inspect unknown files without executing them. Verifying file type, checking timestamps, and extracting readable strings are essential steps in early triage during an investigation.
