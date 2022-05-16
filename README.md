## How to create an SSH key

create ssh connection from local to github

- From terminal `cd .ssh`

- Enter `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

  - The following prompt "Enter a file in which to save the key (/Users/you/.ssh/id_rsa):" Enter file name
  - No passphrase - hit enter

- To access the public key enter `cat eng110_jack.pub` and copy the key

## Saving the SSH key to Github

- From Github head to Settings then SSH and GPG keys

- From SSH and GPG keys select new key
- Name same as SSH key
- Paste key from file.pub

### Testing for success

- From Github head to your repository
- Clone the required repository
  - Github ssh
- From terminal `cd` into desired directory
- `git clone` using the ssh
- Make a change
- `git add` > `git commit -m ""` > `git push`
- If the change has been pushed then it worked as intended

## Creating a job on Jenkins

By default runs on port 88

- Log into Jenkins(this is an EC2 instance on AWS)
- Select 'New jobs' found on the left hand side

  - Name as per usual
  - Select 'Freestyle job'
  - Select 'Configure' from the drop down list
  - Add a description
  - Check 'Discard old builds'
  - Set 'Max instances' to 3
  - Add 'Build step'
  - Select 'Execute shell'
  - Enter commands then save

- Commands =

```
# Which OS does it support?

uname -a

# Which time zone/date

date

# Which environment does it support?

# Does it have node support?
```

- Select the drop down menu from the name of your build and select 'Build now'

- Select the name of your build and look for 'Build history' on left

- Select the job and from the drop down select 'Console output'

## Linking builds together

- Create a new build following the same steps as before

  - In commands enter `printenv`

- On first build:

  - Select the job and from the drop down menu select 'Configure'
  - Scroll down and fine 'Add post build actions'
  - From the drop down menu select 'build other projects'
    - Input the name of the build you wish to linked
    - Check 'Trigger if build is stable'

- Ensure that before jobs are linked that each job has been tested individually so that you know they work

## Deploying a key on a Github repository

- From Github make your way to the repository that you want to deploy a key with
- Head to the 'Settings'
  - 'Deploy key'
    - 'Add key'
    - Paste the private key

## Giving Jenkins SSH access to the Github repository

- Create a new job for Jenkins

- Configuration:

  - Check 'Discard old builds'
  - Set 'Max instances' to 3
  - Github project check
    - Enter the Github URL (HTTPS)
  - Restrict where this project can be run and enter sparta-ubuntu-node

  - In 'Source code management':

    - Select Git
    - In 'Repositories/Repository URL' copy the SSH from the Github repository (Check SSH and copy)

    - In 'Credentials' press 'Add' and select 'Jenkins'
      - 'Kind' should be 'SSH Username with private key'
      - 'Username' should be the same as the SSH name
      - 'Private key' select 'enter directly'
      - The key should be the public key (Found in .ssh `cat 'file'` - NOT the .pub) COPY EVERYTHING

  - In 'Build environment' check 'Provide Node & npm bin/ folder to PATH'

  - In 'Build' set the commands:

```
cd starter-code/app
npm install
npm test
```

- Build job and then check the console output

## Creating a Github webhook

- On your Jenkins' build select configure

  - Check Github hook trigger for GITScm polling

- On your Github repository head to 'Settings' then select 'Webhook'

  - Select 'Add new'
  - Enter the payload URL
  - Select 'application/json' from the drop down menu
  - Check 'Send me everything'

- Tada!

Make a request Jenkins clones the repo to the server

Jenkins dashboard > CI job > configure > branch specifier change to dev branch
post build actions > build other projects >

3rd job
ssh agent > pem file > ssh username > pem key

## Building a job to merge branches

- Change job 1 'Branch specifier' to 'dev'

- Create new build
- Configuration:

  - Check 'Discard old builds'
  - Set 'Max instances' to 3
  - Github project check
    - Enter the Github URL (HTTPS)
  - Restrict where this project can be run and enter sparta-ubuntu-node

  - In 'Source code management':

    - Select Git
    - In 'Repositories/Repository URL' copy the SSH from the Github repository (Check SSH and copy)
    - Add required credentials
    - Change 'Branch Specifier' to required branch i.e. 'dev'
    - Select 'Merge before build' from 'Additional Behaviours'
      - Name of repository 'origin'
      - Branch to merge to 'master'
    - Check Github hook trigger for GITScm polling
    - Post-build Actions check 'Push only if build succeeds' and 'Merge results'
    - Save

## Connecting Jenkins to AWS EC2

- Build a new job
- Configuration:

  - Check 'Discard old builds'
  - Set 'Max instances' to 3
  - Restrict where this project can be run and enter sparta-ubuntu-node
  - Check 'SSH Agent' and select 'eng119'
  - Build with following commands:

    ````
    # ssh into ec2
    # update upgrade, run the provisioning script or install nginx to test
    # scp to copy data from github to ec2
    rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ubuntu@ec2-52-18-33-32.eu-west-1.compute.amazonaws.com:~/.
    ssh -A -o "StrictHostKeyChecking=no" ubuntu@52.18.33.32 << EOF

        #export DB_HOST=mongodb://54.75.96.210:27017/posts
        #sudo apt-get update -y
        #sudo apt-get upgrade -y
        #sudo apt-get install nginx -y
        #sudo systemctl restart nginx
        #sudo systemctl enable nginx
      # scp -i eng110 -r ~/app ubuntu@ec2-52-18-33-32.eu-west-1.compute.amazonaws.com:/home/ubuntu/
        #cd folder/env/app/
        #sudo chmod +x provision.sh
        #sudo /.provision.sh
        #cd app
        #npm install
        #npm start

        # pm2 kill all
    EOF


    # Create a another job for db
    #```
    # rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ubuntu@ip:/home/ubuntu
    # rsync -avz -e "ssh -o StrictHostKeyChecking=no" environment ubuntu@ip:/home/ubuntu
    # ssh -o "StrictHostKeyChecking=no" ubuntu@ip <<EOF
        #sudo bash ./environment/provision.sh
        #cd app
        #pm2 kill
        #pm2 start app.js
    #EOF
    #```
    ````

