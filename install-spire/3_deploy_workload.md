Now that both the SPIRE server and agents are running, let's deploy a debug workload to test the SPIFFE identity system. We'll use the `spirldbg` tool to fetch and inspect SVIDs (SPIFFE Verifiable Identity Documents).

## Deploy the Debug Workload

Let's deploy a simple workload that includes the `spirldbg` debugging tool:

`kubectl create -f debug-workload.yaml`{{exec}}

Wait for the debug workload to be ready:

`kubectl wait --for=condition=ready pod/debug-workload --timeout=300s`{{exec}}

## Create Registration Entry for the Debug Workload

Now let's create a registration entry for our debug workload so it can receive a SPIFFE identity:

```
kubectl exec -n spire spire-server-0 -- \
    /opt/spire/bin/spire-server entry create \
    -spiffeID spiffe://example.org/ns/default/sa/default \
    -parentID spiffe://example.org/ns/spire/sa/spire-agent \
    -selector k8s:ns:default \
    -selector k8s:sa:default
```{{exec}}

## Test SPIFFE Identity Retrieval

Now let's exec into the debug workload and use `spirldbg` to fetch identities from the SPIRE system.

First, let's get an interactive shell in the debug workload:

`kubectl exec -it debug-workload -- /bin/sh`{{exec}}

### Fetch an X.509 SVID

Inside the pod, let's fetch an X.509 SVID (certificate-based identity):

`spirldbg svid-x509`{{exec}}

This command retrieves the X.509 certificate that serves as the workload's cryptographic identity. You should see:
- The SPIFFE ID assigned to this workload
- The X.509 certificate details
- The trust bundle information

### Fetch a JWT SVID

Now let's fetch a JWT SVID (JSON Web Token-based identity):

`spirldbg svid-jwt`{{exec}}

This command retrieves a JWT token that contains the workload's identity claims. You should see:
- The JWT token
- The SPIFFE ID in the token claims
- Token metadata (expiration, audience, etc.)

### Verify Trust Bundle

Let's also check the trust bundle that validates other workloads:

`spirldbg trust-bundle-x509`{{exec}}

### Run Quick Diagnostics

Finally, let's run a comprehensive check of the SPIFFE setup:

`spirldbg check`{{exec}}

This command performs automatic checks to verify:
- Presence of the SPIFFE API socket
- Ability to fetch SVIDs and trust bundles
- Overall system health

Exit the pod when you're done exploring:

`exit`{{exec}}

## Verify SPIRE Registration

You can also verify that the workload was properly attested by checking the SPIRE server logs:

`kubectl logs -n spire spire-server-0 | grep -i attest`{{exec}}

Congratulations! You've successfully deployed SPIRE and verified that workloads can retrieve their SPIFFE identities. The debug workload now has both X.509 and JWT-based identities that can be used for secure service-to-service communication.