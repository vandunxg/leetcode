---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [823. Binary Trees With Factors](https://leetcode.com/problems/binary-trees-with-factors)

[中文文档](/solution/0800-0899/0823.Binary%20Trees%20With%20Factors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên không trùng lặp <code>arr</code>, trong đó mỗi số nguyên <code>arr[i]</code> đều lớn hơn <code>1</code>.</p>

<p>Ta tạo cây nhị phân từ các số nguyên này; mỗi số có thể được dùng nhiều lần. Giá trị của mỗi node không phải lá phải bằng tích giá trị các node con của nó.</p>

<p>Hãy trả về <em>số cây nhị phân có thể tạo được</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,4]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể tạo các cây sau: <code>[2], [4], [4, 2, 2]</code></pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,4,5,10]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Ta có thể tạo các cây sau: <code>[2], [4], [5], [10], [4, 2, 2], [10, 2, 5], [10, 5, 2]</code>.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 1000</code></li>
	<li><code>2 &lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả giá trị trong <code>arr</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta đếm các cây nhị phân có node lấy từ $arr$, trong đó tích giá trị các node con bằng giá trị node cha. Các giá trị khác nhau và $n\le 1000$, nên xử lý theo thứ tự tăng dần để tính các node con trước node cha.
>
> $f[i]$ là số cây có gốc tại $arr[i]$. Với mỗi node con trái $b$, nếu $a/b$ cũng có trong mảng thì cộng $f[b]\cdot f[c]$. Mỗi giá trị đều tạo được cây chỉ có một node; đáp án là tổng các giá trị trong $f$ modulo $10^9+7$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numFactoredBinaryTrees(self, arr: List[int]) -> int:
        mod = 10**9 + 7
        n = len(arr)
        arr.sort()
        idx = {v: i for i, v in enumerate(arr)}
        f = [1] * n
        for i, a in enumerate(arr):
            for j in range(i):
                b = arr[j]
                if a % b == 0 and (c := (a // b)) in idx:
                    f[i] = (f[i] + f[j] * f[idx[c]]) % mod
        return sum(f) % mod
```

#### Java

```java
class Solution {
    public int numFactoredBinaryTrees(int[] arr) {
        final int mod = (int) 1e9 + 7;
        Arrays.sort(arr);
        int n = arr.length;
        long[] f = new long[n];
        Arrays.fill(f, 1);
        Map<Integer, Integer> idx = new HashMap<>(n);
        for (int i = 0; i < n; ++i) {
            idx.put(arr[i], i);
        }
        for (int i = 0; i < n; ++i) {
            int a = arr[i];
            for (int j = 0; j < i; ++j) {
                int b = arr[j];
                if (a % b == 0) {
                    int c = a / b;
                    if (idx.containsKey(c)) {
                        int k = idx.get(c);
                        f[i] = (f[i] + f[j] * f[k]) % mod;
                    }
                }
            }
        }
        long ans = 0;
        for (long v : f) {
            ans = (ans + v) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numFactoredBinaryTrees(vector<int>& arr) {
        const int mod = 1e9 + 7;
        sort(arr.begin(), arr.end());
        unordered_map<int, int> idx;
        int n = arr.size();
        for (int i = 0; i < n; ++i) {
            idx[arr[i]] = i;
        }
        vector<long> f(n, 1);
        for (int i = 0; i < n; ++i) {
            int a = arr[i];
            for (int j = 0; j < i; ++j) {
                int b = arr[j];
                if (a % b == 0) {
                    int c = a / b;
                    if (idx.count(c)) {
                        int k = idx[c];
                        f[i] = (f[i] + 1l * f[j] * f[k]) % mod;
                    }
                }
            }
        }
        long ans = 0;
        for (long v : f) {
            ans = (ans + v) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func numFactoredBinaryTrees(arr []int) int {
	const mod int = 1e9 + 7
	sort.Ints(arr)
	f := make([]int, len(arr))
	idx := map[int]int{}
	for i, v := range arr {
		f[i] = 1
		idx[v] = i
	}
	for i, a := range arr {
		for j := 0; j < i; j++ {
			b := arr[j]
			if c := a / b; a%b == 0 {
				if k, ok := idx[c]; ok {
					f[i] = (f[i] + f[j]*f[k]) % mod
				}
			}
		}
	}
	ans := 0
	for _, v := range f {
		ans = (ans + v) % mod
	}
	return ans
}
```

#### TypeScript

```ts
function numFactoredBinaryTrees(arr: number[]): number {
    const mod = 10 ** 9 + 7;
    arr.sort((a, b) => a - b);
    const idx: Map<number, number> = new Map();
    const n = arr.length;
    for (let i = 0; i < n; ++i) {
        idx.set(arr[i], i);
    }
    const f: number[] = new Array(n).fill(1);
    for (let i = 0; i < n; ++i) {
        const a = arr[i];
        for (let j = 0; j < i; ++j) {
            const b = arr[j];
            if (a % b === 0) {
                const c = a / b;
                if (idx.has(c)) {
                    const k = idx.get(c)!;
                    f[i] = (f[i] + f[j] * f[k]) % mod;
                }
            }
        }
    }
    return f.reduce((a, b) => a + b) % mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
