# Week 2 — Distributed Tracing

## Required Homework/Tasks
All the tasks completed under this section is done using Gitpod workspace.

1)Bringing Up Honeycomb

environement setting up in honeycomb.
Login up to honeycomb [link](https://ui.honeycomb.io/).

Once logged in to honeycomb main page, goto ENVIRONMENT on the left panel of your page as below
![honeycomb](assets/honeycmb1.PNG).

Click Manage Environments to create new environment with description and choose any color for representing .
After creation of new environment file.
![new environment created](assets/new_env_created_honeycmb.PNG).

Now,  the API keys under View API keys. The API keys you use determines the environment that data will land in.

Let's export our API key to gitpod using the commands below:

```bash
# to export it to our gitpod terminal 
export HONEYCOMB_API_KEY="..."

# confirm export 
env | grep HONEY

# to have it permanently saved unto gitpod env variables
# when it starts next time, we don't have to export it again  
gp env HONEYCOMB_API_KEY="..."
```

2. Instrument backend flask to use OpenTelemetry (OTEL) with Honeycomb.io as the provider

Honeycomb is a software debugging tool that can help us to solve problems faster within your distributed services. 
It uses OTEL libraries which is simply the OpenTelemetry libraries. For further [refer](https://www.honeycomb.io/)

Let's set honeycomb env variable for our backend in the docker-compose.yml file. using below code,

```BASH
# honeycomb env variables 
OTEL_SERVICE_NAME: 'backend-flask'
OTEL_EXPORTER_OTLP_ENDPOINT: "https://api.honeycomb.io"
OTEL_EXPORTER_OTLP_HEADERS: "x-honeycomb-team=${HONEYCOMB_API_KEY}"
```

Now, let's install the OTEL libraries into our application. 
Add the following lines to the requirements.txt file located in the backend-flask/ directory

```BASH
opentelemetry-api 
opentelemetry-sdk 
opentelemetry-exporter-otlp-proto-http 
opentelemetry-instrumentation-flask 
opentelemetry-instrumentation-requests # this should instrument outgoing HTTP calls
```

To install the dependencies you listed in the requirements.txt file In the backend-flask directory, run the following command:

```BASH
pip install -r requirements.txt
```

create and initialize honeycomb by adding the following lines of code to app.py file

```PYTHON
# Honeycomb
from opentelemetry import trace
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.instrumentation.requests import RequestsInstrumentor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor

# Initialize tracing and an exporter that can send data to Honeycomb
provider = TracerProvider()
processor = BatchSpanProcessor(OTLPSpanExporter())
provider.add_span_processor(processor)
trace.set_tracer_provider(provider)
tracer = trace.get_tracer(__name__)

# Initialize automatic instrumentation with Flask
app = Flask(__name__) # if this link already exists, DON'T call it again
FlaskInstrumentor().instrument_app(app)
RequestsInstrumentor().instrument()
```

Now time to build the docker using ``` docker compose up ```

* check which API key is set in environment . Do that by checking your env variables env | grep HONEYCOMB_API_KEY
* Create a span, with configured tracer  in the file ```backend-flask/services/home_activities.py``` 
```PHYTHON 
# add under def run():
with tracer.start_as_current_span("home-activities-mock-data"):
# make sure every other line beneath is properly indented under the code you pasted 
```

* Adding attributes to the span in the homea_activities.py file

```PYTHON
# in the def run(): section
# add attribute -> app.now 
span = trace.get_current_span()
span.set_attribute("app.now", now.isoformat())
# at the bottom -> app.result_length
span.set_attribute("app.result_length", len(results))
```
![create a span](assets/honeycmb1.PNG)

![query to collect data](assets/honeyex1.PNG)

![Latency Check](assets/honeyex2.PNG)


