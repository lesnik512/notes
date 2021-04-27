# Monitoring multi-process Python apps with Prometheus

On my work we faced a problem with collecting metrics from python apps running in several processes (using uvicorn). When you have these multi-process applications, you get into a situation where any of the multiple workers can respond to prometheus's scraping request. Each worker then responds with a value for a metric that it knows of.

Official Prometheus Python client has [multiprocess mode](https://github.com/prometheus/client_python/blob/master/README.md#multiprocess-mode-gunicorn). In FastAPI we are using package `prometheus-fastapi-instrumentator` build on top of official client. Here is the part of the source code of `instrumentator`:

```python
if "prometheus_multiproc_dir" in os.environ:
    pmd = os.environ["prometheus_multiproc_dir"]
    if os.path.isdir(pmd):
        registry = CollectorRegistry()
        multiprocess.MultiProcessCollector(registry)
    else:
        raise ValueError(
            f"Env var prometheus_multiproc_dir='{pmd}' not a directory."
        )
else:
    registry = REGISTRY
```

From the link above:

> The `prometheus_multiproc_dir` environment variable must be set to a directory that the client library can use for metrics. This directory must be wiped between Gunicorn runs (before startup is recommended).

So we need to set `prometheus_multiproc_dir` environment variable.