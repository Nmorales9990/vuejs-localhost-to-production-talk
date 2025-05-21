---
theme: gaia
paginate: true
backgroundColor: #000
color: #fff
colorPreset: dark
footer: Vue.js Global Summit 2025
---

![bg left:30% 70%](poster.png)

# **From Localhost to Production: CI/CD for Vue with GitHub Actions**

Seyyed Ali Mohammadiyeh (Max Base)  
Vue.js Global Summit 25 - May 21, 2025

---

# **About Me**

**Seyyed Ali Mohammadiyeh (Max Base)**  
Open-source Maintainer, GitHub  
Senior Software Engineer  
CTO, asrez  
📧 maxbasecode@gmail.com

---

# **Experience**

- 👨‍💻 GitHub: [https://github.com/basemax](https://github.com/basemax)  
- 🧠 10+ years of experience in software development  
- 🎓 Background in pure and applied mathematics with research experience  

---

# **Session Overview**

Explore how **Bun** and **TypeScript** enable the creation of a high-performance, real-time gateway that routes incoming HTTP requests to the appropriate microservices.

- Why traditional reverse proxies (like NGINX or Express) fall short in dynamic routing scenarios.
- How Bun offers minimal latency, simplified logic, and better scalability.

---

## This talk includes:  

✅ Real-world architecture & benchmarks  
✅ Code examples  
✅ Lessons from production

---

# **Goals of This Talk**

1. Learn to build a real-time microservice gateway using **Bun + TypeScript**
2. Discover how **Bun** can outperform Express/NGINX in dynamic routing
3. Understand live request forwarding with minimal latency

---

# **Getting Started with Bun**

```bash
$ bun init

✓ Select a project template: Blank

 + .gitignore
 + index.ts
 + tsconfig.json (for editor autocomplete)

To get started, run:
    bun run index.ts
```

```ts
// index.ts
console.log("Hello via Bun!");
```

----

# Running your first Bun program

```bash
$ bun run index.ts
Hello via Bun!
```

---

# **Why Bun?**

✅ Native HTTP server (no need for Express or external frameworks)  
✅ Written in TypeScript  
✅ Built-in routing based on URL, headers, or tokens  
✅ Blazing fast  
✅ Zero dependencies

**Downsides**:  
⚠️ Type checking and runtime validation in TypeScript is still maturing.  
⚠️ Limited ecosystem maturity compared to Node.js

---

# **Using Bun.serve()**

```ts
const server = Bun.serve({
  port: 9999,
  fetch(req) {
    return new Response("Not Found", { status: 404 });
  },
});
```

- Every incoming request triggers the `fetch()` function.
- You can access request details through the `req` object.

---

# **Accessing Request Details**

```ts
const server = Bun.serve({
  port: 9999,
  fetch(req) {
    const url = new URL(req.url);
    console.log(url);
    return new Response("Not Found", { status: 404 });
  },
});
```

---


Example `req` (request) object:

```ts
Request {
  method: "GET",
  url: "http://localhost:9999/",
  headers: Headers {
    "host": "localhost:9999",
    "user-agent": "...",
    ...
  }
}
```

----

Key values to use:

- `req.method`
- `req.url`
- `req.headers`

---

# **Parsed URL Object**

Output of `new URL(req.url)`:

```ts
URL {
  href: "http://localhost:9999/favicon.ico",
  origin: "http://localhost:9999",
  pathname: "/favicon.ico",
  searchParams: URLSearchParams {}
}
```

---

## Let's Create a Microservice Gateway

Imagine we are working on a large-scale project with over 800 RESTful API routes.

From an architectural perspective, we can split the entire platform into 20 sub-projects.

Each of these 20 sub-projects runs independently and serves on a different HTTP port.

---

## Let's Create a Microservice Gateway

Now, we need a central gateway — a program that initially receives all incoming requests from end users. It should analyze each request to determine which sub-project or microservice it belongs to and then forward the request accordingly.

The gateway must not only forward the request to the appropriate sub-project but also handle the response by forwarding it back to the user.

---

## Let's create a config structure


```json
{
  "host": "0.0.0.0",
  "port": 9999,
  "services": [
    {
      "prefix": "/user/",
      "host": "localhost",
      "port": 8888
    },
    {
      "path": "/my/login/",
      "method": "POST",
      "host": "localhost",
      "port": 6666
    },
    ...
  ]
}
```

---

# Coding

Create a new Bun project:

```
bun init
```

---

# **Wrap Up**

- Bun + TypeScript = high-speed real-time gateways  
- Ideal for scalable microservices with dynamic routing  
- Simpler, faster, and more maintainable than traditional tools

**Thank you!**  

🔗 [github.com/basemax](https://github.com/basemax)  
📧 maxbasecode@gmail.com
