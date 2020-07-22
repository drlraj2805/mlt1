# Task1 
Problem: Create an Automated Web Deployment setup with the integration of GitHub, Docker & Jenkins.

1.Create a git repository and create a Jenkins job which will fetch from the master branch after every update made to the branch.
2.For the developers, create a developer branch and create a Jenkins job which will fetch from the developer branch of GitHub and launch a separate httpd container for the same.
3.In this stage, Jenkins will approach the website running in testing environment and finally merge the dev and master branch after testing.

Solution:

1.First of all, create a git repository in GitHub that will contain all the codes and webpages for the website.
(/Task1t/1.png)


![Webhook config](/Task1t/1.png)

![Webhook config](/Task1t/2.png)

Coming to  the automated Integration part, directories are created on the Base OS where the Docker containers are installed.
The various required services are started ( i.e. Jenkins, Docker).

## Creation of Jenkin jobs

### First, we setup the first Job which is to copy the changes in the feature branch on Github.
Changes are downloaded to Developercode project and a new test environment is set up using docker httpd image. 

```
NOTE : It is recommended to create a new testing environment
```

![Developercode config](/Task1t/3.png)

![Developercode config](/Task1t/4.png)

![Developercode config](/Task1t/5.png)

```

* To copy all the files to test folder linked/mounted to the testing environment:

sudo cp -rvf * /developerchanges

* To remove old test environment (if any) :

if sudo docker ps -a | grep testingenv
then
sudo docker rm -f testingenv
else
echo "no env exists"
fi

* To run new test server using httpd os image:

sudo docker run -dit -p 8085:80 -v /developerchanges:/usr/local/apache2/htdocs --name testingenv httpd
```
### Moving on to the next job, which is to detect changes to master branch and download the files to its workspace which are then transferred to the production environment. This job is Downstream to the QA job and executes only after that in the pieline  

![Mastercode config](/Task1t/6.png)

![Mastercode config](/Task1t/7.png)

![Mastercode config](/Task1t/8.png)

```
* To copy all the files to productionchanges folder linked/mounted to the docker Production environment:

sudo cp -rvf * /productionchanges

* To run new Production env using httpd docker OS:

if sudo docker ps | grep productionenv
then
echo "already running"
else
sudo docker run -itd -p 8084:80 -v /productionchanges:/usr/local/apache2/htdocs/ --name productionenv httpd
fi
```

### Now the nextjob is to merge the developer code to the master code. This is a very challenging step as it involves lots of conceptual knowledge to implement this.

![QA config](/Task1t/9.png)

![QA config](/Task1t/10.png)

![QA config](/Task1t/11.png)


## To put in a nutshell !

We assume that the developer is maintaining branches from the last checkpoint (the code already tested) so the developer creates a feature branch and commits to the changes made
and pushes it to github maintaining two branches of the same code. 

![Commit and Push](/Task1t/12.png)

As soon as the jobs are done running we can view the changes through the browser to the outside world (Client/ Public facing).  

![Output config](/Task1t/13.png)

![Output config](/Task1t/14.png)

![Output config](/Task1t/15.png)

### Key Tips :

* We can create a public IP with ngrok for the private Jenkins IP through tunnelling to make the production page visible to the outside world :
```
./ngrok http 8080
```

![ngrok config](/Task1t/16.png)

* Use of local hooks can help in faster pushing of files to the CVCS or Github

```
gedit .git/hooks/<file_name>
```
* Saving the above created file name within __**" "**__ should be keept in mind whie using Windows Notepad


### Production server updated:
![Output config](/Task1t/.png)
