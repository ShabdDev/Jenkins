### Tasks To Be Performed: Assignment 1
1. Trigger a pipeline using Git when push on develop branch
2. Pipeline should pull Git content to a folder


### Tasks To Be Performed: Assignment 2
1. Add 2 nodes to Jenkins master
2. Create 2 jobs with the following jobs:
a. Push to test
b. Push to prod
3. Once a push is made to test branch, copy Git files to test server
4. Once a push is made to master branch, copy Git files to prod server

### Tasks To Be Performed: Assignment 3
1. Create a pipeline in Jenkins
2. Once push is made to “test” a branch in Git, trigger job “push-to-test”. This will copy Git files to test node
3. If test job is successful, then push-to-prod job should be triggered
4. push-to-prod jobs should copy files to prod node

   
### Solution - Assignment 1
1. CREATE 2 EC2 INSTANCE (Ubuntu 22.04 LTS)
2. Rename the instances Jenkins-m and Jenkins-s1 and connect them
3. update both the machines
   ```
   sudo apt update
   ```
5. install java on master and slave machine
   ```
   sudo apt install openjdk-17-jdk
   ```
6. go to official website to install jenkins on linux master
   - (https://www.jenkins.io/doc/book/installing/linux/)
   - create script file and add commands inside that file
   ```
   sudo nano installation.sh
   
   sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
   https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
   echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
   https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
   /etc/apt/sources.list.d/jenkins.list > /dev/null
   sudo apt update
   sudo apt install jenkins

   bash installation.sh
   ```
7. copy the public ip of master and paste it on browser along with port 8080
8. copy the path from loaded page
9. on master machine paste it and get the contents(administrative password)
    ```
    sudo cat path
    ```
10. paste that password on dashboard
11. select option install suggested plugins
12. register

13. Manage Jenkins => Nodes (master node is already present so create slave node)
14. => New Node => slave-1 => select type as permanent agent => create
15. Remote root directory => /home/ubuntu/jenkins
16. labels => slave-1 and save

17. now select the created slave-1 node
18. configure that node
19. Launch via ssh => host => private ip of slave-1 machine
20. Credentials => add => jenkins => kind => SSH username with private key
21. Username - ubuntu => Private-key => add => paste key pair of slave-1 machine => Add
22. Credentials => select ubuntu
23. host key verification strategy => non verifying verificaition strategy => save

24. now setup the github repo having master and develop branches
25. now back to jenkins dashboard => New Item => task-1 => freestyle project
26. tick Github project => paste github project url => tick Restrict where this project can be run => Label Expression => slave-1
27. under source code management => Git => paste the git url => branches to build => */develop
28. under Triggers => tick Github hook trigger  

19. go to github => settings => select webhooks from left panel => add webhook
20. payload URL => paste jenkins url ______:8080/github-webhook/ => click add webhook
21. complete the configuration
22. click build now 

23. Go to slave1 server refresh that
24. ```
    ls
    cd jenkins/
    ls
    cd workspace/
    ls
    cd task-1 (this job will contain all those files which are there in the develop branch)
    
    ```
25. on github create new file => commit the changes
26. go to jenkins dashboard => build has been automatically started
27. on slave machine new.txt file will also be visible

### Solution - Assignment 2 

1. create new branch test on github => create file test.txt and commit the changes
2. Rename slave ec2 machine as test => refresh the test machine console
3. create new instance(ubuntu) - prod as name
4. on prod machine
   - ```
     sudo apt update
     sudo apt install openjdk-17-jdk
     java --version
     ```
5. change the name of the jenkins node from slave-1 to test and label from slave-1 to test
6. create a new node
    - remote root directory = /home/ubuntu/jenkins 
    - labels = prod
    - launch method = launch agents via SSH
    - host = copy the private IP of prod machine
    - Credentials = keep it same which used in last assignment
    - host key verification strategy = known hosts file verification strategy
7. create test job
   - name = push-to-test
   - item type = freestyle project
   - tick Github project
   - project url = copy the github repo link
   - tick restrict where this project can be run
   - label expression = test
   - source code mgt = tick git
   - repository url = copy the github repo link
   - Branch specifier = */test
   - Triggers = tick Github hook trigger for GITScm polling
   - save
8. test server
   - ```
       ls
       cd jenkins/
       ls
       cd workspace/
       ls
       cd push-to-test
       ls
     ```
9. create new file on git test branch => new.txt => check this file on test server as well
10. create prod job
   - name = push-to-prod
   - item type = freestyle project
   - tick Github project
   - project url = copy the github repo link
   - tick restrict where this project can be run
   - label expression = prod
   - source code mgt = tick git
   - repository url = copy the github repo link
   - Branch specifier = */master
   - Triggers = tick Github hook trigger for GITScm polling
   - save
   - click build now
11. master server
   - ```
       ls
       cd jenkins/
       ls
       cd workspace/
       ls
       cd push-to-prod
       ls
     ```
12. create new file on git master branch => master.txt => check this file on test server as well

### Solution - Assignment 3
1. on jenkins => jobs => push-to-test
   - configure
   - post-build actions
   - build other projects
   - Projects to build = Push-to-prod
   - triggers only if the build is stable
   - build now
   - 
