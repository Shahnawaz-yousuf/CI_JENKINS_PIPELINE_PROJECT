# CI_JENKINS_PIPELINE_PROJECT

## CI/JENKINS_Project with Nexus & SonarQube:

### Prerequisits:

1. Jenkins-sever (t2 small - Ubuntu)
2. Nexus-server  (t2 medium - Ubuntu)
3. SonarQube-server (t2 large - centOS 7)
4. Jenkins Plugins:
      
      (a) Nexus Artifact uploader
      (b) SonarQube scanner
      (c) Pipeline Maven Integration
      (d) Pipeline Utility Steps
      (e) Build Timestamp 

5. Nexus-server Bootstrap script (attached file with the repository)
6. SonarQube-server Bootstrap script (attached file with the repository)  

### Steps to be followed...

Step-1: Launch Jenkins-server: (AMI-Ubuntu 20.04 LTS,t2-small,), security group (ssh-port22, and TCP custome-port8080)

use below bash script:

#!/bin/bash

sudo apt-get update -y

sudo apt install openjdk-11-jdk -y

apt install openjdk-8-jdk -y

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins -y


### Launch instance

Step-2: Go to JENKINS GUI: 

    (a) Go to manage jenkin and click on Tools
   
    (b) JDK installation ==> Add JDK => JDK => Name => OracleJDK8 ==> JAVA_Home => /usr/lib/jvm/java-8-openjdk-amd64
   
    (c) Maven installations ==> Add Maven => Name=> MAVEN3 => Install from Apache => Version => 3.9.6 : Apply & Save


Step-3: Nexus-server setup:

    (a) Now Launch Nexus-server : take AMI:CentOS, t2-Large, security-group: Open ssh(22)(anywhere) & TCP(8081)(anywhere) ==> Advance Details ==> Bash script ==> Launch

    (b) Do SSH login and check the status using sudo systemctl status nexus == Active

    (c) Use Public ip address:8081 and login to WEB UI and set the plugins and your own Password


Step-4: Sonarqube Server Setup:

    (a) Launch an ec2 instance with Ubuntu 20.04 LTS SSD , use t2-medium instance, add SG 22 from my ip, SG 80 Anywhere, SG 9000
        Anywhere and add user data from Github.
    
    (b) Do SSH login and check the status using => sudo systemctl status sonarqube => Active

    (c) Use Public ip address:port no. and login to WEB UI and set the plugins and your own Password.


### SonarQube-Jenkins Integration:


Step-5: Jenkins Integration with SonarQube:

    (a) Go to JENKINS Dashboard ==> Manage Jenkins ==> Tools ==> SonarQube Scanner installation ==> Add SonarQube Scanner ==> Name =>sonar4.7 ==>Install automatically 
        ==>Version => SonarQube Scanner 4.7.0.2747 ==> Apply & Save


    (b) Token Generate ==> Go to SonarQube GUI ==> admin ==> my account ==> security ==> generate token ==> jenkins ==> generate ==> copy 

    (c) Go to JENKINS Dashboard ==> Manage Jenkins ==> System ==> SonarQube servers ==> Click on:Environment variables ==> Name:sonar ==> Server URL ==> http://Private IP 
        (SonarQube-server):9000 ==> Add jenkin ==> kind ==> secret text ==> paste token ==> token id:My_sonar_token ==> Description: My_sonar_Token ==> Add & save.

    (d) Build Timestamp ==> Pattern ==> remove: yy, ss & z ==> Apply & save.

### SonarQube-Jenkins Setup Credentials:

Step-6: First crosscheck the Jenkins credentials: If there is SonarQube credentials present just skip Step-6:


    (a) Go to Jenkins Dashboard ==> Manage Jenkins ==> Credentials==> System ==> Global credentials(unrestricted)
    
    (b) New Credentials ==> user_name:admin ==> Password:admin ==> Id:sonarqubelogin ==> Description: sonarqube login ==> create


### SonarQube GUI setup:

Step-7: Now Go to SonarQube GUI :
   
     (a) Go to Quality Gate ==> Create ==> Name:vprofile-QG, save-add-condition- on overall code- -bugs-100 save

     (b) Go to SonarQube Projects ==> Project settings ==> Quality Gate ==> select the quality gate:vprofile-QG ==> Project settings ==> select: webhook ==> create ==> 
         Name: jenkin-ci-webhook ==> Jenkins URL: http://http://jenkin public ip/sonarqube-webhook => create

     (c) Go to Quality Gate ==> create == Name:vprofile-QG ==> 

   
### Nexus GUI setup:

Step-8: Go to Nexus GUI:

    (a) Create repo: login to nexus server==> goto repo ==> setting-repository ==> create:repo==>select: maven2 hosted ==> Name: vprofile-repo ==> click on create repository


### Nexus-Jenkins Setup Credentials:

Step-9: Nexus setup credentials with JENKINS:

    (a) Go to Jenkins Dashboard ==> Manage Jenkins ==> Credentials==> System ==> Global credentials(unrestricted)

    (b) New Credentials ==> user_name:admin ==> Password:admin ==> Id:nexuslogin ==> Description: nexuslogin ==> create


### Creating the Continuos Integration (CI) Pipeline through Jenkins:


Step-10: Now creating the CI-CD job:


     (a) Click on +New Item ==> Item_Name:CI-CD_Project ==> Pipeline script => Copy & paste the script ==> Update the IP(pvt.IP of nexus-server) => Apply & save.

     (b) Now run the CI-CD job and monitor the complete process by clicking on the left below corner side process.

Step-11: Once the Jenkins CI - Project runs successfully then just go to the:

     (a) SonarQube GUI server 'Quality Gate' page and refresh the page ==> see the result 

     (b) Nexus GUI and see the created artifact in the repository.


 ### Supported Snapshots attached below...
     

     <img width="918" alt="jenkins_sonar_nexus-servers" src="https://github.com/Shahnawaz-yousuf/CI_JENKINS_PIPELINE_PROJECT/assets/146080117/1203c8af-b82e-4441-bcf4- 
2eecfcc03ac0">
     
<img width="439" alt="sonarqube_active_status" src="https://github.com/Shahnawaz-yousuf/CI_JENKINS_PIPELINE_PROJECT/assets/146080117/4ebf6577-444a-431b-b14d-7bc973a21502">

<img width="947" alt="Nexus-GUI" src="https://github.com/Shahnawaz-yousuf/CI_JENKINS_PIPELINE_PROJECT/assets/146080117/75cc8adb-a222-4b4e-bccc-c1abee854b76">

<img width="947" alt="Sonar-GUI" src="https://github.com/Shahnawaz-yousuf/CI_JENKINS_PIPELINE_PROJECT/assets/146080117/145e299e-d834-4a81-9111-4250c0a56eaa">

<img width="952" alt="Jenkins-Credentilas-sonar nexus" src="https://github.com/Shahnawaz-yousuf/CI_JENKINS_PIPELINE_PROJECT/assets/146080117/a1968373-9392-42c7-b08f-71c5d48f82f1">

<img width="949" alt="CI-Result" src="https://github.com/Shahnawaz-yousuf/CI_JENKINS_PIPELINE_PROJECT/assets/146080117/7289cfc4-afa4-40d2-8421-4c58cc9c92e8">

<img width="952" alt="ci-cd_homepage" src="https://github.com/Shahnawaz-yousuf/CI_JENKINS_PIPELINE_PROJECT/assets/146080117/10dc0543-b1d2-41f3-96ae-4b2240512020">

<img width="953" alt="sonarqube-reslt" src="https://github.com/Shahnawaz-yousuf/CI_JENKINS_PIPELINE_PROJECT/assets/146080117/d14f5465-41e1-4471-892e-05dc261fb8b0">

<img width="957" alt="nexus-artifacts" src="https://github.com/Shahnawaz-yousuf/CI_JENKINS_PIPELINE_PROJECT/assets/146080117/6d0b9c0e-7cde-4ab0-9830-53f715f06159">


<<<<<======CONGRATULATIONS YOU HAVE SUCCESSFULLY COMPLETED THE PROJECT=====>>>>>>

























