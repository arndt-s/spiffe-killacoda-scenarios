# Install SPIRE into Kubernetes

Welcome to this hands-on tutorial where you'll learn how to deploy SPIRE (SPIFFE Runtime Environment) into a Kubernetes cluster.

## What is SPIRE?

SPIRE is an open-source implementation of the SPIFFE (Secure Production Identity Framework for Everyone) specification. It provides:

- **Secure Identity Management**: Automatically issues cryptographic identities to workloads
- **Zero-Trust Architecture**: Enables mutual TLS authentication between services
- **Platform-Agnostic**: Works across different platforms and orchestrators
- **Attestation-Based**: Verifies workload identity through platform-specific attestation

## What You'll Learn

In this scenario, you will:

1. **Deploy the SPIRE Server** - The central authority that manages identities and issues SVIDs (SPIFFE Verifiable Identity Documents)
2. **Install SPIRE Agents** - The components that run on each node to attest workloads and retrieve identities
3. **Verify the Installation** - Confirm that agents successfully connect to the server

## Prerequisites

This tutorial uses a single-node Kubernetes cluster that's already configured and ready to use. All necessary YAML manifests are provided in the `/root` directory.

Let's begin by setting up the SPIRE server!

