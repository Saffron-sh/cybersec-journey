# Windows Basics For Saffron:

## Windows CLI Basics:
The command prompt, often called `cmd`, is a text based interface where in stead of clicking icons, you enter text based commands telling the computer exactly what you want to do. Quite like our linux TTY and emulators but in a different language.

**Basic Commands**:
`cd`: Navigation & shows you where you are
`dir`: List contents of the current directory
`dir \a`: Show hidden files like `ls -a`
`dir \s <file>`: Searches for the given file in all folders and subfolders from `.`
`type`: Same as unix `cat`
`whoami`: Similar, but here it also gives you the hostname
`hostname`: Gives you just the hostname
`systeminfo`: Kinda like `uname` but way too verbose
`ipconfig`: Quite like our legacy `ifconfig`

### LSASS
Local Security Authority Subsystem Service (lsass.exe)  
Is responsible for password based authentication and storing passwords in windows based system(s). Meterpreter hashdump is something that prints this info, but can only do so if the process which meterpreter is running on has the required privileges to access lsass.exe. One thing which you can do is migrate your meterpreter session to the PID of the lsass.exe, that should be fairly easy, but then don't ask me why there are 256 armed federal agents outside your house. So, try migrating to a process less monitored.
