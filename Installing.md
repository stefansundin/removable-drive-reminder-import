# Installing #

I offer two kinds of downloads. An installer and an archive.


## Installer ##

You can use the installer to install to the system as well as installing directly to a USB stick.

The installer will request administrator privileges when it is run. I did this since otherwise you have to manually run it as administrator to install to the _Program Files_ directory. If you do not have administrator privileges then simply use the archive instead.


## Archive ##

The archive contains the same files as the installer.

You can use the archive to install to USB sticks as well. Extracting the files to the USB stick is exactly the same as using the installer.


## Mass deployment ##

The installer is capable of running silently. It is a normal NSIS installer, and as such it supports [common NSIS switches](http://nsis.sourceforge.net/Docs/Chapter3.html#3.2). Example:
```
"Removable Drive Reminder-0.1.exe" /S /D=C:\Program Files\Removable Drive Reminder
```

After installing you can copy over a custom ini file, custom sound, etc., and then finally run the program.

Another option is to simply copy the files. The only fancy things the installer really does is to create an uninstaller and a start menu shortcut.


## SCCM Script ##

You can use this bat file to install Removable Drive Reminder with SCCM.

```
xcopy "\\NetworkShare\Path\Removable Drive Reminder\*" "C:\Program Files (x86)\Removable Drive Reminder\" /I /Y
reg add HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /t REG_SZ /v "Removable Drive Reminder" /d "\"C:\Program Files (x86)\Removable Drive Reminder\Removable Drive Reminder.exe\" -hide" /f
del /F /Q "C:\Program Files (x86)\Removable Drive Reminder\install.bat"
```

Name it `install.bat` and put in the network share. Rename paths as appropriate.

# Running the program #

## USB stick ##

If you install to a USB stick, then you have to make sure that you manually launch the program after you plug in your USB stick. The program can not warn you if it isn't running!

## Lab computer ##

If you install the program to a public computer where you only have some control over the files (e.g. in your school's computer lab), then you probably don't have access over the computer's autorun location, so you can not make the program launch automatically when you log in.

  * One option is to make a simple shortcut on the desktop, but you will have to remember to double-click this shortcut when you log in.
  * An even better approach is to replace one of the shortcuts to a program you **always** run when you use that computer, and make it instead launch both the program and Removable Drive Reminder. This option is more forgiving than simply placing a shortcut on the desktop (which you'll forget to click).

My solution was to create a bat file called `firefox.bat`. You can hide away this file somewhere on your personal drive (in my case `H:\`).
```
start "" "H:\Removable Drive Reminder\Removable Drive Reminder.exe" -quiet
start "" "C:\Program Files\Mozilla Firefox\firefox.exe" %*
```

Then create a shortcut to `firefox.bat` and place it on your desktop. Right-click the shortcut and change the icon to Firefox's icon. Also set the `Run` parameter to `Minimized`. Finally, remove your old shortcut and give this shortcut the name `Firefox`.

Now launching Firefox through this shortcut will also launch Removable Drive Reminder.

## Command line switches ##

There are two command line switches you can give the program:
  * `-hide` - Hides the tray icon. Can be used if the program is already running to hide the tray icon.
  * `-quiet` - Prevents the configuration window from opening when run a second time.

Note that you can not use them both simultaneously.