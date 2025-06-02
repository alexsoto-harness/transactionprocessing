# 💻 Code Examples

This page provides practical examples for integrating the **Transaction Processing** library into your project, along with a summary of its core features.

---

## ✨ Key Features

- **Transaction Lifecycle Management:** Start, process, and finalize transactions with built-in state handling.
- **Retry Logic:** Automatic retries for transient failures, configurable backoff strategies.
- **Audit Logging:** Every transaction is logged for traceability and compliance.
- **Extensible Hooks:** Inject custom business logic at key points in the transaction flow.
- **Error Handling:** Standardized error reporting and recovery mechanisms.

---

## 🚀 Basic Integration Example

```typescript
import { TransactionProcessor } from '@bnc/transaction-processing';

const processor = new TransactionProcessor();

processor.process({
  type: 'WIRE_TRANSFER',
  amount: 2500,
  currency: 'CAD',
  onSuccess: (result) => console.log('Transaction successful:', result),
  onError: (err) => console.error('Transaction failed:', err),
});
```

---

## 🔁 Customizing Retry Logic

```typescript
const processor = new TransactionProcessor({
  maxRetries: 5,
  backoffStrategy: 'linear',
});
```

---

## 🪝 Using Extension Hooks

```typescript
const processor = new TransactionProcessor({
  hooks: {
    beforeProcess: (txn) => {
      // Custom validation or enrichment
      console.log('About to process:', txn);
    },
    afterProcess: (result) => {
      // Custom post-processing
      sendNotification(result);
    },
  },
});
```

---

## 📝 Error Handling Example

```typescript
try {
  await processor.process({
    type: 'BILL_PAYMENT',
    amount: 100,
    currency: 'CAD',
  });
} catch (error) {
  // Handle or log the error
  reportError(error);
}
```

---

For more advanced scenarios, see the [Debugging & Runbooks](../sub-page.md) and [Extension Guide](../extensions.md).