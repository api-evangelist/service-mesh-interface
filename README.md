# Service Mesh Interface (SMI)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
