# Honeypot Assignment

**Time spent:** 12 

**Objective:** Create a honeynet using MHN-Admin. Present your findings as if you were requested to give a brief report of the current state of Internet security. Assume that your audience is a current employer who is questioning why the company should allocate anymore resources to the IT security team.

### MHN-Admin Deployment (Required)
GIF Walkthrough:
![ezgif-6-44bbd5aa7127](https://user-images.githubusercontent.com/67130044/142585612-ee99036d-bfa9-4995-a0c2-188589b6f166.gif)

**Summary:** 
The Honeypot was deployed by using GCP through the Web and CLI 

![Screenshot from 2021-11-18 22-34-13](https://user-images.githubusercontent.com/67130044/142576614-081f56ee-29fe-469a-972c-4c02f967dd31.png)
![image](https://user-images.githubusercontent.com/67130044/142576669-ee716992-8208-4b63-af2c-7cbbb180b85c.png)
![image](https://user-images.githubusercontent.com/67130044/142582490-3f7ab8ba-8328-44af-a883-56f7c97c5e7e.png)


### Dionaea Honeypot Deployment (Required)

**Summary:** Briefly in your own words, what does dionaea do?
![Screenshot from 2021-11-18 23-23-47](https://user-images.githubusercontent.com/67130044/142583161-6f2fde24-ba95-40bd-ba9e-50decd746e7d.png)

![image](https://user-images.githubusercontent.com/67130044/142578118-2d476273-d0f3-436b-97da-eb0bcadddbdc.png)

### Database Backup (Required) 

**Summary:** 
The relation database management system that MHN-Admin uses is mongodb. The exported JSON file contains information of the probe or source ip addresses, their ports, the protocol of the connection, and the name of the honeypot. 
with the following command:
```bash
mongoexport --db mnemosyne --collection session > session.json
```



### Deploying Additional Honeypot(s) (Optional)

#### X Honeypot

**Summary:** What does this honeypot simulate and do for a security researcher?
The honeypot simulates a vulnerable target open in the wild with all types of connections and protocols open. This would help test if there were any vulnerabilities beyond the firewall defense mechanisms.


##Commands

```bash
gcloud config list

gcloud compute regions list


gcloud compute firewall-rules list

gcloud compute firewall-rules create http \
    --allow tcp:80 \
    --description="Allow HTTP from Anywhere" \
    --direction ingress \
    --target-tags="mhn-admin"

gcloud compute firewall-rules create honeymap \
    --allow tcp:3000 \
    --description="Allow HoneyMap Feature from Anywhere" \
    --direction ingress \
    --target-tags="mhn-admin"

gcloud compute firewall-rules create hpfeeds \
    --allow tcp:10000 \
    --description="Allow HPFeeds from Anywhere" \
    --direction ingress \
    --target-tags="mhn-admin"

# Create a VM for mhn-admin, instance type- n1-standard-1
# and bind to previously defined fire-wall rules matching mhn-admin
# tag
gcloud compute instances create "mhn-admin" \
    --machine-type "n1-standard-1" \
    --subnet "default" \
    --maintenance-policy "MIGRATE" \
    --tags "mhn-admin" \
    --image-family "ubuntu-minimal-1804-lts" \
    --image-project "ubuntu-os-cloud" \
    --boot-disk-size "10" \
    --boot-disk-type "pd-standard" \
    --boot-disk-device-name "mhn-admin"



Output:


WARNING: You have selected a disk size of under [200GB]. This may result in poor I/O performance. For more information, see: https://developers.google.com/compute/docs/disks#performance.
Created [https://www.googleapis.com/compute/v1/projects/blissful-canyon-332005/zones/us-east1-b/instances/mhn-admin].
NAME       ZONE        MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP    STATUS
mhn-admin  us-east1-b  n1-standard-1               10.142.0.3   34.138.77.219  RUNNING

# Connect to mhn-admin instance using gcloud ssh
gcloud compute ssh mhn-admin


cd /opt/
sudo git clone https://github.com/pwnlandia/mhn.git
cd mhn/

sudo sed -i 's/Flask-SQLAlchemy==2.3.2/Flask-SQLAlchemy==2.5.1/g' server/requirements.txt

sudo ./install.sh









****Welcome*****
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.11.0-1021-gcp x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

0 updates can be applied immediately.


The list of available updates is more than a week old.
To check for new updates run: sudo apt update



***update repos***
sudo apt update
*** git and python-magic 
sudo apt install git python-magic -y


***Download MHN w/ python requirements
cd /opt/
sudo git clone https://github.com/pwnlandia/mhn.git
cd mhn/

sudo sed -i 's/Flask-SQLAlchemy==2.3.2/Flask-SQLAlchemy==2.5.1/g' server/requirements.txt

sudo ./install.sh

# create firewall rule allowing all incoming tcp/udp traffic
gcloud compute firewall-rules create wideopen \
    --description="Allow TCP and UDP from Anywhere" \
    --direction ingress \
    --action=allow \
    --priority=1000 \
    --rules=tcp,udp \
    --source-ranges=0.0.0.0/0 \
    --target-tags="honeypot"


#Create VM

gcloud compute instances create "honeypot-1" \
    --machine-type "n1-standard-1" \
    --subnet "default" \
    --maintenance-policy "MIGRATE" \
    --tags "honeypot" \
    --image-family "ubuntu-minimal-1804-lts" \
    --image-project "ubuntu-os-cloud" \
    --boot-disk-size "10" \
    --boot-disk-type "pd-standard" \
    --boot-disk-device-name "honeypot-1"


WARNING: You have selected a disk size of under [200GB]. This may result in poor I/O performance. For more information, see: https://developers.google.com/compute/docs/disks#performance.
Created [https://www.googleapis.com/compute/v1/projects/blissful-canyon-332005/zones/us-east1-b/instances/honeypot-1].
NAME        ZONE        MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP    STATUS
honeypot-1  us-east1-b  n1-standard-1               10.142.0.4   34.73.181.208  RUNNING

#Deployment Command
wget "http://34.138.77.219/api/script/?text=true&script_id=2" -O deploy.sh && sudo bash deploy.sh http://34.138.77.219 FkO6KMHd

2nd time IP when instance is started is: wget "http://34.138.244.92 /api/script/?text=true&script_id=2" -O deploy.sh && sudo bash deploy.sh http://34.138.244.92  FkO6KMHd
```
## Notes

Describe any challenges encountered while doing the assignment.
