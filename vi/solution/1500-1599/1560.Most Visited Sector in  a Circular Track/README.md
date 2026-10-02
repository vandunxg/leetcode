---
comments: true
difficulty: Easy
rating: 1443
source: Weekly Contest 203 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [1560. Most Visited Sector in a Circular Track](https://leetcode.com/problems/most-visited-sector-in-a-circular-track)

[中文文档](/solution/1500-1599/1560.Most%20Visited%20Sector%20in%20%20a%20Circular%20Track/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> và mảng số nguyên <code>rounds</code>. Có một đường chạy vòng tròn gồm <code>n</code> khu vực được đánh số từ <code>1</code> đến <code>n</code>. Một cuộc marathon được tổ chức trên đường chạy này, gồm <code>m</code> vòng. Vòng thứ <code>i<sup>th</sup></code> bắt đầu tại khu vực <code>rounds[i - 1]</code> và kết thúc tại khu vực <code>rounds[i]</code>. Ví dụ, vòng 1 bắt đầu tại khu vực <code>rounds[0]</code> và kết thúc tại khu vực <code>rounds[1]</code></p>

<p>Trả về <em>mảng các khu vực được đi qua nhiều nhất</em>, được sắp xếp theo thứ tự <strong>tăng dần</strong>.</p>

<p>Lưu ý rằng ta đi trên đường chạy theo thứ tự tăng dần của số khu vực, theo hướng ngược chiều kim đồng hồ (xem ví dụ 1).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1560.Most%20Visited%20Sector%20in%20%20a%20Circular%20Track/images/tmp.jpg" style="width: 433px; height: 341px;" />
<pre>
<strong>Input:</strong> n = 4, rounds = [1,3,1,2]
<strong>Output:</strong> [1,2]
<strong>Giải thích:</strong> Cuộc marathon bắt đầu tại khu vực 1. Thứ tự các khu vực được đi qua là:
1 --&gt; 2 --&gt; 3 (end of round 1) --&gt; 4 --&gt; 1 (end of round 2) --&gt; 2 (end of round 3 and the marathon)
Ta thấy cả khu vực 1 và 2 đều được đi qua hai lần, nên đây là các khu vực được đi qua nhiều nhất. Khu vực 3 và 4 chỉ được đi qua một lần.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 2, rounds = [2,1,2,1,2,1,2,1,2]
<strong>Output:</strong> [2]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> n = 7, rounds = [1,3,5,7]
<strong>Output:</strong> [1,2,3,4,5,6,7]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= m &lt;= 100</code></li>
	<li><code>rounds.length == m + 1</code></li>
	<li><code>1 &lt;= rounds[i] &lt;= n</code></li>
	<li><code>rounds[i] != rounds[i + 1]</code> for <code>0 &lt;= i &lt; m</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Xét mối quan hệ giữa vị trí bắt đầu và kết thúc

<!-- thinking:start -->

> **Tư duy**
>
> Ta đi trên đường chạy vòng tròn theo các đoạn đã cho và cần tìm các khu vực được đi qua nhiều nhất. Các vòng đầy đủ cộng cùng một số lần vào mọi khu vực; chỉ phần đường còn lại từ điểm bắt đầu đầu tiên đến điểm kết thúc cuối cùng tạo ra khác biệt.
>
> Nếu chỉ số bắt đầu không lớn hơn điểm kết thúc, phần còn lại là đoạn đóng $[rounds[0],rounds[-1]]$. Ngược lại, đoạn này đi qua $n$ và là hợp của $[1,rounds[-1]]$ và $[rounds[0],n]$. Liệt kê hợp này giúp tránh mô phỏng từng vòng.

<!-- thinking:end -->

Vì vị trí kết thúc của mỗi chặng là vị trí bắt đầu của chặng tiếp theo, và mỗi chặng đi theo hướng ngược chiều kim đồng hồ, ta có thể xác định số lần đi qua mỗi khu vực dựa trên mối quan hệ giữa vị trí bắt đầu và kết thúc.

Nếu $\textit{rounds}[0] \leq \textit{rounds}[m]$, các khu vực từ $\textit{rounds}[0]$ đến $\textit{rounds}[m]$ được đi qua nhiều nhất, nên ta có thể trả về trực tiếp toàn bộ các khu vực trong đoạn này.

Ngược lại, các khu vực từ $1$ đến $\textit{rounds}[m]$ và từ $\textit{rounds}[0]$ đến $n$ hợp thành các khu vực được đi qua nhiều nhất, nên ta trả về hợp của hai đoạn này.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số khu vực. Không tính không gian của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mostVisited(self, n: int, rounds: List[int]) -> List[int]:
        if rounds[0] <= rounds[-1]:
            return list(range(rounds[0], rounds[-1] + 1))
        return list(range(1, rounds[-1] + 1)) + list(range(rounds[0], n + 1))
```

#### Java

```java
class Solution {
    public List<Integer> mostVisited(int n, int[] rounds) {
        int m = rounds.length - 1;
        List<Integer> ans = new ArrayList<>();
        if (rounds[0] <= rounds[m]) {
            for (int i = rounds[0]; i <= rounds[m]; ++i) {
                ans.add(i);
            }
        } else {
            for (int i = 1; i <= rounds[m]; ++i) {
                ans.add(i);
            }
            for (int i = rounds[0]; i <= n; ++i) {
                ans.add(i);
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
    vector<int> mostVisited(int n, vector<int>& rounds) {
        int m = rounds.size() - 1;
        vector<int> ans;
        if (rounds[0] <= rounds[m]) {
            for (int i = rounds[0]; i <= rounds[m]; ++i) {
                ans.push_back(i);
            }
        } else {
            for (int i = 1; i <= rounds[m]; ++i) {
                ans.push_back(i);
            }
            for (int i = rounds[0]; i <= n; ++i) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func mostVisited(n int, rounds []int) []int {
	m := len(rounds) - 1
	var ans []int
	if rounds[0] <= rounds[m] {
		for i := rounds[0]; i <= rounds[m]; i++ {
			ans = append(ans, i)
		}
	} else {
		for i := 1; i <= rounds[m]; i++ {
			ans = append(ans, i)
		}
		for i := rounds[0]; i <= n; i++ {
			ans = append(ans, i)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function mostVisited(n: number, rounds: number[]): number[] {
    const ans: number[] = [];
    const m = rounds.length - 1;
    if (rounds[0] <= rounds[m]) {
        for (let i = rounds[0]; i <= rounds[m]; ++i) {
            ans.push(i);
        }
    } else {
        for (let i = 1; i <= rounds[m]; ++i) {
            ans.push(i);
        }
        for (let i = rounds[0]; i <= n; ++i) {
            ans.push(i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
