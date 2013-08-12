ec2-automation
==============

Scripts for automating Amazon EC2 instance creation / bootstrapping for coursera startup class.

Setup
-----

1. At first add several additional environment variables to your ~/.bash_profile:

```sh
export EC2_AMI=ami-d0f89fb9 # AMI type to create
export EC2_TYPE=t1.micro
export EC2_REGION=us-east-1
export DEV_SSH_KEY=/path/to/github/private/key
export EC2_KEY=ec2-keypair #located in EC2_KEYS_HOME
export EC2_AUTOMATION_HOME=/path/to/these/scripts/locally
export PATH=$PATH:$EC2_AUTOMATION_HOME
```

$DEV_SSH_KEY should be registered on heroku and github.
For additional AMI types you can check [this link](http://cloud-images.ubuntu.com/releases/precise/release-20130411.1/).

2. Then setup [EC2 API Tools](http://aws.amazon.com/developertools/351) on your local machine as described 
[here](http://www.robertsosinski.com/2008/01/26/starting-amazon-ec2-with-mac-os-x/).

3. Add EC2 ssh-key (which you've created on step 2) environment variable to .bash_profile
```sh
export EC2_KEY=ec2-keypair
```

4. Put scripts from this repo to some directory included in $PATH variable, so you can run them from anywhere.

5. Optionally you can set GIT_USER_NAME and GIT_USER_EMAIL variables in your .bash_profile

Done. Now creation, starting, bootstrapping and stopping of EC2 instances for [startup class](https://class.coursera.org/startup-001/)
is fully automated [and takes now about 5 minutes].


.bashrc changes 
---------------
There are the final changes that I made to my ~/.bashrc file in Cygwin to get this working
```sh
# Stuff for allowing the Amazon EC2 API to work
export EC2_ROOT=$HOME/.ec2
export EC2_API_HOME=$EC2_ROOT/ec2-api-tools-1.6.8.1
export EC2_HOME=$EC2_API_HOME #this is reallky the API home ... needs to be defined as this for the API to function properly
export EC2_KEYS_HOME=$EC2_ROOT/keys
export JAVA_HOME=/cygdrive/c/Windows/System32/java
export EC2_PRIVATE_KEY=`ls $EC2_KEYS_HOME/pk-*.pem`
export EC2_CERT=`ls $EC2_KEYS_HOME/cert-*.pem`
export PATH=$PATH:$EC2_API_HOME/bin
# Stuff for allowing the Amazon EC2 API to work

#Stuff for allowing automation of creating Amazon EC2 instances using the EC2 API
export EC2_AMI=ami-d0f89fb9 # AMI type to create
export EC2_TYPE=t1.micro
export EC2_REGION=us-east-1
export DEV_SSH_KEY=$HOME/.gitHubKeys/id_rsa
export EC2_KEY=ec2-keypair #located in EC2_KEYS_HOME
export EC2_AUTOMATION_HOME=$EC2_ROOT/ec2-automation
export PATH=$PATH:$EC2_AUTOMATION_HOME
#Stuff for allowing automation of creating Amazon EC2 instances using the EC2 API

#Convenience variables for using git and gitHub
export GIT_USER_NAME="Gregory Jaworski"
export GIT_USER_EMAIL="GregoryJaworski@gmail.com"
#Convenience variables for using git and gitHub
```


How To
------
* Start+bootstrap: **ecstart**.
This script creates EC2 instance, starts it and then runs [bootstrap script](https://github.com/mvkvl/startup/blob/master/bootstrap.sh)
 on this instance.

<pre>
$ ecstart
...
...
EC2 instance i-abcdef12 is up and running
connect to it with:
ssh -i ~/.ec2/ec2-keypair ubuntu@w.x.y.z
</pre>

* List running instances: **eclist**

<pre>
$ eclist
i-abc12def  t1.micro  a.b.c.d
i-fed21cba  t1.micro  e.f.g.h
</pre>

* Stop running instance(s): **ecstop**

<pre>
stop defined instance(s)
$ ecstop i-abc12def i-fed21cba

stop all instances
$ ecstop
</pre>

* SSH to running instance: **ecssh**

<pre>
$ ecssh i-12345678
</pre>
