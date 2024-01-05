Sending telemetry data directly from applications instrumented with OpenTelemetry SDKs to Datadog (via the Datadog agent):


1) Using the OpenTelemetry Collector with Datadog Exporter:
* If already using the OpenTelemetry Collector to collect observability data, you can forward this data to Datadog. You need to add datadog and your Datadog API key to the exporters section of your OpenTelemetry Collector configuration file. For example:

exporters:
  datadog:
    api: 
      key: "<API key>"

* Then, enable the exporter in the service section of the configuration file, specifying the components of the OpenTelemetry Collector to be used and defining the pipelines for the types of data to be collected and sent. For instance, to collect infrastructure metrics, you would list hostmetrics as a receiver and include a pipeline with metrics as the data type and datadog/api as the exporter.

2) OTLP Ingestion by the Datadog Agent:
* Since versions 6.32.0 and 7.32.0 of the Datadog Agent, it can ingest OTLP (Open Telemetry Protocol) traces and metrics through gRPC or HTTP. For OTLP logs, it's supported since versions 6.48.0 and 7.48.0.
* To enable OTLP ingestion, you need to update the datadog.yaml file configuration or set the corresponding environment variables. For example, for gRPC on the default port 4317, the configuration would be:

otlp_config:
  receiver:
    protocols:
      grpc:
        endpoint: localhost:4317

* For HTTP on the default port 4318, adjust the configuration similarly. In a containerized environment, use 0.0.0.0 instead of localhost.
* Additionally, for OTLP logs ingestion, you must explicitly enable log collection and set otlp_config.logs.enabled to true.


Refs: 
https://www.datadoghq.com/blog/ingest-opentelemetry-traces-metrics-with-datadog-exporter/
https://lumigo.io/opentelemetry/using-opentelemetry-with-datadog-a-practical-guide/
https://docs.datadoghq.com/opentelemetry/otlp_ingest_in_the_agent/
