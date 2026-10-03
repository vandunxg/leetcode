---
comments: true
difficulty: Medium
rating: 1581
source: Weekly Contest 285 Q2
tags:
    - Stack
    - String
    - Simulation
---

<!-- problem:start -->

# [2211. Count Collisions on a Road](https://leetcode.com/problems/count-collisions-on-a-road)

[中文文档](/solution/2200-2299/2211.Count%20Collisions%20on%20a%20Road/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> xe trên một con đường dài vô hạn. Các xe được đánh số từ <code>0</code> đến <code>n - 1</code> từ trái sang phải và mỗi xe ở một vị trí <strong>khác nhau</strong>.</p>

<p>Bạn được cho một chuỗi <code>directions</code> <strong>đánh số từ 0</strong> có độ dài <code>n</code>. <code>directions[i]</code> có thể là <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code> hoặc <code>&#39;S&#39;</code>, lần lượt biểu thị xe thứ <code>i<sup>th</sup></code> đang di chuyển về <strong>bên trái</strong>, về <strong>bên phải</strong> hoặc <strong>đứng yên</strong> tại vị trí hiện tại. Mọi xe đang di chuyển đều có <strong>cùng tốc độ</strong>.</p>

<p>Số lần va chạm được tính như sau:</p>

<ul>
	<li>Khi hai xe đang di chuyển theo hướng <strong>ngược nhau</strong> va chạm, số lần va chạm tăng thêm <code>2</code>.</li>
	<li>Khi một xe đang di chuyển va chạm với một xe đang đứng yên, số lần va chạm tăng thêm <code>1</code>.</li>
</ul>

<p>Sau khi va chạm, các xe liên quan không thể tiếp tục di chuyển và sẽ dừng lại tại vị trí va chạm. Ngoài trường hợp đó, các xe không thể thay đổi trạng thái hoặc hướng di chuyển.</p>

<p>Trả về <em><strong>tổng số lần va chạm</strong> xảy ra trên đường</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> directions = &quot;RLRSLL&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Các va chạm xảy ra trên đường là:
- Xe 0 và xe 1 va chạm với nhau. Vì chúng di chuyển theo hai hướng ngược nhau, số lần va chạm trở thành 0 + 2 = 2.
- Xe 2 và xe 3 va chạm với nhau. Vì xe 3 đứng yên, số lần va chạm trở thành 2 + 1 = 3.
- Xe 3 và xe 4 va chạm với nhau. Vì xe 3 đứng yên, số lần va chạm trở thành 3 + 1 = 4.
- Xe 4 và xe 5 va chạm với nhau. Sau khi va chạm với xe 3, xe 4 sẽ dừng lại tại vị trí va chạm và bị xe 5 đâm phải. Số lần va chạm trở thành 4 + 1 = 5.
Do đó, tổng số lần va chạm xảy ra trên đường là 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> directions = &quot;LLRR&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không có xe nào va chạm với xe khác. Do đó, tổng số lần va chạm xảy ra trên đường là 0.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= directions.length &lt;= 10<sup>5</sup></code></li>
	<li><code>directions[i]</code> chỉ có thể là <code>&#39;L&#39;</code>, <code>&#39;R&#39;</code> hoặc <code>&#39;S&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mẹo tư duy

<!-- thinking:start -->

> **Tư duy**
>
> Hai xe gặp nhau theo hướng đối diện làm tăng số va chạm thêm $2$, một xe đang di chuyển đâm vào xe đứng yên làm tăng thêm $1$, và mọi xe từng va chạm với xe khác cuối cùng đều dừng lại. Với $n \le 10^5$, việc mô phỏng từng cặp xe là không phù hợp.
>
> Một tiền tố chỉ gồm các xe đi sang trái sẽ không va chạm với xe nào; tương tự, một hậu tố chỉ gồm các xe đi sang phải cũng không va chạm. Sau khi loại bỏ hai đoạn này, mọi xe còn đang di chuyển đều sẽ dừng lại và đóng góp $1$.
>
> Xóa các ký tự $\texttt{L}$ ở đầu và $\texttt{R}$ ở cuối, sau đó đếm số ký tự không phải $\texttt{S}$.

<!-- thinking:end -->

Theo mô tả bài toán, khi hai xe đang di chuyển theo hai hướng ngược nhau va chạm, số lần va chạm tăng thêm $2$, nghĩa là cả hai xe đều dừng lại và đáp án tăng thêm $2$. Khi một xe đang di chuyển va chạm với một xe đứng yên, số lần va chạm tăng thêm $1$, nghĩa là một xe dừng lại và đáp án tăng thêm $1$.

Rõ ràng, tiền tố chỉ gồm $\textit{L}$ và hậu tố chỉ gồm $\textit{R}$ sẽ không va chạm, nên ta chỉ cần đếm số ký tự không phải $\textit{S}$ ở phần giữa.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$ hoặc $O(1)$. Trong đó, $n$ là độ dài chuỗi $\textit{directions}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countCollisions(self, directions: str) -> int:
        s = directions.lstrip("L").rstrip("R")
        return len(s) - s.count("S")
```

#### Java

```java
class Solution {
    public int countCollisions(String directions) {
        char[] s = directions.toCharArray();
        int n = s.length;
        int l = 0, r = n - 1;
        while (l < n && s[l] == 'L') {
            ++l;
        }
        while (r >= 0 && s[r] == 'R') {
            --r;
        }
        int ans = r - l + 1;
        for (int i = l; i <= r; ++i) {
            ans -= s[i] == 'S' ? 1 : 0;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countCollisions(string s) {
        int n = s.size();
        int l = 0, r = n - 1;
        while (l < n && s[l] == 'L') {
            ++l;
        }
        while (r >= 0 && s[r] == 'R') {
            --r;
        }
        return r - l + 1 - count(s.begin() + l, s.begin() + r + 1, 'S');
    }
};
```

#### Go

```go
func countCollisions(directions string) int {
	s := strings.TrimRight(strings.TrimLeft(directions, "L"), "R")
	return len(s) - strings.Count(s, "S")
}
```

#### TypeScript

```ts
function countCollisions(directions: string): number {
    const n = directions.length;
    let [l, r] = [0, n - 1];
    while (l < n && directions[l] == 'L') {
        ++l;
    }
    while (r >= 0 && directions[r] == 'R') {
        --r;
    }
    let ans = r - l + 1;
    for (let i = l; i <= r; ++i) {
        if (directions[i] === 'S') {
            --ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_collisions(directions: String) -> i32 {
        let s = directions.trim_start_matches('L').trim_end_matches('R');
        (s.len() as i32) - (s.matches('S').count() as i32)
    }
}
```

#### JavaScript

```js
/**
 * @param {string} directions
 * @return {number}
 */
var countCollisions = function (directions) {
    const n = directions.length;
    let [l, r] = [0, n - 1];
    while (l < n && directions[l] == 'L') {
        ++l;
    }
    while (r >= 0 && directions[r] == 'R') {
        --r;
    }
    let ans = r - l + 1;
    for (let i = l; i <= r; ++i) {
        if (directions[i] === 'S') {
            --ans;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
