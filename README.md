# eko-k8s
Entregable k8s

Para ejecutar la aplicacion:
- Descargar el repositorio
- Copiar el fichero k8s-app-compose.yaml (unico fichero con multiples definiciones)
- Lanzar el deploy con:
        $ kubectl apply -f k8s-app-compose.yaml
- Verificar que el deploy se realizo correctamente, para eso listar todos los elementos:
  $ kubectl get all

  Deberia generar una salida simimar a:

  NAME                            READY   STATUS    RESTARTS      AGE
pod/apieko-5ff5c5d765-cfb6z     1/1     Running   0             39m
pod/dbeko-6c4bbbb4b6-tvft2      1/1     Running   1 (38m ago)   39m
pod/fronteko-74cbc4f54c-z8fc6   1/1     Running   0             39m
pod/webserver-54fc99c8d-bspb4   1/1     Running   0             36h
pod/webserver-54fc99c8d-gzl9v   1/1     Running   0             36h
pod/webserver-54fc99c8d-j5m4s   1/1     Running   0             36h

NAME                  TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
service/apieko        ClusterIP      10.97.80.244     <none>        8080/TCP         39m
service/dbeko         ClusterIP      10.111.239.186   <none>        5432/TCP         39m
service/fronteko      LoadBalancer   10.100.0.239     <pending>     80:30681/TCP     39m
service/kubernetes    ClusterIP      10.96.0.1        <none>        443/TCP          3d7h
service/web-lb        LoadBalancer   10.105.155.35    <pending>     8080:31229/TCP   36h
service/web-service   NodePort       10.97.88.157     <none>        80:30798/TCP     36h

NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/apieko      1/1     1            1           39m
deployment.apps/dbeko       1/1     1            1           39m
deployment.apps/fronteko    1/1     1            1           39m
deployment.apps/webserver   3/3     3            3           36h

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/apieko-5ff5c5d765     1         1         1       39m
replicaset.apps/dbeko-6c4bbbb4b6      1         1         1       39m
replicaset.apps/fronteko-74cbc4f54c   1         1         1       39m
replicaset.apps/webserver-54fc99c8d   3         3         3       36h


- Verificar la ip expuesta para acceder a la aplicacion:
    $ minikube service list

  Se obtiene una salida similar a:

  ┌──────────────────────┬───────────────────────────┬──────────────┬───────────────────────────┐
│      NAMESPACE       │           NAME            │ TARGET PORT  │            URL            │
├──────────────────────┼───────────────────────────┼──────────────┼───────────────────────────┤
│ default              │ apieko                    │ No node port │                           │
│ default              │ dbeko                     │ No node port │                           │
│ default              │ fronteko                  │ 80           │ http://192.168.49.2:30681 │
│ default              │ kubernetes                │ No node port │                           │
│ default              │ web-lb                    │ 8080         │ http://192.168.49.2:31229 │
│ default              │ web-service               │ 80           │ http://192.168.49.2:30798 │
│ kube-system          │ kube-dns                  │ No node port │                           │
│ kube-system          │ metrics-server            │ No node port │                           │
│ kubernetes-dashboard │ dashboard-metrics-scraper │ No node port │                           │
│ kubernetes-dashboard │ kubernetes-dashboard      │ No node port │                           │
└──────────────────────┴───────────────────────────┴──────────────┴───────────────────────────┘

- Por ultimo, navegar a la URL expuesta por fronteko en el puerto 80

  Aqui debe aparecer una pagina con 3 links que nos permitan hacer peticiones a la api.
  - El primer link es un mensaje de saludo
  - El segundo es un link a /health que testea la conexion a la base de datos y retorna OK en caso de exito
  - El tercer link, retorna la version de la api.
 
