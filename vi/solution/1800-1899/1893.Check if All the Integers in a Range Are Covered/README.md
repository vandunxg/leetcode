---
comments: true
difficulty: Easy
rating: 1307
source: Biweekly Contest 54 Q1
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [1893. Check if All the Integers in a Range Are Covered](https://leetcode.com/problems/check-if-all-the-integers-in-a-range-are-covered)

[中文文档](/solution/1800-1899/1893.Check%20if%20All%20the%20Integers%20in%20a%20Range%20Are%20Covered/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2 chiều <code>ranges</code> và hai số nguyên <code>left</code>, <code>right</code>. Mỗi <code>ranges[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> biểu diễn một khoảng <strong>bao gồm cả hai đầu mút</strong> từ <code>start<sub>i</sub></code> đến <code>end<sub>i</sub></code>.</p>

<p>Trả về <code>true</code> <em>nếu mọi số nguyên trong khoảng bao gồm cả hai đầu mút</em> <code>[left, right]</code> <em>được phủ bởi <strong>ít nhất một</strong> khoảng trong</em> <code>ranges</code>. <em>Ngược lại</em>, trả về <code>false</code>.</p>

<p>Một số nguyên <code>x</code> được phủ bởi khoảng <code>ranges[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> nếu <code>start<sub>i</sub> &lt;= x &lt;= end<sub>i</sub></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> ranges = [[1,2],[3,4],[5,6]], left = 2, right = 5
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Mọi số nguyên từ 2 đến 5 đều được phủ:
- 2 được phủ bởi khoảng đầu tiên.
- 3 và 4 được phủ bởi khoảng thứ hai.
- 5 được phủ bởi khoảng thứ ba.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> ranges = [[1,10],[10,20]], left = 21, right = 21
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> 21 không được phủ bởi bất kỳ khoảng nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= ranges.length &lt;= 50</code></li>
	<li><code>1 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= 50</code></li>
	<li><code>1 &lt;= left &lt;= right &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần quyết định mọi số nguyên trong $[left,right]$ có nằm trong một khoảng đã cho hay không. Miền giá trị nhiều nhất chỉ là $50$, nên một mảng hiệu là đủ.
>
> Cộng $1$ tại đầu trái và $-1$ ngay sau đầu phải của mỗi khoảng. Tổng tiền tố chính là độ phủ tại vị trí đó; nếu có giá trị bằng 0 trong $[left,right]$ thì toàn bộ khoảng chưa được phủ.

<!-- thinking:end -->

Ta có thể dùng ý tưởng mảng hiệu để tạo một mảng hiệu $\textit{diff}$ có độ dài $52$.

Tiếp theo, ta duyệt mảng $\textit{ranges}$. Với mỗi khoảng $[l, r]$, ta tăng $\textit{diff}[l]$ thêm $1$ và giảm $\textit{diff}[r + 1]$ đi $1$.

Sau đó, ta duyệt mảng hiệu $\textit{diff}$, duy trì một tổng tiền tố $s$. Với mỗi vị trí $i$, ta cộng $\textit{diff}[i]$ vào $s$. Nếu $s \le 0$ và $left \le i \le right$, điều đó cho biết số nguyên $i$ trong khoảng $[left, right]$ chưa được phủ, nên ta trả về $\textit{false}$.

Nếu duyệt xong mảng hiệu $\textit{diff}$ mà không trả về $\textit{false}$, nghĩa là mọi số nguyên trong khoảng $[left, right]$ đều được phủ bởi ít nhất một khoảng trong $\textit{ranges}$, nên ta trả về $\textit{true}$.

Độ phức tạp thời gian là $O(n + M)$, còn độ phức tạp không gian là $O(M)$. Ở đây, $n$ là độ dài của mảng $\textit{ranges}$ và $M$ là giá trị lớn nhất của khoảng, trong bài này $M \le 50$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isCovered(self, ranges: List[List[int]], left: int, right: int) -> bool:
        diff = [0] * 52
        for l, r in ranges:
            diff[l] += 1
            diff[r + 1] -= 1
        s = 0
        for i, x in enumerate(diff):
            s += x
            if s <= 0 and left <= i <= right:
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean isCovered(int[][] ranges, int left, int right) {
        int[] diff = new int[52];
        for (int[] range : ranges) {
            int l = range[0], r = range[1];
            ++diff[l];
            --diff[r + 1];
        }
        int s = 0;
        for (int i = 0; i < diff.length; ++i) {
            s += diff[i];
            if (s <= 0 && left <= i && i <= right) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isCovered(vector<vector<int>>& ranges, int left, int right) {
        vector<int> diff(52);
        for (auto& range : ranges) {
            int l = range[0], r = range[1];
            ++diff[l];
            --diff[r + 1];
        }
        int s = 0;
        for (int i = 0; i < diff.size(); ++i) {
            s += diff[i];
            if (s <= 0 && left <= i && i <= right) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isCovered(ranges [][]int, left int, right int) bool {
	diff := [52]int{}
	for _, e := range ranges {
		l, r := e[0], e[1]
		diff[l]++
		diff[r+1]--
	}
	s := 0
	for i, x := range diff {
		s += x
		if s <= 0 && left <= i && i <= right {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function isCovered(ranges: number[][], left: number, right: number): boolean {
    const diff: number[] = Array(52).fill(0);
    for (const [l, r] of ranges) {
        ++diff[l];
        --diff[r + 1];
    }
    let s = 0;
    for (let i = 0; i < diff.length; ++i) {
        s += diff[i];
        if (s <= 0 && left <= i && i <= right) {
            return false;
        }
    }
    return true;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} ranges
 * @param {number} left
 * @param {number} right
 * @return {boolean}
 */
var isCovered = function (ranges, left, right) {
    const diff = Array(52).fill(0);
    for (const [l, r] of ranges) {
        ++diff[l];
        --diff[r + 1];
    }
    let s = 0;
    for (let i = 0; i < diff.length; ++i) {
        s += diff[i];
        if (s <= 0 && left <= i && i <= right) {
            return false;
        }
    }
    return true;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