We do not need to specify the pem file because Jenkins already has access.

`rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ubuntu@ec2-52-18-33-32.eu-west-1.compute.amazonaws.com:~/.` - this takes the app folder from the Github repository and transfers it to the EC2 instance

`ssh -A -o "StrictHostKeyChecking=no" ubuntu@52.18.33.32` - this bypasses the yes/no section of ssh-ing into an EC2

# Pipeline process from job 1 to job 4

Standard configuration for all jobs:

- Check 'Discard old builds'
- Set 'Max instances' to 3
- Github project check
  - Enter the Github URL (HTTPS)
- Restrict where this project can be run and enter 'sparta-ubuntu-node'

- In 'Source code management':

  - Select Git
  - In 'Repositories/Repository URL' copy the SSH from the Github repository (Check SSH and copy)
  - Add required credentials
  - Check Github hook trigger for GITScm polling

## CI

Standard configuration

- In 'Source code management':
  - Branch specifier should be \*/dev
- In 'Build environment':
  - 'Provide Node & npm bin/ folder to PATH'
- In 'Builds':
  - Execute shell from drop down list
    ```
    cd app
    npm install
    npm test
    ```
- In 'Post-build Actions':
  - Choose 'Projects to build' from drop down list
  - Enter CI Merge job name
  - Check 'Trigger only if build is stable'

### CI Merge

Standard configuration

- In 'Source code management':

  - Change 'Branch Specifier' to required branch i.e. 'dev'
  - Select 'Merge before build' from 'Additional Behaviours'
    - Name of repository 'origin'
    - Branch to merge to 'master'

- In 'Post-build Actions':
  - Choose 'Projects to build' from drop down list
  - Enter CD MongoDB job name
  - Check 'Trigger only if build is stable'
  - Choose 'Git Publisher' from drop down list
    - Select 'Push Only If Build Succeeds'
    - Select 'Merge Results'

### CD for the MongoDB

Standard configuration

- In 'Source code management':
  - Branch specifier should be \*/master (or main)
- In 'Build environment':
  - 'SSH Agent'
    - In 'Credentials' press 'Add' and select 'Jenkins'
    - To add pem key:
    - 'Kind' should be 'SSH Username with private key'
    - 'Username' should be the same as the pem file name
    - 'Private key' select 'enter directly'
    - The key should be the pem key (Found in .ssh `cat 'file.pem'`) COPY EVERYTHING
