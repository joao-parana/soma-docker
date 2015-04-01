# poc-docker

### Instalando o Docker no MAC OSX

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
    

### Instalando o Docker Composer
    curl -L https://github.com/docker/compose/releases/download/1.1.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    
### Baixando definição do Repositório GIT
    mkdir ~/tmp
    cd ~/tmp
    git clone https://github.com/joao-parana/soma-docker.git
    cd soma-docker

### Iniciando o SOMA
    docker-compose up

### Abrindo no Browser
    open http://`boot2docker ip`:1443

> Thats all !


