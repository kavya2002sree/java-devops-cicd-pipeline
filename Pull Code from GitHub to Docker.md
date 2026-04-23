# Pull code from Github to Docker

Deploying a Java webapp on a docker container through the use of Jenkins on EC2 instance

## Step-1: setup jenkins server on AWS EC2 instance

* Initially we need to setup a linux EC2 instance for jenkins software
  login in to the AWS and search for EC2 instance. Click on launch instance and create an EC2 instance
    <img width="1920" height="913" alt="Screenshot 2026-04-23 152109" src="https://github.com/user-attachments/assets/90bc9949-f9c9-43e3-8ab2-22f98c11bc62" />

* put name as jenkins_server for easy understanding of what excatly this instance is for , AMI will be linux(OS) 
    <img width="1920" height="922" alt="Screenshot (1)" src="https://github.com/user-attachments/assets/c20fcc7e-afaf-4536-b9f4-42b279f9e30a" />

* Instance type will be t3, for key-pair you can use already created one or you can create it by clicking create new pair key option
    <img width="1920" height="890" alt="Screenshot (2)" src="https://github.com/user-attachments/assets/6282a57c-fcb5-40f4-b8d9-1a16955bbeff" />

* Under the network settings click on edit option and add a new inbound rule in securtiy group with port range 8080 and source type will be anywhere
    <img width="1920" height="904" alt="Screenshot (3)" src="https://github.com/user-attachments/assets/6a53cde8-2fae-438c-8773-b63d5e6987ca" />
    
* click on launch instance. you will see a page shown below. Now lets try to connect EC2 instance, click on connect to instance button shown below
    <img width="1920" height="911" alt="Screenshot (4)" src="https://github.com/user-attachments/assets/093ff900-c767-4fca-8fa0-f815e1408a38" />
    
* If you want to login virtual server(EC2 instance) in SSH client apps you can use MobaXterm,BitVise,Termius and many more.But here iam trying to login to powershell command.
  
* Go to SSH client option and you will see the link to connect it to server. copy the link in the example column and paste it inside the terminal
* click on windows+R and type powershell it will be redirecting you to the powershell terminal.
  
  <img width="1920" height="920" alt="Screenshot (7)" src="https://github.com/user-attachments/assets/db4a618b-f668-4f12-a94b-d04b2d676bf3" />
  <img width="1920" height="1080" alt="Screenshot (6)" src="https://github.com/user-attachments/assets/09dea159-dcde-41cf-90ae-0925e4c3320f" />

  ## Installation of jenkins on EC2 instance

  * Install jenkins following the instructions from the official jenkins website https://pkg.jenkins.io/rpm-stable/

  * Inorder to use this repository we run some commands in the server
    
    ```
     sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
    ```
    sudo- Run commands as a root
    wget- Tool used to download files from the internet or url
    -o - used to save the file
    /etc/yum.repos.d/jenkins.repo- saves the downloaded file with the name jenkins.repo in that /etc/yum location
    https... - is the link of the repository

    ```
    sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
    ```
    rpm- red hat package manager
    import- import GPG key into the system
    The other link is the GPG key package link- GPG key is a package used as Authentication
    Output:
      <img width="1920" height="544" alt="Screenshot (8)" src="https://github.com/user-attachments/assets/5e603be3-4ddf-4244-a9fe-24e78a126134" />

  * Now let's install EPEL(extra packages for enterprise linux) packages for Amazon linux AMI
      if you are using amazon linux 2023 use the below codes
      ```
      sudo dnf install maven -y
      sudo dnf install docker -y
      sudo dnf install git -y
      ```
      
  * Let's install java-openjdk21
    ```
    sudo dnf install java-21-amazon-corretto -y
    ```
    For version check use
    ```
    java --version
    ```
      <img width="1920" height="935" alt="Screenshot (10)" src="https://github.com/user-attachments/assets/59210974-220b-465c-9adb-54ef649bca81" />
    
  ## Now let's install Jenkins
  * To install jenkins use below command
  ```
    sudo yum install jenkins
  ```
    <img width="1920" height="966" alt="Screenshot (11)" src="https://github.com/user-attachments/assets/7e8bd514-a3d6-4cd8-8021-931a91e76f4c" />

  * To enable and start jenkins service in our EC2 instance use below commands
    ```
      sudo systemctl start jenkins
      sudo systemctl status jenkins
    ```
    systemctl-tool used to manage services in linux systems
    
    <img width="1920" height="669" alt="Screenshot (12)" src="https://github.com/user-attachments/assets/005ab90c-9787-43f7-af88-99eb56afbcda" />

  ** Now let’s try to access the Jenkins server through our browser. For that take the public IP of your EC2 instance and paste it into your favorite browser and should see something like this:
  My ip address is 54.210.150.245,so i have used 54.210.150.245:8080 to login to jenkins server

  <img width="1920" height="974" alt="Screenshot (13)" src="https://github.com/user-attachments/assets/753fadb6-5435-4494-92ce-70d73df0d20f" />

  * To unlock Jenkins we need to go to the path /var/lib/jenkins/secrets/initialAdminPassword and fetch the admin password to proceed further:
    ```
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```
    <img width="1228" height="90" alt="Screenshot (14)" src="https://github.com/user-attachments/assets/736b9dc7-f6a9-4747-bbac-01d109b1bf11" />

  * Now on the Customize Jenkins page, we can go ahead and install the suggested plugins
 
    <img width="1920" height="950" alt="Screenshot (15)" src="https://github.com/user-attachments/assets/dac131ee-5bb6-4c35-aee4-b95e952dd225" />

  * create our first Admin user, provide all the required data and proceed to save and continue

      <img width="1920" height="944" alt="Screenshot (16)" src="https://github.com/user-attachments/assets/2c9d7895-c501-46e9-86ec-f35d03dd6e6b" />

  * Ready to use our jenkins server
      <img width="1920" height="945" alt="Screenshot (18)" src="https://github.com/user-attachments/assets/d903dcf4-42b2-49be-9d55-582b2bc07b26" />

  


  



