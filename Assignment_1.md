### Tasks To Be Performed:
1. Trigger a pipeline using Git when push on develop branch
2. Pipeline should pull Git content to a folder

### Solution
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










