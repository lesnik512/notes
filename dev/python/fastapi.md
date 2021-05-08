## Storing state on the app instance (Starlette)

```python
app.state.ADMIN_EMAIL = 'admin@example.org'

# available in request.app.state
```

## Cookie Parameters

```python
from typing import Optional
from fastapi import Cookie, FastAPI

app = FastAPI()

@app.get("/items/")
async def read_items(ads_id: Optional[str] = Cookie(None)):
    return {"ads_id": ads_id}
```

## Header Parameters

```python
from typing import List, Optional
from fastapi import FastAPI, Header

app = FastAPI()

@app.get("/items/")
async def read_items(x_token: Optional[List[str]] = Header(None)):
    return {"X-Token values": x_token}

```

## response_model_include and response_model_exclude

```python
@app.get("/items/{item_id}/public", response_model=Item, response_model_exclude={"tax"})
async def read_item_public_data(item_id: str):
    return items[item_id]
```

## Union response model

```python
@app.get("/items/{item_id}", response_model=Union[PlaneItem, CarItem])
async def read_item(item_id: str):
    return items[item_id]
```

# Notes

- register an exception handler for Starlette's HTTPException