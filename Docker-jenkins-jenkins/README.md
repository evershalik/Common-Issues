# Docker-jenkins/jenkins
If you are using `docker-jenkins/jenkins` then you might face the issue of searching password into your device generally jenkins will be installed locally on the path `/var/lib/jenkins/secrets/initialAdminPassword`.

So to get the initial password to login we use `cat <path>`.

But here the issue is that we haven't installed jenkins locally on our system rather we are installing it into a container of docker so to find the initial password in such case we use container id of jenkins using `docker ps` then finding the container id of the jenkins image and using it in the command
```
docker exec <container_id> cat /var/jenkins_home/secrets/initialAdminPassword
```
