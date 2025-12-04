## Introduction to  Configuration  Management in Kubernetes  with Kustomize


### Lesson 1.1: Understanding Kubernetes Configuration
- Objective

Familiarize with Kubernetes objects and configurations.

Tasks and Detailed Steps
1. Explore Kubernetes Documentation

   Read about :
- Pods

- Deployments

- Services

- ConfigMaps

  ###  2. Write a Sample Pod YAML File
**Step 1:** Create a file named mypod.yaml.
**Step 2:** Insert the following content:

```
   apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: mycontainer
      image: nginx
```
**Explanation**

This YAML file defines a Kubernetes Pod named mypod with a single container named mycontainer running the NGINX image.

**3. Discussion**

Managing Kubernetes configurations consistently is essential because:

**Scalability:**

As applications grow, more Pods, Deployments, Services, and ConfigMaps are introduced. A consistent approach prevents errors and configuration drift.

**Maintainability:**

Standardized YAML files make it easier for teams to understand, audit, and update configurations.

**Reproducibility:**

Consistent configurations ensure that environments (development, staging, production) behave the same way.

**Collaboration:**

Teams can work more effectively when everyone follows a clear configuration structure.

**Reduced Risk:**

Misconfigurations can lead to outages or vulnerabilities. Organized configuration management lowers these risks.

### Lesson 1.3: Setting Up the Environment
**Objective**

Install Kustomize and set up a basic Kubernetes cluster using Minikube.

**Task 1: Install Kustomize**

Kustomize is a tool that allows you to customize raw, template-free YAML files for multiple purposes, essential in Kubernetes deployments.

### Detailed Steps for Linux
### 1. Visit the Kustomize GitHub Releases Page

Open your browser and go to the
   
   - Kustomize GitHub Releases
 page.

### 2. Select the Appropriate Version

- Choose the version suitable for your Linux OS.

- Look for a file that ends with:

```
  linux_amd64.tar.gz
```
 
  **3. Download the tar.gz File**

- Click on the link for the file:

```
 linux_amd64.tar.gz
```

**4. Open Terminal**

- Open your terminal application.

**5. Navigate to the Download Directory**

- Use the cd command to move to where the downloaded file is located.

Example:
 
 ```
  cd ~/Downloads
```
**6. Extract the File**

Run:

```
 tar xzf kustomize_vX.Y.Z_linux_amd64.tar.gz
```
Replace X.Y,z with the right version

**7. Move Kustomize to Your PATH**

- Move the extracted kustomize file to a directory in your system PATH for easy access.
The /usr/local/bin directory is a common choice.
 Run
  
```
   sudo mv kustomize /usr/local/bin/
 ```

**8. Check Kustomize Installation**

- Verify the installation by running:
  
 ```
  kustomize version
 ```
- You should see the installed version number displayed.  

### 1. Kustomize Installation

This part is simply telling you:

Install Kustomize

Move it into /usr/local/bin/

Then confirm installation using:

 ```
 kustomize version
 ```
If this shows a version, Kustomize is correctly installed.

### 2. Setting Up Minikube 

This part teaches you:

**Install Minikube**

Go to Minikube official page and install it for your OS.

Start Minikube
Run:
```
minikube start
```

This starts a Kubernetes cluster on your system.

**Verify Minikube**

Check that Kubernetes is up:

```
 kubectl get nodes
```
Expectated Output:

```
  NAME       STATUS   ROLES    AGE   VERSION
minikube   Ready    control-plane   2m   v1.30.0
```

### 3. Why You Are Seeing These Steps

These steps belong to a practical Kubernetes lab, where you:

- Install Kustomize

- Install Minikube

- Verify both

- Then proceed to work on YAML files

- Then use Kustomize overlays to customize deployments

### verification Task

**1 Check Kustomized version**

Run:

```
 Kustomized version
```

**2 Check Kubctl version**
Run:

```
 Kubctl version
```

### 4. Creating a Kustomize Project

```
   k8s-project/

     ├── base

       │   ├── deployment.yaml

       │   └── kustomization.yaml

     └── overlays
       
        ├── dev
    
           │   └── kustomization.yaml
    
       └── prod
    
              └── kustomization.yaml
 ```

base/deployment.yaml

  ```
       apiVersion: apps/v1
      kind: Deployment
      metadata:
     name: webapp
     spec:
      replicas: 1
     selector:
      matchLabels:
      app: webapp
     template:
      metadata:
       labels:
        app: webapp
     spec:
      containers:
      - name: webapp
        image: nginx:latest
        ports:
        - containerPort: 80
   ```

error: no objects passed to apply

  ```
   base/kustomization.yaml
  
  ```
   resources:
  - deployment.yaml
 ```

### 5. Running Kustomize
Build the final YAML

```
  kustomize build base
```

**Apply directly via kubectl

```
kubectl apply -k overlays/dev
```

overlays/dev/kustomization.yaml

```
  resources:
  - ../../base (uses the base resources)

  patches:
  - patch.yaml(Applies the patch created)
 ```

 overlays/dev/patch.yaml

```
 apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp
spec:
  replicas: 2
```

N/B: Overlays should only override what is different from the base.which include:
- replicas
- service type
- environment
- image tag/version
- resource limit
- ConfigMap

**Apply the dev overlay**

```
kubectl apply -k overlays/dev
```
### 8. Testing Your Setup

```
 kubectl get nodes
```
**Check deployment applied**

```
kubectl get deployments
```

Expected output example:

```
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
webapp    2/2     2            2           30s
```

**Validate final build**

```
kustomize build overlays/dev
```
