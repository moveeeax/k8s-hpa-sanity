# k8s-hpa-sanity

> An autoscaler that can never scale is worse than no autoscaler.

**Status:** 🚧 In development

## Overview

Flag HorizontalPodAutoscalers that can never act: missing metrics, minReplicas equal to maxReplicas, or a target the metrics server does not publish.

## Features

- Flags HPAs where `minReplicas` equals `maxReplicas`, so the workload is pinned while looking autoscaled
- Checks every metric target against what metrics-server and the custom/external metrics APIs actually publish
- Reports HPAs whose `scaleTargetRef` points at a Deployment or StatefulSet that no longer exists
- Catches CPU and memory utilisation targets on containers with no matching resource requests, where the ratio is undefined
- Surfaces the `AbleToScale` and `ScalingActive` condition messages in plain text instead of raw status YAML
- JSON output and a `--fail-on` level, so it runs as a cluster policy check in CI

## Stack

Go + client-go (autoscaling/v2), the metrics client-go and custom-metrics APIs, and cobra.

## Usage

```bash
k8s-hpa-sanity --all-namespaces --output json --fail-on error
```

## License

MIT
