# Task1 
Problem: Create an Automated Web Deployment setup with the integration of GitHub, Docker & Jenkins.

1.Create a git repository and create a Jenkins job which will fetch from the master branch after every update made to the branch.

2.For the developers, create a developer branch and create a Jenkins job which will fetch from the developer branch of GitHub and launch a separate httpd container for the same.

3.In this stage, Jenkins will approach the website running in testing environment and finally merge the dev and master branch after testing.

##Solution:

##Prerequisite:

-Basic linux
-Git and Github
-Jenkins
-php/html

so here we start :

1.First of all, create a git repository in GitHub that will contain all the codes and webpages for the website.

![git](/Home_Assignment/309.png)



## Jenkins Jobs:
## JOB 1:
This job will deploy the website to the main web server. For the we need to integrate our GitHub repository to this job of Jenkins.


![job1](/Home_Assignment/310.png)

Under source code management copy the URL of GitHub repository in Repositories section.


![job1](/Home_Assignment/311.png)
Select the branch Master.

Now, under Build Triggers section select Poll SCM.

![job1](/Home_Assignment/312.png)

Now under the Execute Shell of Buil section write the following code which will launch the httpd container after checking it's pre-existing or not.

# sudo cp -v -r -f * /wpage

# if sudo docker ps | grep myweb6

# then

# echo "Already launched"

# else

# sudo docker run -d -t -i -p 8085:80 -v /wpage:/usr/local/apache2/htdocs --name myweb6 httpd

# fi

![job1](/Home_Assignment/313.png)

## JOB 2:
This will send the code to the Test team for testing.

![job2](/Home_Assignment/314.png)

![job2](/Home_Assignment/315.png)

![job2](/Home_Assignment/316.png)

![job2](/Home_Assignment/317.png)

![job2](/Home_Assignment/327.png)


## JOB 3:
This job will execute after the successful completion of job 2 and deploy the new code and webpages after the testing to the main web server which will publically available.

![job3](/Home_Assignment/318.png)

![job3](/Home_Assignment/319.png)

![job3](/Home_Assignment/320.png)

![job3](/Home_Assignment/326.png)

![job3](/Home_Assignment/321.png)

![job3](/Home_Assignment/322.png)

![job3](/Home_Assignment/307.png)

thanks for reading !
this work is done under guidance of # vimal daga sir 
