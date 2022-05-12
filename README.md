create ssh connection from local to github

cd .ssh

ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

eng110_jack > no passphrase > key displayed

cat eng110_jack.pub > copy key

github > settings > ssh and gpg keys > new key > name same as key > paste key

github > repo > clone this repo > github ssh > git clone ssh > make a change > add/commit/push
