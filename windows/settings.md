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


> Alternative PowerShell CLI
> ```
>  Windows PowerShell
>  Copyright (C) Microsoft Corporation. All rights reserved.
>  Try the new cross-platform PowerShell https://aka.ms/pscore6
>  PS C:\WINDOWS\system32> $mode = Read-host "How do you like your mouse scroll (0 or 1)?"; Get-PnpDevice -Class Mouse -PresentOnly -Status OK | ForEach-Object {  "$($_.Name): $($_.DeviceID)"; Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Enum\$($_.DeviceID)\Device Parameters" -Name FlipFlopWheel -Value $mode; "+--- Value of FlipFlopWheel is set to " + (Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Enum\$($_.DeviceID)\Device Parameters").FlipFlopWheel + "`n" }
>  >>
>  How do you like your mouse scroll (0 or 1)?: 1
>  HID-compliant mouse: HID\DELL099F&COL01\5&22265D5&0&0000
>  +--- Value of FlipFlopWheel is set to 1
>  HID-compliant mouse: HID\VID_046D&PID_C534&MI_01&COL01\7&2E15AD75&0&0000
>  +--- Value of FlipFlopWheel is set to 1
>  PS C:\WINDOWS\system32>
> ```
