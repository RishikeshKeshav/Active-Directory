# Cmpe282-Cloud-Services-Assignment-1-Active-Directory-

### Submitted To-Prof. Andrew Bond
### Team Name: -  Trios
#### Project supervisor :- Prof. Andrew Bond

#### Rishikesh Andhare (016726203)
	

#### Steps:-
●	Create an Active Directory with a domain Name.
●	Create Ec2 Instance to host the virtual Machine as Windows.
●	Configured Public IPs and domain Name in Virtual Machine.
●	Populated Employees data(300k+) into local Database
●	Export the Employees data into output.csv file
●	Using the powershell in Virtual Machine populated the data into Active Directory 






1)	Create and configured the Active directory in AWS with directory DNS name as “cloud-pramathanadig.com”
![image](https://user-images.githubusercontent.com/111613476/220050215-3cc906d0-b747-4077-96d9-2b93e265074d.png)
 
2) In Directory service , directory got created with the directory-name “cloud-pramathanadig.com” and type as Microsoft AD
 
![image](https://user-images.githubusercontent.com/111613476/220050262-5949b2fd-04cf-4adc-bdcf-90257f88ae2f.png)
![image](https://user-images.githubusercontent.com/111613476/220050305-f5e15524-187f-4bbe-8b7b-87b6c008b608.png)
![image](https://user-images.githubusercontent.com/111613476/220050329-48e5ff26-6aaa-4648-881f-214f03d2fdb5.png)

 



 




4) Created EC2 instance , and selected Instance type as t2.large so that users more than 300k+ can be added and instance image as microsoft Windows OS.  
 

 ![image](https://user-images.githubusercontent.com/111613476/220050375-cfd0a489-f053-4f01-a7ad-afcdeafd6915.png)
![image](https://user-images.githubusercontent.com/111613476/220050406-3a722d16-f3a2-4835-b5d2-8b2e7c28d7f4.png)




5) Using the Public IPv4 DNS, connected to the Remote Desktop Connection and using UserName and Password.

![image](https://user-images.githubusercontent.com/111613476/220050457-20dd5869-5a6a-442d-a664-3c8e6b55bbe6.png)

 
6) Using the Username and password connected to the EC2 Instance Windows virtual Machine.

![image](https://user-images.githubusercontent.com/111613476/220050505-f3d7fb6e-05e0-493b-befb-eaba79f2970a.png)

 
7) Configure the DNS IP same as Active Directory in the Virtual Machine.
![image](https://user-images.githubusercontent.com/111613476/220050551-5a2deef7-8dc8-4052-8e2c-315368b75891.png)

 
8) Configure the Domain Name “cloud-pramathanadig.com” in the virtual Machine which is the same in the Active Directory.

![image](https://user-images.githubusercontent.com/111613476/220050607-72d7f044-6fae-4bdc-888b-f9fe46352299.png)

 
9) Restart the virtual Machine and login to it using the Credentials.

10) The Name of the Device got Changed and it got populated with the domain name.

 ![image](https://user-images.githubusercontent.com/111613476/220050658-7e4909f3-d73b-4c0c-bbdb-9ed8e40ec5f9.png)

11) Downloaded the necessary Active Directory tools and Install them in the Windows Virtual Machine .
 
![image](https://user-images.githubusercontent.com/111613476/220050717-373fd442-8c9f-42ba-bd54-a158964ccf0d.png)



12) In the beginning there will Only Admin User. and In the further steps we will populate it with other users
 
![image](https://user-images.githubusercontent.com/111613476/220050777-80071546-582c-4bcc-8625-7ea40ad26153.png)

MYSQL Steps :-
Steps done to populate users into the local MySql database.
1.	Populated with users from the sample user database https://github.com/datacharmer/test_db	into the local database.
2.	Follow and run  all the queries  given in the employee.sql and all the required tables get created.
3.	Populated the database “employees” with all the employees and department data.
 
![image](https://user-images.githubusercontent.com/111613476/220050940-695c4f0a-f8cc-4074-be25-3d5eab763fa7.png)

4.	Done Verification of all the data in the local Mysql database. And more than 300k+ users got populated in the local database.	

 ![image](https://user-images.githubusercontent.com/111613476/220051012-3cb910e8-b70c-48af-bb2d-2593938c8584.png)


1)	Extracted all the data into an output.csv file.

2)	Then copy the output.csv file into the Host Virtual Machine.(Ec2 - Instance)

![image](https://user-images.githubusercontent.com/111613476/220051055-311637ae-13fe-4576-a1b1-2e9f4c8e887d.png)

 
3)Run the Powershell ISE in Virtual Machine as Administrator  and write a script then run the script file .
SCRIPT:-
Import-Module activedirectory
#Store the data from ADUsers.csv in the $ADUsers variable
$ADUsers = Import-csv C:\Users\admin\Desktop\employees.csv
#Loop through each rows containing user details in the CSV file 
foreach ($User in $ADUsers)
{
#Read user data from each field in each row and assign the data to a variable as below	
	$Username 	= $User.userName
	$Password 	= $User.password
	$Firstname 	= $User.firstName
    $DisplayName= $User.displayName
	$Lastname 	= $User.lastName
	$OU 		= $User.OU
    $email      = $User.emailAddress
    $groups     = $User.memberOf


	#Check to see if the user already exists in AD
	if (Get-ADUser -F {SamAccountName -eq $Username})
	{
		 #If user does exist, give a warning
		 Write-Warning "A user account with username $Username already exist in Active Directory."
	}
	else
	{
		#User does not exist then proceed to create the new user account
		
        #Account will be created in the OU provided by the $OU variable read from the CSV file
		New-ADUser `
            -SamAccountName $Username `
            -UserPrincipalName $email `
            -Name "$Firstname $Lastname" `
            -GivenName $Firstname `
            -Surname $Lastname `
            -Enabled $True `
            -Path $OU `
            -DisplayName "$Lastname, $Firstname" `
            -Description $description `
            -AccountPassword (convertto-securestring $Password -AsPlainText -Force) -ChangePasswordAtLogon $False
	}
}

![image](https://user-images.githubusercontent.com/111613476/220051146-663f05e0-fbaf-41cf-ab4c-f6df33443ad5.png)


 
4) Once the script file is running and completed users will get added into the active directory and it will get displayed.
 
 ![image](https://user-images.githubusercontent.com/111613476/220051184-75a2f8ac-9265-44a0-8667-d12782539ea2.png)
![image](https://user-images.githubusercontent.com/111613476/220051216-ab7f5ab2-2e75-4250-8d0f-b8750e069d6c.png)


5) To verify and count how many Users get added into the active directory run the command “  (Get-AdUser -Filter *).Count” Into the powershell, we can see 300056 users got added(More than 300k+ for extra Credit)

 ![image](https://user-images.githubusercontent.com/111613476/220051257-73bda831-842a-40a3-8c55-eaba8357c915.png)



References:-

https://docs.aws.amazon.com/directory-service/index.html


https://docs.aws.amazon.com/ec2/index.html
