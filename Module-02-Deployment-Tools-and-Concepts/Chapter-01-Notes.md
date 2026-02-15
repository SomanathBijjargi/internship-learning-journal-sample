# Week 2 Session 4
## FastAPI Introduction 

FastAPI is a modern Python web framework for building APIs with automatic interactive documentation. It’s fast, easy to use, and designed for building production-ready REST APIs.

Here’s a minimal FastAPI app, app.py:

```
# /// script
# requires-python = ">=3.11"
# dependencies = [
#   "fastapi",
#   "uvicorn",
# ]
# ///
from fastapi import FastAPI
app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello!"}

if __name__ == "__main__":
    import uvicorn

    uvicorn.run(app, host="0.0.0.0", port=8000)

```

you can run this with a command 
```
uv run app.py
```

1. Path Parameter in FastAPI

Path parameters are part of the URL itself. They are used to identify a specific resource. Use these when the data is mandatory for the request to make sense.

```
@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

2. Query Parameters

Query parameters are optional additions that come after a question mark ? in the URL. They are used to sort, filter, or paginate data.

```
@app.get("/items/{item_id}")
async def read_item(item_id: int, language: str = "english"):
    # item_id is a PATH parameter
    # language is a QUERY parameter
    return {"item_id": item_id, "lang": language}
```
3. pydantic 

Pydantic is a library for Python that ensures the data entering your application is exactly what you expect it to be.

Pydantic acts as a blueprint and a filter for your data: it defines the Item class with specific types like str and float, ensuring that any data your API sends back matches this exact structure. By using response_model=Item in the FastAPI decorator, you are telling the server to validate the dictionary returned by the function against that model; if the extra_field is defined in the class, it is included in the output, but if it were removed from the class, Pydantic would automatically "strip" it away before the user sees it. This process guarantees that your API output is consistent, type-safe, and secure by only exposing the specific fields you've explicitly defined in your code.

```
form pydantic import BaseModel

class Item(BaseModel):
    name:str
    price:float

@app.get("/item", responce_model=Item)
def get_Item():
    return {"name":"Mango","price":12.5,"extra_field":"This will be ignored"}

```