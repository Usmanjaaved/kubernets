# WordPress with MySQL and SFTP on Kubernetes

This project demonstrates how to deploy a WordPress site along with MySQL and SFTP services on a local Kubernetes cluster using Minikube and Docker.

## Prerequisites
- **Minikube** and **Docker** installed and running locally.
- Basic knowledge of Kubernetes objects like Deployments, Services, ConfigMaps, and Secrets.

---

## Deployment Overview
This project involves the following components:
1. **MySQL** Deployment and Service:
   - Persistent storage using PersistentVolume and PersistentVolumeClaim.
   - Database initialization script using a ConfigMap.
   - Secure password management using Kubernetes Secrets.

2. **WordPress** Deployment and Service:
   - Persistent storage for WordPress data.
   - Integration with MySQL using Kubernetes Secrets for database credentials.

3. **SFTP** Service:
   - Exposes an SFTP endpoint for file uploads and management.
   - Credentials stored securely using Kubernetes Secrets.

---

## File Descriptions
### 1. MySQL Configuration Files
- **`mysql-deployment.yaml`**: Defines MySQL deployment with persistent storage and initialization script.
- **`mysql-pv.yaml`**: Defines the PersistentVolume for MySQL data.
- **`mysql-pvc.yaml`**: Defines the PersistentVolumeClaim for MySQL.
- **`mysql-secret.yaml`**: Stores the MySQL root password as a Kubernetes Secret.
- **`mysql-service.yaml`**: Exposes MySQL via a NodePort Service.
- **`mysql-init-configmap.yaml`**: Contains the SQL script to initialize the database.

### 2. WordPress Configuration Files
- **`wordpress-deployment.yaml`**: Defines WordPress deployment with database integration.
- **`wordpress-pvc.yaml`**: Defines the PersistentVolumeClaim for WordPress data.
- **`wordpress-service.yaml`**: Exposes WordPress via a NodePort Service.
- **`wordpress-db-secret.yaml`**: Stores WordPress database credentials securely.

### 3. SFTP Configuration Files
- **`sftp-service.yaml`**: Exposes SFTP functionality for WordPress.
- **`sftp-credentials.yaml`**: Stores SFTP credentials as a Kubernetes Secret.

---

## Deployment Steps

1. **Start Minikube:**
   ```bash
   minikube start
   ```

2. **Deploy MySQL:**
   ```bash
   kubectl apply -f mysql-pv.yaml
   kubectl apply -f mysql-pvc.yaml
   kubectl apply -f mysql-secret.yaml
   kubectl apply -f mysql-init-configmap.yaml
   kubectl apply -f mysql-deployment.yaml
   kubectl apply -f mysql-service.yaml
   ```

3. **Deploy WordPress:**
   ```bash
   kubectl apply -f wordpress-pvc.yaml
   kubectl apply -f wordpress-db-secret.yaml
   kubectl apply -f wordpress-deployment.yaml
   kubectl apply -f wordpress-service.yaml
   ```

4. **Deploy SFTP:**
   ```bash
   kubectl apply -f sftp-credentials.yaml
   kubectl apply -f sftp-service.yaml
   ```

5. **Access WordPress:**
   - Get the NodePort of the WordPress Service:
     ```bash
     kubectl get service wordpress-service
     ```
   - Access WordPress using `http://<minikube_ip>:<nodePort>`.

6. **Access SFTP:**
   - Get the NodePort of the SFTP Service:
     ```bash
     kubectl get service wordpress-sftp
     ```
   - Use an SFTP client to connect using the retrieved NodePort and credentials stored in `sftp-credentials.yaml`.

---

## Cleanup
To delete all resources:
```bash
kubectl delete -f .
```

---

## Notes
- Ensure Minikube has sufficient resources (memory and CPUs) allocated.
- Secrets must be Base64-encoded before adding them to the YAML files.
- This setup is for local testing and development purposes only.

---

## License
This project is open-source and available under the MIT License.

