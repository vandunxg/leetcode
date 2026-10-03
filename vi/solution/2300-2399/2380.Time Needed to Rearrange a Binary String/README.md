---
comments: true
difficulty: Medium
rating: 1481
source: Biweekly Contest 85 Q2
tags:
    - String
    - Dynamic Programming
    - Simulation
---

<!-- problem:start -->

# [2380. Time Needed to Rearrange a Binary String](https://leetcode.com/problems/time-needed-to-rearrange-a-binary-string)

[中文文档](/solution/2300-2399/2380.Time%20Needed%20to%20Rearrange%20a%20Binary%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi nhị phân <code>s</code>. Trong một giây, <strong>tất cả</strong> các lần xuất hiện của <code>&quot;01&quot;</code> được <strong>đồng thời</strong> thay thế bằng <code>&quot;10&quot;</code>. Quá trình này <strong>lặp lại</strong> cho đến khi không còn lần xuất hiện nào của <code>&quot;01&quot;</code>.</p>

<p>Trả về<em> số giây cần thiết để hoàn thành quá trình này.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;0110101&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Sau một giây, s trở thành &quot;1011010&quot;.
Sau một giây nữa, s trở thành &quot;1101100&quot;.
Sau giây thứ ba, s trở thành &quot;1110100&quot;.
Sau giây thứ tư, s trở thành &quot;1111000&quot;.
Không còn lần xuất hiện nào của &quot;01&quot;, và quá trình cần 4 giây để hoàn thành,
nên ta trả về 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;11100&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không còn lần xuất hiện nào của &quot;01&quot; trong s, và quá trình cần 0 giây để hoàn thành,
nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<p>Bạn có thể giải bài toán với độ phức tạp thời gian O(n) không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giây, mọi $01$ đều được thay thế bằng $10$ cùng lúc. Vì $n \le 1000$ và có nhiều nhất $n$ vòng lặp, ta có thể thực hiện các lượt thay thế lặp lại.
>
> Lặp lại $replace(01,10)$ cho đến khi không còn lần xuất hiện nào; số lần lặp chính là đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def secondsToRemoveOccurrences(self, s: str) -> int:
        ans = 0
        while s.count('01'):
            s = s.replace('01', '10')
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int secondsToRemoveOccurrences(String s) {
        char[] cs = s.toCharArray();
        boolean find = true;
        int ans = 0;
        while (find) {
            find = false;
            for (int i = 0; i < cs.length - 1; ++i) {
                if (cs[i] == '0' && cs[i + 1] == '1') {
                    char t = cs[i];
                    cs[i] = cs[i + 1];
                    cs[i + 1] = t;
                    ++i;
                    find = true;
                }
            }
            if (find) {
                ++ans;
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
    int secondsToRemoveOccurrences(string s) {
        bool find = true;
        int ans = 0;
        while (find) {
            find = false;
            for (int i = 0; i < s.size() - 1; ++i) {
                if (s[i] == '0' && s[i + 1] == '1') {
                    swap(s[i], s[i + 1]);
                    ++i;
                    find = true;
                }
            }
            if (find) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func secondsToRemoveOccurrences(s string) int {
	cs := []byte(s)
	ans := 0
	find := true
	for find {
		find = false
		for i := 0; i < len(cs)-1; i++ {
			if cs[i] == '0' && cs[i+1] == '1' {
				cs[i], cs[i+1] = cs[i+1], cs[i]
				i++
				find = true
			}
		}
		if find {
			ans++
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 có độ phức tạp bậc hai trong trường hợp xấu nhất. Mỗi $1$ di chuyển sang trái qua các số 0. Một lần duyệt sẽ đếm số lượng số 0 đã gặp: thời điểm một $1$ hoàn tất là giá trị lớn hơn giữa “thời điểm của $1$ trước đó cộng một” và số lượng số 0.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def secondsToRemoveOccurrences(self, s: str) -> int:
        ans = cnt = 0
        for c in s:
            if c == '0':
                cnt += 1
            elif cnt:
                ans = max(ans + 1, cnt)
        return ans
```

#### Java

```java
class Solution {
    public int secondsToRemoveOccurrences(String s) {
        int ans = 0, cnt = 0;
        for (char c : s.toCharArray()) {
            if (c == '0') {
                ++cnt;
            } else if (cnt > 0) {
                ans = Math.max(ans + 1, cnt);
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
    int secondsToRemoveOccurrences(string s) {
        int ans = 0, cnt = 0;
        for (char c : s) {
            if (c == '0') {
                ++cnt;
            } else if (cnt) {
                ans = max(ans + 1, cnt);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func secondsToRemoveOccurrences(s string) int {
	ans, cnt := 0, 0
	for _, c := range s {
		if c == '0' {
			cnt++
		} else if cnt > 0 {
			ans = max(ans+1, cnt)
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
