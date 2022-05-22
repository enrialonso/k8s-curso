# Curso k8s

________________________

### Fuentes

- PeladoNerd - [Canal YouTube](https://www.youtube.com/channel/UCrBzBOMcUVV8ryyAU_c6P5g)
    - Video [KUBERNETES 2021 - De NOVATO a PRO! (CURSO COMPLETO)](https://www.youtube.com/watch?v=DCoBcpOA7W4)
- Udemy: [Kubernetes: Desde cero para principiantes | ¡Con Ejercicios!](https://www.udemy.com/course/kubernetes-aprende/)

Documentacion oficial de k8s: https://kubernetes.io/es/

#### Herramientas esenciales

#### Instalar `kubectl` [link](https://kubernetes.io/docs/tasks/tools/)

<details>
  <summary>En linux</summary>

Descargar el binario

  ```bash
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  ```

Instalar

  ```bash
  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
  ```

Comprobar

  ```bash
  kubectl version --client --output=yaml
  ```

  <details>
    <summary>Salida</summary>

  ```bash
  clientVersion:
    buildDate: "2022-03-16T15:58:47Z"
    compiler: gc
    gitCommit: c285e781331a3785a7f436042c65c5641ce8a9e9
    gitTreeState: clean
    gitVersion: v1.23.5
    goVersion: go1.17.8
    major: "1"
    minor: "23"
    platform: linux/amd64
  ```

  </details>
</details>

#### Instalar `Minikube` [link](https://minikube.sigs.k8s.io/docs/start/)

<details>
  <summary>En linux</summary>

Descargar el binario

  ```bash
  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  ```

Instalar

  ```bash
  sudo install minikube-linux-amd64 /usr/local/bin/minikube
  ```

Comprobar

  ```bash
  minikube version
  ```

  <details>
    <summary>Salida</summary>

  ```bash
  minikube version: v1.25.2
  commit: 362d5fdc0a3dbee389b3d3f1034e8023e72bd3a7
  ```

  </details>
</details>

#### Otras herramientas interesantes para chequear

#### Instalar `kubecolor` [link](https://github.com/hidetatz/kubecolor/releases)

<details>
  <summary>En linux</summary>

- Descargar el binario - [link](https://github.com/hidetatz/kubecolor/releases)
- Descomprimir el binario

```bash
curl -LO https://github.com/hidetatz/kubecolor/releases/download/v0.0.20/kubecolor_0.0.20_Linux_x86_64.tar.gz
tar -xf kubecolor_0.0.20_Linux_x86_64.tar.gz kubecolor
```

**Instalar**

```bash
sudo install kubecolor /usr/local/bin/kubecolor
```

**Comprobar**

```bash
kubecolor version
```

  <details>
    <summary>Salida</summary>

  ```bash
  Client Version: version.Info{Major:"1", Minor:"23", GitVersion:"v1.23.5", GitCommit:"c285e781331a3785a7f436042c65c5641ce8a9e9", GitTreeState:"clean", BuildDate:"2022-03-16T15:58:47Z", GoVersion:"go1.17.8", Compiler:"gc", Platform:"linux/amd64"}
  ```

  </details>

**Configurar**

En el fichero del profile para la terminal (ejemplo: `~/.bashrc`) poner el alias

  ```bash
  alias kubectl="kubecolor"
  ```

</details>

**Lens Desktop** - Un IDE para k8s - **[Link](https://k8slens.dev/desktop.html)**

<details>
  <summary>En linux</summary>

- Descargar el binario - [link](https://k8slens.dev/desktop.html)

```bash
curl -LO https://api.k8slens.dev/binaries/Lens-5.4.6-latest.20220428.1.amd64.deb
```

**Instalar**

```bash
sudo dpkg -i Lens-5.4.6-latest.20220428.1.amd64.deb
```

**Comprobar**

```bash
lens --version 
```

</details>

### Manos a la obra

Montar el cluster en local con `minikube`

La idea es montar un cluster en local y poder jugar con él aprendiendo los conceptos base. Para esto usaremos `minikube` 
y como interfase con el cluster usaremos `kubectl`.

```bash
minikube start
```

Este comando crea un cluster en local con el que podemos jugar, por defecto monta una máquina virtual con 2 cpus 
y 8 Gb de ram.

```bash
minikube dashboard
```

Con este comando abre en el navegador un dashboard web que está instalado por defecto en nuestro `minikube` para 
interactuar con el cluster y obtener informacion de nuestro cluster.

<img height="400" src="images/minikube-dashboard.png"/>

```bash
minikube addons list
```

Lista todos los addons disponibles para `minikube` y podemos ver cuáles son los que están habilitados.

<details>
  <summary>Salida</summary>

```bash
|-----------------------------|----------|--------------|--------------------------------|
|         ADDON NAME          | PROFILE  |    STATUS    |           MAINTAINER           |
|-----------------------------|----------|--------------|--------------------------------|
| ambassador                  | minikube | disabled     | third-party (ambassador)       |
| auto-pause                  | minikube | disabled     | google                         |
| csi-hostpath-driver         | minikube | disabled     | kubernetes                     |
| dashboard                   | minikube | enabled ✅   | kubernetes                     |
...
```

</details>

### Conceptos basicos de Kubernetes

### [Namespaces](https://kubernetes.io/es/docs/concepts/overview/working-with-objects/namespaces/)
Kubernetes soporta múltiples clústeres virtuales respaldados por el mismo clúster físico. Estos clústeres virtuales se 
denominan espacios de nombres (namespaces). Puedes separar de forma logica las cargas de trabajo dentro del cluster. 
Existe algunos namespaces por defecto

<img height="500" src="images/k8s-namespaces.png"/>

```bash
kubectl get namespaces
```

<details>
  <summary>Salida</summary>

```bash
NAME                   STATUS   AGE
default                Active   47h
kube-node-lease        Active   47h
kube-public            Active   47h
kube-system            Active   47h
kubernetes-dashboard   Active   47h
```
</details>

___

### [Pods](https://kubernetes.io/docs/concepts/workloads/pods/)
Son las unidades más pequeñas que se pueden desplegar dentro de un cluster de kubernetes, es la forma que tiene k8s de
agrupar  uno o varios contenedores para una carga de trabajo.

```bash
kubectl get pod
```

<details>
  <summary>Salida</summary>

```bash
NAME                     READY   STATUS    RESTARTS   AGE
nginx-85b98978db-hjf68   1/1     Running   0          55m
```
</details>

Comando crear pod
```bash
kubectl apply -f ./files/simple-pod.yaml
```
Comando estado del pod
```bash
kubectl get pod simple-pod -o wide
```
Comando borrar pod
```bash
kubectl delete pod simple-pod
```

<details>
  <summary>Manifiesto de un pod</summary>

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-pod
spec:
  containers:
  - name: nginx
    image: nginx:alpine
```
</details>

___

### [Deployment](https://kubernetes.io/es/docs/concepts/workloads/controllers/deployment/)
Es un tipo de controlador de k8s, ss la unidad de más alto nivel que podemos gestionar en Kubernetes. 
Nos permite definir diferentes funciones:

- Control de réplicas
- Escabilidad de pods
- Actualizaciones continuas
- Despliegues automáticos
- Rollback a versiones anteriores

```bash
kubectl get deployment
```

<details>
  <summary>Salida</summary>

```bash
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
nginx   1/1     1            1           65m
```
</details>

Ejemplo de manifiesto para un deployment

<details>
  <summary>Manifiesto</summary>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        resources:
          requests:
            memory: "64Mi"
            cpu: "200m"
          limits:
            memory: "128Mi"
            cpu: "500m"
        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 10
        livenessProbe:
          tcpSocket:
            port: 80
          initialDelaySeconds: 15
          periodSeconds: 20
        ports:
        - containerPort: 80
```
</details>

Crear deployment
```bash
kubectl apply -f ./files/simple-deployment.yaml
```
Estado del deployment
```bash
kubectl get deployment nginx-deployment
```
Borrar deployment
```bash
kubectl delete deployment nginx-deployment
```
___
### [Daemonset](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
Es otro tipo de controlador de k8s y muy similar al deployment, pero no tiene réplicas, este controlador lo que hace es 
desplegar un pod o pods por cada máquina que tenga el cluster, es similar a los deployments pero no tiene la propiedad para 
setear las replicas los casos de uso mas frecuentes son:

- Monitoreo de los nodos del cluster
- Recoleccion de logs de los nodos del cluster

```bash
kubectl get daemonset
```

<details>
  <summary>Salida</summary>

```bash
NAME               DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
nginx-deployment   1         1         0       1            0           <none>          4s
```
</details>

Ejemplo de manifiesto para un daemonset

<details>
  <summary>Manifiesto</summary>

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:alpine
        resources:
          requests:
            memory: "64Mi"
            cpu: "200m"
          limits:
            memory: "128Mi"
            cpu: "500m"
        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 10
        livenessProbe:
          tcpSocket:
            port: 80
          initialDelaySeconds: 15
          periodSeconds: 20
        ports:
        - containerPort: 80
```
</details>

Crear daemonset
```bash
kubectl apply -f ./files/simple-daemonset.yaml
```
Estado del daemonset
```bash
kubectl get daemonset nginx-daemonset
```
Borrar daemonset
```bash
kubectl delete daemonset nginx-daemonset
```

___
### [Statefulset](https://kubernetes.io/es/docs/concepts/workloads/controllers/statefulset/)
También es una forma de crear pods, pero con un volumen asociado para mantener el estado, es decir que es como un deployment 
en el que cada pod tiene asociado un volumen de almacenamiento unico por pod en donde el pod y solo ese pod lo usa para 
mantener el estado de la aplicacion.

<img height="600" src="images/statefulset.png"/>

```bash
kubectl get statefulset
```

<details>
  <summary>Salida</summary>

```bash
NAME               DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
nginx-deployment   1         1         0       1            0           <none>          4s
```
</details>

Ejemplo de manifiesto para un daemonset

<details>
  <summary>Manifiesto</summary>

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: simple-statefulset
spec:
  serviceName: nginx
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: k8s.gcr.io/nginx-slim:0.8
          ports:
            - containerPort: 80
              name: web
          volumeMounts:
            - name: www
              mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
    - metadata:
        name: www
      spec:
        accessModes: [ "ReadWriteOnce" ]
        resources:
          requests:
            storage: 1Gi
```
</details>

Crear statefulset
```bash
kubectl apply -f ./files/simple-statefulset.yaml
```
Estado del statefulset
```bash
kubectl get statefulset simple-statefulset
```
Borrar statefulset
```bash
kubectl delete statefulset simple-statefulset
```
____
### Networking en k8s

Las comunicaciones entre aplicaciones esta a la orden del dia, y es muy dificil encontrar una aplicacion hoy por hoy 
que no necesite comunicarse con el entorno que le rodea. Esto no es distinto dentro de un cluster de k8s, es mas, de primeras
complica más las cosas porque k8s va de compartir instancias o nodos entre aplicaciones, donde los pods pueden estar desplegados
en distintos nodos y aun asi tiene que mantener la comunicacion.

En esta parte de la [documentacion oficial](https://kubernetes.io/docs/concepts/services-networking/) puedes tener mas 
detalle.

![](images/basic-networking-k8s.png)

En la imagen de arriba hay varias cosas que tenemos que tener en cuenta

- Las ip de los nodos es distinta a la ip de los pods.
- Cada pod tiene una IP.
- Los containers dentro de un pod comparten la ip del pod que los contiene.
- `etcd` es la base de datos de k8s y es donde se almacena y comparte el estado del cluster entre los nodos.
- Los `CNI` o Container network interface.
- Los agentes que corren en todos los nodos (workers en la imagen) en este caso `calico`
  - `calico` en un CNI que se encarga del manejo de las redes en el cluster, existen otro tipo de CNI, aqui puedes conocer [otros](https://kubernetes.io/docs/concepts/cluster-administration/networking/#calico)
- Las `route-tables` creadas por el agente de CNI en el caso de la imagen `calico`.
- Toda la informacion de las `route-tables` se almacena en `etcd`.

En la imagen tenemos dos nodos con un pod en cada uno, si estos pods quieren comunicarse, el agente CNI almacenaria
en `etcd` las `route-tables` para se pueda establecer la comunicacion entre pods en el mismo o distintos nodos.

