---
comments: true
difficulty: Medium
rating: 1364
source: Weekly Contest 291 Q2
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [2260. Minimum Consecutive Cards to Pick Up](https://leetcode.com/problems/minimum-consecutive-cards-to-pick-up)

[中文文档](/solution/2200-2299/2260.Minimum%20Consecutive%20Cards%20to%20Pick%20Up/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>cards</code>, trong đó <code>cards[i]</code> biểu diễn <strong>giá trị</strong> của lá bài thứ <code>i<sup>th</sup></code>. Một cặp lá bài là <strong>trùng nhau</strong> nếu chúng có <strong>cùng</strong> giá trị.</p>

<p>Trả về <em>số lượng <strong>nhỏ nhất</strong> các lá bài <strong>liên tiếp</strong> cần rút để trong số các lá bài đã rút có một cặp lá bài <strong>trùng nhau</strong>.</em> Nếu không thể có các lá bài trùng nhau, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> cards = [3,4,2,3,4,7]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta có thể rút các lá bài [3,4,2,3], trong đó có một cặp lá bài trùng nhau với giá trị 3. Lưu ý rằng rút các lá bài [4,2,3,4] cũng là phương án tối ưu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> cards = [1,0,5,3]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có cách nào rút một tập hợp các lá bài liên tiếp chứa một cặp lá bài trùng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= cards.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= cards[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm mảng con ngắn nhất chứa một giá trị xuất hiện hai lần. Với $n \le 10^5$, không thể kiểm tra mọi đoạn. Đoạn ngắn nhất như vậy luôn là khoảng cách giữa hai lần xuất hiện liên tiếp của cùng một giá trị.
>
> Một map lưu chỉ số cuối cùng của mỗi giá trị; khi gặp một giá trị lặp lại, ta cập nhật đáp án bằng $i-\textit{last}[x]+1$. Nếu không có giá trị nào như vậy, trả về $-1$.

<!-- thinking:end -->

Ta khởi tạo đáp án bằng $+\infty$. Ta duyệt mảng, với mỗi số $x$, nếu $\textit{last}[x]$ tồn tại thì nghĩa là $x$ tạo thành một cặp lá bài trùng nhau. Khi đó, ta cập nhật đáp án thành $\textit{ans} = \min(\textit{ans}, i - \textit{last}[x] + 1)$. Cuối cùng, nếu đáp án là $+\infty$, ta trả về $-1$; nếu không, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCardPickup(self, cards: List[int]) -> int:
        last = {}
        ans = inf
        for i, x in enumerate(cards):
            if x in last:
                ans = min(ans, i - last[x] + 1)
            last[x] = i
        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    public int minimumCardPickup(int[] cards) {
        Map<Integer, Integer> last = new HashMap<>();
        int n = cards.length;
        int ans = n + 1;
        for (int i = 0; i < n; ++i) {
            if (last.containsKey(cards[i])) {
                ans = Math.min(ans, i - last.get(cards[i]) + 1);
            }
            last.put(cards[i], i);
        }
        return ans > n ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumCardPickup(vector<int>& cards) {
        unordered_map<int, int> last;
        int n = cards.size();
        int ans = n + 1;
        for (int i = 0; i < n; ++i) {
            if (last.count(cards[i])) {
                ans = min(ans, i - last[cards[i]] + 1);
            }
            last[cards[i]] = i;
        }
        return ans > n ? -1 : ans;
    }
};
```

#### Go

```go
func minimumCardPickup(cards []int) int {
	last := map[int]int{}
	n := len(cards)
	ans := n + 1
	for i, x := range cards {
		if j, ok := last[x]; ok && ans > i-j+1 {
			ans = i - j + 1
		}
		last[x] = i
	}
	if ans > n {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minimumCardPickup(cards: number[]): number {
    const n = cards.length;
    const last = new Map<number, number>();
    let ans = n + 1;
    for (let i = 0; i < n; ++i) {
        if (last.has(cards[i])) {
            ans = Math.min(ans, i - last.get(cards[i]) + 1);
        }
        last.set(cards[i], i);
    }
    return ans > n ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
