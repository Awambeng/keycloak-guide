# Working with keycloak operator
A keycloak operator is a tool that simplifies the deployment and management of keycloak in a kubernetes or openshift environment. In this guide we will see how to install the keycloak operator in kubernetes and openshift.

## What is kubernetes?
Kubernetes is an open-source container orchestration tool developed by google. It is used to automate the deployment, scaling and management of containerizd applications.

## What is OpenSift?
OpenShift on the other hand is a container orchectration tool based on kubernetes that was developed by Red Hat. So openshift is build on kubernetes and adds additional features and capabilties for enterprise workload.

## Installing keycloak operator on kubernetes using OLM (Operator Lifecycle Manager)
An operator in kubernetes is a software extension that makes use of custom resources to manage applications and their components on a kubernetes cluster while the operator lifecycle manager is a component in the kubernetes ecosystem that helps to manage the full lifecycle of the operators and the applications they manage.

So to simplify everythig the operator manages applications in a kubernetes cluster while the operator lifecycle manager manages the operators ensuring that they are installed, configured and updated correctly.

## Creating a new virtual machine
Before running this command make sure you have multipass installed.
```bash
multipass launch --name keycloak-operator --cpus 2 --memory 8G --disk 60G
```

## Mount your home directory into the vm
```bash
multipass mount $HOME vm_name
```

## Check status of the vm
```bash
multipass list
```

## Install k3s
k3s is a lightweight kubernetes distribution designed for prouction environment. To install k3s use the command below:
```bash
curl -sfL https://get.k3s.io | sh -
```
To be able to interact with your kubernetes cluster you should install k9s using the following command:
```bash
curl -sS https://webinstall.dev/k9s | sh
```

## Install OLM (Operator Lifecycle Manager)
```bash
curl -sL https://github.com/operator-framework/operator-lifecycle-manager/releases/download/v0.22.0/install.sh | bash -s v0.22.0
```

## Verify that OLM was installed
```bash
kubectl get pods -n olm # the -n option stands for namespace so we are listing the pods that runs in the olm namespace
```

Note: The OLM (Operator Lifecycle Manager) components are installed in the olm namespace.

To verify if you have permission to perform actions across all namespaces use the command below:
```bash
kubectl auth can-i '*' '*' --all-namespaces
```

## Installing keycloak operator by using kubectl without Operator Lifecycle Manager
You can install the operator on a kubernetes cluster using the kubectl command. But before that we need to install the CRDs (Custom Resource Definitions). The custom resource definition is a feature that enables us to create our own resources and once they are created they are also managed by kubernetes like pods, deployment and services. It can be installed by the following commands.
```bash
kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-k8s-resources/25.0.0/kubernetes/keycloaks.k8s.keycloak.org-v1.yml
kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-k8s-resources/25.0.0/kubernetes/keycloakrealmimports.k8s.keycloak.org-v1.yml
```

## Install the Keycloak Operator deployment by entering the following command:
```bash
kubectl apply -f https://raw.githubusercontent.com/keycloak/keycloak-k8s-resources/25.0.0/kubernetes/kubernetes.yml
```
Note: The operator only watches the namespace in which it was installed.

In Kubernetes, a Custom Resource Definition (CRD) is a powerful feature that allows you to extend the Kubernetes API by defining your own custom resources. The role of CRDs in Kubernetes is to provide a way for you to create and manage your own domain-specific, application-centric APIs.

To check in which namespace your keycloak operator is installed you can use the command below which comes with all the running pods and their respective namespaces.
```bash
kubectl get pods --all-namespaces
kubectl get pods --all-namespaces | grep -i "keycloak-operator" # to get only the keycloak operator.
```

