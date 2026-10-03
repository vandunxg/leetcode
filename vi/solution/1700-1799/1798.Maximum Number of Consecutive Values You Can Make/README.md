---
comments: true
difficulty: Medium
rating: 1931
source: Biweekly Contest 48 Q3
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1798. Maximum Number of Consecutive Values You Can Make](https://leetcode.com/problems/maximum-number-of-consecutive-values-you-can-make)

[中文文档](/solution/1700-1799/1798.Maximum%20Number%20of%20Consecutive%20Values%20You%20Can%20Make/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>coins</code> độ dài <code>n</code>, biểu diễn <code>n</code> đồng xu bạn có. Giá trị của đồng xu thứ <code>i<sup>th</sup></code> là <code>coins[i]</code>. Bạn có thể <strong>tạo</strong> giá trị <code>x</code> nếu chọn được một số trong <code>n</code> đồng xu sao cho tổng giá trị của chúng bằng <code>x</code>.</p>

<p>Trả về <em>số lượng lớn nhất các giá trị nguyên liên tiếp mà bạn <strong>có thể</strong> <strong>tạo</strong> bằng các đồng xu, <strong>bắt đầu</strong> từ và <strong>bao gồm</strong> </em><code>0</code>.</p>

<p>Lưu ý rằng bạn có thể có nhiều đồng xu cùng giá trị.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> coins = [1,3]
<strong>Output:</strong> 2
<strong>Giải thích: </strong>Bạn có thể tạo các giá trị sau:
- 0: lấy []
- 1: lấy [1]
Bạn có thể tạo 2 giá trị nguyên liên tiếp bắt đầu từ 0.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> coins = [1,1,1,4]
<strong>Output:</strong> 8
<strong>Giải thích: </strong>Bạn có thể tạo các giá trị sau:
- 0: lấy []
- 1: lấy [1]
- 2: lấy [1,1]
- 3: lấy [1,1,1]
- 4: lấy [4]
- 5: lấy [4,1]
- 6: lấy [4,1,1]
- 7: lấy [4,1,1,1]
Bạn có thể tạo 8 giá trị nguyên liên tiếp bắt đầu từ 0.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> coins = [1,4,10,3,1]
<strong>Output:</strong> 20</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>coins.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 4 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= coins[i] &lt;= 4 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Các tổng của tập con đồng xu cần phủ một đoạn đầu của các số nguyên không âm. Nếu $[0,ans)$ đã tạo được, một đồng xu $v\le ans$ sẽ mở rộng đoạn thành $[0,ans+v)$.
>
> Sắp xếp các đồng xu rồi xử lý từ nhỏ đến lớn; dừng ở $v>ans$ đầu tiên vì khi đó sẽ xuất hiện khoảng trống. $ans$ cuối cùng là độ dài đoạn đầu tạo được.

<!-- thinking:end -->

Trước hết, ta sắp xếp mảng. Sau đó định nghĩa $ans$ là số lượng số nguyên liên tiếp hiện có thể tạo được, ban đầu bằng $1$.

Ta duyệt mảng. Với phần tử hiện tại $v$, nếu $v > ans$, ta không thể tạo $ans+1$ số nguyên liên tiếp nên dừng vòng lặp và trả về $ans$. Ngược lại, ta có thể tạo $ans+v$ số nguyên liên tiếp nên cập nhật $ans$ thành $ans+v$.

Cuối cùng, ta trả về $ans$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMaximumConsecutive(self, coins: List[int]) -> int:
        ans = 1
        for v in sorted(coins):
            if v > ans:
                break
            ans += v
        return ans
```

#### Java

```java
class Solution {
    public int getMaximumConsecutive(int[] coins) {
        Arrays.sort(coins);
        int ans = 1;
        for (int v : coins) {
            if (v > ans) {
                break;
            }
            ans += v;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getMaximumConsecutive(vector<int>& coins) {
        sort(coins.begin(), coins.end());
        int ans = 1;
        for (int& v : coins) {
            if (v > ans) break;
            ans += v;
        }
        return ans;
    }
};
```

#### Go

```go
func getMaximumConsecutive(coins []int) int {
	sort.Ints(coins)
	ans := 1
	for _, v := range coins {
		if v > ans {
			break
		}
		ans += v
	}
	return ans
}
```

#### TypeScript

```ts
function getMaximumConsecutive(coins: number[]): number {
    coins.sort((a, b) => a - b);
    let ans = 1;
    for (const v of coins) {
        if (v > ans) {
            break;
        }
        ans += v;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
