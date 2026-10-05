---
comments: true
difficulty: Easy
rating: 1288
source: Biweekly Contest 170 Q1
tags:
    - Bit Manipulation
    - Math
    - Two Pointers
    - String
---

<!-- problem:start -->

# [3750. Minimum Number of Flips to Reverse Binary String](https://leetcode.com/problems/minimum-number-of-flips-to-reverse-binary-string)

[中文文档](/solution/3700-3799/3750.Minimum%20Number%20of%20Flips%20to%20Reverse%20Binary%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <strong>dương</strong> <code>n</code>.</p>

<p>Gọi <code>s</code> là <strong>biểu diễn nhị phân</strong> của <code>n</code> không có các số 0 ở đầu.</p>

<p><strong>Đảo ngược</strong> một chuỗi nhị phân <code>s</code> là viết các ký tự của <code>s</code> theo thứ tự ngược lại.</p>

<p>Bạn có thể lật bất kỳ bit nào trong <code>s</code> (đổi <code>0 &rarr; 1</code> hoặc <code>1 &rarr; 0</code>). Mỗi lần lật ảnh hưởng đến <strong>chính xác</strong> một bit.</p>

<p>Trả về số lần lật <strong>ít nhất</strong> cần thiết để biến <code>s</code> thành chuỗi đảo ngược của nó ở trạng thái ban đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Biểu diễn nhị phân của 7 là <code>&quot;111&quot;</code>. Chuỗi đảo ngược của nó cũng là <code>&quot;111&quot;</code>, tức là giống với chuỗi ban đầu. Vì vậy, không cần lật bit nào.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 10</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Biểu diễn nhị phân của 10 là <code>&quot;1010&quot;</code>. Chuỗi đảo ngược của nó là <code>&quot;0101&quot;</code>. Cả bốn bit đều phải được lật để hai chuỗi bằng nhau. Do đó, số lần lật ít nhất cần thiết là 4.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi nhị phân bằng chuỗi đảo ngược của nó khi các vị trí đối xứng khớp nhau. $n$ chỉ có $O(\log n)$ bit, nên ta chuyển nó thành chuỗi rồi so sánh các vị trí đối xứng; mỗi cặp không khớp cần lật cả hai đầu, vì vậy đáp án bằng hai lần số cặp không khớp.

<!-- thinking:end -->

Trước hết, ta chuyển số nguyên $n$ thành một chuỗi nhị phân $s$. Sau đó, ta dùng hai con trỏ đi từ hai đầu chuỗi về phía giữa, đếm số vị trí mà các ký tự khác nhau, ký hiệu là $cnt$. Vì mỗi lần lật chỉ có thể ảnh hưởng đến một bit, tổng số lần lật là $cnt \times 2$.

Độ phức tạp thời gian là $O(\log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là số nguyên đầu vào.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumFlips(self, n: int) -> int:
        s = bin(n)[2:]
        m = len(s)
        return sum(s[i] != s[m - i - 1] for i in range(m // 2)) * 2
```

#### Java

```java
class Solution {
    public int minimumFlips(int n) {
        String s = Integer.toBinaryString(n);
        int m = s.length();
        int cnt = 0;
        for (int i = 0; i < m / 2; i++) {
            if (s.charAt(i) != s.charAt(m - i - 1)) {
                cnt++;
            }
        }
        return cnt * 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumFlips(int n) {
        vector<int> s;
        while (n > 0) {
            s.push_back(n & 1);
            n >>= 1;
        }

        int m = s.size();
        int cnt = 0;
        for (int i = 0; i < m / 2; i++) {
            if (s[i] != s[m - i - 1]) {
                cnt++;
            }
        }
        return cnt * 2;
    }
};
```

#### Go

```go
func minimumFlips(n int) int {
    s := strconv.FormatInt(int64(n), 2)
    m := len(s)
    cnt := 0
    for i := 0; i < m/2; i++ {
        if s[i] != s[m-i-1] {
            cnt++
        }
    }
    return cnt * 2
}
```

#### TypeScript

```ts
function minimumFlips(n: number): number {
    const s = n.toString(2);
    const m = s.length;
    let cnt = 0;
    for (let i = 0; i < m / 2; i++) {
        if (s[i] !== s[m - i - 1]) {
            cnt++;
        }
    }
    return cnt * 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
