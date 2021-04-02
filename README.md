# Memcached @ OKE

## Objetivo

O objetivo desse how-to é demonstrar a instalação do Memcached dentro de um cluster kubernetes. Através desse empacotamento é possível determinar o número de PODs para as replicas, instalar o monitoring e determinar um pool específico para instalação. 

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

:heartpulse:
