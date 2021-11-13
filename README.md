# CI_CD_DOCKER <img src="https://open-source-guide.com/var/site_smile/storage/images/guide-os/actualites/le-moteur-docker-disponible-en-version-20-10/869332-1-fre-FR/Le-moteur-Docker-disponible-en-version-20.10_actualite_home.png" width="160" height="130"/>  


## Requirements:
* Install Docker on each of the virtual machines by these commands:
sudo apt-get update && apt-get upgrade -y
curl -fsSL https://get.docker.com/ -o get-docker.sh
bash get-docker.sh
sudo apt-get update && apt-get upgrade -y
* Node.js 12.x or higher
* PostgreSQL (can be installed locally using Docker)
* Free Okta developer account for account registration, login


## Steps:
* Write a Dockerfile for your NodeWeightTracker application and add it to the application’s repository.
* Configure your pipelines using Yaml pipelines that uses different stages to separate the processes and integrates with the “Environments” feature so that, among other things, you can configure approvals for the stage that deploy to production. Your Yaml consists from: CI, CD staging and CD production
* Create Environments in azure devops:
![image](https://user-images.githubusercontent.com/71599740/141457206-681a5592-456c-4823-b916-94ec859592e8.png)
* For each environments add a resource - virtual machine and run a script that makes them agent. 
 ![image](https://user-images.githubusercontent.com/71599740/141457412-12b046ca-0e51-4ac8-b2d9-3e22a2024ccd.png)
![image](https://user-images.githubusercontent.com/71599740/141457436-f8a8a12a-082d-4da1-ac0e-036f58d12c83.png)
![image](https://user-images.githubusercontent.com/71599740/141457486-41b913c5-d7fa-432c-a3ad-790bb72134ef.png)
* For each virtual machine you should get the docker permissions: 
1. cd /var/run 
2. sudo chmod 777 docker.sock (you can add a crontab that run this command automatically)
* Create Environment Variables in azure pipeline such as - OKTA, PGHOST, PGUSERNAME and more and pass them by the command of docker run -d -p 8080:8080  -e PGHOST=$(PHOST)....
(the azure pipeline attached above)
![image](https://user-images.githubusercontent.com/71599740/141465793-5ff47f44-a92e-4d7e-ae1d-dd4c3be3aff0.png)

* Run the pipeline and check that your applications are running :)

The full process:
![image](https://user-images.githubusercontent.com/71599740/141464435-f83b4376-b5f3-4359-91d8-e8542bba40bf.png)

![image](https://user-images.githubusercontent.com/71599740/141467407-40ab1918-1bbb-45cc-9ff3-e4c438780667.png)


The staging:
![Staging_docker](https://user-images.githubusercontent.com/71599740/141458447-02888c6e-c109-4d3c-bb3b-86c10aa381b2.PNG)

The production:
![Production_docker](https://user-images.githubusercontent.com/71599740/141458484-9e04fc8a-6277-46a4-acc5-ca8286cbe008.PNG)


## Branches in azure devops repo:
In this project we will manage the code of our application using one of the most common and simple Git workflow usually called the “Feature Branch Workflow” in which branches named with the prefix “feature/” are used to work on the code independently and then the code is integrated into the master/main branch to be deployed in the target environments.
For Feature Branches:

Whenever a new change is pushed to a feature branch this should start the CI pipeline that will take the Dockerfile from the repository and build an image to ensure that everything is ok (note that we don’t want to push it to the registry since we have not yet reached the integration branch, our only objective is to provide the developer with an indication of whether their code is correct or not).
For Master/Main Branch:

Whenever a new change is pushed to the main/master branch this should start the CI process that will take the Dockerfile from the repository, build an image and push it to a container registry (Azure ACR).
Then the CD consists on pulling the image from the registry and deploying it into the staging environments automatically (Continuous Deployment) and then wait for approval to deploy into the Production environment (Continuous Delivery)

![branches](https://user-images.githubusercontent.com/71599740/141464330-dc1100bb-0a8c-4273-9c68-995688e189c9.PNG)
![commits](https://user-images.githubusercontent.com/71599740/141464343-a592d2eb-cca9-4767-a53c-6583cdc62079.PNG)
![image](https://user-images.githubusercontent.com/71599740/141464504-a2dd0f52-5705-4aca-a614-3134a9eabf1c.png)








# Emphasis:
* To provision the infrastructure I've used my previous project: </br>
The terraform repository - https://github.com/inbalLevi/CI_CD_Terraform </br>
* To install all dependencies on the nodes and to deploy and run the application for the first time I've used this project - https://github.com/inbalLevi/bootcamp-app

