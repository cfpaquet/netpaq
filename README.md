# PowerShell - Documents and Reference
- [Adam The Automator - PowerShell Administration](https://adamtheautomator.com/tag/powershell-administration/)
- [PSCustomObject](https://pscustomobject.github.io/)

# PowerShell - Code Snipets

- Custom Objects
- Calculated Properties
- Import CSV
- Export CSV
- Active Directory LDAP filters
- String formatting
~~~powershell
#    _  _ _____  __
#   | || | __\ \/ /
#   | __ | _| >  < 
#   |_||_|___/_/\_\
#                  

# HEX number without heading zeroes (lowercase) ---> 0x13a                
'0x{0:x}'  -f $decimal                  

# HEX number with heading zeroes (uppercase) CSVDE?? ---> 0x013A
'0x{0:X4}' -f $decimal                  

# HEX number without heading zeroes (lowercase)---> 0x013a
'0x{0:x4}' -f $decimal 
~~~


~~~powershell
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
~~~
