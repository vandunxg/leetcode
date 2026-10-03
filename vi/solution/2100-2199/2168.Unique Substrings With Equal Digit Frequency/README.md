---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Counting
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [2168. Unique Substrings With Equal Digit Frequency 🔒](https://leetcode.com/problems/unique-substrings-with-equal-digit-frequency)

[中文文档](/solution/2100-2199/2168.Unique%20Substrings%20With%20Equal%20Digit%20Frequency/README.md)

## Mô tả

<!-- description:start -->

Cho một chuỗi chữ số <code>s</code>, hãy trả về <em>số lượng <strong>chuỗi con khác nhau</strong> của </em><code>s</code><em> sao cho mọi chữ số xuất hiện cùng số lần.</em>
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1212&quot;
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các chuỗi con thỏa mãn yêu cầu là &quot;1&quot;, &quot;2&quot;, &quot;12&quot;, &quot;21&quot;, &quot;1212&quot;.
Lưu ý rằng dù chuỗi con &quot;12&quot; xuất hiện hai lần, nó chỉ được tính một lần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;12321&quot;
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Các chuỗi con thỏa mãn yêu cầu là &quot;1&quot;, &quot;2&quot;, &quot;3&quot;, &quot;12&quot;, &quot;23&quot;, &quot;32&quot;, &quot;21&quot;, &quot;123&quot;, &quot;321&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các chuỗi con khác nhau mà mọi chữ số xuất hiện đều có cùng tần suất. Với $n\le 1000$, có $O(n^2)$ chuỗi con; duyệt lại từng chuỗi từ đầu sẽ có độ phức tạp bậc ba.
>
> Các prefix count của mười chữ số cho phép lấy vector tần suất trong $O(1)$; sau đó ta kiểm tra xem các tần suất khác 0 có chỉ tạo thành một giá trị duy nhất hay không. Một set dùng để lưu chính các chuỗi con.
>
> Liệt kê mọi đoạn $[i,j]$ và thêm $s[i:j+1]$ khi phép kiểm tra dựa trên hiệu của các prefix cho kết quả đúng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def equalDigitFrequency(self, s: str) -> int:
        def check(i, j):
            v = set()
            for k in range(10):
                cnt = presum[j + 1][k] - presum[i][k]
                if cnt > 0:
                    v.add(cnt)
                if len(v) > 1:
                    return False
            return True

        n = len(s)
        presum = [[0] * 10 for _ in range(n + 1)]
        for i, c in enumerate(s):
            presum[i + 1][int(c)] += 1
            for j in range(10):
                presum[i + 1][j] += presum[i][j]
        vis = set(s[i : j + 1] for i in range(n) for j in range(i, n) if check(i, j))
        return len(vis)
```

#### Java

```java
class Solution {
    public int equalDigitFrequency(String s) {
        int n = s.length();
        int[][] presum = new int[n + 1][10];
        for (int i = 0; i < n; ++i) {
            ++presum[i + 1][s.charAt(i) - '0'];
            for (int j = 0; j < 10; ++j) {
                presum[i + 1][j] += presum[i][j];
            }
        }
        Set<String> vis = new HashSet<>();
        for (int i = 0; i < n; ++i) {
            for (int j = i; j < n; ++j) {
                if (check(i, j, presum)) {
                    vis.add(s.substring(i, j + 1));
                }
            }
        }
        return vis.size();
    }

    private boolean check(int i, int j, int[][] presum) {
        Set<Integer> v = new HashSet<>();
        for (int k = 0; k < 10; ++k) {
            int cnt = presum[j + 1][k] - presum[i][k];
            if (cnt > 0) {
                v.add(cnt);
            }
            if (v.size() > 1) {
                return false;
            }
        }
        return true;
    }
}
```

#### Go

```go
func equalDigitFrequency(s string) int {
	n := len(s)
	presum := make([][]int, n+1)
	for i := range presum {
		presum[i] = make([]int, 10)
	}
	for i, c := range s {
		presum[i+1][c-'0']++
		for j := 0; j < 10; j++ {
			presum[i+1][j] += presum[i][j]
		}
	}
	check := func(i, j int) bool {
		v := make(map[int]bool)
		for k := 0; k < 10; k++ {
			cnt := presum[j+1][k] - presum[i][k]
			if cnt > 0 {
				v[cnt] = true
			}
			if len(v) > 1 {
				return false
			}
		}
		return true
	}
	vis := make(map[string]bool)
	for i := 0; i < n; i++ {
		for j := i; j < n; j++ {
			if check(i, j) {
				vis[s[i:j+1]] = true
			}
		}
	}
	return len(vis)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
