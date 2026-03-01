# interpretation_scheme-smoapp

This smoApp implements a FastMCP-based service that exposes tools to manage Network Slice Booking sessions, which acts as a translator.
It provides arguments for the LLM to translate natural-language intent into a structured result.
ORION adopts the CAMARA NetworkSliceBooking resource model (e.g., slice profile, QoS requirements, area of service, duration) as a
canonical skeleton for slice intents, enabling vendor-agnostic translation and simplifying validation and auditing.

## Features

- Exposes MCP tools over `sse` transport
- Creates, retrieves, and deletes slice booking sessions
- Parses wrapped JSON payloads returned by upstream API responses
- Supports local execution, Docker image and Helm deployment

## Repository Structure

```text
.
├── Dockerfile
├── requirements.txt
├── config/
│   └── config.yaml
├── helm/
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
├── src/
│   ├── server.py
│   └── utils/
│       ├── logger.py
│       └── models.py
```

## Requirements

- Python 3.11+
- Docker
- Kubernetes cluster access
- Helm 3+
- Access to a running [NetworkSliceBooking](https://github.com/camaraproject/NetworkSliceBooking.git) API endpoint

## Docker

Build from the parent repository root (the directory that contains the `mcp_server/` folder), as expected by the Dockerfile `COPY` paths:

```bash
docker build -f mcp_server/Dockerfile -t mcp-server:local .
```

Run:

```bash
docker run --rm -p 8000:8000 mcp-server:local
```

## Helm Deployment

Install with default values:

```bash
helm upgrade --install mcp-server ./helm
```

Customize through `helm/values.yaml` (image, service type/port, config map values, probes, and resources).

## Contributing and Security

- Contribution guidelines: [CONTRIBUTING.md](CONTRIBUTING.md)

## License

This project is licensed under the Apache License 2.0. See [LICENSE](LICENSE).
