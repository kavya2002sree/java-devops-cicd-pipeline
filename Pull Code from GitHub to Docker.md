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

  
  ## step-2:Intergrate GitHub with jenkins software

  * install Git on our EC2 instance before installing GitHub
      ```
        sudo yum install git
      ```
      check the version if it is installed or not -
      ```
        git--version
      ```

    * To install GitHub plugin lets go to jenkins GUI and click on settings icon and now click on manage jenkins
        
      <img width="1920" height="889" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/ebdc405f-5020-4d0f-8471-e35dc1a0e9bd" />

    * click on plugins and move to available plugins and search for github integration.select restart button
      
        <img width="1920" height="914" alt="Screenshot (22)" src="https://github.com/user-attachments/assets/4f75a402-3a24-4c19-9662-58dd5483781e" />

     *  After installtion let's configure git on jenkins go to setting and click on tools you will get the page shown below

         <img width="1920" height="937" alt="Screenshot (23)" src="https://github.com/user-attachments/assets/bebdab3a-083a-4364-b0d9-58d45e5d2316" />

      * Under Git installtion provide name and path , we can either provide the complete path where our Git is installed on the Jenkins machine or just put any name, in my case I put Git to allow Jenkins to automatically search for Git. Then click on Save to complete the installation.

          <img width="1920" height="919" alt="Screenshot (24)" src="https://github.com/user-attachments/assets/f7487ed5-af88-4f3e-ac10-f099f4118919" />

    ## step-3: Integrate Maven with jenkins

      * To install Maven on our Jenkins Server we will switch to the /opt directory and download the Maven package. extract the tar.gz file:
        if the file is present you can extract the packages using the tar.
        ```
        wget https://downloads.apache.org/maven/maven-3/3.9.11/binaries/apache-maven-3.9.11-bin.tar.gz
        tar -xvzf apache-maven-3.9.9-bin.tar.gz
        ```

        <img width="1920" height="977" alt="Screenshot (25)" src="https://github.com/user-attachments/assets/63405dbf-6519-4e7f-aa20-42ba28bce1fd" />

       * Inorder to access the maven from any location we have to set up environmental variables for our root user in bash_profile.
         
          <img width="1920" height="120" alt="Screenshot (29)" src="https://github.com/user-attachments/assets/5d46e413-725a-4100-8552-29494a22ab33" />


      * In the .bash_profile file, we need to add Maven and Java paths and load these values
        
        ```
        export JAVA_HOME=/usr/lib/jvm/java-21-amazon-corretto.x86_64/bin/java
        export M2_HOME=/maven
        export M2=/maven/bin
        
        export PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$M2_HOME/bin
        ```
        
         <img width="1920" height="942" alt="Screenshot (35)" src="https://github.com/user-attachments/assets/7f34074f-e28a-47fc-bb98-279424cca176" />


      * To verify follow the below steps:

          <img width="1920" height="158" alt="Screenshot (27)" src="https://github.com/user-attachments/assets/df357342-ef0e-4424-b2d0-556283a46206" />


      * With this setup, we can execute maven commands from anywhere on the server:

          <img width="1920" height="181" alt="Screenshot (30)" src="https://github.com/user-attachments/assets/29005cb7-afdc-4ef6-a6d5-093e43c95c8f" />
    
      * Now we need to update the paths where we have installed maven and java for that we need to open the plugins from the jekins software and install maven

          <img width="1920" height="921" alt="Screenshot (33)" src="https://github.com/user-attachments/assets/ca0fcd87-fe04-4591-9e8a-121f8c7d0e4a" />
          <img width="1920" height="881" alt="Screenshot (34)" src="https://github.com/user-attachments/assets/e31ce24a-e138-4594-8acd-9c6b817721f1" />

     ## step-4:Setup a DockerHost

      * Create a new EC2 instance or edit the existing instance. Edit the inbound rules by adding a new security group with 8081-9000 port range.

          <img width="1920" height="911" alt="Screenshot (36)" src="https://github.com/user-attachments/assets/5dcf83f1-97fd-4812-9989-5c6830f1b3ae" />
  
      * Now install docker on ec2 instance
        ```
          sudo yum install docker
        ```
      * after installtion check the docker version
        
          ```
          docker --version
          ```
       * Lets enable and start the docker

           ```
           sudo systemctl enable docker
           sudo systemctl start docker
           sudo systemctl status docker
           ```
           <img width="1920" height="662" alt="Screenshot (38)" src="https://github.com/user-attachments/assets/06132eac-c20b-4d4e-a8aa-8301f9ad6903" />
 
      ## create tomcat docker container

       * We will first pull the official Tomcat docker image from the Docker Hub and then run the container out of the same image.

          ```
          sudo docker pull tomcat
          ```
         <img width="1920" height="347" alt="Screenshot (39)" src="https://github.com/user-attachments/assets/97e80864-f694-4654-befd-c00ba5b43251" />

        
       * now we create container from the image that we have pulled it from docker hub

          ```
           sudo docker run -d --name tomcat-container -p 8081:8080 tomcat
          ```
          This command creates and runs a Docker container in detached mode using the Tomcat image, assigns it a name, and maps a host port to the container port            for external access

          <img width="1920" height="188" alt="Screenshot (40)" src="https://github.com/user-attachments/assets/af0d0aca-042d-49f5-9441-860681d7def2" />

      * verify the running container on our EC2 machine:

        ```
          sudo docker ps
        ```
        <img width="1794" height="154" alt="Screenshot (41)" src="https://github.com/user-attachments/assets/30899e2d-58fc-4856-9793-003ddd121c1a" />

      *  whenever we try to access the Tomcat server from the browser it will look for the files in /webapps directory which is empty and the actual files are              being stored in /webapps.dist.

      so in order to fix this issue we will copy all the content from webapps.dist to webapps directory and that will resolve the issue.

      Let's access our tomcat container and perform the steps as shown below:

      ```
         sudo docker exec -it tomcat-container bash
      ```
    <img width="1920" height="243" alt="Screenshot (42)" src="https://github.com/user-attachments/assets/0d9be7f7-cdd6-49e2-9073-0c1444cba0e2" />

      * Once we are in the tomcat container go to the /webapps.dist directory and copy all the content to webapps directory:

        <img width="1920" height="363" alt="Screenshot (43)" src="https://github.com/user-attachments/assets/8eefd5fd-8e90-47fe-ab9b-62781506f965" />


      * when you try to paste your ipv4 address along with 8081 port you can able to see the apache tomcat container
   
         <img width="1920" height="967" alt="Screenshot (44)" src="https://github.com/user-attachments/assets/03f35d10-f114-4180-895e-6babb3b53769" />

      ## create a customised dockerfile for tomcat

      * To create the Dockerfile we will use the official Image of Tomcat and with it will mention the step to copy the contents from the directory /webapps.dist          to /webapps

        




















    





