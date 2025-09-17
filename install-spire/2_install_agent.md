Now that the SPIRE server is running, we need to install the SPIRE agent on each node. The SPIRE agent is responsible for attesting workloads and retrieving SVIDs from the SPIRE server.

First, let's create the necessary resources for the SPIRE agent:

- A ServiceAccount for the SPIRE agent: `kubectl create -f agent-account.yaml`{{exec}}
- A ClusterRole to allow the agent to query the Kubernetes API: `kubectl create -f agent-cluster-role.yaml`{{exec}}
- A ConfigMap with the SPIRE agent configuration: `kubectl create -f agent-configmap.yaml`{{exec}}

Now we can deploy the SPIRE agent as a DaemonSet to ensure it runs on every node:

`kubectl create -f agent-daemonset.yaml`{{exec}}

Let's wait for the SPIRE agents to be ready:

`kubectl wait --for=condition=ready pods -l app=spire-agent -n spire --timeout=300s`{{exec}}

## Register the Agent with SPIRE Server

Now we need to create a registration entry for the SPIRE agent. This tells the server what identities the agent is allowed to attest:

```
kubectl exec -n spire spire-server-0 -- \
    /opt/spire/bin/spire-server entry create \
    -node  \
    -spiffeID spiffe://example.org/ns/spire/sa/spire-agent \
    -selector k8s_psat:agent_ns:spire \
    -selector k8s_psat:agent_sa:spire-agent
```{{exec}}

Finally, let's verify that the agents have successfully attested to the server:

`kubectl exec -n spire spire-server-0 -- /opt/spire/bin/spire-server agent list`{{exec}}