                                                                    

Exercise 1.1 : Instance Creation in IBM Security Directory Server (SDS)
1. Aim: To create a new instance in IBM Security Directory Server (SDS) 6.4.0.20, enabling secure  directory management and understanding the setup process.
2. Objective:
1. To learn how to set up and configure a new directory instance in SDS.
2. To understand the tools and commands used for instance creation.
3. To validate the successful creation and operation of the instance.
3. Tools and Requirements
1. Software Required:
    - IBM Security Directory Server 6.4.0.20.
    - IBM Directory Server Administration Tool.
2. System Requirements:
    - Operating system: Linux/Windows.
    -VM ware Workstation Player 17 pro.
4.Theory:
1. What is Instance Creation?
Instance creation refers to the process of setting up and configuring multiple independent directory server environments within the IBM Security Directory Server (SDS) on a single machine. Each instance operates as a separate server with its own configuration, port assignments, and data storage.
 2. What is  SDS Version 6.4.0.20
IBM SDS 6.4.0.20 supports running multiple directory server instances on a single machine. It is secure, scalable, and used for centralized authentication and directory-based management.
3. What is idsldap1?
The idsldap1 is the name assigned to the primary directory server instance in this lab. It is configured to run on port 1389 and is the main instance used for LDAP queries and authentication in this setup.
4. What is idsldap2?
The idsldap2 is the name assigned to the secondary directory server instance. It operates on port 2389 and provides an additional directory.
Steps :
Step1: Command Explanation: /opt/ibm/ldap/V6.4/sbin/idsilist -a
-This command is used in IBM Security Directory Server (SDS) to list all the directory server instances configured on the machine, along with their detailed information.
 -a (All flag):  When used, this flag displays detailed information about each instance, such as: 
- Instance name
- Configuration path
- Ports in use (LDAP, admin, etc.)
 - Current status (running/stopped).
Output:
 
Step2: Open Terminal from Desktop and navigate to the SDS folder as below:
  cd /opt/ibm/ldap/V6.4/sb
Output : 
 
Step 3 : This command is used to create two new users, idsldap1 and idsldap2, as owners of two separate SDS instances  : 
. /idsadduser -u idsldap1 -w P@ssw0rd -l /home/idsldap1 -g idsldap -
	-u idsldap1: Specifies the username to be created (e.g., idsldap1).
	-w P@ssw0rd: Sets the password for the user (P@ssw0rd).
	-l /home/idsldap1: Specifies the home directory for the user (/home/idsldap1).
	-g idsldap: Assigns the user to the group idsldap.
	-n: Prevents interactive prompts during execution, making it a non-interactive command.
Output : 
 
Step4 : Similarly, add the second user idsldap2. This command is used to create new users,  idsldap2, as owners of two separate SDS instances  :
 ./idsadduser -u idsldap2 -w P@ssw0rd -l /home/idsldap2 -g idsldap -n
	-u idsldap2: Specifies the username to be created (e.g., idsldap1).
	-w P@ssw0rd: Sets the password for the user (P@ssw0rd).
	-l /home/idsldap1: Specifies the home directory for the user (/home/idsldap1).
Output:  
Step5 : Create the instance for the idsldap1 user using idsicrt command as below : 
./idsicrt -I idsldap1 -e encryptionseed -l /home/idsldap1 -n
Here in the  command :
	-I: Specifies the name of the instance to be created.
	-e: Provides the encryption seed for the SDS instance.
	-l: Defines the location where the instance will be stored.
	-n: Ensures the command runs without any prompts in the console.
	-idsicrt : The idsicrt command is used to add a DB2 instance for the SDS in the backend and create the instance.
Output: 
Step6 :  Similarly, create the instance for the idsldap2 user using idsicrt command as below : ./idsicrt -I idsldap2 -e encryptionseed -l /home/idsldap2 -n
 
Step7:  So, to check the details of the new instances idsldap1 and idsldap2, you can use the following command in the terminal:
/opt/ibm/ldap/V6.4/sbin/idsilist -a 
	The -a flag stands for "all."
	It provides detailed information about each instanc, such as:
	Instance name
	Port numbers
	Configuration paths
	Status (running/stopped)
Note : We can see that ports 1389 and 2389 are by default assigned to idsldap1 and idsldap2, respectively. This is the standard behavior of SDS.   You can also specify a custom port using the -p argument with the idsicrt command if needed.
Output: :   
 
Step8: Once the instances are created, we will configure the DB2 database for the SDS instance. The DB2 database serves as the backend to store all LDAP entries securely.
./idscfgdb -I idsldap1 -w P@ssw0rd -a idsldap1 -t idsldap1 -l  /home/idsldap1 -n
Note: Here in the command 
	-I is the instance name.
	-w is the password for the instance owner.
	-a is the database admin user.
	-t is the database name.
	-n prevents any prompts during execution.
Output :
 
Step9 : Similarly, to configure the database for the second instance idsldap2, use the following command:  ./idscfgdb -I idsldap2 -w P@ssw0rd -a idsldap2 -t idsldap2 -l /home/idsldap2 -n
The database idsldap2 is created in the idsldap2 DB2 instance. After that, all the default SDS tables are added to this database.
Output:
 
 
Step10 : Minimize the Terminal window, then double-click the Home icon on the Desktop. In the left pane, click Other Locations, then double-click Computer and Home. You’ll see two folders: idsldap1 and idsldap2, which are the home directories for the SDS instance owners.

  
Step11 :  Double-click idsldap1 directory and you can see idsslapd-idsldap1 folder which have all instance related configurations and log files.
 
Step12:Create admin user (cn=root) who can be used to do the administrative task on the ldap instances
./idsdnpw -I idsldap1 -u cn=root -p P@ssw0rd -n
Note : Here in the command 
	-I is instance name 
	-u user name of the instance admin
	-p for password of admin use
	r -n for no prompt.
Output :
 
Step 13: Similarly, for the idsldap2 instance, create the admin user cn=root using the following command:
 /idsdnpw -I idsldap2 -u cn=root -p P@ssw0rd -n
 
The user is successfully created. This admin user will be used to connect to the LDAP and perform administrative tasks.
Step 14  Close terminal .
Learning outcomes :
	-We gained an understanding of how to create and configure directory server instances in IBM Security Directory Server (SDS).
	We learned how to use commands like idsadduser, idsicrt, idscfgdb, and idsdnpw to create instances, configure databases, and handle administrative tasks.
	-We successfully created and set up idsldap1 and idsldap2 instances with unique configurations such as ports, encryption seeds, and database settings.
	We also understood how to link and configure DB2 databases for SDS instances, allowing efficient management of LDAP entries.



                                                                                                                               Shashwat sharma
