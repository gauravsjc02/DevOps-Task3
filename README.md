<h1>DevOps-Task3</h1>
<h2>Objective :-</h2>
<h3>To Integrate Kubernetes with Jenkins.</h3>

<h3>Steps :-</h3>

1. Create container image that has Jenkins already installed using dockerfile or We can use the Jenkins Server on RHEL 8/7
2.  When we launch this image, it should automatically start Jenkins service in the container.
3.  Create a job chain of job1, job2, job3 and  job4 using build pipeline plugin in Jenkins. 
4.  Job1 : Pull  the Github repo automatically when some developers push repo to Github.
5. Job2 : 
    1. By looking at the code or program file, Jenkins should automatically start the respective language interpreter installed image container to deploy code on top of Kubernetes ( eg. If code is of  PHP, then Jenkins should start the container that has PHP already installed )
    2.  Expose our pod so that testing team could perform the testing on the pod
    3. Make the data to remain persistent ( If server collects some data like logs, other user information )
6.  Job3 : Test our app if it  is working or not.
7.  Job4 : If app is not working , then send email to developer with error messages and redeploy the application after code is being edited by the developer

<h3>Requirements :-</h3>
 
 1. Knowledge of Docker.
 
 2. Knowledge of Kubernetes.
 
 3. Knowledge of Jenkins.
 
 <h3>Solution :-</h3>
 
 First of all, I have created a Dockerfile having kubectl and jenkins configured.
 
 ![1](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/dockerfile.png)
 
 After this we build it using the command <b> docker build -t kubejenkins:v2<b>
  
 Here, kubejenkins is the image name and v2 is the version name.
 
 To upload this image on DockerHub use:-
 
 <b>1. docker login.  
 2. docker tag kubejenkins:v2  gauravsjc02/kubejenkins:v2  
 3. docker push gauravsjc02/kubejenkins:v1 <b>
  
 gauravsjc02 is my DockerHub account name.
 
 Now, creating Deployment and Pvc for Jenkins using the above image.
 
 <b>PVC and Deployment :-<b>
 
 ![2](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/jenkinpvc.png)   
 
 ![3](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/jenkindeploy.png)
 
 We can Expose the deployment using the command:-
 
 <b>kubectl expose deployment myjenkins --type=NodePort --port=8080<b> 
  
 Now, To get the port for jenkins server use:-
 
 <b>kubectl get all <b>
  
 ![4](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/ip.png)
 
 <b>Setting up Jenkins:-<b>
  
  To get the jenkins password use the command :-
   
  <b>kubectl logs podname<b>     
  
  Here,the podname should be the name of our pod.
  
  ![5](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/jenkinpass.png)
  
  After the setup is complete, we'll create our jobs.
  
  <h3>JOB 1 :-</h3>
  
  ![6](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/job1.png)
  ![7](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/job1.1.png)
  
  Job 1 is the upstream job which will pull the repositories from the github. Here webhook triggers are used for automatically connecting or informing the jenkins :- as soon as the developers pushes any code or changes anything in the repositories.
  
  <h3>JOB 2 :-</h3>
  
  ![7](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/job2.png)
  ![8](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/job2.1.png)
  
  It is the downstream job of Job 1. Job 2 will automatically start the respective language interpreter by looking at the code.
  
  <h3>JOB 3 :-</h3>
  
  ![9](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/job3.png)
  
  This job is downstream job of Job 2. This job is used for testing purpose. It will check whether the site or code is working or not.
  
  <h3>JOB 4 :-</h3>
  
  ![10](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/job4.png)
  ![11](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/job4.1.png)
  
  This job will only work if our Job 3 fails, as this job is used for notifying the developers about the errors.
  
  It will redeploy the application after the code is edited by some developers.
  
  <h3>Build Pipeline View :-</h3>
  
  ![12](https://github.com/gauravsjc02/DevOps-Task3/blob/master/task3.0/build.png)
  
  We can see that our job 4 is not triggered as our job 3 is successfull.
  
  
  

 
 
