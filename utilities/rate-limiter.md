# 🧱 Simple Java-Based Token Bucket Rate Limiter

## 🧠 Concept

A **token bucket** is a rate-limiting algorithm where:

- Tokens are added to the bucket at a fixed rate
- Each request consumes 1 token
- If no tokens available → request is rejected or delayed

## ⚙️ Use Case

Prevent users from making more than X API requests per second.

---

## 🧪 Java Sketch (Concept Only)

```java
public class TokenBucketLimiter {
    private final int capacity;
    private final int refillRate;
    private int tokens;
    private long lastRefillTimestamp;

    public TokenBucketLimiter(int capacity, int refillRate) {
        this.capacity = capacity;
        this.refillRate = refillRate;
        this.tokens = capacity;
        this.lastRefillTimestamp = System.nanoTime();
    }

    public synchronized boolean allowRequest() {
        refill();
        if (tokens > 0) {
            tokens--;
            return true;
        }
        return false;
    }

    private void refill() {
        long now = System.nanoTime();
        long elapsed = now - lastRefillTimestamp;
        int tokensToAdd = (int) (elapsed / 1_000_000_000L) * refillRate;
        if (tokensToAdd > 0) {
            tokens = Math.min(capacity, tokens + tokensToAdd);
            lastRefillTimestamp = now;
        }
    }
}
```
