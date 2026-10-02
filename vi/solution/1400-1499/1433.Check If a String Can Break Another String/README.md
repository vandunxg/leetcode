---
comments: true
difficulty: Medium
rating: 1436
source: Biweekly Contest 25 Q3
tags:
    - Greedy
    - String
    - Sorting
---

<!-- problem:start -->

# [1433. Check If a String Can Break Another String](https://leetcode.com/problems/check-if-a-string-can-break-another-string)

[Tài liệu tiếng Trung](/solution/1400-1499/1433.Check%20If%20a%20String%20Can%20Break%20Another%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai chuỗi: <code>s1</code> và <code>s2</code> có cùng kích thước, hãy kiểm tra xem một hoán vị nào đó của chuỗi <code>s1</code> có thể break một hoán vị nào đó của chuỗi <code>s2</code> hay ngược lại. Nói cách khác, <code>s2</code> có thể break <code>s1</code> hoặc ngược lại.</p>

<p>Một chuỗi <code>x</code> có thể break chuỗi <code>y</code> (cả hai đều có kích thước <code>n</code>) nếu <code>x[i] &gt;= y[i]</code> (theo thứ tự alphabet) với mọi <code>i</code> từ <code>0</code> đến <code>n-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;abc&quot;, s2 = &quot;xya&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> &quot;ayx&quot; là một hoán vị của s2=&quot;xya&quot; và có thể break chuỗi &quot;abc&quot;, vốn là một hoán vị của s1=&quot;abc&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;abe&quot;, s2 = &quot;acd&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Tất cả hoán vị của s1=&quot;abe&quot; là: &quot;abe&quot;, &quot;aeb&quot;, &quot;bae&quot;, &quot;bea&quot;, &quot;eab&quot; và &quot;eba&quot;, còn tất cả hoán vị của s2=&quot;acd&quot; là: &quot;acd&quot;, &quot;adc&quot;, &quot;cad&quot;, &quot;cda&quot;, &quot;dac&quot; và &quot;dca&quot;. Tuy nhiên, không có hoán vị nào của s1 có thể break một hoán vị nào đó của s2 và ngược lại.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s1 = &quot;leetcodee&quot;, s2 = &quot;interview&quot;
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>s1.length == n</code></li>
	<li><code>s2.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 10^5</code></li>
	<li>Tất cả chuỗi đều chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 10^5$, nên chúng ta không thể thử mọi hoán vị. Một chuỗi break được chuỗi còn lại khi và chỉ khi tồn tại một cách ghép sao cho mọi ký tự của nó đều lớn hơn hoặc bằng ký tự tương ứng.
>
> Sắp xếp cả hai chuỗi rồi so sánh theo từng vị trí: nếu một phía luôn $\ge$ hoặc luôn $\le$ phía kia thì tồn tại hoán vị phù hợp. Ghép các ký tự đã sắp xếp là tối ưu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkIfCanBreak(self, s1: str, s2: str) -> bool:
        cs1 = sorted(s1)
        cs2 = sorted(s2)
        return all(a >= b for a, b in zip(cs1, cs2)) or all(
            a <= b for a, b in zip(cs1, cs2)
        )
```

#### Java

```java
class Solution {
    public boolean checkIfCanBreak(String s1, String s2) {
        char[] cs1 = s1.toCharArray();
        char[] cs2 = s2.toCharArray();
        Arrays.sort(cs1);
        Arrays.sort(cs2);
        return check(cs1, cs2) || check(cs2, cs1);
    }

    private boolean check(char[] cs1, char[] cs2) {
        for (int i = 0; i < cs1.length; ++i) {
            if (cs1[i] < cs2[i]) {
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
    bool checkIfCanBreak(string s1, string s2) {
        sort(s1.begin(), s1.end());
        sort(s2.begin(), s2.end());
        return check(s1, s2) || check(s2, s1);
    }

    bool check(string& s1, string& s2) {
        for (int i = 0; i < s1.size(); ++i) {
            if (s1[i] < s2[i]) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func checkIfCanBreak(s1 string, s2 string) bool {
	cs1 := []byte(s1)
	cs2 := []byte(s2)
	sort.Slice(cs1, func(i, j int) bool { return cs1[i] < cs1[j] })
	sort.Slice(cs2, func(i, j int) bool { return cs2[i] < cs2[j] })
	check := func(cs1, cs2 []byte) bool {
		for i := range cs1 {
			if cs1[i] < cs2[i] {
				return false
			}
		}
		return true
	}
	return check(cs1, cs2) || check(cs2, cs1)
}
```

#### TypeScript

```ts
function checkIfCanBreak(s1: string, s2: string): boolean {
    const cs1: string[] = Array.from(s1);
    const cs2: string[] = Array.from(s2);
    cs1.sort();
    cs2.sort();
    const check = (cs1: string[], cs2: string[]) => {
        for (let i = 0; i < cs1.length; i++) {
            if (cs1[i] < cs2[i]) {
                return false;
            }
        }
        return true;
    };
    return check(cs1, cs2) || check(cs2, cs1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
