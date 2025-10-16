# Client SDK Architecture for Error Logging Service

## **Overview**

The **Client SDK** is a lightweight library that developers integrate into their applications to capture, format, and send error logs to the backend API. It supports web and server and provides deep integration with popular frontend frameworks like **React**, **Vue**, and **Angular**.

---

## **1. Goals of the SDK**

| Goal | Description |
|------|--------------|
| **Error Capture** | Automatically capture unhandled exceptions, HTTP errors, and manual logs. |
| **Context Enrichment** | Include metadata such as environment, user info, session ID, and device/browser data. |
| **Queue & Retry** | Retry sending logs after temporary network failures. |
| **Batching** | Send logs in optimized batches to reduce network load. |
| **Security** | Encrypted communication via HTTPS and signed tokens. |
| **Framework Integration** | Provide adapters for React, Vue, and Angular. |
---

## **2. Recommended Technologies**

| Component | Technology | Justification |
|------------|-------------|----------------|
| **Language** | TypeScript | Ensures type safety and works well across all major frameworks. |
| **Transport Layer** | `fetch` / `axios` | Reliable and supports interceptors, retries, and batching. |
| **Compression** | `pako` (gzip) | Reduces payload size, optimizing network performance. |
| **Error Capture (Frontend)** | `window.onerror`, `window.onunhandledrejection` | Native browser hooks for runtime error detection. |
| **Error Capture (Backend)** | `process.on('uncaughtException')`, `process.on('unhandledRejection')` | Ensures server-side runtime errors are captured. |
| **Fallback Storage** | `localStorage` or IndexedDB | For offline or delayed log sending. |

To ensure flexibility, maintainability, and smaller bundle sizes, the SDK will be published as **separate NPM packages**:

| Package | Description | Target |
|----------|--------------|--------|
| `@develops-today/logger-core` | Core logic for error capture, batching, retries, and transport | All platforms |
| `@develops-today/logger-js` | Browser-specific implementation (hooks into `window.onerror`, etc.) | Vanilla JS apps |
| `@develops-today/logger-react` | React integration with `<ErrorBoundary>` component | React apps |
| `@develops-today/logger-vue` | Vue 3 plugin integration | Vue apps |
| `@develops-today/logger-angular` | Angular service and HTTP interceptor | Angular apps |
| `@develops-today/logger-node` | Node.js SDK for backend apps | Server-side apps |

This modular approach makes it easy to install only the relevant SDK for each environment while sharing a common **core** that ensures consistent behavior.

---

## **2. Goals of the SDK**

| Goal | Description |
|------|--------------|
| **Automatic Error Capture** | Capture unhandled exceptions, promise rejections, and HTTP errors. |
| **Context Awareness** | Enrich logs with environment info, user session, and browser/device metadata. |
| **Retry & Queueing** | Store logs locally during offline periods or failures. |
| **Secure Transport** | Send data safely over HTTPS with public API key authentication. |
| **Framework Adaptation** | Integrate natively with React, Vue, Angular, and Node environments. |
| **Lightweight Footprint** | Modular builds to minimize bundle impact. |

---

## **3. Example Package Dependencies**

    A[@develops-today/logger-core] --> B[@develops-today/logger-js]
    A --> C[@develops-today/logger-node]
    B --> D[@develops-today/logger-react]
    B --> E[@develops-today/logger-vue]
    B --> F[@develops-today/logger-angular]
    A --> G[@develops-today/logger-capacitor]
