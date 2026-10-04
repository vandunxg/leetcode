---
comments: true
difficulty: Medium
rating: 1504
source: Weekly Contest 352 Q2
tags:
    - Array
    - Math
    - Enumeration
    - Number Theory
---

<!-- problem:start -->

# [2761. Prime Pairs With Target Sum](https://leetcode.com/problems/prime-pairs-with-target-sum)

[中文文档](/solution/2700-2799/2761.Prime%20Pairs%20With%20Target%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>. Ta gọi hai số nguyên <code>x</code> và <code>y</code> tạo thành một cặp số nguyên tố nếu:</p>

<ul>
	<li><code>1 &lt;= x &lt;= y &lt;= n</code></li>
	<li><code>x + y == n</code></li>
	<li><code>x</code> và <code>y</code> là các số nguyên tố</li>
</ul>

<p>Trả về <em>danh sách 2D các cặp số nguyên tố được sắp xếp</em> <code>[x<sub>i</sub>, y<sub>i</sub>]</code>. Danh sách phải được sắp xếp theo thứ tự <strong>tăng dần</strong> của <code>x<sub>i</sub></code>. Nếu không có cặp số nguyên tố nào, trả về <em>một mảng rỗng</em>.</p>

<p><strong>Lưu ý:</strong> Số nguyên tố là một số tự nhiên lớn hơn <code>1</code> chỉ có đúng hai ước là chính nó và <code>1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10
<strong>Đầu ra:</strong> [[3,7],[5,5]]
<strong>Giải thích:</strong> Trong ví dụ này, có hai cặp số nguyên tố thỏa mãn các điều kiện.
Đó là [3,7] và [5,5], được trả về theo thứ tự đã mô tả trong đề bài.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Có thể thấy không có cặp số nguyên tố nào có tổng bằng 2, nên ta trả về một mảng rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Tìm các cặp số nguyên tố $x\le y$ sao cho $x+y=n$. Việc chia thử cho mọi $x$ sẽ chậm khi $n\le 10^6$.
>
> Dùng sàng để xác định tính nguyên tố trên $[2,n)$, sau đó liệt kê $x\in[2,n/2]$ và đưa cặp vào kết quả khi cả $x$ và $n-x$ đều là số nguyên tố.

<!-- thinking:end -->

Trước hết, ta tiền xử lý tất cả số nguyên tố trong phạm vi của $n$ và lưu chúng vào mảng $primes$, trong đó $primes[i]$ là `true` nếu $i$ là số nguyên tố.

Tiếp theo, ta liệt kê $x$ trong khoảng $[2, \frac{n}{2}]$. Khi đó, $y = n - x$. Nếu cả $primes[x]$ và $primes[y]$ đều là `true`, thì $(x, y)$ là một cặp số nguyên tố và được thêm vào đáp án.

Sau khi liệt kê xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n \log \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findPrimePairs(self, n: int) -> List[List[int]]:
        primes = [True] * n
        for i in range(2, n):
            if primes[i]:
                for j in range(i + i, n, i):
                    primes[j] = False
        ans = []
        for x in range(2, n // 2 + 1):
            y = n - x
            if primes[x] and primes[y]:
                ans.append([x, y])
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> findPrimePairs(int n) {
        boolean[] primes = new boolean[n];
        Arrays.fill(primes, true);
        for (int i = 2; i < n; ++i) {
            if (primes[i]) {
                for (int j = i + i; j < n; j += i) {
                    primes[j] = false;
                }
            }
        }
        List<List<Integer>> ans = new ArrayList<>();
        for (int x = 2; x <= n / 2; ++x) {
            int y = n - x;
            if (primes[x] && primes[y]) {
                ans.add(List.of(x, y));
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> findPrimePairs(int n) {
        bool primes[n];
        memset(primes, true, sizeof(primes));
        for (int i = 2; i < n; ++i) {
            if (primes[i]) {
                for (int j = i + i; j < n; j += i) {
                    primes[j] = false;
                }
            }
        }
        vector<vector<int>> ans;
        for (int x = 2; x <= n / 2; ++x) {
            int y = n - x;
            if (primes[x] && primes[y]) {
                ans.push_back({x, y});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findPrimePairs(n int) (ans [][]int) {
	primes := make([]bool, n)
	for i := range primes {
		primes[i] = true
	}
	for i := 2; i < n; i++ {
		if primes[i] {
			for j := i + i; j < n; j += i {
				primes[j] = false
			}
		}
	}
	for x := 2; x <= n/2; x++ {
		y := n - x
		if primes[x] && primes[y] {
			ans = append(ans, []int{x, y})
		}
	}
	return
}
```

#### TypeScript

```ts
function findPrimePairs(n: number): number[][] {
    const primes: boolean[] = new Array(n).fill(true);
    for (let i = 2; i < n; ++i) {
        if (primes[i]) {
            for (let j = i + i; j < n; j += i) {
                primes[j] = false;
            }
        }
    }
    const ans: number[][] = [];
    for (let x = 2; x <= n / 2; ++x) {
        const y = n - x;
        if (primes[x] && primes[y]) {
            ans.push([x, y]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
