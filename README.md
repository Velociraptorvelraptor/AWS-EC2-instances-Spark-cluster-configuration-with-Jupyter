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
  
 
