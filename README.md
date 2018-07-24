# Data_Science_Toolkit

## To configure a Jupyter Data Science Notebook Server on Amazon Web Services

To create a SSH key pair (where id_rsa is “the key” or private key and id_rsa.pub is “the lock” or public key),
* first check if there are existing keys using: `ls ~/.ssh`
* `mkdir -p .ssh` <note: use -p because it will create an error if it exists already>
* `ssh -keygen` <note: to create a SSH key pair>
* `/Users/[user]/.ssh/id_rsa` <note: this is where to put SSH keys>. Don’t enter a passphrase -> just press ENTER twice
* `cat ~/.ssh/id_rsa.pub` <note: to see key>
* `cat ~/.ssh/id_rsa.pub` | pbcopy <note: to copy key to clipboard>

Update AWS with your SSH public key:
* Go to EC2, under Resources= “Key Pairs”, import key pair and upload the file

## Amazon EC2 & Security Groups. (Elastic compute cloud = ECC = EC2)

Go to EC2 and then launch instance

Step 1: Choose AMI : Ubuntu Server 16.04 LTS (HVM), SSD Volume Type. AMI = amazon machine image. Someone created a Ubuntu server 16.04 and saved it with an image somewhere so that we could use it as a “master copy” and use it to create our own space.
Ubuntu is: [Class = image = a saved master = eg: linear regression = SMI = container]
Whereas what we’re creating now is an: [Object = instance = a copy = eg: lm_model = my EC2 instance = image]

Step 2: Choose instance type: Select the default "t2.micro"

Step 3: Configure instance:

Step 4: Add storage: increase Size to 20GB to take advantage of what we have access to (even if we may not need it). The max available is 30 GB.

Step 5: Add tags:

Step 6: Configure security group: Select an existing security group or configure a new one to show: <note: we want source to be Anywhere so that we are able to access this from any computer so even if our IP address changes the next time we’re in class or at home, etc. we will be able to use it>

HTTP: source=Anywhere: 80
HTTPS: source=Anywhere: 443
Custom TCP Rule: source=Anywhere: 27016 (note: 27017 is open to anyone and where hackers usually go. 27016 is also open to anyone, but is less likely to have issues)
PostgreSQL: source=Anywhere: 5432
SSH: source=Anywhere: 5432
Review and Launch

## Docker Installation
* `ssh ubuntu@[AWS IP address]`
* “Are you sure you want to continue connection (yes/no?)….enter yes
* We should now see a green prompt: `ubuntu@ip-[AWS IP address]`
* `curl -sSL https://get.docker.com | sh` <note:  this will install docker onto our system>
* Add our user (ubuntu) to the docker group on our system: `sudo usermod -oG docker ubuntu`
* For this to take effect:  logout (logout or exit) and log back in. (`ssh ubuntu@[AWS IP address]`)
* To verify docker was successfully installed:  `docker -v` (where v=version of docker running)

## Obtaining the correct Docker image
* NOTE:  docker containers need to be stateless = because if we exit something, the code must still be available.
* Home/jovyan = is where all of the notebooks will be written - so we don’t care about which container is used
* docker ps = to see container ID
* docker exec [1st 4 characters of the container ID] jupyter notebook list = to see token
* ubuntu@[AWS IP address]:-$ docker pull jupyter/datascience-notebook.
* Verify that docker images were successfully installed: docker images

To simplify the name of the notebook and make it easier to get to this image each time: docker tag [IMAGE ID] alias. (e.g., dsnb = data science notebook)
Running the correct Docker image as a container
* To run this image:  (this returns the image ID)
    * `ubuntu@ip-[IP address]:~$ docker run \
    * > -d \   
    * > -p 443:8888 \
    * > -v /home/ubuntu:/home/jovyan \
    * > jupyter/datascience-notebook`
* 
* d= in detached mode = if we close our shell, we won’t lose anything
* p = attaching ports to the AWS hosts. [-p Host:container] = 8888 is inside this container
* v = to mount this [what to attach it to] [what to attach]
* 
* `docker ps -a` = to see all containers, even th

## TO RUN A JUPYTER IMAGE as a NEW CONTAINER:

* Open browser, then go to: [AWS IP address]
* Password or token: (in BASH: docker exec [container ID] jupyter notebook list)
* Open browser, then go to: [AWS IP address]

## Sample budget of the costs of running a Jupyter Data Science Notebook Server for three months
vCPU Storage Per Hour Per 3 months
t2.micro 1 GiB  0.01160.0116 25.06
t2.small 2 GiB  0.02300.0230 49.68
t2.medium 4 GiB  0.04640.0464 100.22
t2.large 8 GiB  0.09280.0928 200.45

