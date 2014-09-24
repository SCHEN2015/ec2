ec2
===

ec2 is a simple shell script that helps you to log into your EC2 instances using the tag "Name".

![Screenshot](https://raw.github.com/sirio7g/ec2/master/docs/login.png "Screenshot")

# Setting up

## Donwload ec2 script

Download the source code from github:

```
git clone https://github.com/sirio7g/ec2.git
```

## Install boto library

ec2 script is built on top of boto library then, first of all, install them using pip:

```
pip install boto

#
# If 'pip' is not installed on your machine, try with:
# sudo easy_install pip
#
```

In most case if is enough but if you are running Mountain Lion you might need to export the variable as follow:

```
export PYTHONPATH=/usr/bin/python2.7
export PYTHONPATH=${PYTHONPATH}:$HOME/code/python/boto
```

change the python version according to your own.

## Edit the configuration file

Since ec2 script wants to be a very quick shortcut to jump into your instances, it uses a property file in order to load default settings i.e. username, default pem file, default region, etc. Therefore, you must edit configuration file:

```
vi conf/.ec2-default.props
```

Then you have to add the ec2 directory to your PATH env variable:

```
export EC2_SCRIPT_HOME=/path_of_ec2/
export PATH=$PATH:$EC2_SCRIPT_HOME/bin
```

persistent it by adding the above line to your ~/.bash_profile or ~/.profile.

# Usage

```
ec2 [-i] [-u user] [-x] [-c "command"] [eu|us|us-west-1|region]

Arguments:
	-u user
	-i print instances's details
	-k PEM file
	-x Export display
	-c "command (sent by ssh)"
```

ec2 allows to run command (by ssh) on a server or even onto a list of servers using the “-c” argument, look at the example below:

![Screenshot](https://raw.github.com/sirio7g/ec2/master/docs/login2.png "Screenshot")

## Example

List all your instance in your default US region:

```
sirio7g@laptop$ ec2 us
hadoop-master (ec2-51-227-4-45.eu-west-1.compute.amazonaws.com)
hadoop-node1 (ec2-41-53-145-104.eu-west-1.compute.amazonaws.com)
hadoop-node2 (ec2-44-138-149-215.eu-west-1.compute.amazonaws.com)
Pick your option(s) (use space to specify more instances, i.e: 1 2 3 4):
```

Join as "ubuntu" user:

```
sirio7g@laptop$ ec2 -u ubuntu
hadoop-master (ec2-51-227-4-45.eu-west-1.compute.amazonaws.com)
hadoop-node1 (ec2-41-53-145-104.eu-west-1.compute.amazonaws.com)
hadoop-node2 (ec2-44-138-149-215.eu-west-1.compute.amazonaws.com)
Pick your option(s) (use space to specify more instances, i.e: 1 2 3 4): 1
Connecting to hadoop-master 
ssh -i /home/sirio7g/my.pem ubuntu@ec2-51-227-4-45.eu-west-1.compute.amazonaws.com

ubuntu@hadoop-master:~$ 

```

Join as "ubuntu" user, print "date" on a list of instances and exit:

```
sirio7g@laptop$ ec2 -u ubuntu -c "date; exit"
hadoop-master (ec2-51-227-4-45.eu-west-1.compute.amazonaws.com)
hadoop-node1 (ec2-41-53-145-104.eu-west-1.compute.amazonaws.com)
hadoop-node2 (ec2-44-138-149-215.eu-west-1.compute.amazonaws.com)
Pick your option(s) (use space to specify more instances, i.e: 1 2 3 4): 1 2
Connecting to hadoop-master       
ssh -i /home/sirio7g/my.pem ubuntu@ec2-51-227-4-45.eu-west-1.compute.amazonaws.com
Fri Feb 15 10:42:50 CET 2013
Connecting to hadoop-node1
ssh -i /home/sirio7g/my.pem ubuntu@ec2-41-53-145-104.eu-west-1.compute.amazonaws.com
Fri Feb 15 10:42:50 CET 2013
sirio7g@laptop$
```

