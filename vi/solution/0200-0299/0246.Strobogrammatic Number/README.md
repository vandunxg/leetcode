---
comments: true
difficulty: Easy
tags:
    - Hash Table
    - Two Pointers
    - String
---

<!-- problem:start -->

# [246. Strobogrammatic Number 🔒](https://leetcode.com/problems/strobogrammatic-number)

[中文文档](/solution/0200-0299/0246.Strobogrammatic%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>num</code> biểu diễn một số nguyên, hãy trả về <code>true</code> <em>nếu</em> <code>num</code> <em>là một <strong>số strobogrammatic</strong></em>.</p>

<p><strong>Số strobogrammatic</strong> là số trông giống nhau khi xoay <code>180</code> độ (nhìn ngược từ dưới lên).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;69&quot;
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;88&quot;
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;962&quot;
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num.length &lt;= 50</code></li>
	<li><code>num</code> chỉ gồm các chữ số.</li>
	<li><code>num</code> không có số 0 ở đầu, trừ trường hợp chính nó là số 0.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng bằng two pointers

<!-- thinking:start -->

> **Tư duy**
>
> Một số strobogrammatic khi xoay $180^\circ$ vẫn đọc như cũ: $0,1,8$ giữ nguyên, còn $6$ đổi thành $9$ và ngược lại. Dùng hai con trỏ để kiểm tra từng cặp chữ số theo quy tắc này.

<!-- thinking:end -->

Ta định nghĩa mảng $d$, trong đó $d[i]$ là chữ số thu được khi xoay chữ số $i$ đi 180°. Nếu $d[i]$ bằng $-1$, nghĩa là không thể xoay chữ số $i$ đi 180° để được một chữ số hợp lệ.

Ta dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến hai đầu trái và phải của chuỗi. Sau đó, di chuyển hai con trỏ về phía giữa, đồng thời kiểm tra $d[num[i]]$ có bằng $num[j]$ hay không. Nếu không bằng nhau, chuỗi không phải số strobogrammatic nên ta có thể trả về $false$ ngay. Nếu $i > j$, tức là đã duyệt hết chuỗi, ta trả về $true$.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isStrobogrammatic(self, num: str) -> bool:
        d = [0, 1, -1, -1, -1, -1, 9, -1, 8, 6]
        i, j = 0, len(num) - 1
        while i <= j:
            a, b = int(num[i]), int(num[j])
            if d[a] != b:
                return False
            i, j = i + 1, j - 1
        return True
```

#### Java

```java
class Solution {
    public boolean isStrobogrammatic(String num) {
        int[] d = new int[] {0, 1, -1, -1, -1, -1, 9, -1, 8, 6};
        for (int i = 0, j = num.length() - 1; i <= j; ++i, --j) {
            int a = num.charAt(i) - '0', b = num.charAt(j) - '0';
            if (d[a] != b) {
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
    bool isStrobogrammatic(string num) {
        vector<int> d = {0, 1, -1, -1, -1, -1, 9, -1, 8, 6};
        for (int i = 0, j = num.size() - 1; i <= j; ++i, --j) {
            int a = num[i] - '0', b = num[j] - '0';
            if (d[a] != b) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isStrobogrammatic(num string) bool {
	d := []int{0, 1, -1, -1, -1, -1, 9, -1, 8, 6}
	for i, j := 0, len(num)-1; i <= j; i, j = i+1, j-1 {
		a, b := int(num[i]-'0'), int(num[j]-'0')
		if d[a] != b {
			return false
		}
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
