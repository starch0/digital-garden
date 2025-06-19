# Low-Level Article: Clusters in JavaScript with Functional Programming and TypeScript Examples

JavaScript is a traditionally single-threaded language, meaning it can only process one operation at a time per process. However, with the advent of Node.js, JavaScript gained the ability to utilize system-level features like multi-core processing through the **cluster** module. This article explores details of clusters in JavaScript.

## What are Clusters in JavaScript?

A **cluster** in Node.js is a set of Node processes running simultaneously and sharing the same server port. Each process is called a **worker**. Clustering enables you to fully utilize all CPU cores on a machine, improving performance for compute-intensive and high-traffic applications.

Under the hood, Node.js uses the `child_process.fork()` method to spawn worker processes. The **primary process** (sometimes called the "master") is responsible for managing the workers.

## Why Use Clusters?

- **CPU Utilization**: Node.js applications are usually CPU-bound. Using clusters allows you to spawn as many processes as CPU cores.
- **Fault Tolerance**: If a worker crashes, the master process can spawn a new one.
- **Scalability**: Clusters allow horizontal scaling on a single machine.

## Functional Programming Principles

Functional programming (FP) emphasizes pure functions, immutability, and statelessness. When used with clusters, FP can help avoid shared mutable state between workers, thus preventing race conditions and hard-to-debug errors.

## Example: Basic Cluster Setup in TypeScript

Install Node.js types for TypeScript:

```bash
npm install --save-dev @types/node
```

### cluster-server.ts

```typescript
import cluster from 'cluster';
import { cpus } from 'os';
import http from 'http';

// Pure function: generates a request handler
const createRequestHandler = () => (req: http.IncomingMessage, res: http.ServerResponse) => {
  res.writeHead(200);
  res.end(`Handled by worker ${process.pid}\n`);
};

const PORT = 3000;

if (cluster.isPrimary) {
  const numCPUs = cpus().length;
  console.log(`Primary ${process.pid} is running`);
  
  // Fork workers
  Array.from({ length: numCPUs }).forEach(() => cluster.fork());

  cluster.on('exit', (worker, code, signal) => {
    console.log(`Worker ${worker.process.pid} died. Restarting...`);
    cluster.fork();
  });
} else {
  // Workers share the same TCP connection
  http.createServer(createRequestHandler()).listen(PORT, () => {
    console.log(`Worker ${process.pid} started`);
  });
}
```

### Explanation

- **No Shared State**: Each worker is isolated; no mutable state is shared.
- **Pure Functions**: The request handler factory returns a new handler function.
- **Immutability**: All configuration (like `PORT`) is constant.

## Advanced Example: Functional Composition

You can compose middleware using FP patterns:

```typescript
// Higher-order function: logs before handling request
const withLogging = (handler: http.RequestListener): http.RequestListener =>
  (req, res) => {
    console.log(`[${process.pid}] ${req.method} ${req.url}`);
    handler(req, res);
  };

http.createServer(withLogging(createRequestHandler())).listen(PORT);
```

This functional approach allows you to build middleware pipelines, each responsible for a single aspect (logging, authentication, etc.), and compose them together.

## Best Practices

- **Stateless Workers**: Do not share state between workers; use external storage (e.g., Redis) if necessary.
- **Functional Patterns**: Use pure functions and higher-order functions for composability and testability.
- **Graceful Shutdown**: Handle worker crashes and restarts cleanly.

## Conclusion

Clustering in JavaScript (Node.js) is a powerful technique for scaling applications. By leveraging functional programming principles and TypeScript, you can create robust, maintainable, and scalable cluster-based applications with minimal risk of concurrency bugs.

---
**References:**
- [Node.js Cluster Documentation](https://nodejs.org/api/cluster.html)
- [Functional Programming in JavaScript](https://www.freecodecamp.org/news/functional-programming-in-js-with-practical-examples/)