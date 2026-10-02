---
comments: true
difficulty: Easy
tags:
    - String
---

<!-- problem:start -->

# [551. Student Attendance Record I](https://leetcode.com/problems/student-attendance-record-i)

[中文文档](/solution/0500-0599/0551.Student%20Attendance%20Record%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>s</code> biểu diễn hồ sơ điểm danh của một học sinh. Mỗi ký tự cho biết học sinh vắng mặt, đi muộn hay có mặt trong ngày đó. Hồ sơ chỉ gồm ba ký tự sau:</p>

<ul>
	<li><code>&#39;A&#39;</code>: Vắng mặt.</li>
	<li><code>&#39;L&#39;</code>: Đi muộn.</li>
	<li><code>&#39;P&#39;</code>: Có mặt.</li>
</ul>

<p>Học sinh đủ điều kiện nhận phần thưởng chuyên cần nếu đáp ứng <strong>cả hai</strong> tiêu chí sau:</p>

<ul>
	<li>Tổng số ngày học sinh vắng mặt (<code>&#39;A&#39;</code>) <strong>nhỏ hơn</strong> 2.</li>
	<li>Học sinh <strong>không bao giờ</strong> đi muộn (<code>&#39;L&#39;</code>) trong 3 ngày <strong>liên tiếp</strong> trở lên.</li>
</ul>

<p>Trả về <code>true</code><em> nếu học sinh đủ điều kiện nhận phần thưởng chuyên cần, ngược lại trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;PPALLP&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Học sinh vắng mặt ít hơn 2 ngày và không có lần nào đi muộn 3 ngày liên tiếp trở lên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;PPALLL&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Học sinh đi muộn 3 ngày liên tiếp trong 3 ngày cuối, nên không đủ điều kiện nhận phần thưởng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s[i]</code> là một trong các ký tự <code>&#39;A&#39;</code>, <code>&#39;L&#39;</code> hoặc <code>&#39;P&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hồ sơ hợp lệ có tối đa một `'A'` và không chứa ba `'L'` liên tiếp. Chỉ cần duyệt một lượt, không cần regular expression.
>
> Đếm số lần xuất hiện của `'A'` và kiểm tra chuỗi con `LLL`. Cả hai điều kiện đều phải thỏa mãn. Độ dài tối đa là $1000$, nên duyệt tuyến tính là đủ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkRecord(self, s: str) -> bool:
        return s.count('A') < 2 and 'LLL' not in s
```

#### Java

```java
class Solution {
    public boolean checkRecord(String s) {
        return s.indexOf("A") == s.lastIndexOf("A") && !s.contains("LLL");
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkRecord(string s) {
        return count(s.begin(), s.end(), 'A') < 2 && s.find("LLL") == string::npos;
    }
};
```

#### Go

```go
func checkRecord(s string) bool {
	return strings.Count(s, "A") < 2 && !strings.Contains(s, "LLL")
}
```

#### TypeScript

```ts
function checkRecord(s: string): boolean {
    return s.indexOf('A') === s.lastIndexOf('A') && s.indexOf('LLL') === -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
