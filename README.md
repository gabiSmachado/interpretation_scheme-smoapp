# interpretation_scheme-smoapp

This smoApp implements a FastMCP-based server that exposes tools to manage Network Slice Booking sessions, which acts as a translator.
It provides arguments for the LLM to translate natural-language intent into a structured result.
ORION adopts the CAMARA NetworkSliceBooking resource model (e.g., slice profile, QoS requirements, area of service, duration) as a
canonical skeleton for slice intents, enabling vendor-agnostic translation and simplifying validation and auditing.

## Overview

- Exposes MCP tools over `sse` transport
- Creates, retrieves, and deletes slice booking sessions
- Parses wrapped JSON payloads returned by upstream API responses
- Supports local execution, Docker image and Helm deployment

## Requirements

- Python 3.11+
- Docker
- Kubernetes + Helm 3+
- Access to a running [NetworkSliceBooking](https://github.com/camaraproject/NetworkSliceBooking.git) API endpoint

## Repository Structure

```text
.
├── build_and_push_image.sh
├── Dockerfile
├── requirements.txt
├── helm/
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
├── src/
│   ├── server.py
│   ├── config/
│   │   └── config.yaml
│   └── utils/
│       ├── logger.py
│       └── models.py
```

## Build

To build and push the docker image change the REGISTRY to your address in `build_and_push_image.sh` and run:

```bash
bash build_and_push_image.sh
```

## Deploy

To deploy the smoApp use:

```bash
helm upgrade --install mcp-server ./helm -n smo
```

Customize through `helm/values.yaml` (image, service type/port, config map values, probes, and resources).

## Contributing

Contribution guidelines: [CONTRIBUTING.md](CONTRIBUTING.md)

## License

Licensed under Apache 2.0. See [LICENSE](LICENSE).
