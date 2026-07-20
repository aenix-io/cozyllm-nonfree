# cozyllm-nonfree

Non-free companion catalog to [cozyllm](https://github.com/aenix-io/cozyllm) — external AI applications for [Cozystack](https://cozystack.io) whose upstream projects are distributed under licenses that are not OSI-approved open source. The apps integrate into the Cozystack dashboard exactly like the free catalog: deploy with a click, configure through a generated form, scale and tear down independently.

## What's inside

| App | What it is | Upstream license |
| --- | --- | --- |
| **[n8n](docs/apps/n8n.md)** | General workflow automation with native AI nodes | [Sustainable Use License](https://github.com/n8n-io/n8n/blob/master/LICENSE.md) |
| **[open-webui](docs/apps/open-webui.md)** | Chat interface over any OpenAI-compatible API | [Open WebUI License](https://github.com/open-webui/open-webui/blob/main/LICENSE) |

## Why these apps are separate

The free cozyllm catalog only carries apps whose upstream payloads are open source. These two are not, so they live here — installing this catalog is an explicit opt-in to their vendors' terms:

- **n8n** is distributed under the Sustainable Use License — "fair-code", not open source. Internal business use is broadly permitted, but **offering n8n as a managed service to third parties requires a commercial entitlement from n8n** (an Embed or Enterprise agreement). If you run a platform where tenants other than your own organization deploy n8n, you need such an agreement.
- **Open WebUI** is distributed under the Open WebUI License, a BSD-3-derived license with a branding-preservation clause: deployments serving more than 50 users must keep the Open WebUI branding intact unless you hold an enterprise license from the vendor. This chart does not alter or remove any branding.

You — the operator installing this catalog — are responsible for ensuring your deployment complies with these terms. Nothing in this repository grants you any rights to the upstream products.

## BYOL: bring your own license

The n8n chart exposes a `vendorLicense` knob for operators who hold an n8n entitlement. Create a Secret in the app namespace whose key `N8N_LICENSE_ACTIVATION_KEY` contains your activation key, then set `spec.vendorLicense.secretRef` to the Secret's name — the chart injects the key into the n8n instance. See [docs/apps/n8n.md](docs/apps/n8n.md) for details.

## Install

Apply the bootstrap manifest to a Cozystack 1.4+ cluster (typically alongside the free catalog):

```bash
kubectl apply -f https://raw.githubusercontent.com/aenix-io/cozyllm-nonfree/main/init.yaml
```

This registers a FluxCD `GitRepository` (`cozyllm-nonfree` in `cozy-public`) and a platform `HelmRelease` (`cozyllm-nonfree` in `cozy-system`). Flux pulls every minute; within ~2 minutes the `n8n` and `open-webui` `ApplicationDefinition` resources appear in the dashboard.

End-to-end scenarios wiring these apps to the free catalog's model serving and gateway are in [docs/examples.md](docs/examples.md).

## Licensing of this repository

The code in this repository — Helm charts, templates, platform manifests, documentation — is licensed under the [Apache License 2.0](LICENSE). Nothing non-Apache is stored or redistributed here: the application payloads (container images, upstream Helm charts) are fetched from the vendors' own sources at deploy time and retain their upstream licenses. See [NOTICE](NOTICE).

This repository is maintained by the cozyllm authors and is **not part of the CNCF Cozystack project**.
