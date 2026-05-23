# Service Mesh Interface (SMI)

> **Status: Archived.** CNCF archived the Service Mesh Interface project on **2023-10-03**, and the upstream `servicemeshinterface` GitHub organization was marked read-only on **2023-10-20**. Active development has moved to the [Kubernetes Gateway API GAMMA Initiative](https://gateway-api.sigs.k8s.io/mesh/gamma/), which reached GA in the Gateway API Standard Channel with v1.1.0. **New deployments should target Gateway API GAMMA, not SMI.**

This repository is part of the [API Evangelist Network](https://github.com/api-evangelist) and profiles the **Service Mesh Interface (SMI)** specification as a historical standard. It exists so consumers of the network can:

1. Recognize legacy SMI manifests still deployed in the wild.
2. Understand the conceptual lineage that fed into Gateway API GAMMA.
3. Migrate existing SMI configuration off the spec.

## What SMI Was

Per [smi-spec.io](https://smi-spec.io), SMI was *"a standard interface for service meshes on Kubernetes."* It was a CNCF Sandbox project that defined a vendor-neutral set of Kubernetes Custom Resource Definitions (CRDs) for the three most common service-mesh concerns:

- **Traffic Policy** — who can talk to whom (authorization).
- **Traffic Telemetry** — what traffic looks like (observability).
- **Traffic Management** — how traffic is routed (canary, blue/green, A/B).

SMI was implemented (in whole or in part) by Linkerd, Open Service Mesh (OSM), Consul Connect, Traefik Mesh, Gloo Mesh, Flagger, Meshery, Argo Rollouts, and Istio (via the `smi-adapter-istio` shim). The final spec release was **v0.6.0**.

## Why It Was Archived

The CNCF [announcement](https://www.cncf.io/blog/2023/10/03/cncf-archives-the-service-mesh-interface-smi-project/) cited consolidation: *"the maintainers have decided to consolidate efforts on a service mesh under the auspices of GAMMA under the Kubernetes SIG Network initiative."* No further SMI work had happened since July 2022. The Kubernetes Gateway API absorbed the mesh use cases that SMI was originally created to address, and the CNCF Service Mesh Working Group concluded SMI had not gained sufficient adoption to justify continued investment as a separate standard.

## The Four SMI APIs

| API | API Version | Kinds | Purpose |
|---|---|---|---|
| **Traffic Access Control** | `access.smi-spec.io/v1alpha3` | `TrafficTarget` | Authorize source identities to talk to destination identities under named rules. |
| **Traffic Specs** | `specs.smi-spec.io/v1alpha4` | `HTTPRouteGroup`, `TCPRoute`, `UDPRoute` | Describe the shape of traffic (HTTP routes, TCP/UDP ports) consumed by other SMI resources. |
| **Traffic Split** | `split.smi-spec.io/v1alpha4` | `TrafficSplit` | Incrementally direct percentages of traffic between backend services for canary and progressive delivery. |
| **Traffic Metrics** | `metrics.smi-spec.io/v1alpha1` | `TrafficMetrics`, `TrafficMetricsList` | Kubernetes APIService exposing per-resource latency (p50/p90/p99) and success/failure counts. |

## Artifacts in This Repo

| Folder | Contents |
|---|---|
| [`apis.yml`](./apis.yml) | API Evangelist index documenting the four SMI APIs and their common properties (specification, archival notice, successor, implementations). |
| [`json-schema/`](./json-schema/) | JSON Schema (Draft 2020-12) definitions for `TrafficTarget`, `HTTPRouteGroup`, `TCPRoute`, `UDPRoute`, `TrafficSplit`, and `TrafficMetrics`. |
| [`json-structure/`](./json-structure/) | Logical structure mapping the four SMI API groups, their kinds, and inter-resource references. |
| [`json-ld/`](./json-ld/) | JSON-LD context aligning SMI vocabulary with `schema.org` and the Kubernetes API namespace; encodes the `supersededBy` link to Gateway API. |
| [`examples/`](./examples/) | Canonical YAML/JSON examples of each SMI resource (taken verbatim from the spec where possible). |
| [`vocabulary/`](./vocabulary/) | Normative vocabulary — concepts, resource kinds, metrics, implementations, related standards, archival notice. |

## Migration to Gateway API GAMMA

Rough mapping for migrators (not exhaustive — consult the [Gateway API mesh docs](https://gateway-api.sigs.k8s.io/mesh/) for authoritative guidance):

| SMI resource | Gateway API equivalent |
|---|---|
| `TrafficTarget` (access control) | `HTTPRoute` + `ReferenceGrant` + mesh-specific policy attachments |
| `HTTPRouteGroup` | `HTTPRoute` (matches expressed inline) |
| `TCPRoute` (SMI) | `TCPRoute` (Gateway API) |
| `UDPRoute` (SMI) | `UDPRoute` (Gateway API) |
| `TrafficSplit` | `HTTPRoute` with weighted `backendRefs` |
| `TrafficMetrics` API | Mesh-implementation-specific (no direct Gateway API successor; see Linkerd Viz, Istio telemetry v2, etc.) |

## References

- Specification (archived): https://github.com/servicemeshinterface/smi-spec
- Project site (archived): https://smi-spec.io
- GitHub org (archived): https://github.com/servicemeshinterface
- CNCF archival announcement: https://www.cncf.io/blog/2023/10/03/cncf-archives-the-service-mesh-interface-smi-project/
- Gateway API GAMMA: https://gateway-api.sigs.k8s.io/mesh/gamma/
- License: Apache-2.0
