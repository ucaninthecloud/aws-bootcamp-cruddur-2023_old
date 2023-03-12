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


**AWS X-Ray***

1. I added the below line to the requirements.txt, so that it can be loaded when starting the container

```bash
aws-xray-sdk
```

2. As I wanted to continue sending data to Honeycomb, I just added two lines to the environment section for the backend-flask service in the docker-compose.yml:

```yml
      AWS_XRAY_URL: "*4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}*"
      AWS_XRAY_DAEMON_ADDRESS: "xray-daemon:2000"
```

3. A new service container was also added to the same docker-compose.yml to start the xray-daemon.

```yml
  xray-daemon:
    image: "amazon/aws-xray-daemon"
    environment:
      AWS_ACCESS_KEY_ID: "${AWS_ACCESS_KEY_ID}"
      AWS_SECRET_ACCESS_KEY: "${AWS_SECRET_ACCESS_KEY}"
      AWS_REGION: "us-east-1"
    command:
      - "xray -o -b xray-daemon:2000"
    ports:
      - 2000:2000/udp
```

4. At this point I realized I needed to use the AWS cli to create a new XRAY group and set my running session environment variables with the data I had in my [gitpod_vars](../_reference/gitpod_vars) file.

5. Once they were set, the Cruddur group was created:

```bash
aws xray create-group \
--group-name "Cruddur" \
--filter-expression "service(\"backend-flask\")"
```

6. As mentioned via the instructions, I created a sampling rule and saved it as [xray.json](../aws/json/xray.json)

```json
{
  "SamplingRule": {
      "RuleName": "Cruddur",
      "ResourceARN": "*",
      "Priority": 9000,
      "FixedRate": 0.1,
      "ReservoirSize": 5,
      "ServiceName": "backend-flask",
      "ServiceType": "*",
      "Host": "*",
      "HTTPMethod": "*",
      "URLPath": "*",
      "Version": 1
  }
}
```

7. After this, I "saved" or pushed this sampling rule to AWS via the following command:

```bash
aws xray create-sampling-rule --cli-input-json file://aws/json/xray.json
```

8. The last thing was to set X-Ray in the app.py. First I added the modules:

```python
# X-Ray 
from aws_xray_sdk.core import xray_recorder
from aws_xray_sdk.ext.flask.middleware import XRayMiddleware
```

9. And configured the recorder:

```python
# XRay recorder 
xray_url = os.getenv("AWS_XRAY_URL")
xray_recorder.configure(service='backend-flask', dynamic_naming=xray_url)
```



**Challenge homework**

I added another instrumentation to the app, it is the remote ip_address. It is not working as expected so I will investigate a little bit further on it.

In order for it to work, I added ```from flask import request``` at the beginning of the home_activities.py file and at the bottom the following:

```python
      ip_address = request.remote_addr
      span.set_attribute("ucloud.ip", str(ip_address))
```

This results in the following in Honeycomb:

<img src="assets/week2/2023-03-02-Trace02.png">

