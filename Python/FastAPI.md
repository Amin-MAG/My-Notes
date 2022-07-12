# Fast API

- Fast to code
- Easy
- Robust

## Get started

```python
from typing import Union
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def index():
	return {"ping": "pong"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: Union[str, None] = None):
	return {"item_id": item_id, "q": q}
```

To run the code

```bash
uvicorn main:app --reload
```

## Docs

Automatically a swagger page will be created in `/docs`. It also create an alternative document page in `/redoc`.