# Locust

It is a load-testing tool written in python.

## Installation

```bash
pip3 install locust

# To start the locust
locust
```

Simple script for `locustfile.py`

```python
from locust import HttpUser, task

class HelloWorldUser(HttpUser):

	@task
	def hello_world(self):
		 self.client.get("/ping")
```