create ssh connection from local to github

cd .ssh

ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

eng110_jack > no passphrase > key displayed

cat eng110_jack.pub > copy key

github > settings > ssh and gpg keys > new key > name same as key > paste key

github > repo > clone this repo > github ssh > git clone ssh > make a change > add/commit/push

## Creating a job on Jenkins

By default runs on port 88

Log into Jenkins(this is an EC2 instance on AWS) > new jobs(left) > name > freestyle job > configure > Discard old builds - 3 > add build step > select execute shell > enter commands > save

commands =

```
# Which OS does it support?

uname -a

# Which time zone/date

date

# Which environment does it support?

# Does it have node support?
```

name > dropdown > build now

outcome = name > build history on left > drop down > console output

new build > execute shell = `printenv`

on first job > configure > add post build actions > build other projects > name > trigger if build is stable

test jobs individually but they can be linked

github > repo > setting > deploy key > add deploy key > add key

## Private key to Jenkins

ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

grab key

github > repo > setting > deploy key > add deploy key > add key

github project check

restrict where this project can be run > sparta-ubuntu-node

source code management > git > copy SSH > add key > jenkins > ssh username with private key > user name > private key > enter directly > key > print ssh

build environment > Provide Node & npm bin/ folder to PATH

build > commands >

```
cd starter-code/app
npm install
npm test
```

build job > console output
