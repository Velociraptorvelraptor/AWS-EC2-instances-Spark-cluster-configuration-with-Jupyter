# AWS-EC2-instances-Spark-cluster-configuration-with-Jupyter

## Initialazing EC2 instances at Amazon Web Services

1. Set up two EC2 Ubuntu (16.04 or 18.04) instances with all default settings (note: 1CPU 1GB t2.micro is a free trial).
2. Generate new or use already existing public key
3. After succesfull initialazing one need to configure Security Groups (go to NETWORK&SECURITY) and add new or edit already existing rule.
   - SSH, port: 22, source: anywhere
   - HTTPS, port: default, source: anywhere
   - TCP, port: 8888, source: anywhere

## Connecting to the instances through SSH

After succesfull initialazing EC2 instances one need to connect to them through SSH.
Personally I do it everytime using Putty:
- Host name: ubuntu@<your-public-DNS-of-an-instance>
- Remember of generating a private key (you can use PuttyGen to generate a private key from a public one)
  and loading it in section (Connection -> SSH -> Auth -> Private key file ...)
- Set tunneling (Connection -> SSH -> Tunnels): 
  - source: 8000, destination: localhost:8888
  - source: 8001, destination: localhost:8080
  
## Apache Spark configuration

When in ubuntu terminal type following command:
```
$ wget http://ftp.man.poznan.pl/apache/spark/spark-2.4.4/spark-2.4.4-bin-hadoop2.7.tgz
```
then unpack the file using tar xvzf file-name command.
Then install Java and Scala:
```
$ sudo apt-get -y install openjdk-8-jdk-headless
$ sudo apt-get install scala
```

## Jupyter installation

Download latest version of Anaconda and use bash command to run installation script.
```
$ wget http://repo.continuum.io/archive/Anaconda3-4.1.1-Linux-x86_64.sh
$ bash Anaconda3–4.1.1-Linux-x86_64.sh
```
Jupyter certifiation configuration using below commands and answering several questions:
```
$ jupyter notebook --generate-config
$ mkdir certs
$ cd certs
$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
```
Last step is to configure the jupyter notebook, go to the jupyter_notebook_config.py and use vim editor.
```
$ cd ~/.jupyter/
$ vim jupyter_notebook_config.py
```
At ther very beggining of the file paste the following script:
```
c = get_config()
c.NotebookApp.certfile = u'/home/ubuntu/certs/mycert.pem' 
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False 
c.NotebookApp.port = 8888
```
Run jupyter notebook simply by using command:
```
$ jupyter notebook
```
Jupyter panel is now available by entering following address in the browser:
```
https://public-dns-of-your-ec2-machine:8888/
```
