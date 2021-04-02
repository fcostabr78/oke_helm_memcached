# Memcached @ OKE

## Objetivo

O objetivo desse how-to é demonstrar a instalação do Memcached dentro de um cluster kubernetes. Através desse empacotamento é possível determinar o número de PODs para as replicas, instalar o monitoring e determinar um pool específico para instalação.<br>
Este empacotamento instalará o Memcached na versão 1.5.10, o mcrouter e o PHP Memcached Admin.


## Pré-requisito

1. Cluster Kubernetes provisiondo no Oracle Cloud Infrastruture <br>
   https://docs.oracle.com/pt-br/iaas/Content/ContEng/Concepts/contengoverview.htm

1.1 OCI CLI instalado e kubeconfig criado localmente


2. helm instalado localmente


## Etapas

Abra o Terminal e ingresse os seguintes comandos:

```
$ alias k=kubectl
```

```
$ helm init --service-account tiller --history-max 200 --upgrade
```

```
$ helm repo add memcached-oke 'https://raw.githubusercontent.com/fcostabr78/oke_helm_memcached/master/' 
```

```
$ helm repo update
```

```
$ k create ns memcached
```

```
$ helm install memcached-oke memcached-oke/oke-helm-memcached
```

<br> 
Ao executar o comando $k -n memcached get all, teremos a seguinte saida o mcrouter, memcache e phpadmin criado com o IP  publico e loadbalancer.


## Configuração do PHP Memcached Admin

Esse pacote faz a instalação de PHP Memcached admin. Como parte da atividade, foi criado um recurso de tipo service no OKE (Oracle Kubernetes). Obtenha o IP externo atribuído ao serviço. Ele estará apontando a porta 80:

```
$ k -n memcached get service/srv-memcachedadm
```

Copie o IP e acesse a página deste endereço de IP pelo navegador de sua preferência.

<table>
    <tbody>
        <tr>
        <th><img align="left" width="600" src="https://objectstorage.us-ashburn-1.oraclecloud.com/n/idsvh8rxij5e/b/imagens_git/o/Screenshot%20from%202021-04-02%2013-45-06.png"/></th>
        </tr>
    </tbody>
</table>


Clique no link "Edit Configuration"

Será aberta a seguinte tela. Há o bloco "Server List". Ingresse lá os dados do cluster do memcached.<br>
Como foi instalado com duas replicas, dois POD foram criados e devemos registrar os dois na lista. <br>

```
Cluster: mcrouter

Name: memcached-0
IP/Hostname: memcached-0.memcached.memcached.svc.cluster.local
Port: 11211

Name: memcached-1
IP/Hostname: memcached-1.memcached.memcached.svc.cluster.local
Port: 11211
```

<table>
    <tbody>
        <tr>
        <th><img align="left" width="600" src="https://objectstorage.us-ashburn-1.oraclecloud.com/n/idsvh8rxij5e/b/imagens_git/o/Screenshot%20from%202021-04-02%2013-46-43.png"/></th>
        </tr>
    </tbody>
</table>


Após adicionar clique no botão "Save Servers Configuration".

Para validar que tudo está ok, clique no link "See Live Stats" e aguarde 5s.

<table>
    <tbody>
        <tr>
        <th><img align="left" width="600" src="https://objectstorage.us-ashburn-1.oraclecloud.com/n/idsvh8rxij5e/b/imagens_git/o/Screenshot%20from%202021-04-02%2013-59-56.png"/></th>
        </tr>
    </tbody>
</table>




:heartpulse:
