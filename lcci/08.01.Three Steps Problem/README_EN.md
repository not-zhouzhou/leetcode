# [08.01. Three Steps Problem](https://leetcode.cn/problems/three-steps-problem-lcci)

[中文文档](/lcci/08.01.Three%20Steps%20Problem/README.md)

## Description

<p>A child is running up a staircase with n steps and can hop either 1 step, 2 steps, or 3 steps at a time. Implement a method to count how many possible ways the child can run up the stairs.&nbsp;The result may be large, so return it modulo 1000000007.</p>

<p><strong>Example1:</strong></p>

<pre>

<strong> Input</strong>: n = 3 

<strong> Output</strong>: 4

</pre>

<p><strong>Example2:</strong></p>

<pre>

<strong> Input</strong>: n = 5

<strong> Output</strong>: 13

</pre>

<p><strong>Note:</strong></p>

1. `1 <= n <= 1000000`

## Solutions

<!-- tabs:start -->

### **Python3**

```python
class Solution:
    def waysToStep(self, n: int) -> int:
        a, b, c = 1, 2, 4
        mod = 10**9 + 7
        for _ in range(n - 1):
            a, b, c = b, c, (a + b + c) % mod
        return a
```

```python
class Solution:
    def waysToStep(self, n: int) -> int:
        mod = 10**9 + 7

        def mul(a: List[List[int]], b: List[List[int]]) -> List[List[int]]:
            m, n = len(a), len(b[0])
            c = [[0] * n for _ in range(m)]
            for i in range(m):
                for j in range(n):
                    for k in range(len(a[0])):
                        c[i][j] = (c[i][j] + a[i][k] * b[k][j] % mod) % mod
            return c

        def pow(a: List[List[int]], n: int) -> List[List[int]]:
            res = [[4, 2, 1]]
            while n:
                if n & 1:
                    res = mul(res, a)
                n >>= 1
                a = mul(a, a)
            return res

        if n < 4:
            return 2 ** (n - 1)
        a = [[1, 1, 0], [1, 0, 1], [1, 0, 0]]
        return sum(pow(a, n - 4)[0]) % mod
```

```python
import numpy as np


class Solution:
    def waysToStep(self, n: int) -> int:
        if n < 4:
            return 2 ** (n - 1)
        mod = 10**9 + 7
        factor = np.mat([(1, 1, 0), (1, 0, 1), (1, 0, 0)], np.dtype("O"))
        res = np.mat([(4, 2, 1)], np.dtype("O"))
        n -= 4
        while n:
            if n & 1:
                res = res * factor % mod
            factor = factor * factor % mod
            n >>= 1
        return res.sum() % mod
```

### **Java**

```java
class Solution {
    public int waysToStep(int n) {
        final int mod = (int) 1e9 + 7;
        int a = 1, b = 2, c = 4;
        for (int i = 1; i < n; ++i) {
            int t = a;
            a = b;
            b = c;
            c = (((a + b) % mod) + t) % mod;
        }
        return a;
    }
}
```

```java
class Solution {
    private final int mod = (int) 1e9 + 7;

    public int waysToStep(int n) {
        if (n < 4) {
            return (int) Math.pow(2, n - 1);
        }
        long[][] a = {{1, 1, 0}, {1, 0, 1}, {1, 0, 0}};
        long[][] res = pow(a, n - 4);
        long ans = 0;
        for (long x : res[0]) {
            ans = (ans + x) % mod;
        }
        return (int) ans;
    }

    private long[][] mul(long[][] a, long[][] b) {
        int m = a.length, n = b[0].length;
        long[][] c = new long[m][n];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < b.length; ++k) {
                    c[i][j] = (c[i][j] + a[i][k] * b[k][j] % mod) % mod;
                }
            }
        }
        return c;
    }

    private long[][] pow(long[][] a, int n) {
        long[][] res = {{4, 2, 1}};
        while (n > 0) {
            if ((n & 1) == 1) {
                res = mul(res, a);
            }
            a = mul(a, a);
            n >>= 1;
        }
        return res;
    }
}
```

### **C++**

```cpp
class Solution {
public:
    int waysToStep(int n) {
        const int mod = 1e9 + 7;
        int a = 1, b = 2, c = 4;
        for (int i = 1; i < n; ++i) {
            int t = a;
            a = b;
            b = c;
            c = (((a + b) % mod) + t) % mod;
        }
        return a;
    }
};
```

```cpp
class Solution {
public:
    int waysToStep(int n) {
        if (n < 4) {
            return pow(2, n - 1);
        }
        vector<vector<ll>> a = {{1, 1, 0}, {1, 0, 1}, {1, 0, 0}};
        vector<vector<ll>> res = qpow(a, n - 4);
        ll ans = 0;
        for (ll x : res[0]) {
            ans = (ans + x) % mod;
        }
        return ans;
    }

private:
    using ll = long long;
    const int mod = 1e9 + 7;
    vector<vector<ll>> mul(vector<vector<ll>>& a, vector<vector<ll>>& b) {
        int m = a.size(), n = b[0].size();
        vector<vector<ll>> c(m, vector<ll>(n));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                for (int k = 0; k < b.size(); ++k) {
                    c[i][j] = (c[i][j] + a[i][k] * b[k][j] % mod) % mod;
                }
            }
        }
        return c;
    }

    vector<vector<ll>> qpow(vector<vector<ll>>& a, int n) {
        vector<vector<ll>> res = {{4, 2, 1}};
        while (n) {
            if (n & 1) {
                res = mul(res, a);
            }
            a = mul(a, a);
            n >>= 1;
        }
        return res;
    }
};
```

### **Go**

```go
func waysToStep(n int) int {
	const mod int = 1e9 + 7
	a, b, c := 1, 2, 4
	for i := 1; i < n; i++ {
		a, b, c = b, c, (a+b+c)%mod
	}
	return a
}
```

```go
const mod = 1e9 + 7

func waysToStep(n int) (ans int) {
	if n < 4 {
		return int(math.Pow(2, float64(n-1)))
	}
	a := [][]int{{1, 1, 0}, {1, 0, 1}, {1, 0, 0}}
	res := pow(a, n-4)
	for _, x := range res[0] {
		ans = (ans + x) % mod
	}
	return
}

func mul(a, b [][]int) [][]int {
	m, n := len(a), len(b[0])
	c := make([][]int, m)
	for i := range c {
		c[i] = make([]int, n)
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			for k := 0; k < len(b); k++ {
				c[i][j] = (c[i][j] + a[i][k]*b[k][j]%mod) % mod
			}
		}
	}
	return c
}

func pow(a [][]int, n int) [][]int {
	res := [][]int{{4, 2, 1}}
	for n > 0 {
		if n&1 == 1 {
			res = mul(res, a)
		}
		a = mul(a, a)
		n >>= 1
	}
	return res
}
```

### **JavaScript**

```js
/**
 * @param {number} n
 * @return {number}
 */
var waysToStep = function (n) {
    let [a, b, c] = [1, 2, 4];
    const mod = 1e9 + 7;
    for (let i = 1; i < n; ++i) {
        [a, b, c] = [b, c, (a + b + c) % mod];
    }
    return a;
};
```

```js
/**
 * @param {number} n
 * @return {number}
 */

const mod = 1e9 + 7;

var waysToStep = function (n) {
    if (n < 4) {
        return Math.pow(2, n - 1);
    }
    const a = [
        [1, 1, 0],
        [1, 0, 1],
        [1, 0, 0],
    ];
    let ans = 0;
    const res = pow(a, n - 4);
    for (const x of res[0]) {
        ans = (ans + x) % mod;
    }
    return ans;
};

function mul(a, b) {
    const [m, n] = [a.length, b[0].length];
    const c = Array.from({ length: m }, () => Array.from({ length: n }, () => 0));
    for (let i = 0; i < m; ++i) {
        for (let j = 0; j < n; ++j) {
            for (let k = 0; k < b.length; ++k) {
                c[i][j] =
                    (c[i][j] + Number((BigInt(a[i][k]) * BigInt(b[k][j])) % BigInt(mod))) % mod;
            }
        }
    }
    return c;
}

function pow(a, n) {
    let res = [[4, 2, 1]];
    while (n) {
        if (n & 1) {
            res = mul(res, a);
        }
        a = mul(a, a);
        n >>= 1;
    }
    return res;
}
```

### **C**

```c
int waysToStep(int n) {
    const int mod = 1e9 + 7;
    int a = 1, b = 2, c = 4;
    for (int i = 1; i < n; ++i) {
        int t = a;
        a = b;
        b = c;
        c = (((a + b) % mod) + t) % mod;
    }
    return a;
}
```

### **Rust**

```rust
impl Solution {
    pub fn ways_to_step(n: i32) -> i32 {
        let (mut a, mut b, mut c) = (1, 2, 4);
        let m = 1000000007;
        for _ in 1..n {
            let t = a;
            a = b;
            b = c;
            c = ((a + b) % m + t) % m;
        }
        a
    }
}
```

<!-- tabs:end -->
