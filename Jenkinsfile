
node {
    
sh '''
 #!/bin/bash
echo "deploy to mutiple servers at same time"
echo $ENVIRONMENTS

#if-else conditions FOR  SELECTING DIFFERENT ENVIRONMENTS
if [ $ENVIRONMENT == "First" ] 
then 
my_IP="172.31.28.50,172.31.14.233"
else 
my_IP="172.31.14.233,172.31.28.50"
fi
echo $my_IP
IFS=',' read -r -a IP_array <<< "$my_IP"
echo ${IP_array[0]}
#giving permissions for pem key
sudo chmod 600 /opt/swamymumbainewkey.pem
#forloop for deploying to multiple servers based on if-else conditions
for i in "${IP_array[@]}"
do
echo "Inside for loop"
echo $i
#dowload war or ear file from s3 bucket
aws s3 cp s3://s3upload999/hello-1.war .
#To see list
ls -ltr
#host verification key for not getting we will use this  .....if we r getting pseudo terminal errors using "-T" otherwise dont use this
sudo ssh -T -i /opt/swamymumbainewkey.pem   -o 'StrictHostKeyChecking=no' ec2-user@$i
#deploying files to some other servers
sudo scp -i /opt/swamymumbainewkey.pem  hello-1.war  ec2-user@$i:/home/ec2-user
#executing commands from other servers
sudo ssh  -T -i /opt/swamymumbainewkey.pem   ec2-user@$i "ls -ltr"
done
'''
}
