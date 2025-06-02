# 🧩 Plugins & Extension Points

The **TransactionProcessing** library is designed for extensibility, allowing you to adapt transaction workflows to your business needs at Banque Nationale du Canada (BNC). This page explains how to leverage and implement extension points, as well as other advanced features.

---

## 🔌 Extension Hooks

You can inject custom logic at key points in the transaction lifecycle using hooks. This enables validation, enrichment, notifications, or integration with other services.

<!-- prettier-ignore -->
!!! info
    Hooks allow you to customize the transaction flow without modifying the core library. Use them for validation, notifications, or integrating with other BNC services.

**Example: Adding Custom Validation and Notifications**

```typescript
const processor = new TransactionProcessor({
  hooks: {
    beforeProcess: (txn) => {
      if (!txn.referenceId) throw new Error('Missing referenceId');
    },
    afterProcess: (result) => {
      sendAuditNotification(result);
    },
    onError: (err, txn) => {
      alertOpsTeam(err, txn);
    },
  },
});
```

**Available Hooks:**

| Hook Name      | When It Runs                | Typical Use Cases                  |
|----------------|----------------------------|------------------------------------|
| beforeProcess  | Before transaction starts   | Validation, enrichment             |
| afterProcess   | After transaction completes | Notifications, logging             |
| onError        | On transaction failure      | Alerting, custom error handling    |

---

## 🛠️ Custom Retry Strategies

You can override the default retry logic to suit your service’s reliability requirements.

```typescript
const processor = new TransactionProcessor({
  maxRetries: 7,
  backoffStrategy: (attempt) => 1000 * attempt, // Custom backoff in ms
});
```

<!-- prettier-ignore -->
!!! tip
    Use a custom backoff strategy for services with variable reliability or to avoid overwhelming downstream dependencies.

---

## 🏗️ Adding New Transaction Types

To support new transaction types, simply pass a new `type` value and implement any required business logic in your hooks.

```typescript
processor.process({
  type: 'CRYPTO_PURCHASE',
  amount: 500,
  currency: 'CAD',
  onSuccess: handleCryptoSuccess,
  onError: handleCryptoError,
});
```

<!-- prettier-ignore -->
!!! note
    New transaction types can be introduced without changes to the core library. Use hooks to handle any custom logic.

---

## 🧪 Testing Extensions

When extending the library, ensure your custom hooks and logic are covered by tests.  
See [Code Examples](code/code-sample.md) for integration patterns.

<!-- prettier-ignore -->
!!! warn
    Always test your hooks and custom logic in a staging environment before deploying to production.

---

## 📊 Transaction Processing Flow

Below is a UML sequence diagram showing a typical transaction processing flow with extension hooks:

```plantuml format="svg" classes="uml myDiagram" alt="Transaction Processing UML" title="Transaction Processing UML" width="600px" height="300px"
actor User
participant "Your Service" as Service
participant "TransactionProcessor" as TP
participant "Audit Service" as Audit
participant "Downstream System" as Downstream

User -> Service: Initiate Transaction
Service -> TP: process(transaction)
TP -> TP: beforeProcess Hook
TP -> Audit: logStart()
TP -> Downstream: sendTransaction()
Downstream --> TP: response
TP -> TP: afterProcess Hook
TP -> Audit: logResult()
TP --> Service: onSuccess/onError callback
Service --> User: Result
```

---

## 📚 Related Resources

- [Debugging & Runbooks](sub-page.md)
- [Code Examples](code/code-sample.md)
- [Official Backstage TechDocs Guide](https://backstage.io/docs/features/techdocs/)

---

## 📝 Best Practices

<!-- prettier-ignore -->
!!! tip
    - Keep custom logic in hooks idempotent and side-effect free where possible.
    - Use the audit logging feature for traceability of all custom actions.
    - Document your extension points for future maintainers.

---

<!-- prettier-ignore -->
???+ note "How do I get support?"
    You can get support by contacting the BNC Platform Engineering team or by visiting the [Runbooks](sub-page.md) for troubleshooting tips.