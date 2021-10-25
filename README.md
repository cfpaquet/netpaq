# PowerShell - Documents and Reference
- [Adam The Automator - PowerShell Administration](https://adamtheautomator.com/tag/powershell-administration/)
- [PSCustomObject](https://pscustomobject.github.io/)

# PowerShell - Code Snipets

- Custom Objects
- Calculated Properties
- Import CSV
- Export CSV
- Active Directory LDAP filters

```powershell
<#
Returns information about the operating system in a human readable format
#>
 function Get-FriendlyOsInfo {
        param (
            [switch]$BuildNumberOnly
        )
        $dateFormat = 'yyyy-MM-dd hh:mm:ss'
    
        $properties = @{Label='OS';Expression={$_.Caption}},
                      #'Caption',
                      #'OSType',
                      'Version',
                    'BuildNumber',
                    'OSArchitecture',
                    #'OSLanguage', # 1033 = English
                    @{Label='Language';Expression={switch ($_.OSLanguage) { {$_ -in 1033, 4105} { 'English' }; 3084 {'French'}; Default {$_} } } },
                    @{Label='LastBootup';Expression={get-date -date $_.LastBootupTime -Format $dateFormat}},
                    @{Label='RunningMinutes';Expression={[int](New-TimeSpan -Start $_.LastBootupTime).TotalMinutes}},
                    @{Label='TimeZoneOffset';Expression={$_.CurrentTimeZone/60}}
    
        $osInfo = Get-CimInstance Win32_OperatingSystem|Select-Object $properties
    
        if ($BuildNumberOnly) {
            $osInfo.BuildNumber
        }
        else {
            $osInfo
        }
    }
```