- In 'Builds':

  - Execute shell from drop down list

    ```
    ssh -A -o "StrictHostKeyChecking=no" ubuntu@34.244.29.41 << EOF

      sudo apt-get update -y

      sudo apt-get upgrade -y

      sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv D68FA50FEA312927

      echo "deb https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list

      sudo apt-get update -y

      sudo apt-get upgrade -y

      sudo apt-get install -y mongodb-org=3.2.20 mongodb-org-server=3.2.20 mongodb-org-shell=3.2.20 mongodb-org-mongos=3.2.20 mongodb-org-tools=3.2.20

      sudo systemctl status mongod

      sudo systemctl start mongod

      sudo systemctl enable mongod

      sudo chown ubuntu: /etc/.
      sed '24d' /etc/mongod.conf -i
      awk 'NR==24{print "  bindIp: 0.0.0.0"}7' /etc/mongod.conf > change && mv change /etc/mongod.conf
      sudo chown root: /etc/.
      sudo systemctl restart mongod
      sudo systemctl enable mongod
      sudo systemctl status mongod
    ```

  EOF

  ```

  ```

- In 'Post-build Actions':
  - Choose 'Projects to build' from drop down list
  - Enter CD job name

### CD

Standard configuration

- In 'Source code management':
  - Branch specifier should be \*/master (or main)
- In 'Build environment':
  - 'SSH Agent'
    - In 'Credentials' press 'Add' and select 'Jenkins'
    - To add pem key:
    - 'Kind' should be 'SSH Username with private key'
    - 'Username' should be the same as the pem file name
    - 'Private key' select 'enter directly'
    - The key should be the pem key (Found in .ssh `cat 'file.pem'`) COPY EVERYTHING
- In 'Builds':

  - Execute shell from drop down list

  ````
      # ssh into ec2
      # update upgrade, run the provisioning script or install nginx to test
      # scp to copy data from github to ec2
      rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ubuntu@ec2-52-18-33-32.eu-west-1.compute.amazonaws.com:~/.
      ssh -A -o "StrictHostKeyChecking=no" ubuntu@52.18.33.32 << EOF

          export DB_HOST=mongodb://34.244.29.41:27017/posts

          sudo echo "export DB_HOST=mongodb://34.244.29.41:27017/posts" >> ~/.bashrc

          source ~/.bashrc

          sudo apt-get update -y
          sudo apt-get upgrade -y
          sudo apt-get install nginx -y
          sudo systemctl restart nginx
          sudo systemctl enable nginx

          cd app

          curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

        sudo apt-get install -y nodejs

        sudo apt-get update -y

        npm install

        sudo node seeds/seed.js

          nohup node app.js > /dev/null 2>&1 &

          #cd app
          #npm install
          #npm start

          # pm2 kill all
      EOF


      # Create a another job for db
      #```
      # rsync -avz -e "ssh -o StrictHostKeyChecking=no" app ubuntu@ip:/home/ubuntu
      # rsync -avz -e "ssh -o StrictHostKeyChecking=no" environment ubuntu@ip:/home/ubuntu
      # ssh -o "StrictHostKeyChecking=no" ubuntu@ip <<EOF
          #sudo bash ./environment/provision.sh
          #cd app
          #pm2 kill
          #pm2 start app.js
      #EOF
      #```
  ````

#### To reverse proxy

```
# Allows us to edit the file at /etc/nginx/sites-available/default
    sudo chown ubuntu: /etc/nginx/sites-available/.

    # Deletes lines 49-51 in the default file we now have permission to
    sed '49,51d' /etc/nginx/sites-available/default -i
    # Inserts all the necessary lines in the default file to set up a reverse proxy for nginx
    awk 'NR==49{print "             proxy_pass http://localhost:3000;"}7' /etc/nginx/sites-available/default > change && mv change /etc/nginx/sites-available/default
    awk 'NR==50{print "             proxy_http_version 1.1;"}7' /etc/nginx/sites-available/default > change && mv change /etc/nginx/sites-available/default
    awk 'NR==51{print "             proxy_set_header Upgrade \$http_upgrade;"}7' /etc/nginx/sites-available/default > change && mv change /etc/nginx/sites-available/default
    awk 'NR==52{print "             proxy_set_header Connection 'upgrade';"}7' /etc/nginx/sites-available/default > change && mv change /etc/nginx/sites-available/default
    awk 'NR==53{print "             proxy_set_header Host \$host;"}7' /etc/nginx/sites-available/default > change && mv change /etc/nginx/sites-available/default
    awk 'NR==54{print "             proxy_cache_bypass \$http_upgrade;"}7' /etc/nginx/sites-available/default > change && mv change /etc/nginx/sites-available/default

    # Restores the permissions we changed to edit the default file
    sudo chown root: /etc/nginx/sites-available/.
    sudo chown root: /etc/nginx/sites-available/default

    # restarts nginx as we have changed the default file
    sudo systemctl restart nginx
```
