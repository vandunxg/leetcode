---
comments: true
difficulty: Medium
rating: 1487
source: Weekly Contest 227 Q2
tags:
    - Greedy
    - Math
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1753. Maximum Score From Removing Stones](https://leetcode.com/problems/maximum-score-from-removing-stones)

[中文文档](/solution/1700-1799/1753.Maximum%20Score%20From%20Removing%20Stones/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang chơi một trò chơi một người với <strong>ba đống</strong> đá có kích thước lần lượt là <code>a</code>​​​​​​, <code>b</code> và <code>c</code>​​​​​​. Mỗi lượt, bạn chọn hai đống <strong>khác nhau và không rỗng</strong>, lấy một viên đá từ mỗi đống rồi cộng <code>1</code> điểm. Trò chơi dừng khi có <strong>ít hơn hai đống không rỗng</strong> (nghĩa là không còn nước đi nào).</p>

<p>Cho ba số nguyên <code>a</code>​​​​​, <code>b</code>​​​​​ và <code>c</code>​​​​​, trả về <em>giá trị</em> <strong><em>điểm số lớn nhất</em> </strong><em><strong>mà bạn có thể đạt được</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 2, b = 4, c = 6
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Trạng thái ban đầu là (2, 4, 6). Một dãy nước đi tối ưu là:
- Lấy từ đống thứ 1 và thứ 3, trạng thái trở thành (1, 4, 5)
- Lấy từ đống thứ 1 và thứ 3, trạng thái trở thành (0, 4, 4)
- Lấy từ đống thứ 2 và thứ 3, trạng thái trở thành (0, 3, 3)
- Lấy từ đống thứ 2 và thứ 3, trạng thái trở thành (0, 2, 2)
- Lấy từ đống thứ 2 và thứ 3, trạng thái trở thành (0, 1, 1)
- Lấy từ đống thứ 2 và thứ 3, trạng thái trở thành (0, 0, 0)
Có ít hơn hai đống không rỗng nên trò chơi kết thúc. Tổng cộng: 6 điểm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 4, b = 4, c = 6
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Trạng thái ban đầu là (4, 4, 6). Một dãy nước đi tối ưu là:
- Lấy từ đống thứ 1 và thứ 2, trạng thái trở thành (3, 3, 6)
- Lấy từ đống thứ 1 và thứ 3, trạng thái trở thành (2, 3, 5)
- Lấy từ đống thứ 1 và thứ 3, trạng thái trở thành (1, 3, 4)
- Lấy từ đống thứ 1 và thứ 3, trạng thái trở thành (0, 3, 3)
- Lấy từ đống thứ 2 và thứ 3, trạng thái trở thành (0, 2, 2)
- Lấy từ đống thứ 2 và thứ 3, trạng thái trở thành (0, 1, 1)
- Lấy từ đống thứ 2 và thứ 3, trạng thái trở thành (0, 0, 0)
Có ít hơn hai đống không rỗng nên trò chơi kết thúc. Tổng cộng: 7 điểm.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> a = 1, b = 8, c = 8
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Một dãy nước đi tối ưu là lấy từ đống thứ 2 và thứ 3 trong 8 lượt cho đến khi chúng rỗng.
Sau đó có ít hơn hai đống không rỗng nên trò chơi kết thúc.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= a, b, c &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt lấy một viên đá từ hai đống không rỗng. Luôn giảm hai đống lớn nhất hiện tại là tối ưu, và tổng số đá đủ nhỏ để mô phỏng.
>
> Sắp xếp bộ ba, liên tục giảm hai phần tử lớn nhất rồi sắp xếp lại; số lượt chính là điểm số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumScore(self, a: int, b: int, c: int) -> int:
        s = sorted([a, b, c])
        ans = 0
        while s[1]:
            ans += 1
            s[1] -= 1
            s[2] -= 1
            s.sort()
        return ans
```

#### Java

```java
class Solution {
    public int maximumScore(int a, int b, int c) {
        int[] s = new int[] {a, b, c};
        Arrays.sort(s);
        int ans = 0;
        while (s[1] > 0) {
            ++ans;
            s[1]--;
            s[2]--;
            Arrays.sort(s);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumScore(int a, int b, int c) {
        vector<int> s = {a, b, c};
        sort(s.begin(), s.end());
        int ans = 0;
        while (s[1]) {
            ++ans;
            s[1]--;
            s[2]--;
            sort(s.begin(), s.end());
        }
        return ans;
    }
};
```

#### Go

```go
func maximumScore(a int, b int, c int) (ans int) {
	s := []int{a, b, c}
	sort.Ints(s)
	for s[1] > 0 {
		ans++
		s[1]--
		s[2]--
		sort.Ints(s)
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 mô phỏng từng lượt. Với $a\le b\le c$, nếu $a+b\le c$ thì hai đống nhỏ sẽ hết trước và điểm số là $a+b$; ngược lại, điểm số là $\lfloor(a+b+c)/2\rfloor$. Thời gian chạy là hằng số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumScore(self, a: int, b: int, c: int) -> int:
        a, b, c = sorted([a, b, c])
        if a + b < c:
            return a + b
        return (a + b + c) >> 1
```

#### Java

```java
class Solution {
    public int maximumScore(int a, int b, int c) {
        int[] s = new int[] {a, b, c};
        Arrays.sort(s);
        if (s[0] + s[1] < s[2]) {
            return s[0] + s[1];
        }
        return (a + b + c) >> 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumScore(int a, int b, int c) {
        vector<int> s = {a, b, c};
        sort(s.begin(), s.end());
        if (s[0] + s[1] < s[2]) return s[0] + s[1];
        return (a + b + c) >> 1;
    }
};
```

#### Go

```go
func maximumScore(a int, b int, c int) int {
	s := []int{a, b, c}
	sort.Ints(s)
	if s[0]+s[1] < s[2] {
		return s[0] + s[1]
	}
	return (a + b + c) >> 1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
