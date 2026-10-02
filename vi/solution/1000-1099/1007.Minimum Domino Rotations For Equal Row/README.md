---
comments: true
difficulty: Medium
rating: 1541
source: Weekly Contest 127 Q3
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [1007. Minimum Domino Rotations For Equal Row](https://leetcode.com/problems/minimum-domino-rotations-for-equal-row)

[中文文档](/solution/1000-1099/1007.Minimum%20Domino%20Rotations%20For%20Equal%20Row/README.md)

## Mô tả

<!-- description:start -->

<p>Trong một hàng domino, <code>tops[i]</code> và <code>bottoms[i]</code> lần lượt biểu thị nửa trên và nửa dưới của quân domino thứ <code>i</code>. (Mỗi quân domino có hai số từ 1 đến 6, mỗi số nằm ở một nửa.)</p>

<p>Ta có thể xoay quân domino thứ <code>i</code> để hoán đổi giá trị <code>tops[i]</code> và <code>bottoms[i]</code>.</p>

<p>Hãy trả về số lần xoay ít nhất để tất cả giá trị trong <code>tops</code> giống nhau hoặc tất cả giá trị trong <code>bottoms</code> giống nhau.</p>

<p>Nếu không thể thực hiện, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1000-1099/1007.Minimum%20Domino%20Rotations%20For%20Equal%20Row/images/domino.png" style="height: 300px; width: 421px;" />
<pre>
<strong>Đầu vào:</strong> tops = [2,1,2,4,2,2], bottoms = [5,2,6,2,3,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 
Hình đầu tiên mô tả trạng thái các quân domino theo tops và bottoms trước khi xoay.
Nếu xoay quân domino thứ hai và thứ tư, ta có thể làm cho mọi giá trị ở hàng trên đều bằng 2, như hình thứ hai minh họa.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> tops = [3,5,1,2,3], bottoms = [3,6,3,3,4]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> 
Trong trường hợp này, không thể xoay các quân domino để làm cho các giá trị trong một hàng bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= tops.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>bottoms.length == tops.length</code></li>
	<li><code>1 &lt;= tops[i], bottoms[i] &lt;= 6</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Mặt domino chỉ có giá trị từ $1$ đến $6$, nên thử từng giá trị ứng viên trên cả hai hàng mất thời gian tuyến tính theo $n\le 2\times 10^4$. Phần lớn ứng viên bị loại vì có cột mà cả hai mặt đều không mang giá trị đó.
>
> Nếu tồn tại giá trị chung, nó phải là $tops[0]$ hoặc $bottoms[0]$; nếu không, cột đầu tiên không thể có giá trị đó.
>
> Ta tính $f(x)$ cho hai ứng viên này: nếu có cột không chứa $x$ thì không thể tạo hàng đồng nhất; ngược lại, số lần xoay bằng $n$ trừ đi số lần xuất hiện lớn hơn trong hai hàng. Đáp án là giá trị nhỏ hơn trong hai kết quả.

<!-- thinking:end -->

Theo đề bài, để làm cho tất cả giá trị trong $tops$ hoặc $bottoms$ giống nhau, giá trị đó phải là $tops[0]$ hoặc $bottoms[0]$.

Do đó, ta định nghĩa hàm $f(x)$ biểu thị số lần xoay ít nhất cần thiết để đưa mọi giá trị về $x$. Đáp án là $\min\{f(\textit{tops}[0]), f(\textit{bottoms}[0])\}$.

Cách tính hàm $f(x)$ như sau:

Ta dùng hai biến $cnt1$ và $cnt2$ để đếm số lần $x$ xuất hiện lần lượt trong $tops$ và $bottoms$. Lấy $n$ trừ đi giá trị lớn hơn giữa $cnt1$ và $cnt2$ sẽ cho số lần xoay ít nhất cần thiết để mọi giá trị bằng $x$. Lưu ý, nếu không có giá trị nào bằng $x$ trong cả $tops$ lẫn $bottoms$, ta gán $f(x)$ bằng một số rất lớn, ở đây là $n+1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDominoRotations(self, tops: List[int], bottoms: List[int]) -> int:
        def f(x: int) -> int:
            cnt1 = cnt2 = 0
            for a, b in zip(tops, bottoms):
                if x not in (a, b):
                    return inf
                cnt1 += a == x
                cnt2 += b == x
            return len(tops) - max(cnt1, cnt2)

        ans = min(f(tops[0]), f(bottoms[0]))
        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    private int n;
    private int[] tops;
    private int[] bottoms;

    public int minDominoRotations(int[] tops, int[] bottoms) {
        n = tops.length;
        this.tops = tops;
        this.bottoms = bottoms;
        int ans = Math.min(f(tops[0]), f(bottoms[0]));
        return ans > n ? -1 : ans;
    }

    private int f(int x) {
        int cnt1 = 0, cnt2 = 0;
        for (int i = 0; i < n; ++i) {
            if (tops[i] != x && bottoms[i] != x) {
                return n + 1;
            }
            cnt1 += tops[i] == x ? 1 : 0;
            cnt2 += bottoms[i] == x ? 1 : 0;
        }
        return n - Math.max(cnt1, cnt2);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minDominoRotations(vector<int>& tops, vector<int>& bottoms) {
        int n = tops.size();
        auto f = [&](int x) {
            int cnt1 = 0, cnt2 = 0;
            for (int i = 0; i < n; ++i) {
                if (tops[i] != x && bottoms[i] != x) {
                    return n + 1;
                }
                cnt1 += tops[i] == x;
                cnt2 += bottoms[i] == x;
            }
            return n - max(cnt1, cnt2);
        };
        int ans = min(f(tops[0]), f(bottoms[0]));
        return ans > n ? -1 : ans;
    }
};
```

#### Go

```go
func minDominoRotations(tops []int, bottoms []int) int {
	n := len(tops)
	f := func(x int) int {
		cnt1, cnt2 := 0, 0
		for i, a := range tops {
			b := bottoms[i]
			if a != x && b != x {
				return n + 1
			}
			if a == x {
				cnt1++
			}
			if b == x {
				cnt2++
			}
		}
		return n - max(cnt1, cnt2)
	}
	ans := min(f(tops[0]), f(bottoms[0]))
	if ans > n {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minDominoRotations(tops: number[], bottoms: number[]): number {
    const n = tops.length;
    const f = (x: number): number => {
        let [cnt1, cnt2] = [0, 0];
        for (let i = 0; i < n; ++i) {
            if (tops[i] !== x && bottoms[i] !== x) {
                return n + 1;
            }
            cnt1 += tops[i] === x ? 1 : 0;
            cnt2 += bottoms[i] === x ? 1 : 0;
        }
        return n - Math.max(cnt1, cnt2);
    };
    const ans = Math.min(f(tops[0]), f(bottoms[0]));
    return ans > n ? -1 : ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_domino_rotations(tops: Vec<i32>, bottoms: Vec<i32>) -> i32 {
        let n = tops.len() as i32;
        let f = |x: i32| -> i32 {
            let mut cnt1 = 0;
            let mut cnt2 = 0;
            for i in 0..n as usize {
                if tops[i] != x && bottoms[i] != x {
                    return n + 1;
                }
                if tops[i] == x {
                    cnt1 += 1;
                }
                if bottoms[i] == x {
                    cnt2 += 1;
                }
            }
            n - cnt1.max(cnt2)
        };

        let ans = f(tops[0]).min(f(bottoms[0]));
        if ans > n { -1 } else { ans }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
