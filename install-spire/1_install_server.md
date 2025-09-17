The SPIRE server is the core component of the SPIRE system that issues and manages identities for workloads. Let's start by setting up the server infrastructure.

First, we need to create a dedicated namespace for our SPIRE deployment:

`kubectl create namespace spire`{{exec}}

Now let's create the foundational resources that the SPIRE server requires:

**ServiceAccount and RBAC Configuration:**

Create the ServiceAccount for the SPIRE server:

`kubectl create -f server-account.yaml`{{exec}}

Set up the ClusterRole to allow the server to read nodes and pods for agent attestation:

`kubectl create -f server-clusterrole.yaml`{{exec}}

Create the Role for managing the trust bundle ConfigMap within the SPIRE namespace:

`kubectl create -f server-role.yaml`{{exec}}

**Trust Bundle and Configuration:**

Create the ConfigMap that will store the SPIRE trust bundle (used by agents to verify the server):

`kubectl create -f spire-bundle-configmap.yaml`{{exec}}

Deploy the SPIRE server configuration:

`kubectl create -f server-configmap.yaml`{{exec}}

**Network Service:**

Create the Service to enable network access to the SPIRE server:

`kubectl create -f server-service.yaml`{{exec}}

**Deploy the SPIRE Server:**

Now we can deploy the SPIRE server as a StatefulSet:

`kubectl create -f server-statefulset.yaml`{{exec}}

Let's wait for the SPIRE server to be fully ready:

`kubectl wait --for=condition=ready pod/spire-server-0 -n spire --timeout=300s`{{exec}}

You can verify the server is running by checking its logs:

`kubectl logs -n spire spire-server-0`{{exec}}