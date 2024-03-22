
# <center> CI/CD <center/>


- [ CI/CD ](#-cicd-)
  - [What is CI?](#what-is-ci)
  - [Where is CI?](#where-is-ci)
  - [Why do we use CI?](#why-do-we-use-ci)
  - [How do we use CI?](#how-do-we-use-ci)
  - [When do we use CI?](#when-do-we-use-ci)
- [  Jenkins  ](#--jenkins--)
  - [Link the first job to second:](#link-the-first-job-to-second)
- [CI: Build with github:](#ci-build-with-github)
  - [Github](#github)
  - [Jenkins](#jenkins)
- [  Webhooks ](#--webhooks-)
- [Blockers](#blockers)
- [Jenkins continued.](#jenkins-continued)
  - [step 1 - Change the payload URL in webhook](#step-1---change-the-payload-url-in-webhook)
  - [Step 2: create a dev branch - change the branch in Jenkins job](#step-2-create-a-dev-branch---change-the-branch-in-jenkins-job)
  - [step 3: make change on job to listen on dev](#step-3-make-change-on-job-to-listen-on-dev)
  - [step 4: make change to code check works on code](#step-4-make-change-to-code-check-works-on-code)
  - [Merge job/build](#merge-jobbuild)
- [CD: Incorporating AWS](#cd-incorporating-aws)
  - [Make Instance](#make-instance)
  - [Jenkins](#jenkins-1)
      - [rsync](#rsync)
- [References](#references)



![alt text](diagram.png)
## What is CI?


The DevOps practice of automating build and running tests after a group of devs have frequently merged there code into a central place (repository) is called Continuous Integration or CI. The goal of CI is to reduce the time taken to find and fix bugs, release software and improve general software quality.


## Where is CI?

With continuous integration, it is always possibly deployment ready, even during development as  the system always runs.

In regards to how we can use CI in practice, please see below in **How do we use CI**


## Why do we use CI?

Previously developers would work in isolation and not push code frequently, only when work was completed. The effects of this were that bugs would go unnoticed for a long time, and therefore take longer to fix. In the longterm, quickly delivering updates to customers became very difficult. CI solves this issue. The quick feedback is necessary for quick deployment

## How do we use CI?

As aforementioned, developers commit code to their shared repository frequently with Version Control systems like Git. Devs will normally run theor own unit tests to provide an additional check before merging with other devs code. Then a CI service such as Jenkins will automatically build and run tests on new code.

An example of CI within agile development:
<br>
*   Develop: Develop describes the practices necessary to implement stories and commit the code and components to version control 
*   Build: techniques needed to create files & merge development branches into main.
*   Test: Test end-to-end describes the testing to ensure we have reached our solution.
*   Stage describes the steps required to host and validate solutions in a staging environment before production
![alt text](<Screenshot 2024-03-20 at 10.18.46.png>)



## When do we use CI?

Ideally before you build it is best to start your pipeline build plan.


#  <center>  Jenkins  <center/>

Jenkins is a self-contained, open-source automation server used to assist in automating a variety of tasks in regards to building, testing, delivering, and deploying software. It allows us to help the process of continuos integration and continuos delivery

Jenkins can be installed through native system packages, container tools such as Docker, or simply ran standalone by a machine with the Java Runtime Environment (JRE) installed.
Everything we do on Jenkins is called a job.

To create a jenkins build we must select **new item**, **name** our project and select **freestyle**. 
<br>

![alt text](<Screenshot 2024-03-20 at 11.52.37.png>)

![alt text](<Screenshot 2024-03-20 at 11.19.40.png>)

<br>

Next choose **discard old jobs** and as a **maximum number of builds to keep** select 3. 

At present we are skipping the other options so scroll down to **add build step** or you can navigate across the top to **build** and choose the 3rd option: **execute shell**
![alt text](<Screenshot 2024-03-20 at 11.55.28.png>)


Just as a test this is the data I entered:
``` 
whoami
hostname
uname -a
``` 
<br>

Once job is saved click **build now** :

<br>

![alt text](<Screenshot 2024-03-20 at 11.38.10.png>)


As you can see below we have a **succesful build**:
<br>

![alt text](<Screenshot 2024-03-20 at 11.31.41.png>)

## Link the first job to second:


Now that both jobs have been succesful:

In the homepages on our first job hover in homepages and choose **configure**.

![alt text](<Screenshot 2024-03-20 at 12.02.53.png>)


Now within **Post-build Actions** choose to **trigger only if build is stable**.
<be>

![alt text](<Screenshot 2024-03-20 at 11.44.04.png>)

Then save and click **build now** you will then see the build history click on the build you want.

![alt text](<Screenshot 2024-03-20 at 11.38.10.png>)
![alt text](<Screenshot 2024-03-20 at 11.49.39.png>)

You can then check **console outlook** and you can see it has triggered the second job.

![alt text](<Screenshot 2024-03-20 at 11.48.10.png>)


#  CI: Build with github:


## Github
First you must make sure the repo is linked to a specific key. so create a key in your terminal then go to **settings** within repo, **deploy keys** and input your private key, avoiding whitespaces. Next allow write acces so we can pull as well as push.

## Jenkins
Next we go over to Jenkins and select **new build**, **freestlye project**, 	**Max # of builds to keep** as 3 and select **GitHub project**

* First: We used the **HTTPS** link to repo and inputted that. 

![alt text](<Screenshot 2024-03-20 at 13.33.09.png>)

I found that weirdly it didn't always work when I didn't add a "/" so to be safe, add it.
![alt text](<Screenshot 2024-03-22 at 12.42.03.png>)

* Next we chose our label expression **sparta-ubuntu-node**:
  
![alt text](<Screenshot 2024-03-20 at 13.34.10.png>)

* After this, we used the ssh of our git repo link:
![alt text](<Screenshot 2024-03-20 at 13.35.40.png>)

* There after we linked our key. by choosing the options below and inputting our **Private key* including the 
  * -----BEGIN OPENSSH PRIVATE KEY-----
  * -----END OPENSSH PRIVATE KEY-----
  * All the whitespace after the end private key

![alt text](<Screenshot 2024-03-20 at 12.22.58.png>)


![alt text](<Screenshot 2024-03-22 at 12.45.15.png>)

![alt text](<Screenshot 2024-03-22 at 12.47.08.png>)

*To note we change this branch to dev later once CI job is esatblished.

**Build environment**
![alt text](<Screenshot 2024-03-21 at 17.56.43.png>)

our user data:
``` 
cd app
npm install
npm test
``` 

Below we can see our buil was successful.
![alt text](<Screenshot 2024-03-20 at 14.53.54.png>)

# <center>  Webhooks <center/>

Webhooks let you subscribe to events happening in a software system and automatically receive a delivery of data to your server whenever those events occur. Webhooks are used to receive data as it happens, in this case my intention was to get my webhook to connect with jenkins and create automatic builds.


Next we needed to configure a web hook to get the automatic builds.

Within our Github repo navigate to **settings** then to **webhooks** on the left hand pane.

Once there enter the jenkins url endpoint and **"github-webhook/"**:
<br>
``` 
http://35.176.97.54:8080/github-webhook/
```

Note: this endpoint will change but must always be called **github-webhook/**. If you ever need to change webhook url it is better to delete and recreate webhook, as update is not always effective.

![alt text](<Screenshot 2024-03-20 at 15.40.01.png>)

A **secret** is optional, so I left that and left **content type* blank.

I found my webhook did ot work without a name, so I added a name, after looking up my issue.
As you can see my webhook has been created and is pinging.
![alt text](<Screenshot 2024-03-20 at 15.09.10.png>)
![alt text](<Screenshot 2024-03-20 at 15.39.32.png>)

Once webhook is working, navigate to jenkins and make this change.

![alt text](<Screenshot 2024-03-20 at 15.36.28.png>)

You will see I only had 4 builds:


![alt text](<Screenshot 2024-03-20 at 15.08.18.png>)

I went to my vsc and made some changes to a file to test it and as you can see below, a 5th build was triggered.

![alt text](<Screenshot 2024-03-20 at 15.37.18.png>)


Lastly, I did a few other pushes and for the penultimate one, you can see what is happeneing in real time!

![alt text](<Screenshot 2024-03-20 at 16.33.54.png>)




# Blockers

It seemed we had a different version of Node to Sharukh, as we were all getting errors, so we had to git clone his repo, delete his .git folder with

```
an
ls -a
rm -rf .git
```
I manually copied and pasted all files to my other repo.

Then push back up too our repos. Once we did this, we no longer got Jenkins build errors on the 2nd builds.

# Jenkins continued.

**using the same jenkins file as yesterday**

## step 1 - Change the payload URL in webhook

AWS uses dynamic ip's, they change everytime you stop and start an instance so we must update the webhook. Azure uses static, so this step would not be needed with azure.
To do this we had to update the webhook with a new endpoint. 
I found that when I just **updated the url it didnt work, so I deleted and remade another webhook**, which was successful. After that I pushed some code to check the CI worked as it did before, after making that webhook change.

## Step 2: create a dev branch - change the branch in Jenkins job
Dev is important to work in so that we do not make errors, we will now remain in dev.

Next we had to create our dev branch which I did by running

```
git branch dev
git checkout dev <--- this will switch you to dev
```
I then made a change to my read.me within the dev branch to check I could push to dev.<br>

## step 3: make change on job to listen on dev
**Next I had to make some changes in jenkins to ensure success.
Make a change on the job to listen on dev, by changing the branch my job is paying attention too.
![alt text](<Screenshot 2024-03-21 at 16.42.54.png>)
<br>

## step 4: make change to code check works on code

By pushing to github I was able to succesfully trigger the ci job on jenkins.

## Merge job/build
Now that I had jenkins monitoring dev, my next focus was to create a job that merges code from our dev branch to main.



I created a **new build** with a **freestyle** project, **Discard old builds** and **Max # of builds to keep** as 3. I also made a note of what this build was for.

![alt text](<Screenshot 2024-03-22 at 12.56.48.png>)

Next choosing our githib project: 
To note I copied and pasted my HTTPS repo but found it was acting up unless I added a / at the end.

![alt text](<Screenshot 2024-03-21 at 16.56.36.png>)


Under **Source Code Management**:

it is important we specify **dev** as that is the branch we want to merge **from** to master branch. 

![alt text](<Screenshot 2024-03-21 at 16.58.38.png>)

I left **Build Triggers** and **Build Environment** blank.

Under **Post-build Actions** then lastly selected **git publisher** and choose the **branch to push** as master and **target remote name** as **Origin**.
![alt text](<Screenshot 2024-03-21 at 17.01.07.png>)


Now that I had set this job, I had to go back to the first job and link the two. You will notice, this merge job that we just created will only trigger if the first job is built in a stable way.


![alt text](<Screenshot 2024-03-21 at 17.04.25.png>)


Next, I changed some code in my repo (in dev) and pushed it.


Originally, I was getting this notice because my branch in my merge job was set as master instead of dev.
But once I had configured my Ci job file properly it dissapeared.
![alt text](giterror.png)
But once I had amended it to dev within my merge file it dissapeared and the jobs ran!


# CD: Incorporating AWS

## Make Instance
We made an EC2 instance with a **security group** that had 
 port 3000, 80 and 22 inbound enabled.We used a specific image called **ami-02f0341ac93c96375**

## Jenkins
next we made a new CD build on jenkins. <br> 
**Source Code Management**
![alt text](<Screenshot 2024-03-21 at 17.14.48.png>)
<br>

note: leave **build triggers** empty
<br>
**Build Environment**
![alt text](<Screenshot 2024-03-21 at 17.15.50.png>)
<br>
Then in **Build** > **EXECUTE SHELL**
we want to do the following:
* Ensure security group allows ssh to jenkins ip
* Ensure file.pem provided
* Run an upgrade
* Install nginx

```
ssh -o "StrictHostKeyChecking=no" ubuntu@18.201.178.162 <<EOF
	sudo apt-get update -y
    sudo apt-get upgrade -y
    sudo apt-get install nginx -y
    sudo systemctl restart nginx
```
The ip address is the ip of my aws instance.

once you run this build you should find that if you enter the below into a browser:
<br>
```
http://<ipaddress>:3000
```
You will see the sparta app!


Lastly, as I could see that my environment and app folder was in my merge workspace:

![alt text](<Screenshot 2024-03-21 at 17.47.24.png>)


<br>

####  rsync

I added these rsync lines above to move the app folder and the environment folder over to our vm instances, note I have added ip addreess, and updated the ones that were there before because vm was restarted.

![alt text](<Screenshot 2024-03-21 at 17.46.03.png>)
```
rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ubuntu@18.201.178.162:/home/ubuntu/
rsync -avz -e "ssh -o StrictHostKeyChecking=no" environment ubuntu@18.201.178.162:/home/ubuntu/
```



I ssh'd into my ec2 vm and followed the next steps:

Before following the steps, its important to note the file structureto note that I go into the app folder in environment, then into the other in home.
![alt text](<Screenshot 2024-03-21 at 17.28.56.png>)


```
cd environment/app

# Give the script executable permissions
sudo chmod +x provision.sh

# execute the script
./provision.sh

# navigate to new app folder(in home)
cd 
cd app

# install npm 
npm install

# start npm 
npm start
```


Again I was able to see the Sparta app working!
Next I went back to the merge job (our second job) to add the post build trigger, again only to be built if the job before it was succesful.
![alt text](<Screenshot 2024-03-21 at 17.31.23.png>)

To check this on my **personal computer**, I went back to my **dev branch** and made a change to the app htmlby adding "2024" to the last sentence.
![alt text](<Screenshot 2024-03-21 at 17.33.53.png>)
![alt text](<Screenshot 2024-03-21 at 17.32.32.png>)

I then pushed this to github and lastly watched to see if the jobs ran on jenkins, which they did. I then went to my ip:3000 and my app had changed inline with my changes!

![alt text](<Screenshot 2024-03-21 at 16.54.06.png>)

#Jenkins further

*working on cd jenkins job as above*

Now we want to edit the job so that the
job allows to never need to ssh into our ec2 instances again.

To do this we added the following to our final cd job.

```
  cd app
   kill -all -9 npm
   npm install
   nohup node app.js > /dev/null 2>&1 &

```
All in all our final job code looks like this:

```
rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ubuntu@54.217.159.235:/home/ubuntu/
rsync -avz -e "ssh -o StrictHostKeyChecking=no" environment ubuntu@54.217.159.235:/home/ubuntu/

# ensure security group allows ssh to jenkins ip
# ensure file.pem provided
# ensure ec2 running
#navigate to new app folder
# npm nstall
# npm start
# do not worry about nginx 
#once it works - go back to merge job and add post build actions to trigger cd job.
#2024 end of first line
ssh -o "StrictHostKeyChecking=no" ubuntu@54.217.159.235 <<EOF
	sudo apt-get update -y
    sudo apt-get upgrade -y
    sudo apt-get install nginx -y
    sudo systemctl restart nginx
    
    
   cd app
   kill -all -9 npm
   npm install
   nohup node app.js > /dev/null 2>&1 &
```


![alt text](<Screenshot 2024-03-22 at 12.09.19.png>)

Once that was completed I went into my app > views > index.ejs and made some changes to the html text > as you can see below it changed on the ip:3000....

![alt text](<Screenshot 2024-03-22 at 12.21.03.png>)

# References

 *   https://medium.com/@ahshahkhan/devops-culture-and-cicd-3761cfc62450
 *   https://aws.amazon.com/devops/continuous-integration/\https://scaledagileframework.com/continuous-integration/
 *   https://scaledagileframework.com/continuous-integration/#:~:text=Continuous%20integration%20is%20a%20critical,potentially%20deployable%2C%20even%20during%20development.
*    https://docs.github.com/en/webhooks/using-webhooks/creating-webhooks
* https://www.youtube.com/watch?v=Uu8_cb0WRAw


