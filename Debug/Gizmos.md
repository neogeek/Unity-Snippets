# Gizmos

## Draw Circle at Bottom Left Of GameObject

```csharp
void OnDrawGizmosSelected() {
    Gizmos.DrawWireSphere(GetComponent<Renderer>().bounds.min, 1.0f);
}
```
