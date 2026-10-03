---
comments: true
difficulty: Medium
rating: 1649
source: Weekly Contest 326 Q4
tags:
    - Math
    - Number Theory
    - Primality Test
    - Sieve
    - Sieve of Eratosthenes
---

<!-- problem:start -->

# [2523. Closest Prime Numbers in Range](https://leetcode.com/problems/closest-prime-numbers-in-range)

[中文文档](/solution/2500-2599/2523.Closest%20Prime%20Numbers%20in%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên dương <code>left</code> và <code>right</code>, hãy tìm hai số nguyên <code>num1</code> và <code>num2</code> sao cho:</p>

<ul>
	<li><code>left &lt;= num1 &lt; num2 &lt;= right </code>.</li>
	<li>Cả <code>num1</code> và <code>num2</code> đều là <span data-keyword="prime-number">số nguyên tố</span>.</li>
	<li><code>num2 - num1</code> là <strong>nhỏ nhất</strong> trong tất cả các cặp khác thỏa mãn các điều kiện trên.</li>
</ul>

<p>Trả về mảng số nguyên dương <code>ans = [num1, num2]</code>. Nếu có nhiều cặp thỏa mãn các điều kiện trên, trả về cặp có giá trị <code>num1</code> <strong>nhỏ nhất</strong>. Nếu không tồn tại cặp nào như vậy, trả về <code>[-1, -1]</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> left = 10, right = 19
<strong>Đầu ra:</strong> [11,13]
<strong>Giải thích:</strong> Các số nguyên tố trong khoảng từ 10 đến 19 là 11, 13, 17 và 19.
Khoảng cách nhỏ nhất giữa bất kỳ cặp nào là 2, đạt được với [11,13] hoặc [17,19].
Vì 11 nhỏ hơn 17 nên ta trả về cặp đầu tiên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> left = 4, right = 6
<strong>Đầu ra:</strong> [-1,-1]
<strong>Giải thích:</strong> Trong khoảng đã cho chỉ có một số nguyên tố, nên không thể thỏa mãn các điều kiện.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= left &lt;= right &lt;= 10<sup>6</sup></code></li>
</ul>

<p>&nbsp;</p>
<style type="text/css">.spoilerbutton {display:block; border:dashed; padding: 0px 0px; margin:10px 0px; font-size:150%; font-weight: bold; color:#000000; background-color:cyan; outline:0; 
}
.spoiler {overflow:hidden;}
.spoiler > div {-webkit-transition: all 0s ease;-moz-transition: margin 0s ease;-o-transition: margin 0s ease;transition: margin 0s ease;}
.spoilerbutton[value="Show Message"] + .spoiler > div {margin-top:-500%;}
.spoilerbutton[value="Hide Message"] + .spoiler {padding:5px;}
</style>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sàng tuyến tính

<!-- thinking:start -->

> **Tư duy**
>
> Tìm cặp số nguyên tố kề nhau trong $[\textit{left},\textit{right}]$ có khoảng cách nhỏ nhất. Vì $\textit{right}\le 10^6$, việc thử chia từng số trong khoảng sẽ lặp lại rất nhiều công việc.
>
> Sàng tuyến tính tạo ra mọi số nguyên tố không vượt quá $\textit{right}$. Giữ lại các số nằm trong khoảng rồi duyệt các khoảng cách giữa những số liên tiếp. Nếu có ít hơn hai số nguyên tố thì trả về $[-1,-1]$.

<!-- thinking:end -->

Với khoảng $[\textit{left}, \textit{right}]$, ta có thể dùng phương pháp sàng tuyến tính để tìm tất cả các số nguyên tố. Sau đó, duyệt các số nguyên tố theo thứ tự tăng dần để tìm cặp số nguyên tố kề nhau có hiệu nhỏ nhất; đó sẽ là đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n = \textit{right}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def closestPrimes(self, left: int, right: int) -> List[int]:
        cnt = 0
        st = [False] * (right + 1)
        prime = [0] * (right + 1)
        for i in range(2, right + 1):
            if not st[i]:
                prime[cnt] = i
                cnt += 1
            j = 0
            while prime[j] <= right // i:
                st[prime[j] * i] = 1
                if i % prime[j] == 0:
                    break
                j += 1
        p = [v for v in prime[:cnt] if left <= v <= right]
        mi = inf
        ans = [-1, -1]
        for a, b in pairwise(p):
            if (d := b - a) < mi:
                mi = d
                ans = [a, b]
        return ans
```

#### Java

```java
class Solution {
    public int[] closestPrimes(int left, int right) {
        int cnt = 0;
        boolean[] st = new boolean[right + 1];
        int[] prime = new int[right + 1];
        for (int i = 2; i <= right; ++i) {
            if (!st[i]) {
                prime[cnt++] = i;
            }
            for (int j = 0; prime[j] <= right / i; ++j) {
                st[prime[j] * i] = true;
                if (i % prime[j] == 0) {
                    break;
                }
            }
        }
        int i = -1, j = -1;
        for (int k = 0; k < cnt; ++k) {
            if (prime[k] >= left && prime[k] <= right) {
                if (i == -1) {
                    i = k;
                }
                j = k;
            }
        }
        int[] ans = new int[] {-1, -1};
        if (i == j || i == -1) {
            return ans;
        }
        int mi = 1 << 30;
        for (int k = i; k < j; ++k) {
            int d = prime[k + 1] - prime[k];
            if (d < mi) {
                mi = d;
                ans[0] = prime[k];
                ans[1] = prime[k + 1];
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
    vector<int> closestPrimes(int left, int right) {
        int cnt = 0;
        bool st[right + 1];
        memset(st, 0, sizeof st);
        int prime[right + 1];
        for (int i = 2; i <= right; ++i) {
            if (!st[i]) {
                prime[cnt++] = i;
            }
            for (int j = 0; prime[j] <= right / i; ++j) {
                st[prime[j] * i] = true;
                if (i % prime[j] == 0) {
                    break;
                }
            }
        }
        int i = -1, j = -1;
        for (int k = 0; k < cnt; ++k) {
            if (prime[k] >= left && prime[k] <= right) {
                if (i == -1) {
                    i = k;
                }
                j = k;
            }
        }
        vector<int> ans = {-1, -1};
        if (i == j || i == -1) return ans;
        int mi = 1 << 30;
        for (int k = i; k < j; ++k) {
            int d = prime[k + 1] - prime[k];
            if (d < mi) {
                mi = d;
                ans[0] = prime[k];
                ans[1] = prime[k + 1];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func closestPrimes(left int, right int) []int {
	cnt := 0
	st := make([]bool, right+1)
	prime := make([]int, right+1)
	for i := 2; i <= right; i++ {
		if !st[i] {
			prime[cnt] = i
			cnt++
		}
		for j := 0; prime[j] <= right/i; j++ {
			st[prime[j]*i] = true
			if i%prime[j] == 0 {
				break
			}
		}
	}
	i, j := -1, -1
	for k := 0; k < cnt; k++ {
		if prime[k] >= left && prime[k] <= right {
			if i == -1 {
				i = k
			}
			j = k
		}
	}
	ans := []int{-1, -1}
	if i == j || i == -1 {
		return ans
	}
	mi := 1 << 30
	for k := i; k < j; k++ {
		d := prime[k+1] - prime[k]
		if d < mi {
			mi = d
			ans[0], ans[1] = prime[k], prime[k+1]
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
