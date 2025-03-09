# Creating Users, Managing Group Policy, and Accounts

<p align="center">
<img src="https://github.com/user-attachments/assets/0bb09e28-c161-451f-a72d-9e0315fc433d" alt="Microsoft Active Directory Logo"/>
</p>


<h1>Creating Users, Managing Group Policy, and Accounts</h1>
In this lab we will be Adding users and Managing Group Policy 

## Previous Steps

[Deploying Active Directory in Azure](https://github.com/RobGaughan/Deploying-Active-Directory-in-Azure/)

<h2>Operating Systems Used </h2>

- Windows Server 2022 Datacenter: Azure Edition - x64 Gen2  
- Windows 10 Pro, version 22H2 - x64 Gen2  

<h2>High-Level Deployment and Configuration Steps</h2>

1. Allow Domain users to access remote desktop
2. Run Powershell script to add 1000 users to our domain
3. create an account lockout based on failed log in attempts
4. Enabling and Disabling users

<h2>Deployment and Configuration Steps</h2>

### Allow Domain users to access remote desktop
#### Connect to Client-1 using admin jane_admin

Find client-1 public IP address:  
Virtual Machines > Client-1 > Networking > Network settings 

![image](https://github.com/user-attachments/assets/35965722-d05f-4c90-be6c-a737d71cd161)


login using Remote Desktop Connection 

Make sure to login using domain context:
Username: EXAMPLEDOMAIN\jane_admin  
Password: LabPassword123

![1](https://github.com/user-attachments/assets/9c63688e-c4d1-4e88-8a89-51f1cc083a79)


Inside Client-1 Right click windows icon and lick "System"

![2](https://github.com/user-attachments/assets/aefe1fff-8043-477f-b5aa-df8eb360dd01)

Click on "Remote Desktop" 

![3-cropped-resized](https://github.com/user-attachments/assets/656515c0-3079-4c75-a0b0-037804794971)

Click on "Select users that can remotely access this PC"  

![4](https://github.com/user-attachments/assets/87fad318-4313-442d-9686-98934a81d4dd)

Click "ADD"

![5](https://github.com/user-attachments/assets/9c895dfc-6a48-4628-86ad-3d2fda6f8783)

Type "Domain Users" and click "OK"  

![6](https://github.com/user-attachments/assets/4f7b37f6-dfce-4563-ad13-3f0514b45bdd)

Now we have configured that all domain users can remotely access this client

### Create Users

#### Login to Domain-Controller-1

#### Create users using powershell Script

Open "Powershell ISE" and run it as Admin

![9](https://github.com/user-attachments/assets/0a24286e-2123-44f1-96b5-e22cd18f66ae)


Then copy and paste this script into Powershell ISE:

```
 # ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "LabPassword123"
$NUMBER_OF_ACCOUNTS_TO_CREATE = 1000
# ------------------------------------------------------ #

Function generate-random-name() {
    $consonants = @('b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','z')
    $vowels = @('a','e','i','o','u','y')
    $nameLength = Get-Random -Minimum 3 -Maximum 7
    $count = 0
    $name = ""

    while ($count -lt $nameLength) {
        if ($($count % 2) -eq 0) {
            $name += $consonants[$(Get-Random -Minimum 0 -Maximum $($consonants.Count - 1))]
        }
        else {
            $name += $vowels[$(Get-Random -Minimum 0 -Maximum $($vowels.Count - 1))]
        }
        $count++
    }

    return $name

}

$count = 1
while ($count -lt $NUMBER_OF_ACCOUNTS_TO_CREATE) {
    $fisrtName = generate-random-name
    $lastName = generate-random-name
    $username = $fisrtName + '.' + $lastName
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $firstName `
               -Surname $lastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_EMPLOYEES,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
    $count++
}

```

It should look like this: 

![image](https://github.com/user-attachments/assets/9440e721-ea7f-49e7-8450-a8875f8e856b)

Press the green run button to run the script"

![8](https://github.com/user-attachments/assets/c42cf480-6785-4db6-b10d-782f3aea9de3)

Now the script will make 1000 random users with the password: LabPassword123

### Verify that users were created

Navigate to "Active Directory Users and Computers"  
![10](https://github.com/user-attachments/assets/657fa35d-c384-453f-be04-7172b9d812f3)

Navitage to _EMPLOYEES OU and verify user accounts were created

![10](https://github.com/user-attachments/assets/30be5521-8a2a-4612-a5e7-f16943003a2a)

### Creating an account lockout based on failed log in attempts

Press windows key + r and type in "gpmc.msc" and click ok

![image](https://github.com/user-attachments/assets/a57eba77-f82e-43f3-a10b-ff85729860a7)

Next right click on "Default Domain Policy" and click "Edit"

![11](https://github.com/user-attachments/assets/7b9459af-4d58-42ff-a79a-f3e423919cbe)

This will bring up the Group Policy Management Editor navitgate to:
Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy


![12](https://github.com/user-attachments/assets/7a3935e9-074b-4635-ad9f-9b4cca6b92a2)

Double click "Account Lockout Duration" and set it to 30mins

![13](https://github.com/user-attachments/assets/1c1a57c2-ea38-4be0-bf81-667f3440e8c4)

One we do that and click apply windows will automatically set the failed login attempts to 5 attempts before a lockout occurs

![14](https://github.com/user-attachments/assets/30b92a95-90ee-4630-bc36-037435758359)


### Updating the group Policy 

Option 1 - Wait around 90 mins for the group policy to propagate 
Option 2 - Force update on  the Client

Since we don't want to wait 90 mins for the group policy ot propagate we will force the update manually 

#### Connect to Client-1 

Find client-1 public IP address:  
Virtual Machines > Client-1 > Networking > Network settings 

![image](https://github.com/user-attachments/assets/35965722-d05f-4c90-be6c-a737d71cd161)


login using Remote Desktop Connection

![image](https://github.com/user-attachments/assets/9466d64c-5c51-404f-afd6-e4684a9c70ab)
