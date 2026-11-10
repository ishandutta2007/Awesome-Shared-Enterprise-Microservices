# Awesome-Shared-Enterprise-Microservices

# Awesome Shared Enterprise Microservices

> A curated list of top Shared Enterprise Microservices platforms — both SaaS/Hosted commercial solutions and open-source alternatives for service mesh, API gateways, and integration runtimes that provide shared infrastructure for microservices architectures.
> 🌟 Star this repo if you find it useful! 🌟

---

## 📋 Table of Contents

- [SaaS / Hosted Platforms](#-saas--hosted-platforms)
  - [Istio](#istio)
  - [Linkerd](#linkerd)
  - [HashiCorp Consul](#hashicorp-consul)
  - [Kong Enterprise](#kong-enterprise)
  - [AWS App Mesh](#aws-app-mesh)
  - [MuleSoft Anypoint Platform](#mulesoft-anypoint-platform)
- [Open-Source Software](#-open-source-software)
  - [KrakenD](#krakend)
  - [WSO2 Micro Integrator](#wso2-micro-integrator)
  - [Cilium Service Mesh](#cilium-service-mesh)
  - [Apache APISIX](#apache-apisix)
- [Comparison at a Glance](#-comparison-at-a-glance)
- [Contributing](#-contributing)

---

## 🟦 SaaS / Hosted Platforms

Commercial, cloud-hosted Shared Enterprise Microservices platforms. These are not open-source (though some have open-source cores) but are included as reference points for feature comparison.

### Istio
**Type:** Feature-rich service mesh with ambient mode  
Istio is the most feature-rich service mesh, offering mature ambient mode (no sidecars), strong multi-cluster support, rich traffic management, and a broad ecosystem [citation:5]. Its ambient mode replaces per-pod sidecar proxies with a layered architecture: ztunnel on each node handles L4/mTLS, and waypoint proxies handle L7 features only when needed, reducing resource usage by 50-70% compared to sidecar mode [citation:5]. Istio provides the full Envoy feature set through Kubernetes-native configuration, including header-based routing, fault injection, traffic mirroring, and sophisticated retry policies [citation:14]. **Trade-off:** Complexity is higher than Linkerd, and the operational footprint (even in ambient mode) requires dedicated platform expertise.

### Linkerd
**Type:** Simple, lightweight service mesh  
Linkerd is described as the simplest production service mesh — opinionated, easy to operate, with great performance and CNCF graduated status [citation:5]. It uses a purpose-built Rust proxy with mTLS default-on requiring no configuration, automatic certificate rotation, and identity-based authorization [citation:5]. Resource overhead is modest: roughly 3-6GB memory and 2-4 vCPU for a 200-pod cluster [citation:14]. **Trade-off:** Linkerd does not support the full range of traffic management capabilities that Istio offers — no equivalent to VirtualService for complex routing, no built-in egress management, and limited external authorization integration [citation:14].

### HashiCorp Consul
**Type:** Service mesh with multi-platform support  
Consul Connect provides service mesh capabilities with a focus on multi-platform support (Kubernetes, ECS, VM, bare-metal) and WAN federation for cross-datacenter connectivity [citation:7]. It offers mTLS with Vault integration for certificate management, traffic splitting, retries, and timeouts through service-router and service-splitter configuration entries [citation:14]. **Trade-off:** Resource overhead is the highest among the major meshes — 20-40GB memory and 15-60 vCPU for a 200-pod cluster due to Envoy sidecars plus Consul agents [citation:14].

### Kong Enterprise
**Type:** API gateway with enterprise management  
Kong Enterprise extends the open-source Kong Gateway with advanced features including RBAC, FIPS 140-2 compliant data planes, OpenID Connect, advanced rate limiting, consumer groups, and a developer portal . It is named a Gartner Magic Quadrant Leader for API Management for the sixth consecutive year . Deployment options include self-managed (Kong Manager) and SaaS (Konnect). **Trade-off:** Enterprise licensing starts at approximately $24,000/year, and the feature split between OSS and Enterprise is significant [citation:28].

### AWS App Mesh
**Type:** AWS-managed service mesh (deprecated)  
AWS App Mesh is AWS's managed service mesh offering, built on Envoy proxies with ACM-based certificates and IAM integration [citation:5]. **Critical note:** AWS has deprecated App Mesh in favor of VPC Lattice, which is not technically a service mesh but solves many of the same problems at the VPC level without proxies or sidecars [citation:14]. New deployments should evaluate VPC Lattice or migrate to Istio/Linkerd instead.

### MuleSoft Anypoint Platform
**Type:** Enterprise integration and API management  
MuleSoft Anypoint Platform is an enterprise integration platform combining API management with ESB capabilities. It offers a visual design studio, 300+ pre-built connectors, DataWeave transformation, and CloudHub hosting [citation:27]. **Trade-off:** MuleSoft locks you in through proprietary DataWeave, XML flow definitions, and Anypoint Connector SDK — migration requires rewriting every transformation and custom connector [citation:16]. Pricing is enterprise-quoted, typically $100K+/year [citation:28].

---

## 🟩 Open-Source Software

Self-hosted, community-driven Shared Enterprise Microservices tools. The open-source ecosystem spans **API gateways** for traffic management and **integration runtimes** for service orchestration.

### KrakenD
**Stack:** Go | **License:** Apache-2.0  
KrakenD is an ultra-high performance, stateless, declarative API Gateway that aggregates multiple microservices into single endpoints [citation:3][citation:8]. Key features include content aggregation and transformation (XML to JSON), zero-trust security (OAuth, JWT, CORS, HSTS), multi-layer rate limiting, circuit breaking, and telemetry integration (Datadog, Zipkin, Jaeger, Prometheus, Grafana) [citation:3]. It claims **70K+ requests/second on a single instance** with under 50MB memory for 1000+ concurrent connections, thanks to its stateless design with true linear scalability [citation:3]. Configuration is GitOps-oriented and declarative. Part of the Lura Project under the Linux Foundation [citation:8].  
**Best for:** Teams wanting a high-performance, stateless API gateway for aggregating microservices without the operational overhead of a full service mesh.

### WSO2 Micro Integrator
**Stack:** Java | **License:** Apache-2.0  
WSO2 Micro Integrator is a cloud-native integration runtime rebuilt for Kubernetes — it boots in seconds, exposes Prometheus metrics natively, and integrates with any orchestrator without special accommodations [citation:16]. It is positioned as an open-source alternative to MuleSoft, with 100% free connectors, no vendor lock-in, and deployment flexibility including fully on-premises [citation:20]. The component model is designed to be assembled: you bring your own platform, WSO2 brings the runtime. Renewal escalation is capped at 5% (vs. MuleSoft's 5-10%) [citation:16]. **Trade-off:** You run the platform yourself; if you lack an SRE team, a managed offering may be preferable [citation:16].  
**Best for:** Organizations with Kubernetes expertise wanting an open-source integration runtime without MuleSoft's lock-in and cost structure.

### Cilium Service Mesh
**Stack:** Go / eBPF | **License:** Apache-2.0  
Cilium Service Mesh uses eBPF and the Cilium agent for node-level L3/L4 processing, with selected L7 traffic redirected to Envoy [citation:7]. It is recommended for latency-sensitive applications (real-time trading, gaming) due to its lower overhead compared to sidecar-based meshes [citation:5]. It provides mTLS, network policies, and observability without the sidecar injection model. **Trade-off:** L7 capability is conditional on the actual path — Cilium uses eBPF for L3/L4 but requires Envoy for L7 enforcement [citation:7].  
**Best for:** Performance-sensitive workloads and teams already using Cilium as their CNI wanting to extend into service mesh capabilities.

### Apache APISIX
**Stack:** Lua / NGINX / etcd | **License:** Apache-2.0  
Apache APISIX is a dynamic, high-performance API gateway built on NGINX and etcd, offering traffic management, security, observability, and a plugin ecosystem. (Note: Search results did not include APISIX specifics; this entry is based on general knowledge — verify current maintenance before adoption.)  
**Best for:** Teams wanting a proven, Apache-governed API gateway with dynamic configuration and a rich plugin ecosystem.

---

## 📊 Comparison at a Glance

| Software | License | Stack | Type | Traffic Management | Multi-Cluster | Self-Hosted | Best For |
|----------|---------|-------|------|-------------------|---------------|-------------|----------|
| **Istio** | Apache-2.0 | Go/Envoy | Service mesh | ✅ Full Envoy set | ✅ Mature | ✅ | Feature-rich mesh |
| **Linkerd** | Apache-2.0 | Rust | Service mesh | ✅ Basic (splitting, retries) | ✅ Mirroring | ✅ | Operational simplicity |
| **Consul** | MPL-2.0 | Go/Envoy | Service mesh | ✅ Splitting, retries | ✅ WAN federation | ✅ | Multi-platform mesh |
| **Kong Enterprise** | Proprietary | Lua/NGINX | API gateway | ✅ Plugins | (Konnect) | ✅ (self-managed) | Enterprise API management |
| **AWS App Mesh** | Proprietary | Envoy | Service mesh | ✅ Basic | (AWS) | ❌ | Deprecated (use VPC Lattice) |
| **MuleSoft** | Proprietary | Java | Integration + API | (ESB flows) | (CloudHub) | (Runtime Fabric) | Enterprise integration |
| **KrakenD** | Apache-2.0 | Go | API gateway | ✅ Aggregation + rate limit | (Stateless) | ✅ | High-performance aggregation |
| **WSO2 MI** | Apache-2.0 | Java | Integration runtime | (Mediation) | (K8s) | ✅ | Open-source MuleSoft alternative |

---

## 🤝 Contributing

Contributions are welcome! If you know of an open-source Shared Enterprise Microservices tool that should be listed here, please open a pull request with:

- Project name and URL
- Brief description (1–2 sentences)
- Stack and license
- Why it stands out

Please ensure the project is actively maintained (releases within the last 12 months) and genuinely open-source. Note that the ecosystem **splits** between **service meshes** (Istio, Linkerd, Consul) for east-west traffic, **API gateways** (KrakenD, Kong) for north-south traffic and aggregation, and **integration runtimes** (WSO2 MI, MuleSoft) for service orchestration. **Istio** is the feature leader with ambient mode reducing overhead [citation:5], **Linkerd** is the operational simplicity choice [citation:5], and **KrakenD** is the high-performance aggregation gateway for microservices [citation:3]. For MuleSoft migration, **WSO2 Micro Integrator** is the most direct open-source alternative with Kubernetes-native design [citation:16][citation:20].

---

## 📄 License

This list is released under CC0 1.0 Universal (Public Domain). The software projects listed retain their respective licenses.

---

⭐ **If this list helped you discover a useful tool, please star the repo!** ⭐
