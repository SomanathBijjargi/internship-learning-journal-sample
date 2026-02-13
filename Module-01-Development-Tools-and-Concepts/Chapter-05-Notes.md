# Week 2 Session 2
## Github Pages

GitHub Pages is a free web hosting service provided by GitHub that allows you to turn a repository into a live website. It is designed specifically for static sites websites composed of HTML, CSS, and JavaScript files that look the same for every visitor.

When you push code to a specific branch (usually main) or a designated folder (like /docs), GitHub’s servers automatically build and host that content.

## Key Features
- Free Hosting: No monthly fees or hosting costs for public repositories.

- Custom Domains: You can use the default username.github.io address or connect your own domain (e.g., www.yourname.com).

- Built-in HTTPS: GitHub provides automatic SSL certificates to keep your site secure.

- Jekyll Integration: It has native support for Jekyll, a static site generator that lets you build blogs and documentation using Markdown instead of writing raw HTML for every page.

## Introduction to FastAPI

FastAPI is a modern, high-performance web framework for building APIs with Python based on standard Python type hints. Its "fast" reputation comes from both its execution speed (comparable to NodeJS and Go) and the speed at which you can develop.

```
from fastapi import FastAPI
app = FastAPI()            
@app.get("/")               
def read_root():
    return {"Hello": "World"}
```

## Understanding of ngrok

Think of ngrok as a secure "wormhole" for the internet. It creates a public-facing URL that connects directly to a specific port on your local machine, allowing anyone with the link to see what you are building in real-time. This bypasses the need for complex router configurations, port forwarding, or deploying to a cloud provider just to show a demo.

## Cors(cross - origin resources sharing)

is a security mechanism built to modern web browser

- HTTP headers to define which external domains are permitted to access restricted resources on your server.