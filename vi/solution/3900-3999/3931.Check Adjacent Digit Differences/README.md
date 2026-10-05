---
comments: true
difficulty: Easy
rating: 1165
source: Weekly Contest 502 Q1
tags:
    - String
---

<!-- problem:start -->

# [3931. Check Adjacent Digit Differences](https://leetcode.com/problems/check-adjacent-digit-differences)

[中文文档](/solution/3900-3999/3931.Check%20Adjacent%20Digit%20Differences/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ số.</p>

<p>Trả về <code>true</code> nếu <strong>độ chênh lệch tuyệt đối</strong> giữa mọi cặp chữ số <strong>liền kề</strong> không vượt quá 2, nếu không thì trả về <code>false</code>.</p>

<p>Độ chênh lệch tuyệt đối giữa <code>a</code> và <code>b</code> được định nghĩa là <code>abs(a - b)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;132&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Độ chênh lệch tuyệt đối giữa các chữ số tại <code>s[0]</code> và <code>s[1]</code> là <code>abs(1 - 3) = 2</code>.</li>
	<li>Độ chênh lệch tuyệt đối giữa các chữ số tại <code>s[1]</code> và <code>s[2]</code> là <code>abs(3 - 2) = 1</code>.</li>
	<li>Vì cả hai độ chênh lệch đều không vượt quá 2, đáp án là true.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;129&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Độ chênh lệch tuyệt đối giữa các chữ số tại <code>s[0]</code> và <code>s[1]</code> là <code>abs(1 - 2) = 1</code>.</li>
	<li>Độ chênh lệch tuyệt đối giữa các chữ số tại <code>s[1]</code> và <code>s[2]</code> là <code>abs(2 - 9) = 7</code>, lớn hơn 2.</li>
	<li>Do đó, đáp án là false.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi có độ dài tối đa là $100$; ta chỉ cần kiểm tra mọi cặp chữ số liền kề có độ chênh lệch không vượt quá $2$. Chuyển các ký tự thành số nguyên rồi kiểm tra độ chênh lệch tuyệt đối của từng cặp $\textit{pairwise}$.
>
> Không cần cấu trúc dữ liệu bổ sung ngoài một lần duyệt tuyến tính.

<!-- thinking:end -->

Ta có thể mô phỏng quá trình được mô tả trong đề bài: duyệt qua từng cặp chữ số liền kề trong chuỗi và tính độ chênh lệch tuyệt đối của chúng. Nếu có bất kỳ cặp nào có độ chênh lệch tuyệt đối lớn hơn 2, trả về $\text{false}$. Nếu duyệt xong mà không tìm thấy cặp nào như vậy, trả về $\text{true}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isAdjacentDiffAtMostTwo(self, s: str) -> bool:
        return all(abs(x - y) <= 2 for x, y in pairwise(map(int, list(s))))
```

#### Java

```java
class Solution {
    public boolean isAdjacentDiffAtMostTwo(String s) {
        for (int i = 1; i < s.length(); ++i) {
            if (Math.abs(s.charAt(i - 1) - s.charAt(i)) > 2) {
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
    bool isAdjacentDiffAtMostTwo(string s) {
        for (int i = 1; i < s.size(); ++i) {
            if (abs(s[i - 1] - s[i]) > 2) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isAdjacentDiffAtMostTwo(s string) bool {
	for i := 1; i < len(s); i++ {
		if abs(int(s[i-1])-int(s[i])) > 2 {
			return false
		}
	}
	return true
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function isAdjacentDiffAtMostTwo(s: string): boolean {
    for (let i = 1; i < s.length; i++) {
        if (Math.abs(Number(s[i]) - Number(s[i - 1])) > 2) {
            return false;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
