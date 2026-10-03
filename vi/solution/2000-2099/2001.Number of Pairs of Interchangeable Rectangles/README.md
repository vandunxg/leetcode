---
comments: true
difficulty: Medium
rating: 1435
source: Weekly Contest 258 Q2
tags:
    - Array
    - Hash Table
    - Math
    - Counting
    - Number Theory
---

<!-- problem:start -->

# [2001. Number of Pairs of Interchangeable Rectangles](https://leetcode.com/problems/number-of-pairs-of-interchangeable-rectangles)

[中文文档](/solution/2000-2099/2001.Number%20of%20Pairs%20of%20Interchangeable%20Rectangles/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp <code>n</code> hình chữ nhật được biểu diễn bằng một mảng số nguyên 2 chiều <code>rectangles</code> <strong>đánh chỉ số từ 0</strong>, trong đó <code>rectangles[i] = [width<sub>i</sub>, height<sub>i</sub>]</code> biểu thị chiều rộng và chiều cao của hình chữ nhật thứ <code>i<sup>th</sup></code>.</p>

<p>Hai hình chữ nhật <code>i</code> và <code>j</code> (<code>i &lt; j</code>) được xem là <strong>có thể hoán đổi</strong> nếu chúng có <strong>cùng tỉ lệ chiều rộng trên chiều cao</strong>. Cụ thể hơn, hai hình chữ nhật <strong>có thể hoán đổi</strong> nếu <code>width<sub>i</sub>/height<sub>i</sub> == width<sub>j</sub>/height<sub>j</sub></code> (sử dụng phép chia thập phân, không phải phép chia số nguyên).</p>

<p>Trả về <em><strong>số lượng</strong> cặp hình chữ nhật <strong>có thể hoán đổi</strong> trong </em><code>rectangles</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> rectangles = [[4,8],[3,6],[10,20],[15,30]]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các cặp hình chữ nhật có thể hoán đổi theo chỉ số (đánh chỉ số từ 0) là:
- Hình chữ nhật 0 và hình chữ nhật 1: 4/8 == 3/6.
- Hình chữ nhật 0 và hình chữ nhật 2: 4/8 == 10/20.
- Hình chữ nhật 0 và hình chữ nhật 3: 4/8 == 15/30.
- Hình chữ nhật 1 và hình chữ nhật 2: 3/6 == 10/20.
- Hình chữ nhật 1 và hình chữ nhật 3: 3/6 == 15/30.
- Hình chữ nhật 2 và hình chữ nhật 3: 10/20 == 15/30.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> rectangles = [[4,5],[7,8]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có cặp hình chữ nhật nào có thể hoán đổi.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == rectangles.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>rectangles[i].length == 2</code></li>
	<li><code>1 &lt;= width<sub>i</sub>, height<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Hai hình chữ nhật có thể hoán đổi khi và chỉ khi chúng có cùng tỉ lệ chiều rộng trên chiều cao. Với $n \le 10^5$, việc so sánh từng cặp có độ phức tạp bậc hai.
>
> Dùng giá trị floating-point $w/h$ làm key không an toàn. Chia $(w,h)$ cho $\gcd(w,h)$ sẽ ánh xạ mỗi tỉ lệ thành một cặp số nguyên duy nhất.
>
> Vì vậy, ta dùng hash table để đếm từng cặp đã rút gọn; với mỗi hình chữ nhật, ta cộng số lượng hiện tại rồi tăng lên, nhờ đó các tổ hợp được tích lũy trong một lượt duyệt.

<!-- thinking:end -->

Để biểu diễn duy nhất một hình chữ nhật, ta cần rút gọn tỉ lệ chiều rộng trên chiều cao của hình chữ nhật về phân số tối giản. Vì vậy, ta tìm ước chung lớn nhất của chiều rộng và chiều cao của mỗi hình chữ nhật, rồi rút gọn tỉ lệ chiều rộng trên chiều cao về phân số tối giản. Tiếp theo, ta dùng hash table để đếm số hình chữ nhật tương ứng với mỗi phân số tối giản, sau đó tính số tổ hợp từ số lượng hình chữ nhật của từng phân số tối giản để nhận đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ và $M$ lần lượt là số lượng hình chữ nhật và độ dài cạnh lớn nhất của các hình chữ nhật.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def interchangeableRectangles(self, rectangles: List[List[int]]) -> int:
        ans = 0
        cnt = Counter()
        for w, h in rectangles:
            g = gcd(w, h)
            w, h = w // g, h // g
            ans += cnt[(w, h)]
            cnt[(w, h)] += 1
        return ans
```

#### Java

```java
class Solution {
    public long interchangeableRectangles(int[][] rectangles) {
        long ans = 0;
        int n = rectangles.length + 1;
        Map<Long, Integer> cnt = new HashMap<>();
        for (var e : rectangles) {
            int w = e[0], h = e[1];
            int g = gcd(w, h);
            w /= g;
            h /= g;
            long x = (long) w * n + h;
            ans += cnt.getOrDefault(x, 0);
            cnt.merge(x, 1, Integer::sum);
        }
        return ans;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long interchangeableRectangles(vector<vector<int>>& rectangles) {
        long long ans = 0;
        int n = rectangles.size();
        unordered_map<long long, int> cnt;
        for (auto& e : rectangles) {
            int w = e[0], h = e[1];
            int g = gcd(w, h);
            w /= g;
            h /= g;
            long long x = 1ll * w * (n + 1) + h;
            ans += cnt[x];
            cnt[x]++;
        }
        return ans;
    }
};
```

#### Go

```go
func interchangeableRectangles(rectangles [][]int) int64 {
	ans := 0
	n := len(rectangles)
	cnt := map[int]int{}
	for _, e := range rectangles {
		w, h := e[0], e[1]
		g := gcd(w, h)
		w, h = w/g, h/g
		x := w*(n+1) + h
		ans += cnt[x]
		cnt[x]++
	}
	return int64(ans)
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### JavaScript

```js
/**
 * @param {number[][]} rectangles
 * @return {number}
 */
var interchangeableRectangles = function (rectangles) {
    const cnt = new Map();
    let ans = 0;
    for (let [w, h] of rectangles) {
        const g = gcd(w, h);
        w = Math.floor(w / g);
        h = Math.floor(h / g);
        const x = w * (rectangles.length + 1) + h;
        ans += cnt.get(x) | 0;
        cnt.set(x, (cnt.get(x) | 0) + 1);
    }
    return ans;
};

function gcd(a, b) {
    if (b == 0) {
        return a;
    }
    return gcd(b, a % b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
