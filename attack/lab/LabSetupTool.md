```
# banner
$banner = @()
$banner += ''
$banner += '   ___    ___    ___                  __ _             __         __ '
$banner += '  / _ |  / _ \  / _ ) ___ _ ___ ___  / /(_)___  ___   / /  ___ _ / / '
$banner += ' / __ | / // / / _  |/ _ `/(_-</ -_)/ // // _ \/ -_) / /__/ _ `// _ \'
$banner += '/_/ |_|/____/ /____/ \_,_//___/\__//_//_//_//_/\__/ /____/\_,_//_.__/'
$banner += '2020.10.31 ver1.1 @apt773'
$banner += ''
$banner | foreach-object { Write-Host $_ -ForegroundColor "Yellow" }

# Usage
Write-Host "Usage" -ForegroundColor "yellow"
Write-Host "Note! Make Administrator Account of DC before running this script" -ForegroundColor "green"
Write-Host "Note! If necessary, Command 'Set-ExecutionPolicy bypass'" -ForegroundColor "green"
Write-Host "Import-Module ./ADBaselineLab.ps1" -ForegroundColor "yellow"
Write-Host "------------------------------------------------------" -ForegroundColor "yellow"
Write-Host "For Domain Controller : CreateDomainControllerStep1" -ForegroundColor "yellow"
Write-Host "                        CreateDomainControllerStep2" -ForegroundColor "yellow"
Write-Host "                        CreateDomainControllerStep3" -ForegroundColor "yellow"
Write-Host "------------------------------------------------------" -ForegroundColor "yellow"
Write-Host "For Client PC         : CreatePC1" -ForegroundColor "yellow"
Write-Host "                        CreatePC2" -ForegroundColor "yellow"
Write-Host "                        DisableWindowsDefender" -ForegroundColor "yellow"
Write-Host "------------------------------------------------------" -ForegroundColor "yellow"
Write-Host "For Attack            : CreateKerberoast" -ForegroundColor "yellow"
Write-Host "                        CreateASRepRoast" -ForegroundColor "yellow"
Write-Host "                        CreateSMBRelay" -ForegroundColor "yellow"
Write-Host "                        CreateDNSAdmins" -ForegroundColor "yellow"



function CreateDomainControllerStep1 {
	Write-host "Creating Domain Controller Step1..."  -foregroundcolor "yellow"
    
	# Change hostname
	Rename-Computer -NewName "DC-Server-1"

	# Change IP-Address
	Get-NetAdapter | New-NetIPAddress -AddressFamily IPv4 -IPAddress 192.168.1.100 -PrefixLength 24
    
	# Uninstall Windows Defender
	Uninstall-WindowsFeature -Name Windows-Defender
    
	Start-Sleep -Seconds 3
	# Reboot
	Restart-Computer
}

function CreateDomainControllerStep2 {
	Write-host "Creating Domain Controller Step2..."  -foregroundcolor "yellow"

	# Enable Active Directory
	Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
	Install-ADDSForest -DomainName "labcorp.local" -ForestMode "WinThreshold" -DomainMode "WinThreshold" -DomainNetbiosName "LABCORP" -Force -SafeModeAdministratorPassword (ConvertTo-SecureString -String "P@ssword!" -AsPlainText -Force)

	Start-Sleep -Seconds 3
}

function CreateDomainControllerStep3 {
	Write-host "Creating Domain Controller Step3..."  -foregroundcolor "yellow"

	# Create Users
	
	# Alice
	New-ADUser -Name "Alice" -DisplayName "Alice" -Description "Alice" -Enabled $true -PasswordNeverExpires $true -AccountPassword (ConvertTo-SecureString -String "p@ssw0rd" -AsPlainText -Force)
    	
	# Bob in Domain Admins group
	New-ADUser -Name "Bob" -DisplayName "Bob" -Description "Bob" -Enabled $true -PasswordNeverExpires $true -AccountPassword (ConvertTo-SecureString -String "Prince123" -AsPlainText -Force)
	# Domain Admins group
	Add-ADGroupMember -Identity "Domain Admins" -Members "Bob"

	# DB_Service account
	New-ADUser -Name "DB_Service" -DisplayName "DB_Service" -Description "DB_Service" -Enabled $true -PasswordNeverExpires $true -AccountPassword (ConvertTo-SecureString -String "MYpassword123#" -AsPlainText -Force)

	Start-Sleep -Seconds 3
}

