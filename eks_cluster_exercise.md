
# Ejercicio Práctico Paso a Paso: Creación de un Cluster Básico en AWS EKS

## Configuración de la CLI de AWS:

### Instalar y configurar AWS CLI.
```bash
# Instalar AWS CLI (si no está instalado)
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /

# Configurar AWS CLI
aws configure
```
Ingresar las credenciales de AWS (Access Key, Secret Key, región, formato de salida).

# Instalar eksctl:

## En Windows:
1. Descarga eksctl desde [este enlace](https://github.com/weaveworks/eksctl/releases).
2. Extrae el archivo y agrega `eksctl.exe` a tu PATH.

## En macOS:
```bash
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl
```

## En Linux:
```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

## Crear el clúster con `eksctl`
Ejecuta el siguiente comando para crear un clúster de Amazon EKS:

```bash
eksctl create cluster \
  --name camp-cluster \
  --version 1.26 \
  --region us-east-1 \
  --nodegroup-name camp-workers \
  --node-type t3.medium \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 4 \
  --managed
```

Nota: Ajusta los parámetros según la región y necesidades específicas.

## Verificación del Cluster con kubectl:

### Instalar kubectl si no está instalado.
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```
### Verificar el estado del clúster
```bash
eksctl get cluster --name camp-cluster --region us-east-1
```

### Configurar kubectl para apuntar al clúster
```bash
aws eks --region us-east-1 update-kubeconfig --name camp-cluster
```

### Verifica el contexto de kubectl:

```bash
kubectl config get-contexts

```

### Paso 1.4: Verificar los nodos
Verifica que los nodos estén en estado **Ready**:

```bash
kubectl get nodes
```

### Salida esperada:

| NAME                             | STATUS   | ROLES  | AGE     | VERSION                  |
|-----------------------------------|----------|--------|---------|--------------------------|
| ip-192-168-59-211.ec2.internal    | Ready    | <none> | 3m14s   | v1.26.15-eks-f479e3c      |
| ip-192-168-59-26.ec2.internal     | Ready    | <none> | 2m59s   | v1.26.15-eks-f479e3c      |
| ip-192-168-8-65.ec2.internal      | Ready    | <none> | 3m13s   | v1.26.15-eks-f479e3c      |


# Desplegar una aplicación Nginx

## Paso 2.1: Crear un manifiesto de despliegue para Nginx

Crea un archivo llamado `nginx-deployment.yaml` con el siguiente contenido:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
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
        image: nginx:latest
        ports:
        - containerPort: 80
```
### Aplicar el manifiesto para desplegar Nginx

```bash
kubectl apply -f nginx-deployment.yaml
```

### Salida esperada
```bash
deployment.apps/nginx-deployment created
```

### Exponer el despliegue como un servicio de tipo LoadBalancer
```bash
kubectl expose deployment nginx-deployment --type=LoadBalancer --name=nginx-service
```

### Salida esperada
```bash
service/nginx-service exposed
```

## Conclusión

En este tutorial, has desplegado una aplicación simple de Nginx en un clúster de Amazon EKS y la has expuesto al exterior utilizando un balanceador de carga. ¡Felicidades por completar el ejercicio!
