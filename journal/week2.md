# Week 2 — Distributed Tracing

The first thing I did for week 2 was to update my ../reference/gitpod_vars file by adding the Honeycomb variables

```bash
export HONEYCOMB_API_KEY=""
export HONEYCOMB_SERVICE_NAME="Cruddur"
```

As suggested by Jessica (and demonstrated by Andrew) I also updated the docker-compose.yml to include the OTEL environment variables:

```yml
OTEL_SERVICE_NAME: 'backend-flask'
OTEL_EXPORTER_OTLP_ENDPOINT: "https://api.honeycomb.io"
OTEL_EXPORTER_OTLP_HEADERS: "x-honeycomb-team=${HONEYCOMB_API_KEY}"
```

Later I added the OpenTelemetry python packages to requirements.txt as well as added the Initialize code.

The compose up started the containers but it was not sending data to Honeycomb (even after hitting an endpoint more than 5 times). I realized the Honeycomb API key variable is not passed to the container so I needed to save it to Gitpod using:

```bash
gp env HONEYCOMB_API_KEY="MySuperSecretKey"
```

I then closed my whole workspace and re-launched it. After composing up one more time, it started to send data right away!

Added one new span and two fields on that new span. I called them ucloud:

<img src="assets/week2/2023-03-02-Trace.png">

Just for documentation and to remember later, https://honeycomb-whoami.glitch.me can be used to determine the environment for a specific key.

https://docs.honeycomb.io/getting-data-in/opentelemetry/python/



**Challenge homework**

I added another instrumentation to the app, it is the remote ip_address. It is not working as expected so I will investigate a little bit further on it.

In order for it to work, I added ```from flask import request``` at the beginning of the home_activities.py file and at the bottom the following:

```python
      ip_address = request.remote_addr
      span.set_attribute("ucloud.ip", str(ip_address))
```

This results in the following in Honeycomb:

<img src="assets/week2/2023-03-02-Trace02.png">