function CreatePC1 {
	Write-Host "Create PC1..." -ForegroundColor "Green"

	# Change hostname
	Try {
    	Rename-Computer -NewName "KY1011" -ErrorAction Stop
		}
	Catch {
	    Write-Host "hostname already exists" -ForegroundColor "yellow"
		}

	# Change IP-Address
	Try {
    	Get-NetAdapter | New-NetIPAddress -AddressFamily IPv4 -IPAddress 192.168.1.1 -PrefixLength 24 -ErrorAction Stop
		Get-NetAdapter | Set-DnsClientServerAddress -ServerAddresses 192.168.1.100 -ErrorAction Stop
		}
	Catch {
	    Write-Host "IPAddress already exists" -ForegroundColor "yellow"
		}	
	
	# Enable file/printer sharing
	Set-NetFirewallRule -DisplayGroup "ファイルとプリンターの共有" -Enabled True -Profile Domain
	
	# Disable Windows Update
	$setting = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
	$setting.NotificationLevel = 1
	$setting.Save

	# Join domain
	Write-Host "Join domain" -ForegroundColor "yellow"
	
	# Join domain Alice
	$credential = New-Object PSCredential -ArgumentList ([PSCustomObject]@{
		UserName = "Alice"
		Password = (ConvertTo-SecureString -String 'p@ssw0rd' -AsPlainText -Force)[0]
	})
	Add-Computer -Domain "LABCORP" -Credential $credential
    
	# Join domain DB_Service
	#$credential = New-Object PSCredential -ArgumentList ([PSCustomObject]@{
	#	UserName = "DB_Service"
	#	Password = (ConvertTo-SecureString -String 'MYpassword123#' -AsPlainText -Force)[0]
	#})
	#Add-Computer -Domain "LABCORP" -Credential $credential
    
	# Add local Administrators group
   	Write-Host "Add local Administrators group" -ForegroundColor "yellow"
	Add-LocalGroupMember -Group "Administrators" -Member "LABCORP\Alice"
	#Add-LocalGroupMember -Group "Administrators" -Member "LABCORP\DB_Service"
    	
	# Remove Domain Admins from local Administrators group
   	Write-Host "ARemove Domain Admins from local Administrators group" -ForegroundColor "yellow"
	Remove-LocalGroupMember -Group "Administrators" -Member "Domain Admins"

	# Reboot
	Restart-Computer -Force
}

function CreatePC2 {
	Write-Host "Create PC2..." -ForegroundColor "yellow"

	# Change hostname
	Try {
    	Rename-Computer -NewName "KY2204" -ErrorAction Stop
		}
	Catch {
	    Write-Host "hostname already exists" -ForegroundColor "yellow"
		}
		
	# Change IP-Address
	Try {
    	Get-NetAdapter | New-NetIPAddress -AddressFamily IPv4 -IPAddress 192.168.1.2 -PrefixLength 24 -ErrorAction Stop
		Get-NetAdapter | Set-DnsClientServerAddress -ServerAddresses 192.168.1.100 -ErrorAction Stop
		}
	Catch {
	    Write-Host "IPAddress already exists" -ForegroundColor "yellow"
		}
	
	# Enable file/printer sharing
	Set-NetFirewallRule -DisplayGroup "ファイルとプリンターの共有" -Enabled True -Profile Domain
	
	# Disable Windows Update
	$setting = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
	$setting.NotificationLevel = 1
	$setting.Save

	# Join domain
	Write-Host "Join domain" -ForegroundColor "yellow"
	
	# Join domain Bob
	$credential = New-Object PSCredential -ArgumentList ([PSCustomObject]@{
		UserName = "Bob"
		Password = (ConvertTo-SecureString -String 'Prince123' -AsPlainText -Force)[0]
	})
	Add-Computer -Domain "LABCORP" -Credential $credential
    
	# Join domain DB_Service
	#$credential = New-Object PSCredential -ArgumentList ([PSCustomObject]@{
	#	UserName = "DB_Service"
	#	Password = (ConvertTo-SecureString -String 'MYpassword123#' -AsPlainText -Force)[0]
	#})
	#Add-Computer -Domain "LABCORP" -Credential $credential
      
	# Add local Administrators group
	Write-Host "Add local Administrators group" -ForegroundColor "yellow"
	Add-LocalGroupMember -Group "Administrators" -Member "LABCORP\Bob"
	#Add-LocalGroupMember -Group "Administrators" -Member "LABCORP\DB_Service"

	# Remove Domain Admins from local Administrators group
   	Write-Host "ARemove Domain Admins from local Administrators group" -ForegroundColor "yellow"
	Remove-LocalGroupMember -Group "Administrators" -Member "Domain Admins"
		
	# Reboot
	Restart-Computer -Force
}


function DisableWindowsDefender {
	Write-Host "Disable Windows Defender..." -ForegroundColor "yellow"

	# Disable Windows Defender
	Set-MpPreference -DisableRealtimeMonitoring 1
}


function CreateKerberoast {
	Write-Host "Create Kerberoast Setting..." -ForegroundColor "yellow"

	net localgroup Administrators LABCORP\DB_Service /add
	setspn -s dbsrv/labcorp.local:1433 DB_Service
}


function CreateASRepRoast {
	Write-Host "Create ASRepRoast Setting..." -ForegroundColor "yellow"

	Set-ADAccountControl DB_Service -DoesNotRequirePreAuth $True

}


function CreateSMBRelay {
	Write-Host "Create SMBRelay Setting..." -ForegroundColor "yellow"

	Set-SmbClientConfiguration -RequireSecuritySignature 0 -EnableSecuritySignature 0 -Confirm -Force

}


function CreateDNSAdmins {
	Write-Host "Create DNSAdmins Setting..." -ForegroundColor "yellow"

	net localgroup "DnsAdmins" Alice /add

}
```