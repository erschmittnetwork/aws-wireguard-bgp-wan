# AWS WireGuard BGP WAN with Automated HA Hub Failover

This repository contains the complete documentation sources and publishing pipeline for an AWS-hosted hub-and-spoke WAN lab implementing encrypted transport, dynamic routing, and automated high-availability failover.

The lab is implemented as a proof-of-concept environment but is intentionally designed to align with enterprise-grade architectural principles, including deterministic routing behavior, encrypted overlay transport, automated failure handling, and operational observability under realistic small-to-medium enterprise constraints.

Infrastructure provisioning artifacts and configuration scripts are referenced in the lab document but are not included in this repository.

---

## Project Overview

This lab documents the design, implementation, and validation of an AWS-based WAN architecture using:

* **WireGuard** for encrypted transport
* **FRRouting (BGP)** for dynamic routing
* **AWS-native automation** for active/standby hub failover
* **GitHub Actions CI/CD** for deterministic documentation build and deployment

The architecture demonstrates that a minimal infrastructure footprint—two cloud-based hubs and multiple remote spokes—can deliver:

* Encrypted WAN connectivity
* Dynamic route propagation via BGP
* Automated hub failover without proprietary SD-WAN platforms

The solution intentionally models realistic enterprise design tradeoffs while remaining operationally constrained and technically focused.

---

## Architecture Summary

The documented topology consists of:

* Two AWS hub nodes (active/standby model)
* Multiple remote spokes
* WireGuard overlay tunnels
* BGP peering between hubs and spokes
* Automated Elastic IP reassignment for failover

### Design Characteristics

* Deterministic BGP routing behavior
* Encrypted overlay transport independent of cloud-native VPN constructs
* Automated hub failover orchestration
* Explicit separation of control plane (BGP) and encrypted data plane (WireGuard)
* Validation testing of failure scenarios

While not feature-complete relative to large-scale production WANs (e.g., VRFs, active/active hubs, SD-WAN controllers), architectural decisions reflect realistic operational constraints and failure models encountered in SME environments.

---

## Repository Structure

```
.
├── .github/workflows
│   └── deploy.yml
├── assets
│   ├── doc.css
│   └── header.html
├── diagrams
│   └── logical-architecture.mmd
├── docs
│   ├── aws-wireguard-bgp-wan.tex
│   └── index.html
└── index.html
```

### Directory Overview

**.github/workflows/**
GitHub Actions pipeline responsible for building and deploying site artifacts.

**assets/**
Custom CSS styling and reusable header components injected into generated HTML output.

**diagrams/**
Mermaid source diagrams describing the logical architecture.

**docs/**
Primary LaTeX source for the lab documentation.

**index.html**
Portfolio landing page for the published documentation.

---

## Documentation Build and CI/CD Pipeline

This repository uses GitHub Actions to produce deterministic HTML and PDF artifacts on every push to `main`.

### Build Steps

1. Checkout repository
2. Install Pandoc and LaTeX dependencies
3. Render Mermaid diagrams to PNG via Dockerized `mermaid-cli`
4. Copy static assets
5. Build HTML documentation from LaTeX source using Pandoc
6. Build PDF documentation using `pdflatex`
7. Assume AWS IAM role via OIDC
8. Sync generated artifacts to S3
9. Invalidate CloudFront cache

### Artifact Generation

The pipeline produces:

* `dist/docs/aws-wireguard-bgp-wan.html`
* `dist/docs/aws-wireguard-bgp-wan.pdf`
* Rendered architecture diagrams
* Static homepage assets

Deployment target:

* **Amazon S3** for static hosting
* **CloudFront** for global distribution and cache control

Authentication is performed using GitHub OIDC and an AWS IAM role, eliminating the need for long-lived static credentials.

---

## Published Documentation

The generated documentation is publicly available:

HTML:
[https://erschmitt.com/docs/aws-wireguard-bgp-wan.html](https://erschmitt.com/docs/aws-wireguard-bgp-wan.html)

PDF:
[https://erschmitt.com/docs/aws-wireguard-bgp-wan.pdf](https://erschmitt.com/docs/aws-wireguard-bgp-wan.pdf)

---

## Scope and Intent

This repository is intentionally documentation-focused.

It serves as:

* A reproducible technical artifact
* A formal architectural reference
* A portfolio demonstration of network engineering design discipline
* An example of CI/CD-driven documentation publishing

Infrastructure provisioning code, instance configurations, and automation scripts are intentionally excluded.

---

## Versioning

Current documented version: 1.3

Version history and metadata are maintained within the LaTeX source and rendered output.
