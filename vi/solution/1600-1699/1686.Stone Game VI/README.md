---
comments: true
difficulty: Medium
rating: 2000
source: Biweekly Contest 41 Q3
tags:
    - Greedy
    - Minimax
    - Array
    - Math
    - Game Theory
    - Sorting
    - Heap (Priority Queue)
    - Zero-Sum Game
---

<!-- problem:start -->

# [1686. Stone Game VI](https://leetcode.com/problems/stone-game-vi)

[中文文档](/solution/1600-1699/1686.Stone%20Game%20VI/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob lần lượt chơi một trò chơi, Alice đi trước.</p>

<p>Có <code>n</code> viên đá trong một đống. Mỗi lượt, người chơi có thể <strong>lấy</strong> một viên đá khỏi đống và nhận điểm dựa trên giá trị viên đá. Alice và Bob có thể <strong>đánh giá các viên đá khác nhau</strong>.</p>

<p>Cho hai mảng số nguyên độ dài <code>n</code>, <code>aliceValues</code> và <code>bobValues</code>. <code>aliceValues[i]</code> và <code>bobValues[i]</code> lần lượt là giá trị viên đá thứ <code>i<sup>th</sup></code> theo đánh giá của Alice và Bob.</p>

<p>Người có nhiều điểm nhất sau khi chọn hết các viên đá là người thắng. Nếu hai người có cùng số điểm, trò chơi hòa. Cả hai sẽ chơi <strong>tối ưu</strong>.&nbsp;Mỗi người đều biết giá trị của đối phương.</p>

<p>Hãy xác định kết quả trò chơi:</p>

<ul>
<li>Nếu Alice thắng, trả về <code>1</code>.</li>
<li>Nếu Bob thắng, trả về <code>-1</code>.</li>
<li>Nếu trò chơi hòa, trả về <code>0</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> aliceValues = [1,3], bobValues = [2,1]
<strong>Output:</strong> 1
<strong>Giải thích:</strong>
Nếu Alice lấy viên đá 1 (đánh chỉ số từ 0) trước, Alice nhận 3 điểm.
Bob chỉ có thể chọn viên đá 0 và nhận 2 điểm.
Alice thắng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> aliceValues = [1,2], bobValues = [3,1]
<strong>Output:</strong> 0
<strong>Giải thích:</strong>
Nếu Alice lấy viên đá 0 và Bob lấy viên đá 1, cả hai đều có 1 điểm.
Hòa.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> aliceValues = [2,4,3], bobValues = [1,6,7]
<strong>Output:</strong> -1
<strong>Giải thích:</strong>
Bất kể Alice chơi thế nào, Bob vẫn có thể đạt nhiều điểm hơn Alice.
Ví dụ, nếu Alice lấy viên đá 1, Bob có thể lấy viên đá 2, rồi Alice lấy viên đá 0, Alice có 6 điểm còn Bob có 7 điểm.
Bob thắng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == aliceValues.length == bobValues.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= aliceValues[i], bobValues[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Người chơi lần lượt lấy đá, vừa nhận giá trị của mình vừa ngăn đối phương nhận giá trị của họ. Khi so sánh cần tính cả “ta được $a_i$” và “đối phương mất $b_i$”, nên chọn các viên đá theo thứ tự giảm dần của $a_i+b_i$.
>
> Sau khi sắp xếp, Alice lấy các vị trí chẵn và Bob lấy các vị trí lẻ; so sánh hai điểm số để trả về $1$, $0$ hoặc $-1$.

<!-- thinking:end -->

Chiến lược tối ưu là vừa tối đa hóa điểm của mình vừa khiến đối phương mất nhiều điểm nhất có thể. Vì vậy, ta tạo mảng $vals$, trong đó $vals[i] = (aliceValues[i] + bobValues[i], i)$ biểu diễn tổng giá trị và chỉ số của viên đá thứ $i$. Sau đó sắp xếp $vals$ theo tổng giá trị giảm dần.

Tiếp theo, Alice và Bob lần lượt chọn đá theo thứ tự của $vals$. Alice chọn các viên đá ở vị trí chẵn trong $vals$, còn Bob chọn các viên đá ở vị trí lẻ trong $vals$. Cuối cùng, so sánh điểm của Alice và Bob rồi trả về kết quả tương ứng.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài các mảng `aliceValues` và `bobValues`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def stoneGameVI(self, aliceValues: List[int], bobValues: List[int]) -> int:
        vals = [(a + b, i) for i, (a, b) in enumerate(zip(aliceValues, bobValues))]
        vals.sort(reverse=True)
        a = sum(aliceValues[i] for _, i in vals[::2])
        b = sum(bobValues[i] for _, i in vals[1::2])
        if a > b:
            return 1
        if a < b:
            return -1
        return 0
```

#### Java

```java
class Solution {
    public int stoneGameVI(int[] aliceValues, int[] bobValues) {
        int n = aliceValues.length;
        int[][] vals = new int[n][0];
        for (int i = 0; i < n; ++i) {
            vals[i] = new int[] {aliceValues[i] + bobValues[i], i};
        }
        Arrays.sort(vals, (a, b) -> b[0] - a[0]);
        int a = 0, b = 0;
        for (int k = 0; k < n; ++k) {
            int i = vals[k][1];
            if (k % 2 == 0) {
                a += aliceValues[i];
            } else {
                b += bobValues[i];
            }
        }
        if (a == b) {
            return 0;
        }
        return a > b ? 1 : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int stoneGameVI(vector<int>& aliceValues, vector<int>& bobValues) {
        vector<pair<int, int>> vals;
        int n = aliceValues.size();
        for (int i = 0; i < n; ++i) {
            vals.emplace_back(aliceValues[i] + bobValues[i], i);
        }
        sort(vals.rbegin(), vals.rend());
        int a = 0, b = 0;
        for (int k = 0; k < n; ++k) {
            int i = vals[k].second;
            if (k % 2 == 0) {
                a += aliceValues[i];
            } else {
                b += bobValues[i];
            }
        }
        if (a == b) {
            return 0;
        }
        return a > b ? 1 : -1;
    }
};
```

#### Go

```go
func stoneGameVI(aliceValues []int, bobValues []int) int {
	vals := make([][2]int, len(aliceValues))
	for i, a := range aliceValues {
		vals[i] = [2]int{a + bobValues[i], i}
	}
	slices.SortFunc(vals, func(a, b [2]int) int { return b[0] - a[0] })
	a, b := 0, 0
	for k, v := range vals {
		i := v[1]
		if k%2 == 0 {
			a += aliceValues[i]
		} else {
			b += bobValues[i]
		}
	}
	if a > b {
		return 1
	}
	if a < b {
		return -1
	}
	return 0
}
```

#### TypeScript

```ts
function stoneGameVI(aliceValues: number[], bobValues: number[]): number {
    const n = aliceValues.length;
    const vals: number[][] = [];
    for (let i = 0; i < n; ++i) {
        vals.push([aliceValues[i] + bobValues[i], i]);
    }
    vals.sort((a, b) => b[0] - a[0]);
    let [a, b] = [0, 0];
    for (let k = 0; k < n; ++k) {
        const i = vals[k][1];
        if (k % 2 == 0) {
            a += aliceValues[i];
        } else {
            b += bobValues[i];
        }
    }
    if (a === b) {
        return 0;
    }
    return a > b ? 1 : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
