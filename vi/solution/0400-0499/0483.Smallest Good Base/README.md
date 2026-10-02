---
comments: true
difficulty: Hard
tags:
    - Math
    - Binary Search
---

<!-- problem:start -->

# [483. Smallest Good Base](https://leetcode.com/problems/smallest-good-base)

[中文文档](/solution/0400-0499/0483.Smallest%20Good%20Base/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code> được biểu diễn dưới dạng chuỗi, hãy trả về <em><strong>cơ số tốt</strong> nhỏ nhất của</em> <code>n</code>.</p>

<p>Ta gọi <code>k &gt;= 2</code> là <strong>cơ số tốt</strong> của <code>n</code> nếu mọi chữ số của <code>n</code> trong cơ số <code>k</code> đều là <code>1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = &quot;13&quot;
<strong>Đầu ra:</strong> &quot;3&quot;
<strong>Giải thích:</strong> 13 trong cơ số 3 được viết là 111.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = &quot;4681&quot;
<strong>Đầu ra:</strong> &quot;8&quot;
<strong>Giải thích:</strong> 4681 trong cơ số 8 được viết là 11111.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = &quot;1000000000000000000&quot;
<strong>Đầu ra:</strong> &quot;999999999999999999&quot;
<strong>Giải thích:</strong> 1000000000000000000 trong cơ số 999999999999999999 được viết là 11.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n</code> là số nguyên trong khoảng <code>[3, 10<sup>18</sup>]</code>.</li>
	<li><code>n</code> không có chữ số 0 ở đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm cơ số nhỏ nhất $k\ge 2$ sao cho biểu diễn của $n$ chỉ gồm các chữ số 1. $k=n-1$ luôn thỏa mãn ($11_k$), nhưng vì $n$ có thể lên đến $10^{18}$ nên không thể thử lần lượt từng $k$.
>
> Nếu biểu diễn có $m+1$ chữ số 1 thì $m<60$. Thử $m$ từ lớn xuống nhỏ và dùng tìm kiếm nhị phân để tìm $k$ thỏa mãn $1+k+\cdots+k^m=n$. $m$ càng lớn thì $k$ càng nhỏ, nên duyệt theo thứ tự giảm dần sẽ tìm được cơ số nhỏ nhất trước.
>
> Tổng cấp số nhân tăng theo $k$, nên có thể dùng tìm kiếm nhị phân. Nếu không tìm được giá trị nào thỏa mãn, dùng $n-1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestGoodBase(self, n: str) -> str:
        def cal(k, m):
            p = s = 1
            for i in range(m):
                p *= k
                s += p
            return s

        num = int(n)
        for m in range(63, 1, -1):
            l, r = 2, num - 1
            while l < r:
                mid = (l + r) >> 1
                if cal(mid, m) >= num:
                    r = mid
                else:
                    l = mid + 1
            if cal(l, m) == num:
                return str(l)
        return str(num - 1)
```

#### Java

```java
class Solution {
    public String smallestGoodBase(String n) {
        long num = Long.parseLong(n);
        for (int len = 63; len >= 2; --len) {
            long radix = getRadix(len, num);
            if (radix != -1) {
                return String.valueOf(radix);
            }
        }
        return String.valueOf(num - 1);
    }

    private long getRadix(int len, long num) {
        long l = 2, r = num - 1;
        while (l < r) {
            long mid = l + r >>> 1;
            if (calc(mid, len) >= num)
                r = mid;
            else
                l = mid + 1;
        }
        return calc(r, len) == num ? r : -1;
    }

    private long calc(long radix, int len) {
        long p = 1;
        long sum = 0;
        for (int i = 0; i < len; ++i) {
            if (Long.MAX_VALUE - sum < p) {
                return Long.MAX_VALUE;
            }
            sum += p;
            if (Long.MAX_VALUE / p < radix) {
                p = Long.MAX_VALUE;
            } else {
                p *= radix;
            }
        }
        return sum;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string smallestGoodBase(string n) {
        long long num = stoll(n);
        for (int len = 63; len >= 2; --len) {
            long long radix = getRadix(len, num);
            if (radix != -1) {
                return to_string(radix);
            }
        }
        return to_string(num - 1);
    }

    long long getRadix(int len, long long num) {
        long long l = 2, r = num - 1;
        while (l < r) {
            long long mid = (l + r) >> 1;
            if (calc(mid, len) >= num) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return calc(r, len) == num ? r : -1;
    }

    long long calc(long long radix, int len) {
        long long p = 1, sum = 0;
        for (int i = 0; i < len; ++i) {
            if (LLONG_MAX - sum < p) {
                return LLONG_MAX;
            }
            sum += p;
            if (LLONG_MAX / p < radix) {
                p = LLONG_MAX;
            } else {
                p *= radix;
            }
        }
        return sum;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
