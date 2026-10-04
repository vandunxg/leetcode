---
comments: true
difficulty: Medium
rating: 1450
source: Weekly Contest 375 Q2
tags:
    - Array
    - Math
    - Simulation
---

<!-- problem:start -->

# [2961. Double Modular Exponentiation](https://leetcode.com/problems/double-modular-exponentiation)

[中文文档](/solution/2900-2999/2961.Double%20Modular%20Exponentiation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng 2 chiều <code>variables</code> được đánh chỉ số từ <strong>0</strong>, trong đó <code>variables[i] = [a<sub>i</sub>, b<sub>i</sub>, c<sub>i,</sub> m<sub>i</sub>]</code>, và một số nguyên <code>target</code>.</p>

<p>Chỉ số <code>i</code> được gọi là <strong>good</strong> nếu thỏa mãn công thức sau:</p>

<ul>
	<li><code>0 &lt;= i &lt; variables.length</code></li>
	<li><code>((a<sub>i</sub><sup>b<sub>i</sub></sup> % 10)<sup>c<sub>i</sub></sup>) % m<sub>i</sub> == target</code></li>
</ul>

<p>Trả về <em>một mảng gồm các chỉ số <strong>good</strong> theo <strong>bất kỳ thứ tự nào</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> variables = [[2,3,3,10],[3,3,3,1],[6,1,1,4]], target = 2
<strong>Đầu ra:</strong> [0,2]
<strong>Giải thích:</strong> Với mỗi chỉ số i trong mảng variables:
1) Với chỉ số 0, variables[0] = [2,3,3,10], (2<sup>3</sup> % 10)<sup>3</sup> % 10 = 2.
2) Với chỉ số 1, variables[1] = [3,3,3,1], (3<sup>3</sup> % 10)<sup>3</sup> % 1 = 0.
3) Với chỉ số 2, variables[2] = [6,1,1,4], (6<sup>1</sup> % 10)<sup>1</sup> % 4 = 2.
Vì vậy, ta trả về [0,2] làm đáp án.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> variables = [[39,3,1000,1000]], target = 17
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Với mỗi chỉ số i trong mảng variables:
1) Với chỉ số 0, variables[0] = [39,3,1000,1000], (39<sup>3</sup> % 10)<sup>1000</sup> % 1000 = 1.
Vì vậy, ta trả về [] làm đáp án.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= variables.length &lt;= 100</code></li>
	<li><code>variables[i] == [a<sub>i</sub>, b<sub>i</sub>, c<sub>i</sub>, m<sub>i</sub>]</code></li>
	<li><code>1 &lt;= a<sub>i</sub>, b<sub>i</sub>, c<sub>i</sub>, m<sub>i</sub> &lt;= 10<sup>3</sup></code></li>
	<li><code><font face="monospace">0 &lt;= target &lt;= 10<sup>3</sup></font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng + Lũy thừa nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra xem $(a^b \bmod 10)^c \bmod m$ có bằng $target$ hay không. Số mũ và modulo đều không vượt quá $10^3$, nên chỉ cần tính hai lũy thừa modulo.
>
> Duyệt qua $variables$ và thu thập các chỉ số khớp.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp theo mô tả bài toán. Để tính lũy thừa modulo, ta có thể sử dụng phương pháp lũy thừa nhanh nhằm tăng tốc độ tính toán.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ là độ dài của mảng $variables$; còn $M$ là giá trị lớn nhất trong $b_i$ và $c_i$, với bài toán này $M \le 10^3$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getGoodIndices(self, variables: List[List[int]], target: int) -> List[int]:
        return [
            i
            for i, (a, b, c, m) in enumerate(variables)
            if pow(pow(a, b, 10), c, m) == target
        ]
```

#### Java

```java
class Solution {
    public List<Integer> getGoodIndices(int[][] variables, int target) {
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < variables.length; ++i) {
            var e = variables[i];
            int a = e[0], b = e[1], c = e[2], m = e[3];
            if (qpow(qpow(a, b, 10), c, m) == target) {
                ans.add(i);
            }
        }
        return ans;
    }

    private int qpow(long a, int n, int mod) {
        long ans = 1;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ans = ans * a % mod;
            }
            a = a * a % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getGoodIndices(vector<vector<int>>& variables, int target) {
        vector<int> ans;
        auto qpow = [&](long long a, int n, int mod) {
            long long ans = 1;
            for (; n; n >>= 1) {
                if (n & 1) {
                    ans = ans * a % mod;
                }
                a = a * a % mod;
            }
            return (int) ans;
        };
        for (int i = 0; i < variables.size(); ++i) {
            auto e = variables[i];
            int a = e[0], b = e[1], c = e[2], m = e[3];
            if (qpow(qpow(a, b, 10), c, m) == target) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getGoodIndices(variables [][]int, target int) (ans []int) {
	qpow := func(a, n, mod int) int {
		ans := 1
		for ; n > 0; n >>= 1 {
			if n&1 == 1 {
				ans = ans * a % mod
			}
			a = a * a % mod
		}
		return ans
	}
	for i, e := range variables {
		a, b, c, m := e[0], e[1], e[2], e[3]
		if qpow(qpow(a, b, 10), c, m) == target {
			ans = append(ans, i)
		}
	}
	return
}
```

#### TypeScript

```ts
function getGoodIndices(variables: number[][], target: number): number[] {
    const qpow = (a: number, n: number, mod: number) => {
        let ans = 1;
        for (; n; n >>= 1) {
            if (n & 1) {
                ans = Number((BigInt(ans) * BigInt(a)) % BigInt(mod));
            }
            a = Number((BigInt(a) * BigInt(a)) % BigInt(mod));
        }
        return ans;
    };
    const ans: number[] = [];
    for (let i = 0; i < variables.length; ++i) {
        const [a, b, c, m] = variables[i];
        if (qpow(qpow(a, b, 10), c, m) === target) {
            ans.push(i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
