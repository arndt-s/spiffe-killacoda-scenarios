At first we need to prepare the namespace used for our SPIRE installation:

`kubectl create namespace spire`{{exec}}

Once the namespace is created we need to create a couple of more resources that the server requires:

- A ServiceAccount, `kubectl create -f /root/server-account.yaml`{{exec}}
- A ClusterRole to allow reading nodes and pods for the attestation of SPIRE agents (PSAT attestation). `kubectl create -f /root/server-role.yaml`{{exec}}
- A ConfigMap where the latest bundle is stored. This is picked up by SPIRE agents in order to verify the server certificate of the SPIRE server. `kubectl create -f /root/spire-bundle-configmap.yaml`{{exec}}
- A Role to allow modification of above config map in the SPIRE namespace to store the latest bundle in it. `kubectl create -f /root/server-clusterrole.yaml`{{exec}}
- A Service to route traffic to SPIRE servers. `kubectl create -f /root/server-service.yaml`{{exec}}

Once these resources are created we can create the actual server StatefulSet:

`kubectl create -f /root/server-statefulset.yaml`{{exec}}