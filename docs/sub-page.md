# 🔍 Debugging & Runbooks

Welcome to the in-depth guide for debugging and operating the **TransactionProcessing** library. This page covers common issues, troubleshooting steps, available GitHub Actions (GHA) workflows, and runbooks for rapid resolution.

---

## 🛠️ Common Debugging Scenarios

<!-- prettier-ignore -->
???+ warn "Warning: Sensitive Data"
    Always ensure that logs and debugging output do **not** contain sensitive customer or transaction data. Follow BNC compliance guidelines when sharing logs.

### 1. Transaction Fails with `RETRY_LIMIT_EXCEEDED`

<!-- prettier-ignore -->
??? note "Symptoms & Steps"
    **Symptom:** Transaction does not complete, logs show repeated retries.

    **Possible Causes:** Downstream service unavailable, invalid transaction data.

    **Resolution Steps:**
    1. Check logs for error details (`/var/log/transaction-processing/*.log`).
    2. Validate transaction payload for required fields.
    3. Confirm downstream service health.
    4. [See Retry Logic Details](#retry-logic).

---

### 2. Audit Trail Missing for Transaction

<!-- prettier-ignore -->
??? note "Symptoms & Steps"
    **Symptom:** Transaction processed, but no audit entry found.

    **Possible Causes:** Audit service misconfiguration, network partition.

    **Resolution Steps:**
    1. Verify `AUDIT_SERVICE_URL` environment variable.
    2. Check network connectivity to audit service.
    3. Review library configuration for audit hooks.

---

### 3. Unexpected Transaction Status

<!-- prettier-ignore -->
??? note "Symptoms & Steps"
    **Symptom:** Transaction stuck in `PENDING` or `UNKNOWN` state.

    **Possible Causes:** Callback not invoked, business logic extension error.

    **Resolution Steps:**
    1. Inspect custom hooks for thrown exceptions.
    2. Review callback registration in your integration code.
    3. [See Extension Points](extensions.md).

---

## 🔄 Retry Logic

By default, the library retries failed transactions up to **3 times** with exponential backoff.  
You can configure this via the `maxRetries` and `backoffStrategy` options:

```typescript
const processor = new TransactionProcessor({
  maxRetries: 5,
  backoffStrategy: 'linear',
});
```

<!-- prettier-ignore -->
??? tip "Best Practice"
    Tune your retry settings based on the reliability of downstream services and the criticality of the transaction.

---

## ⚙️ Available GitHub Actions Workflows

| Workflow Name                | Description                        | Link                                      |
|------------------------------|------------------------------------|-------------------------------------------|
| CI Pipeline                  | Lints, builds, and tests the code  | [View Workflow](.github/workflows/ci.yml) |
| Release & Publish            | Publishes package to registry      | [View Workflow](.github/workflows/release.yml) |
| Security Scan                | Runs security checks               | [View Workflow](.github/workflows/security.yml) |
| Integration Test             | Runs integration tests             | [View Workflow](.github/workflows/integration.yml) |

---

## 📖 Runbooks

<!-- prettier-ignore -->
??? info "What is a runbook?"
    Runbooks are step-by-step guides for resolving common issues or performing operational tasks.

| Runbook Name                       | Description                                      | Link                |
|-------------------------------------|--------------------------------------------------|---------------------|
| Retry Failures                     | Steps to diagnose and resolve retry issues        | [Open Runbook](runbooks/retry-failures.md) |
| Audit Trail Troubleshooting         | How to debug missing audit logs                  | [Open Runbook](runbooks/audit-trail.md)    |
| Transaction Status Investigation    | Resolving stuck or unknown transaction states    | [Open Runbook](runbooks/status-investigation.md) |
| Library Upgrade Guide               | Safely upgrading to a new library version        | [Open Runbook](runbooks/upgrade-guide.md)   |

---

## 🧑‍💻 Need More Help?

<!-- prettier-ignore -->
???+ note "How do I get support?"
    You can get support by contacting the BNC Platform Engineering team or by visiting the [Debugging & Runbooks](sub-page.md) section.

- [Extension Points](extensions.md)
- [Code Examples](code/code-sample.md)

---