# Curso k8s

________________________

### Fuentes

- PeladoNerd - [channel](https://www.youtube.com/channel/UCrBzBOMcUVV8ryyAU_c6P5g)
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

Instalar

  ```bash
  sudo install kubecolor /usr/local/bin/kubecolor
  ```

Comprobar

  ```bash
  kubecolor version
  ```

  <details>
    <summary>Salida</summary>

  ```bash
  Client Version: version.Info{Major:"1", Minor:"23", GitVersion:"v1.23.5", GitCommit:"c285e781331a3785a7f436042c65c5641ce8a9e9", GitTreeState:"clean", BuildDate:"2022-03-16T15:58:47Z", GoVersion:"go1.17.8", Compiler:"gc", Platform:"linux/amd64"}
  ```

  </details>

Configurar

En el fichero del profile para la terminal (ejemplo: `~/.bashrc`) poner el alias

  ```bash
  alias kubectl="kubecolor"
  ```

</details>

**Lens Desktop** - Un IDE para k8s - **[Link](https://k8slens.dev/desktop.html)**

<details>
  <summary>En linux</summary>

- Descargar el binario - [link](https://k8slens.dev/desktop.html)

Instalar

  ```bash
  sudo dpkg -i Lens-5.4.4-latest.20220325.1.amd64.deb
  ```

Comprobar

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

#### [Namespaces](https://kubernetes.io/es/docs/concepts/overview/working-with-objects/namespaces/)
Kubernetes soporta múltiples clústeres virtuales respaldados por el mismo clúster físico. Estos clústeres virtuales se 
denominan espacios de nombres (namespaces). Puedes separar de forma logica las cargas de trabajo dentro del cluster. 
Existe algunos namespaces por defecto

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
Son las unidades más pequeñas que se pueden desplegar dentro de un cluster de kubernetes, son un set de contenedores el 
cual puede albergar uno a más contenedores dentro.

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
kubectl apply -f ./files/simple-pod-nginx.yaml
```
Comando estado del pod
```bash
kubectl get pod nginx -o wide
```
Comando borrar pod
```bash
kubectl delete pod nginx
```

<details>
  <summary>Manifiesto de un pod</summary>

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:alpine
```
</details>

___

### [Deployment](https://kubernetes.io/es/docs/concepts/workloads/controllers/deployment/)
Es un tipo de controlador de k8s, Es la unidad de más alto nivel que podemos gestionar en Kubernetes. 
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
desplegar un pod por cada máquina que tenga el cluster, es similar a los deployments pero no tiene la propiedad para 
setear las replicaslos casos de uso mas frecuentes son:

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















