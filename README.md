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
    - Input the name of the build you wish to linke
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
    - In 'Repositories' copy the SSH from the Github repository (Check SSH and copy)

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

test 5
