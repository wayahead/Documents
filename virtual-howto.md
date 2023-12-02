# Virtualbox Howto

## Disable Hyper-V on Windows

* [Reference](https://forums.virtualbox.org/viewtopic.php?f=25&t=99390)

```
1. To turn off the Windows Memory Integrity security feature, on the Windows host navigate to Start > Settings > Update & Security > Windows Security > Device security > Core isolation > Memory integrity. Alternatively, you can disable VBS completely in the Group Policy Editor under Computer Configuration > Administrative Templates > System > Device Guard > Turn On Virtualization Based Security.

2. Find the Command Prompt icon, right click it and choose Run As Administrator.

3. Enter this command:

`bcdedit /set hypervisorlaunchtype off`

4. Some report this command was needed also:

`DISM /Online /Disable-Feature:Microsoft-Hyper-V`

5. Enter this command:

`shutdown -s -t 2`

6. When the computer turns off, unplug it for 20 seconds. Then plug it in again and boot up again. (Some laptops have built-in unremovable batteries. These devices may have a BIOS switch that can "deactivate" the battery, which could cause the unplug needed for this step.)

Your Virtualbox should be running now. If the green turtle still appears and the tell-tale lines are in the log, try all the steps again. If you don't get the standard virtualization icon, post back exactly what you did and we'll try to help some more.
```
