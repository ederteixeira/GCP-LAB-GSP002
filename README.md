# GCP-LAB01
## Lab: GSP002
## Getting Started with Cloud Shell and gcloud

### Task 01:
Ativar o Cloud Shell e autorizar:

    gcloud auth list

Listar o projeto:

    gcloud config list project
    
Exemplo de Regiôes e Zonas:

Western US: us-west1-a ou us-west1-b

Central US: us-central1-a, us-central1-b, us-central1-c ou us-central1-f

Eastern US: us-east1-b, us-east1-c ou us-east1-d

Western Europe: europe-west1-b, europe-west1-c ou europe-west1-d

Eastern Asia: asia-east1-a, asia-east1-b ou asia-east1-c

Setando a região: 

    gcloud config set compute/region us-west1 
    
Validação:

    gcloud config get-value compute/region

Setando a Zona:
    
    gcloud config set compute/zone us-west1-b
    
Validação:

    gcloud config get-value compute/zone

Verificar o ID do projeto:

    gcloud config get-value project
    
Verificar as info do projet:

    gcloud compute project-info describe --project $(gcloud config get-value project)

Criar uma variavel com o valor do ID do projeto:

    export PROJECT_ID=$(gcloud config get-value project)

Criar uma variavel com o valor da Zona:
    
    export ZONE=$(gcloud config get-value compute/zone)

Validação dos valores das variaveis:

    echo -e "PROJECT ID: $PROJECT_ID\nZONE: $ZONE"

Criando uma máquina virtual:

    gcloud compute instances create gcelab2 --machine-type e2-medium --zone $ZONE

Exibe ajuda das instâncias criadas.

    gcloud compute instances create --help

Exibe ajuda do GCloud: 
    
    gcloud -h

Exibe ajuda do GCloud:    
   
    gcloud config --help

Exibe a lista de configuração do ambiente:

    gcloud config list

Exibe a lista de configuração e propriedades do ambiente:
    
    gcloud config list --all
    
Exibe a lista de componentes:
    
    gcloud components list

### Task 02

Liste a instância de computação disponível no projeto:

    gcloud compute instances list

Liste a máquina virtual gcelab2

    gcloud compute instances list --filter="name=('gcelab2')"

Liste as regras de firewall do projeto:

    gcloud compute firewall-rules list
    
Liste as regras de firewall da rede padrão:
    
    gcloud compute firewall-rules list --filter="network='default'"
    
Liste as regras de firewall da rede padrão em que a regra de permissão corresponda a uma regra ICMP:

    gcloud compute firewall-rules list --filter="NETWORK:'default' AND ALLOW:'icmp'"

### Task 03

Conectando à VM via SSH:

    gcloud compute ssh gcelab2 --zone $ZONE

Instalando o servidor Web "nginx" na máquina virtual
    
    sudo apt install -y nginx
   
Desconectar da máquina no SSH
 
    exit

### Task 04

Listar as regras de firewall do projeto:

    gcloud compute firewall-rules list
    
Adicionar uma tag à máquina virtual:

    gcloud compute instances add-tags gcelab2 --tags http-server,https-server
    
Atualizando a regra de firewall para permitir:
    
    gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

Listar as regras de Firewall do projeto:

    gcloud compute firewall-rules list --filter=ALLOW:'80'
    
Validar se a máquina virtual aceita a comunicação HTTP:

    curl http://$(gcloud compute instances list --filter=name:gcelab2 --format='value(EXTERNAL_IP)')

### Task 05:

Exibir os logs disponíveis no sistema:
    
    gcloud logging logs list
    
Exibir os logs dos recursos da máquina:

    gcloud logging logs list --filter="compute"

Ler os logs do tipo de recurso de gce_instance

    gcloud logging read "resource.type=gce_instance" --limit 5

Ler os logs da máquina virtual:

    gcloud logging read "resource.type=gce_instance AND labels.instance_name='gcelab2'" --limit 5

