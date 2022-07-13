
# Docker Engine Windows 10/11
## Instalar WSL 2
### Habilite o WSL no Windows 10/11
Execute no windows terminal como adminstrador

    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

Reinicie a maquina

### Atribua a versão default do WSL para a versão 2

    wsl --set-default-version 2

### Instale distribuição linux

    wsl --install -d Ubuntu

### Execute o comando no terminal

    wsl


### O que o WSL 2 pode usar de recursos da sua máquina
Crie um arquivo chamado .wslconfig na raiz da sua pasta de usuário (C:\Users\<seu_usuario>) e defina estas configurações:

    [wsl2] 
    memory=8GB 
    processors=4 
    wap=2GB

## Integrar Docker com WSL 2

### Instalar o Docker com Docker Engine (Docker Nativo)

Instale os pré-requisitos:

    sudo apt update && sudo apt upgrade
    sudo apt remove docker docker-engine docker.io containerd runc
    sudo apt-get install \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg \
        lsb-release

Adicione o repositório do Docker na lista de sources do Ubuntu:

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
----
    echo \
          "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
          $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

  
  Instale o Docker Engine:
  

    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io

  
  Dê permissão para rodar o Docker com seu usuário corrente:

    sudo usermod -aG docker $USER

  
  Instale o Docker Compose:
  

    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

    sudo chmod +x /usr/local/bin/docker-compose

    sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

Inicie o serviço do Docker:

    sudo service docker start

Este comando acima terá que ser executado toda vez que Linux for reiniciado. Se caso o serviço do Docker não estiver executando, mostrará esta mensagem de erro:

Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

**Dica para Windows 11**

No Windows 11 é possível especificar um comando padrão para ser executados sempre que o WSL for iniciado, isto permite que já coloquemos o serviço do docker para iniciar automaticamente. Edite o arquivo `/etc/wsl.conf`:

Rode o comando para editar:

    sudo vim /etc/wsl.conf

Aperte a letra i e cole o conteúdo:

    [boot]
    command="service docker start"

Aperte a tecla `:`, digite `wq` para salvar/sair e pressione enter. Pronto, para reiniciar o WSL com o comando `wsl --shutdown` no DOS ou PowerShell para testar. Após abrir o WSL novamente, digite o comando `docker ps` para avaliar se o comando não retorna a mensagem acima: `Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?`

## Configurar sua pasta local de projeto no docker

Acessar pelo explorer o WSL:

`\\wsl$`

Botão direito sobre a pasta do SO "Ubuntu"

Clicar em mais opções > Mapear unidade de rede

Vai aparecer o projeto como unidade de rede em seu computador
* Executar o VScode apartir da unidade de rede sem root para clonar os projetos