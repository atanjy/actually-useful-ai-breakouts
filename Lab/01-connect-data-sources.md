# Connect Data Sources
### Prometheus metrics data source
1. Go to **Connections > Data sources** in the left menu, then click **Add data source**.
2. Add a new Prometheus data source with the following settings:
  - Name: `PromCorrelation`
  - Enter URL: `https://prometheus-prod-56-prod-us-east-2.grafana.net/api/prom`
  - Enable the basic auth toggle
  - Enter Username: `2458181`, Password: `glc_eyJvIjoiNjMxOTY1IiwibiI6Imxva2ktd29ya3Nob3AtYXR0ZW5kZWVzLWxva2ktd29ya3Nob3AtMjAyNWIiLCJrIjoiZEJiNDJqMGFNOTI1TTdPQVYxYzlXQ000IiwibSI6eyJyIjoicHJvZC11cy1lYXN0LTAifX0=`
3. Click **Save and Test**.
### Tempo tracing data source
1. Add a new Tempo data source with the following settings:
  - Name: `TempoCorrelation`
  - Enter URL: `https://tempo-prod-26-prod-us-east-2.grafana.net/tempo`
  - Enable the basic auth toggle
  - Enter Username: `1219078`, Password: `glc_eyJvIjoiNjMxOTY1IiwibiI6Imxva2ktd29ya3Nob3AtYXR0ZW5kZWVzLWxva2ktd29ya3Nob3AtMjAyNWIiLCJrIjoiZEJiNDJqMGFNOTI1TTdPQVYxYzlXQ000IiwibSI6eyJyIjoicHJvZC11cy1lYXN0LTAifX0=`
2. Click **Save and Test**.
### Loki logs data source
1. Add a new Loki data source with the following settings:
  - Name: `LokiCorrelation`
  - Enter URL: `https://logs-prod-036.grafana.net`
  - Enable the basic auth toggle
  - Enter Username: `1224767`, Password: `glc_eyJvIjoiNjMxOTY1IiwibiI6Imxva2ktd29ya3Nob3AtYXR0ZW5kZWVzLWxva2ktd29ya3Nob3AtMjAyNWIiLCJrIjoiZEJiNDJqMGFNOTI1TTdPQVYxYzlXQ000IiwibSI6eyJyIjoicHJvZC11cy1lYXN0LTAifX0=`
  -  Click Add in the Derived fields section, and fill in the following details:
      - Name: TraceId
      - Regex: `.*tempo_trace_id".*?"(.*?)".*`
      - URL: `${__value.raw}`
      - Enable the internal link and select the TempoCorrelation datasource
3. Click **Save and Test**.
### Loki NGINX datasource
1. Add a new Loki data source with the following settings:
  - Name: `LokiNGINX`
  - Enter URL: `https://logs-prod-036.grafana.net`
  - Enable the basic auth toggle
  - Enter Username: `1224767`, Password: `glc_eyJvIjoiNjMxOTY1IiwibiI6Imxva2ktd29ya3Nob3AtYXR0ZW5kZWVzLWxva2ktd29ya3Nob3AtMjAyNWIiLCJrIjoiZEJiNDJqMGFNOTI1TTdPQVYxYzlXQ000IiwibSI6eyJyIjoicHJvZC11cy1lYXN0LTAifX0=`
2. Click ***Save and Test***
