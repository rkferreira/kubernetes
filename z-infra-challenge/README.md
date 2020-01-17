# Z infra test

Bom dia!

O pacote "mycv" faz o deploy da imagem oficial da atlassian/bitbucket no DockerHub.
Sao provisionados 3 PODs, atras de um servico "mycv" e eh exposto externamente usando o nginx ingress com support a TLS.
Pode faltar alguns parametros para configuracao do bitbucket (volume, jvm options, etc), que acredito que voce queria um Dockerfile para setar algumas destas coisas.
Tem um arquivo de teste  `bitbucket-single_pod.yaml` tambem onde seto alguns parametros do env para o bitbucket para exemplo/teste.

No meu laptop com minikube, tenho problemas de recursos para rodar tudo :)

Abs

```
helm install nginx-ingress stable/nginx-ingress --set controller.replicaCount=2  \
                  --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux  \
              --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```


`helm install mycv mycv-0.1.0.tgz`


------------------------------------

```
$ kubectl get all

NAME                                                 READY   STATUS        RESTARTS   AGE
pod/mycv-b49cb544d-4nfms                             0/1     Pending       0          9m51s
pod/mycv-b49cb544d-gt42c                             0/1     Pending       0          9m51s
pod/mycv-b49cb544d-pnxrt                             0/1     Pending       0          9m51s
pod/nginx-ingress-controller-8cfc5766d-gqk77         0/1     Pending       0          18m
pod/nginx-ingress-controller-8cfc5766d-s7w7f         0/1     Pending       0          18m
pod/nginx-ingress-default-backend-564c96d9c4-s874m   0/1     Pending       0          18m


NAME                                    TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)                      AGE
service/kubernetes                      ClusterIP      10.96.0.1       <none>        443/TCP                      86m
service/mycv                            ClusterIP      10.96.90.61     <none>        7990/TCP                     9m51s
service/nginx-ingress-controller        LoadBalancer   10.96.113.185   <pending>     80:31397/TCP,443:31038/TCP   18m
service/nginx-ingress-default-backend   ClusterIP      10.96.153.71    <none>        80/TCP                       18m


NAME                                            READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mycv                            0/3     3            0           9m51s
deployment.apps/nginx-ingress-controller        0/2     2            0           18m
deployment.apps/nginx-ingress-default-backend   0/1     1            0           18m

NAME                                                       DESIRED   CURRENT   READY   AGE
replicaset.apps/mycv-b49cb544d                             3         3         0       9m51s
replicaset.apps/nginx-ingress-controller-8cfc5766d         2         2         0       18m
replicaset.apps/nginx-ingress-default-backend-564c96d9c4   1         1         0       18m




$ kubectl get ingress
NAME   HOSTS             ADDRESS   PORTS     AGE
mycv   mycv.myrepo.com             80, 443   10m
```

------------------------------------
