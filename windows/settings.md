# Windows basic setting like mac

### invert mouse wheel
> this should be done on 'powershell under administrator mode'

PS C:\WINDOWS\system32> $mode = Read-host "How do you like your mouse scroll (0 or 1)?"; Get-PnpDevice -Class Mouse -PresentOnly -Status OK | ForEach-Object { "$($_.Name): $($_.DeviceID)"; Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Enum\$($_.DeviceID)\Device Parameters" -Name FlipFlopWheel -Value $mode; "+--- Value of FlipFlopWheel is set to " + (Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Enum\$($_.DeviceID)\Device Parameters").FlipFlopWheel + "`n" }
How do you like your mouse scroll (0 or 1)?: 1
HID-compliant mouse: HID\DELL099F&COL01\5&22265D5&0&0000
+--- Value of FlipFlopWheel is set to 1

HID-compliant mouse: HID\VID_046D&PID_C534&MI_01&COL01\7&63C42C0&0&0000
+--- Value of FlipFlopWheel is set to 1

PS C:\WINDOWS\system32>


### Alternative PowerShell CLI
reference: https://answers.microsoft.com/en-us/windows/forum/all/reverse-mouse-wheel-scroll/657c4537-f346-4b8b-99f8-9e1f52cd94c2 
> Step 1: Open Windows PowerShell in Administrator Mode. You can do this by going to Start Menu, type PowerShell, and click Run as Administrator.
> Step 2: Copy the following code and paste it in the command line of Windows PowerShell:
$mode = Read-host "How do you like your mouse scroll (0 or 1)?"; Get-PnpDevice -Class Mouse -PresentOnly -Status OK | ForEach-Object { "$($_.Name): $($_.DeviceID)"; Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Enum\$($_.DeviceID)\Device Parameters" -Name FlipFlopWheel -Value $mode; "+--- Value of FlipFlopWheel is set to " + (Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Enum\$($_.DeviceID)\Device Parameters").FlipFlopWheel + "`n" }
> The code above is one-liner PowerShell script. Copy-paste it as-is.
> Step 3: It will ask how do you like your mouse to scroll.
> Downward wheel motion makes the page...
>   0 - Move up so you see contents below (Default Mode, Windows behavior)
>   1 - Move down so you can see contents above (Natural Mode, Mac behavior, reverse mode)
>   Type the number that corresponds to your scroll preference.
> Step 4: Restart your computer. Your settings will take effect after you restart.

![image](https://user-images.githubusercontent.com/59367560/173343151-1f3f1191-f33f-4038-a82f-47ade64a7dad.png)

