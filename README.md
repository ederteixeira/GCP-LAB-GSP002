# Google Cloud Computing
## Laboratório: GSP002
### Getting Started with Cloud Shell and gcloud

#### Task 01:

1- Ativar o cloud shell e autorizar.
    
    gcloud auth login

2- Listar o Projeto
    
    gcloud config list project
    	

3- Setando uma Região
    
    gcloud config set compute/region us-east1
No exemplo acima foi configurado para a Região us-east1

4- Validando a Região.
    
    gcloud config get-value compute/region
    

5- Setando a Zona.
    
    gcloud config set compute/zone us-east1-c
    		
No exemplo acima foi configurado para a Zona us-east1-c

6- Validando a Zona.
    
    gcloud config get-value compute/zone


7- Verificar o ID do projeto:
    
    gcloud config get-value project
    

8- Verificar as informações do projeto:
    
    gcloud compute project-info describe --project $(gcloud config get-value project)
    

9- Criar uma variavel com o valor do ID do projeto:
    
    export PROJECT_ID=$(gcloud config get-value project)
    

10- Criar uma variavel com o valor da Zona:
    
    export ZONE=$(gcloud config get-value compute/zone)
    

11- Verificar os valores armazenados nas variáveis
    
    echo -e "PROJECT ID: $PROJECT_ID\nZONE: $ZONE"
    

12- Criando uma máquina virtual:
    
    gcloud compute instances create gcelab2 --machine-type e2-medium --zone $ZONE
    

13- Exibir ajuda das instâncias criadas.
    
    gcloud compute instances create --help
    

14- Exibe ajuda do GCloud: 
    
    gcloud -h

    gcloud --help
    

15- Exibe a lista de configuração do ambiente:
    
    gcloud compute instances create --help
    

16- Exibe a lista de configuração do ambiente:
    
    gcloud config list
    

17- Exibe a lista de configuração e propriedades do ambiente:
    
    gcloud config list --all

18- Exibe a lista de componentes:
    
    gcloud components list

#### Task 02:

1- Liste a instância de computação disponível no projeto:

    gcloud compute instances list

2- Liste a máquina virtual gcelab2

    gcloud compute instances list --filter="name=('gcelab2')"

3- Liste as regras de firewall do projeto:

    gcloud compute firewall-rules list
   
4- Liste as regras de firewall da rede padrão:
    
    gcloud compute firewall-rules list --filter="network='default'"
    
5- Liste as regras de firewall da rede padrão em que a regra de permissão corresponda a uma regra ICMP:

    gcloud compute firewall-rules list --filter="NETWORK:'default' AND ALLOW:'icmp'"

#### Task 03:

1- Conectando à VM via SSH:

    gcloud compute ssh gcelab2 --zone $ZONE

2- Instalando o servidor Web "nginx" na máquina virtual
    
    sudo apt install -y nginx
   
3- Desconectar da máquina no SSH
 
    exit

#### Task 04:

1- Listar as regras de firewall do projeto:

    gcloud compute firewall-rules list
    
2- Adicionar uma tag à máquina virtual:

    gcloud compute instances add-tags gcelab2 --tags http-server,https-server
    
3- Atualizando a regra de firewall para permitir:
    
    gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http-server

4- Listar as regras de Firewall do projeto:

    gcloud compute firewall-rules list --filter=ALLOW:'80'
    
5- Validar se a máquina virtual aceita a comunicação HTTP:

    curl http://$(gcloud compute instances list --filter=name:gcelab2 --format='value(EXTERNAL_IP)')


#### Task 05:

1- Exibir os logs disponíveis no sistema:
    
    gcloud logging logs list
    
2- Exibir os logs dos recursos da máquina:

    gcloud logging logs list --filter="compute"

3- Ler os logs do tipo de recurso de gce_instance

    gcloud logging read "resource.type=gce_instance" --limit 5

4- Ler os logs da máquina virtual:

    gcloud logging read "resource.type=gce_instance AND labels.instance_name='gcelab2'" --limit 5

