# soma-docker

### Instalando o Docker 1.6 (ou superior) no MAC OSX

> No Linux é parecido

    brew install docker 
    brew install boot2docker 
    
    boot2docker init  
    boot2docker up 
    boot2docker start  
    # To connect the Docker client to the Docker daemon, please set: 
    export DOCKER_HOST=tcp://192.168.59.103:2376 
    export DOCKER_CERT_PATH=/Users/joao_parana/.boot2docker/certs/boot2docker-vm 
    export DOCKER_TLS_VERIFY=1
    # Just do it ! (veja os exports acima. Na sua maquina pode ser diferente)
    

### Instalando o Docker Composer 1.2 no Linux ou Mac OSX

    curl -L https://github.com/docker/compose/releases/download/1.2.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
  
### Criando shell utilitária
    vi ~/bin/docker-clean  # coloque o conteúdo abaixo no arquivo e salve

    #!/bin/sh                                                                                                                                                                            

    remove_dangling() {
      echo "Removing dangling images ..."
      docker rmi $(docker images -f dangling=true -q)
    }


    remove_stopped_containers() {
       echo "Removing stopped containers ..."
       docker rm $(docker ps -qa)
    }

    case $1 in
       images)
           remove_dangling
           ;;
       containers)
           read -p "Are you sure you want to remove all stopped containers?" -n 1 -r
           echo  #
           if [[ $REPLY =~ ^[Yy]$ ]]
           then
               remove_stopped_containers
           fi
           ;;
       *)
           echo "usage: docker-clean containers|images   -  containers - removes all stopped containers it can.   images - removes dangling (un-needed) image layers - images you no longer need"
           ;;

    esac

### Baixando definição do Repositório GIT

    mkdir ~/tmp
    cd ~/tmp
    git clone https://github.com/joao-parana/soma-docker.git
    cd soma-docker

### Iniciando o SOMA

    docker-compose up

### Listando os Serviços
    docker-compose ps
          Name                    Command              State   Ports                                 
    -------------------------------------------------------------------------------------------------------------------------------
    somadocker_db_1     /docker-entrypoint.sh mysqld   Up      3306/tcp
    somadocker_soma_1   /bin/sh -c /bin/start-oracle   Up      0.0.0.0:1521->1521/tcp, 0.0.0.0:4460->22/tcp, 0.0.0.0:1443->8080/tcp

### Acessando o Database

    sqlplus system/oracle@`boot2docker ip`/XE

### Abrindo no Browser (no futuro)

    open http://`boot2docker ip`:1443

### Procurando algo nos logs
    echo $(docker logs somadocker_db_1 | grep password)
    echo $(docker logs somadocker_soma_1  | grep db-server)

### Parando os Contêineres
    docker stop somadocker_soma_1 
    docker stop somadocker_db_1

### Limpando o ambiente
    docker-clean containers
    docker-clean images

> Thats all !


