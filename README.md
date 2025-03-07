# Creating Users, Managing Group Policy, and Accounts

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
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



