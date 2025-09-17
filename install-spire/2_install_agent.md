Now that the SPIRE server is running, we need to install the SPIRE agent on each node. The SPIRE agent is responsible for attesting workloads and retrieving SVIDs from the SPIRE server.

First, let's create the necessary resources for the SPIRE agent:

- A ServiceAccount for the SPIRE agent: `kubectl create -f agent-account.yaml`{{exec}}
- A ClusterRole to allow the agent to query the Kubernetes API: `kubectl create -f agent-cluster-role.yaml`{{exec}}
- A ConfigMap with the SPIRE agent configuration: `kubectl create -f agent-configmap.yaml`{{exec}}

Now we can deploy the SPIRE agent as a DaemonSet to ensure it runs on every node:

`kubectl create -f agent-daemonset.yaml`{{exec}}

Let's wait for the SPIRE agents to be ready:

`kubectl wait --for=condition=ready pods -l app=spire-agent -n spire --timeout=300s`{{exec}}