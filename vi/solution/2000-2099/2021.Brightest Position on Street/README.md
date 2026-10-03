---
comments: true
difficulty: Medium
tags:
    - Array
    - Ordered Set
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2021. Brightest Position on Street 🔒](https://leetcode.com/problems/brightest-position-on-street)

[中文文档](/solution/2000-2099/2021.Brightest%20Position%20on%20Street/README.md)

## Mô tả

<!-- description:start -->

<p>Một con đường hoàn toàn thẳng được biểu diễn bằng một trục số. Trên đường có các đèn đường, được biểu diễn bằng một mảng số nguyên 2 chiều <code>lights</code>. Mỗi <code>lights[i] = [position<sub>i</sub>, range<sub>i</sub>]</code> cho biết có một đèn đường tại vị trí <code>position<sub>i</sub></code>, chiếu sáng khu vực từ <code>[position<sub>i</sub> - range<sub>i</sub>, position<sub>i</sub> + range<sub>i</sub>]</code> (<strong>bao gồm cả hai đầu mút</strong>).</p>

<p><strong>Độ sáng</strong> của một vị trí <code>p</code> được định nghĩa là số đèn đường chiếu sáng vị trí <code>p</code>.</p>

<p>Cho <code>lights</code>, hãy trả về <em>vị trí <strong>sáng nhất</strong> trên</em><em> đường. Nếu có nhiều vị trí sáng nhất, trả về vị trí <strong>nhỏ nhất</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2021.Brightest%20Position%20on%20Street/images/image-20210928155140-1.png" style="width: 700px; height: 165px;" />
<pre>
<strong>Đầu vào:</strong> lights = [[-3,2],[1,2],[3,3]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>
Đèn đường thứ nhất chiếu sáng khu vực từ [(-3) - 2, (-3) + 2] = [-5, -1].
Đèn đường thứ hai chiếu sáng khu vực từ [1 - 2, 1 + 2] = [-1, 3].
Đèn đường thứ ba chiếu sáng khu vực từ [3 - 3, 3 + 3] = [0, 6].

Vị trí -1 có độ sáng bằng 2, được chiếu sáng bởi đèn đường thứ nhất và thứ hai.
Các vị trí 0, 1, 2 và 3 có độ sáng bằng 2, được chiếu sáng bởi đèn đường thứ hai và thứ ba.
Trong tất cả các vị trí này, -1 là vị trí nhỏ nhất, nên trả về vị trí này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> lights = [[1,0],[0,1]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Đèn đường thứ nhất chiếu sáng khu vực từ [1 - 0, 1 + 0] = [1, 1].
Đèn đường thứ hai chiếu sáng khu vực từ [0 - 1, 0 + 1] = [-1, 1].

Vị trí 1 có độ sáng bằng 2, được chiếu sáng bởi đèn đường thứ nhất và thứ hai.
Trả về 1 vì đây là vị trí sáng nhất trên đường.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> lights = [[1,2]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>
Đèn đường thứ nhất chiếu sáng khu vực từ [1 - 2, 1 + 2] = [-1, 3].

Các vị trí -1, 0, 1, 2 và 3 có độ sáng bằng 1, được chiếu sáng bởi đèn đường thứ nhất.
Trong tất cả các vị trí này, -1 là vị trí nhỏ nhất, nên trả về vị trí này.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= lights.length &lt;= 10<sup>5</sup></code></li>
	<li><code>lights[i].length == 2</code></li>
	<li><code>-10<sup>8</sup> &lt;= position<sub>i</sub> &lt;= 10<sup>8</sup></code></li>
	<li><code>0 &lt;= range<sub>i</sub> &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng sai phân + Hash Table + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đèn chiếu sáng đoạn $[pos-r, pos+r]$; ta cần tìm tọa độ sáng nhất ở bên trái. Vì các tọa độ có thể rất lớn, chỉ các điểm thay đổi, tổng cộng $O(n)$ điểm, mới quan trọng.
>
> Ghi nhận sai phân $+1$ tại $l$ và $-1$ tại $r+1$, sắp xếp các key, rồi duyệt với tổng hiện tại $s$ và giá trị lớn nhất $mx$.
>
> Khi $s$ tạo ra một giá trị lớn nhất mới, ghi lại vị trí đó.

<!-- thinking:end -->

Ta có thể xem phạm vi được mỗi đèn đường chiếu sáng là một đoạn, với đầu mút trái $l = position_i - range_i$ và đầu mút phải $r = position_i + range_i$. Ta có thể sử dụng ý tưởng mảng sai phân. Với mỗi đoạn $[l, r]$, ta cộng $1$ vào giá trị tại vị trí $l$ và trừ $1$ vào giá trị tại vị trí $r + 1$. Ta dùng một hash table để lưu giá trị thay đổi tại mỗi vị trí.

Sau đó, ta duyệt qua từng vị trí theo thứ tự tăng dần và tính độ sáng $s$ tại vị trí hiện tại. Nếu độ sáng lớn nhất trước đó $mx < s$, ta cập nhật độ sáng lớn nhất $mx = s$ và lưu lại vị trí hiện tại $ans = i$.

Cuối cùng, trả về $ans$.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của `lights`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def brightestPosition(self, lights: List[List[int]]) -> int:
        d = defaultdict(int)
        for i, j in lights:
            l, r = i - j, i + j
            d[l] += 1
            d[r + 1] -= 1
        ans = s = mx = 0
        for k in sorted(d):
            s += d[k]
            if mx < s:
                mx = s
                ans = k
        return ans
```

#### Java

```java
class Solution {
    public int brightestPosition(int[][] lights) {
        TreeMap<Integer, Integer> d = new TreeMap<>();
        for (var x : lights) {
            int l = x[0] - x[1], r = x[0] + x[1];
            d.merge(l, 1, Integer::sum);
            d.merge(r + 1, -1, Integer::sum);
        }
        int ans = 0, s = 0, mx = 0;
        for (var x : d.entrySet()) {
            int v = x.getValue();
            s += v;
            if (mx < s) {
                mx = s;
                ans = x.getKey();
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
    int brightestPosition(vector<vector<int>>& lights) {
        map<int, int> d;
        for (auto& x : lights) {
            int l = x[0] - x[1], r = x[0] + x[1];
            ++d[l];
            --d[r + 1];
        }
        int ans = 0, s = 0, mx = 0;
        for (auto& [i, v] : d) {
            s += v;
            if (mx < s) {
                mx = s;
                ans = i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func brightestPosition(lights [][]int) (ans int) {
	d := map[int]int{}
	for _, x := range lights {
		l, r := x[0]-x[1], x[0]+x[1]
		d[l]++
		d[r+1]--
	}
	keys := make([]int, 0, len(d))
	for i := range d {
		keys = append(keys, i)
	}
	sort.Ints(keys)
	mx, s := 0, 0
	for _, i := range keys {
		s += d[i]
		if mx < s {
			mx = s
			ans = i
		}
	}
	return
}
```

#### JavaScript

```js
/**
 * @param {number[][]} lights
 * @return {number}
 */
var brightestPosition = function (lights) {
    const d = new Map();
    for (const [i, j] of lights) {
        const l = i - j;
        const r = i + j;
        d.set(l, (d.get(l) ?? 0) + 1);
        d.set(r + 1, (d.get(r + 1) ?? 0) - 1);
    }
    const keys = [];
    for (const k of d.keys()) {
        keys.push(k);
    }
    keys.sort((a, b) => a - b);
    let ans = 0;
    let s = 0;
    let mx = 0;
    for (const i of keys) {
        s += d.get(i);
        if (mx < s) {
            mx = s;
            ans = i;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
