# PowerShell
## Documents and Reference
- [Adam The Automator - PowerShell Administration](https://adamtheautomator.com/tag/powershell-administration/)
- [PSCustomObject](https://pscustomobject.github.io/)

## Code Snipets

### Custom Objects

~~~powershell
#     ___        _                ___  _     _        _   
#    / __|  _ __| |_ ___ _ __    / _ \| |__ (_)___ __| |_ 
#   | (_| || (_-<  _/ _ \ '  \  | (_) | '_ \| / -_) _|  _|
#    \___\_,_/__/\__\___/_|_|_|  \___/|_.__// \___\__|\__|
#                                         |__/            

[PSCustomObject]@{
    PropertyName1 = 'PropertyValue'
    PropertyArray = 'One','Two','Three'
    PropertyNull  = $null
}
~~~

### To-Do
- Calculated Properties
- Import CSV
- Export CSV
- Active Directory LDAP filters


### String and number formatting

~~~powershell
#    _  _                 _        _            _         ___         _            _ 
#   | || |_____ ____ _ __| |___ __(_)_ __  __ _| |  ___  |   \ ___ __(_)_ __  __ _| |
#   | __ / -_) \ / _` / _` / -_) _| | '  \/ _` | | |___| | |) / -_) _| | '  \/ _` | |
#   |_||_\___/_\_\__,_\__,_\___\__|_|_|_|_\__,_|_|       |___/\___\__|_|_|_|_\__,_|_|
#                                                                                    

# Decimal to Hexadecimal
'0x{0:X}'  -f 314           #---> 0x13A (Uppercase digits)      
'0x{0:x}'  -f 314           #---> 0x13a (Lowercase digits)
'0x{0:X4}' -f 314           #---> 0x013A (Minimum of 4 characters excluding the 0x prefix and uppercase digits)  
'0x{0:x4}' -f 314           #---> 0x013a (Minimum of 4 characters excluding the 0x prefix and lowercase digits)             
[Convert]::ToString(314,16) #---> 13a

# Hexadecimal to Decimal
[int]'0x013A'                       #---> 314
[int]'0x013a'                       #---> 314
[int]'0x00000013A'                  #---> 314
[int]'0X013A'                       #---> 314
[Convert]::ToInt32('0x13A', 16)     #----> 314

#     ___     _        _         ___         _            _ 
#    / _ \ __| |_ __ _| |  ___  |   \ ___ __(_)_ __  __ _| |
#   | (_) / _|  _/ _` | | |___| | |) / -_) _| | '  \/ _` | |
#    \___/\__|\__\__,_|_|       |___/\___\__|_|_|_|_\__,_|_|
#                                                           
$octal = 0472
$decimal = 

# Decimal to Octal
"0$([Convert]::ToString(314, 8))"   #---> 0472

# Octal to Decimal
[Convert]::ToInt32("0472", 8)       #----> 314
[Convert]::ToInt32("472", 8)        #----> 314
[Convert]::ToInt32(472, 8)          #----> 314


#     ___     _        _         _  _                 _        _            _ 
#    / _ \ __| |_ __ _| |  ___  | || |_____ ____ _ __| |___ __(_)_ __  __ _| |
#   | (_) / _|  _/ _` | | |___| | __ / -_) \ / _` / _` / -_) _| | '  \/ _` | |
#    \___/\__|\__\__,_|_|       |_||_\___/_\_\__,_\__,_\___\__|_|_|_|_\__,_|_|
#                                                                             

# Octal to Hexadecimal
'0x{0:X}'  -f [Convert]::ToInt32("0472", 8)     #---> 0x13A     

# Hexadecimal to Octal
"0$([Convert]::ToString([int]'0x13A', 8))"      #---> 0472 


#    ___                  _ _                            _                
#   | _ \___ _  _ _ _  __| (_)_ _  __ _   _ _ _  _ _ __ | |__  ___ _ _ ___
#   |   / _ \ || | ' \/ _` | | ' \/ _` | | ' \ || | '  \| '_ \/ -_) '_(_-<
#   |_|_\___/\_,_|_||_\__,_|_|_||_\__, | |_||_\_,_|_|_|_|_.__/\___|_| /__/
#                                 |___/                                   

# Banker's rounding is the default in PowerShell for mathematical functions and casting to integer
# .NET 4.5 -  https://docs.microsoft.com/en-us/dotnet/api/system.midpointrounding?view=netframework-4.5
# .NET Core - https://docs.microsoft.com/en-us/dotnet/api/system.midpointrounding?view=netcore-3.1

# Banker's rounding is rounding up OR down to the closest even number
[int]3.1    #--->  3
[int]3.5    #--->  4
[int]3.9    #--->  4
[int]4.1    #--->  4
[int]4.5    #--->  4
[int]4.9    #--->  5
[int]-3.1   #---> -3
[int]-3.5   #---> -4
[int]-3.9   #---> -4
[int]-4.1   #---> -4
[int]-4.5   #---> -4
[int]-4.9   #---> -5

[Math]::Round(3.1)   #--->  3
[Math]::Round(3.5)   #--->  4
[Math]::Round(3.9)   #--->  4
[Math]::Round(4.1)   #--->  4
[Math]::Round(4.5)   #--->  4
[Math]::Round(4.9)   #--->  5
[Math]::Round(-3.1)  #---> -3
[Math]::Round(-3.5)  #---> -4
[Math]::Round(-3.9)  #---> -4
[Math]::Round(-4.1)  #---> -4
[Math]::Round(-4.5)  #---> -4
[Math]::Round(-4.9)  #---> -5

[Math]::Round(3.1, [System.MidpointRounding]::ToEven)         #--->  3
[Math]::Round(3.5, [System.MidpointRounding]::ToEven)         #--->  4
[Math]::Round(3.9, [System.MidpointRounding]::ToEven)         #--->  4
[Math]::Round(4.1, [System.MidpointRounding]::ToEven)         #--->  4
[Math]::Round(4.5, [System.MidpointRounding]::ToEven)         #--->  4
[Math]::Round(4.9, [System.MidpointRounding]::ToEven)         #--->  5
[Math]::Round(-3.1, [System.MidpointRounding]::ToEven)        #---> -3
[Math]::Round(-3.5, [System.MidpointRounding]::ToEven)        #---> -4
[Math]::Round(-3.9, [System.MidpointRounding]::ToEven)        #---> -4
[Math]::Round(-4.1, [System.MidpointRounding]::ToEven)        #---> -4
[Math]::Round(-4.5, [System.MidpointRounding]::ToEven)        #---> -4
[Math]::Round(-4.9, [System.MidpointRounding]::ToEven)        #---> -5

# "Traditional" rounding
[Math]::Round(3.1, [System.MidpointRounding]::AwayFromZero)   #--->  3
[Math]::Round(3.5, [System.MidpointRounding]::AwayFromZero)   #--->  4
[Math]::Round(3.9, [System.MidpointRounding]::AwayFromZero)   #--->  4
[Math]::Round(4.1, [System.MidpointRounding]::AwayFromZero)   #--->  4
[Math]::Round(4.5, [System.MidpointRounding]::AwayFromZero)   #--->  5
[Math]::Round(4.9, [System.MidpointRounding]::AwayFromZero)   #--->  5
[Math]::Round(-3.1, [System.MidpointRounding]::AwayFromZero)  #---> -3
[Math]::Round(-3.5, [System.MidpointRounding]::AwayFromZero)  #---> -4
[Math]::Round(-3.9, [System.MidpointRounding]::AwayFromZero)  #---> -4
[Math]::Round(-4.1, [System.MidpointRounding]::AwayFromZero)  #---> -4
[Math]::Round(-4.5, [System.MidpointRounding]::AwayFromZero)  #---> -5
[Math]::Round(-4.9, [System.MidpointRounding]::AwayFromZero)  #---> -5
function round( $value  ) {
    [Math]::Round( $value, [System.MidpointRounding]::AwayFromZero )
}
round(4.5)    #---> 5
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

### Loops
#### Types
-foreach
-for
-switch

#### Break
-Terminates the overall loop processing

#### Continue
-Skips the remainder of the body of the loop and continues on with the next loop iteration.
