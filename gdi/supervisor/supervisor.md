(supervisor)=

# Supervisor
<meta name="description" content="Use this Splunk Observability Cloud integration for the Supervisor monitor. See benefits, install, configuration, and metrics">

## Description

The {ref}`Splunk Distribution of OpenTelemetry Collector <otel-intro>` provides this integration as the `supervisor` monitor type for the Smart Agent Receiver.

Use this integration to retrieve the state of processes running by the Supervisor.

This integration is available for Kubernetes, Windows, and Linux.

### Benefits

```{include} /_includes/benefits.md
```

## Installation

```{include} /_includes/collector-installation.md
```

## Configuration

```{include} /_includes/configuration.md
```

```{note}
Provide a Supervisor monitor entry in your Collector or Smart Agent (deprecated) configuration. Use the appropriate form for your agent type.
```

### Splunk Distribution of OpenTelemetry Collector

To activate this monitor in the Splunk Distribution of OpenTelemetry Collector, add the following to your agent configuration:

```yaml 
receivers:
  smartagent/supervisor:
    type: supervisor
    ...  # Additional config
```

To complete the monitor activation, you must also include the `smartagent/supervisor` receiver item in a `metrics` pipeline. To do this, add the receiver item to the `service.pipelines.metrics.receivers` section of your configuration file. For example:

```yaml
service:
  pipelines:
    metrics:
      receivers: [smartagent/supervisor]
```

### Smart Agent

To activate this monitor in the Smart Agent, add the following to your agent configuration:

```yaml
monitors:  # All monitor config goes under this key
  - type: supervisor
    ...  # Additional config
```

See {ref}`smart-agent` for an autogenerated example of a YAML configuration file, with default values where applicable.

### Configuration settings 

The following table shows the configuration options for this monitor:

| Option | Required | Type | Description |
| --- | --- | --- | --- |
| `host` | No | `string` | The host/IP address of the Supervisor XML-RPC API. Used to build the `url` option if a URL is not provided. |
| `port` | No | `integer` | The port of the Supervisor XML-RPC API. Used to build the `url` option if a URL not provided. For example, `localhost`. The default value is `9001`. |
| `useHTTPS` | No | `bool` | If true, the monitor connects to the Supervisor using the HTTPS protocol instead of the HTTP protocol. The default value is `false`. |
| `path` | No | `string` | The URL path to use for the scrape URL for Supervisor. The default value is `/RPC2`. |
| `url` | No | `string` | URL on which to scrape Supervisor XML-RPC API. If this is not provided, it's derived from the `host`, `port`, `useHTTPS`, and `path` options. For example, `http://localhost:9001/RPC2`. |

## Metrics

The following metrics are available for this integration:

<div class="metrics-yaml" url="https://raw.githubusercontent.com/signalfx/signalfx-agent/main/pkg/monitors/supervisor/metadata.yaml"></div>

### Notes

```{include} /_includes/metric-defs.md
```

## Get help

```{include} /_includes/troubleshooting.md
```