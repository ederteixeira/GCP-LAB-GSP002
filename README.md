# GCP-LAB01
Lab: GSP002
Getting Started with Cloud Shell and gcloud

Task 01:
Ativar o Cloud Shell e autorizar:

    gcloud auth list

Listar o projeto:

    gcloud config list project
    
Exemplo de Regiôes:

    Western US: us-west1-a ou us-west1-b
    Central US: us-central1-a, us-central1-b, us-central1-c ou us-central1-f
    Eastern US: us-east1-b, us-east1-c ou us-east1-d
    Western Europe: europe-west1-b, europe-west1-c ou europe-west1-d
    Eastern Asia: asia-east1-a, asia-east1-b ou asia-east1-c

Setando a região: 

    gcloud config set compute/region us-west1 
gcloud config get-value compute/region

gcloud config set compute/zone
gcloud config get-value compute/zone

gcloud config get-value project
gcloud compute project-info describe --project $(gcloud config get-value project)

export PROJECT_ID=$(gcloud config get-value project)
export ZONE=$(gcloud config get-value compute/zone)
echo -e "PROJECT ID: $PROJECT_ID\nZONE: $ZONE"

Criar a máquina virtual
gcloud compute instances create gcelab2 --machine-type e2-medium --zone $ZONE

gcloud compute instances create --help

gcloud -h
gcloud config --help

gcloud config list
gcloud config list --all
gcloud components list

Task 02
gcloud compute instances list
gcloud compute instances list --filter="name=('gcelab2')"
gcloud compute firewall-rules list
gcloud compute firewall-rules list --filter="network='default'"
gcloud compute firewall-rules list --filter="NETWORK:'default' AND ALLOW:'icmp'"

Task 03

gcloud compute ssh gcelab2 --zone $ZONE
sudo apt install -y nginx
exit

Task 04
gcloud compute firewall-rules list
gcloud compute instances add-tags gcelab2 --tags http-server,https-server
gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

gcloud compute firewall-rules list --filter=ALLOW:'80'
curl http://$(gcloud compute instances list --filter=name:gcelab2 --format='value(EXTERNAL_IP)')

Task 05:
gcloud logging logs list
gcloud logging logs list --filter="compute"
gcloud logging read "resource.type=gce_instance" --limit 5
gcloud logging read "resource.type=gce_instance AND labels.instance_name='gcelab2'" --limit 5


























