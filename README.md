# Google Microservices Demo Sample

## Prerequisites

- OpenChoreo 0.9 single cluster instance with observability plane enabled
- LLM API key (OpenAI preferred)

## Install RCA Agent to Observability Plane

Set the required environment variables:

```bash
export RCA_MODEL_NAME=<your-model-name>
export RCA_LLM_API_KEY=<your-api-key>
```

> [!TIP]
> We recommend using OpenAI gpt-5

```bash
helm upgrade --install openchoreo-observability-plane \
  oci://ghcr.io/openchoreo/helm-charts/openchoreo-observability-plane \
  --version 0.9.0 \
  --namespace openchoreo-observability-plane \
  --reuse-values \
  --set rca.enabled=true \
  --set rca.oauth.tokenUrl=http://thunder-service.openchoreo-control-plane.svc.cluster.local:8090/oauth2/token \
  --set rca.llm.modelName=$RCA_MODEL_NAME \
  --set rca.llm.apiKey=$RCA_LLM_API_KEY \
  --set observer.image.repository=rashadxyz/openchoreo-observer \
  --set observer.image.tag=demo \
  --set rca.image.repository=rashadxyz/ai-rca-agent \
  --set rca.image.tag=demo
```

## Deploy Components

```bash
kubectl apply -f gcp-microservice-demo-project.yaml
kubectl apply -f components/
```

Access the frontend application in your browser:

```
http://frontend-development.openchoreoapis.localhost:19080
```