# HELLO WORLD :D
This is a test post, designed to confirm formatting and rendering of markdown content.

**Bold Text**

*Italic Text*

1. Ordered list item 1
2. Ordered list item 2
3. Ordered list item 3

- unordered item 1
- unordered item 2
- unordered item 3

```cuda
__global__
void vecAddKernel(float* A, float* B, float* C, int n) {
    int i = threadIdx.x + blockDim.x * blockIdx.x;
    if(i < n) {
        C[i] = A[i] + B[i];
    }
}
```
| Table | Element |
| ----------- | ----------- |
| Benchmark 1 | 0.0 seconds |
| Benchmark 2 | 67 seconds |

# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6


Fenced

As more formats are added (example, LaTeX, code), more test content will be added
