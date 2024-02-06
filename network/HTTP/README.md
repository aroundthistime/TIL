# HTTP Protocol

### 1. Idempotence (멱등성)
- To call a task is idempotent, it must **always give out the same result no matter on the situtaion and the number of time it was called**.
- For the major HTTP request methods:
    - **Idempotent**: `GET`, `PUT`, `DELETE`
    - **Not Idempotent**: `POST` (could create new resource every time or the result could depend on the resource state), `PATCH` (depends on the previous resource state)
- Reason why idempotent is important with network requests:
    - Requests could fail by various reasons (eg. network status). In those case, the request should be **retried**. But if the same request was sent multiple times for some reason (eg. conflict), there could be unexpected isssue.
    - For several types of request (eg. processing a payment), idempotence is a very serious issue.
- **Idempotent request does not mean that it always gives out the same response**. The key point is **whether the result after processing the request is always the same**.
    - (ex) `DELETE` request would return `200` if corresponding resource exists and the request has permission to. If there is no resource, `404` will be returned. And depending on the permisison of the request, `401` or `403` could be returned. But to see the state of the resource of the request, it is ensured that *the resource does not exist if the user has permission to delete*. If the API is implemented in that behavior, you could say that the `DELETE` API is idempotent.
