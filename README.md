# Configs-Proxy-Http-Injector
Configuraçoes Proxy Http Injector  para uso no pc linux


[![Stage](https://img.shields.io/badge/Release-Stable-brightgreen.svg)]()
[![Build](https://img.shields.io/badge/Supported_OS-Linux-orange.svg)]()


Este git é de uso pessoal e para interessados em configurar seu proxy para usar via WIFI com o compartilhamento Tethering do HTTP Injector.


# Considerações:
Como sabemos o http injector faz uma conexao ssh e o uso de uma payload para utilizar gratuitamente internet da operadora vivo e claro ate o momento (set/2019), porem para usar estes serviços roteados temos duas alternativas, no windows usamos o PDANET com um aplicativo no celular e outro no computador via USB, porem no #Linux isto não é possivel e assim necessitamos da conexao via wifi, e para isto usamos o TETHERING disponivel no proprio aplicativo do Http Injector, porem o mesmo da o uso de proxy e assim devemos configurar alguns parametros para que o proxy funcione e faça as devidas conexões.

--- como usei internet ssh no pc---
pluguei o wifi no pc
liguei o wifi do cel
ativei o wifi pelo http injector

tive q usar proxy em muitos casos e alguns deles foram no navegador e nas configs do pc, alem do apt.

#proxy usado:
192.168.43.1:44355

-----------------------------------------------------------
#quanto ao apt:
Crie uma nova configuração chamada de proxy.conf.

    sudo touch /etc/apt/apt.conf.d/proxy.conf

Abra o arquivo com seu editor de texto preferido

    sudo vi /etc/apt/apt.conf.d/proxy.conf

Adicione esta linha no documento para ter  HTTP proxy.

    Acquire::http::Proxy "http://192.168.43.1:44355/";

Adicione esta linha no documento para ter  HTTPS proxy.

    Acquire::https::Proxy "http://192.168.43.1:44355/";

salve seu arquivo e o apt estara funcionando, para usar ele sem proxy basta comentar com #

------------------------------------------------------------------
#quanto a git hub:

-------ativar o proxy:

git config --global http.proxy http://192.168.43.1:44355

-----desativar o proxy:

git config --global --unset http.proxy http://192.168.43.1:44355

-----------------------------------------------
#Quanto a conexoes externas ssh

Criar um documento dentro de .ssh chamado config e inserir co codigo abaixo:

ProxyCommand /usr/bin/ncat --proxy-type http --proxy 192.168.43.1:44355 %h %p

-------ativar desativar o proxy: 

$sudo gedit .ssh/config     

remover ou colocar o sustenido na frente do codigo que puxa o ssh da net vivo
do http injector:

#ProxyCommand /usr/bin/ncat --proxy-type http --proxy 192.168.43.1:44355 %h %p

-----------------------------------------------
-----------------------------------------------
Quanto ao snap

---crie ou edite o arquivo:

$ sudo gedit /etc/systemd/system/snapd.service.d/http-proxy.conf
---Adicione as linhas:

[Service]
Environment="http_proxy=http://192.168.43.1:44355/"
---crie ou edite o arquivo:

sudo gedit /etc/systemd/system/snapd.service.d/https-proxy.conf

---Adicione as linhas:

[Service]
Environment="https_proxy=http://192.168.43.1:44355/"

--reinicie os serviços:

     sudo systemctl daemon-reload
     sudo systemctl restart snapd
