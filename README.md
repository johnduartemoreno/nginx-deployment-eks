
# Introducción a Kubernetes

## 1. Historia de Kubernetes y la Evolución de los Contenedores

La historia de Kubernetes comienza con el desarrollo de los contenedores, una tecnología que ha revolucionado la forma en que las aplicaciones se despliegan y gestionan en entornos de producción. Antes de los contenedores, las aplicaciones se ejecutaban en máquinas físicas, lo que implicaba que cada aplicación tenía su propio conjunto de dependencias y bibliotecas. Este enfoque resultaba ineficiente, ya que muchas veces las aplicaciones competían por los mismos recursos, y el aislamiento entre ellas era difícil de mantener.

A medida que las organizaciones comenzaron a moverse hacia la virtualización, esta proporcionó un nivel de abstracción que permitía ejecutar múltiples máquinas virtuales en un solo servidor físico. Sin embargo, las máquinas virtuales eran pesadas, con sistemas operativos completos que consumían recursos, lo que dificultaba la eficiencia. Aquí es donde entran los contenedores. Los contenedores, al compartir el mismo núcleo del sistema operativo, son mucho más ligeros y permiten un uso eficiente de los recursos, lo que ha llevado a su adopción masiva.

Kubernetes fue creado por Google, basado en su experiencia interna gestionando contenedores a gran escala con su sistema Borg. La idea detrás de Kubernetes era ofrecer una plataforma que pudiera gestionar de manera eficiente los contenedores en un clúster de servidores, asegurando que las aplicaciones puedan escalar, recuperarse de fallos, y mantenerse siempre disponibles.

En 2014, Google donó Kubernetes a la Cloud Native Computing Foundation (CNCF), lo que permitió que la comunidad de código abierto contribuyera a su desarrollo y adopción. Desde entonces, Kubernetes se ha convertido en el estándar de facto para la orquestación de contenedores, superando a otras alternativas como Docker Swarm y Apache Mesos.

## 2. Conceptos Fundamentales de Kubernetes

- **Pod**: El pod es la unidad más pequeña y básica en Kubernetes. Un pod representa uno o más contenedores que comparten el mismo entorno, red y almacenamiento. Los contenedores dentro de un pod están estrechamente relacionados y generalmente se ejecutan juntos, compartiendo recursos. Los pods son efímeros; si un pod falla, Kubernetes lo reemplaza con uno nuevo.
  
  Ejemplo práctico: Una aplicación de frontend en React que se comunica con un servidor backend en Node.js puede ejecutarse en un mismo pod, ya que están estrechamente acoplados y necesitan estar juntos en el mismo entorno.

- **Servicios**: Kubernetes abstrae los pods mediante los servicios, lo que permite acceder a las aplicaciones mediante una IP estable, sin importar cuántos pods se creen o destruyan detrás de ese servicio. Tipos de servicios:
  - `ClusterIP`: Permite la comunicación entre los pods dentro del clúster.
  - `NodePort`: Expone un puerto específico en cada nodo para el servicio.
  - `LoadBalancer`: Crea un balanceador de carga externo, útil en entornos en la nube.

- **Deployments**: Un deployment define cómo se gestionan los pods. Garantiza que un número específico de réplicas de un pod esté en ejecución. Si un pod falla, el deployment crea uno nuevo. También permite actualizaciones sin tiempo de inactividad a través de estrategias como _rolling updates_.

- **ConfigMaps y Secrets**: Los ConfigMaps se utilizan para inyectar variables de configuración en los contenedores, mientras que los Secrets manejan información sensible como contraseñas. Estos recursos permiten gestionar configuraciones sin acoplarlas al contenedor, mejorando la seguridad y flexibilidad.

## 3. Arquitectura de Kubernetes

Kubernetes sigue un modelo cliente-servidor, donde el **Master Node** gestiona el clúster, y los **Worker Nodes** ejecutan las aplicaciones.

### 3.1 Master Node

- **Etcd**: Base de datos distribuida que almacena toda la información del clúster, esencial para la recuperación de fallos.
- **Kube-scheduler**: Asigna los pods a los nodos disponibles, basándose en la disponibilidad de recursos y afinidad.
- **Kube-controller-manager**: Controla que el estado actual del clúster coincida con el estado deseado. Si faltan réplicas de pods, las crea automáticamente.

### 3.2 Worker Nodes

- **Kubelet**: Agente que garantiza que los contenedores estén en ejecución y saludables.
- **Kube-proxy**: Gestiona las reglas de red y asegura el enrutamiento correcto del tráfico de red hacia los pods, además de manejar el balanceo de carga dentro del clúster.

## 4. Componentes de Networking y Storage en Kubernetes

Kubernetes proporciona una red plana entre los pods, permitiendo que se comuniquen sin configuraciones adicionales. En cuanto al almacenamiento, ofrece **Persistent Volumes (PV)** y **Persistent Volume Claims (PVC)**, que permiten a los pods acceder a almacenamiento externo dinámicamente.

Ejemplo práctico: Una base de datos MySQL en Kubernetes puede utilizar un Persistent Volume que apunte a un disco en AWS o Azure. Los pods solicitan acceso a este almacenamiento mediante un PVC.

## 5. Interacción Práctica: Comandos de Kubectl en la Terminal

Algunos comandos básicos de `kubectl` para interactuar con un clúster de Kubernetes:

- Ver los pods en el clúster:
  ```bash
  kubectl get pods
  ```

- Ver los servicios disponibles:
  ```bash
  kubectl get services
  ```

- Describir un pod específico:
  ```bash
  kubectl describe pod <nombre-del-pod>
  ```

- Ver los logs de un pod:
  ```bash
  kubectl logs <nombre-del-pod>
  ```

Realizar estas interacciones puede ayudarte a demostrar cómo interactuar con el clúster de manera efectiva.
