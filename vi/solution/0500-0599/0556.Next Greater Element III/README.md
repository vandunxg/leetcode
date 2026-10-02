---
comments: true
difficulty: Medium
tags:
    - Math
    - Two Pointers
    - String
---

<!-- problem:start -->

# [556. Next Greater Element III](https://leetcode.com/problems/next-greater-element-iii)

[中文文档](/solution/0500-0599/0556.Next%20Greater%20Element%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên dương <code>n</code>, hãy tìm <em>số nguyên nhỏ nhất có đúng các chữ số như số nguyên</em> <code>n</code> <em>và có giá trị lớn hơn</em> <code>n</code>. Nếu không tồn tại số nguyên dương như vậy, hãy trả về <code>-1</code>.</p>

<p><strong>Lưu ý</strong> số nguyên trả về phải nằm trong phạm vi <strong>số nguyên 32-bit</strong>. Nếu có đáp án hợp lệ nhưng không nằm trong phạm vi <strong>số nguyên 32-bit</strong>, hãy trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> n = 12
<strong>Đầu ra:</strong> 21
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> n = 21
<strong>Đầu ra:</strong> -1
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số nguyên lớn hơn tiếp theo là một hoán vị của các chữ số trong $n$. Đây chính là thuật toán next permutation.
>
> Tìm vị trí giảm dần ngoài cùng bên phải $i$, sau đó tìm vị trí ngoài cùng bên phải $j$ có giá trị lớn hơn $cs[i]$, đổi chỗ hai chữ số rồi đảo ngược hậu tố để đưa nó về thứ tự tăng dần. Nếu không có vị trí giảm dần thì $n$ đã là giá trị lớn nhất. Nếu kết quả vượt quá giới hạn 32-bit, cũng trả về $-1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nextGreaterElement(self, n: int) -> int:
        cs = list(str(n))
        n = len(cs)
        i, j = n - 2, n - 1
        while i >= 0 and cs[i] >= cs[i + 1]:
            i -= 1
        if i < 0:
            return -1
        while cs[i] >= cs[j]:
            j -= 1
        cs[i], cs[j] = cs[j], cs[i]
        cs[i + 1 :] = cs[i + 1 :][::-1]
        ans = int(''.join(cs))
        return -1 if ans > 2**31 - 1 else ans
```

#### Java

```java
class Solution {
    public int nextGreaterElement(int n) {
        char[] cs = String.valueOf(n).toCharArray();
        n = cs.length;
        int i = n - 2, j = n - 1;
        for (; i >= 0 && cs[i] >= cs[i + 1]; --i)
            ;
        if (i < 0) {
            return -1;
        }
        for (; cs[i] >= cs[j]; --j)
            ;
        swap(cs, i, j);
        reverse(cs, i + 1, n - 1);
        long ans = Long.parseLong(String.valueOf(cs));
        return ans > Integer.MAX_VALUE ? -1 : (int) ans;
    }

    private void swap(char[] cs, int i, int j) {
        char t = cs[i];
        cs[i] = cs[j];
        cs[j] = t;
    }

    private void reverse(char[] cs, int i, int j) {
        for (; i < j; ++i, --j) {
            swap(cs, i, j);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int nextGreaterElement(int n) {
        string s = to_string(n);
        n = s.size();
        int i = n - 2, j = n - 1;
        for (; i >= 0 && s[i] >= s[i + 1]; --i)
            ;
        if (i < 0) return -1;
        for (; s[i] >= s[j]; --j)
            ;
        swap(s[i], s[j]);
        reverse(s.begin() + i + 1, s.end());
        long ans = stol(s);
        return ans > INT_MAX ? -1 : ans;
    }
};
```

#### Go

```go
func nextGreaterElement(n int) int {
	s := []byte(strconv.Itoa(n))
	n = len(s)
	i, j := n-2, n-1
	for ; i >= 0 && s[i] >= s[i+1]; i-- {
	}
	if i < 0 {
		return -1
	}
	for ; j >= 0 && s[i] >= s[j]; j-- {
	}
	s[i], s[j] = s[j], s[i]
	for i, j = i+1, n-1; i < j; i, j = i+1, j-1 {
		s[i], s[j] = s[j], s[i]
	}
	ans, _ := strconv.Atoi(string(s))
	if ans > math.MaxInt32 {
		return -1
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
