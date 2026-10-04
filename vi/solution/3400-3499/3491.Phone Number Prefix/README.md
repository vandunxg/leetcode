---
comments: true
difficulty: Easy
tags:
    - Trie
    - Array
    - String
    - Sorting
---

<!-- problem:start -->

# [3491. Phone Number Prefix 🔒](https://leetcode.com/problems/phone-number-prefix)

[中文文档](/solution/3400-3499/3491.Phone%20Number%20Prefix/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>numbers</code> biểu diễn các số điện thoại. Trả về <code>true</code> nếu không có số điện thoại nào là tiền tố của bất kỳ số điện thoại nào khác; ngược lại, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">numbers = [&quot;1&quot;,&quot;2&quot;,&quot;4&quot;,&quot;3&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số nào là tiền tố của số khác, nên kết quả là <code>true</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">numbers = [&quot;001&quot;,&quot;007&quot;,&quot;15&quot;,&quot;00153&quot;]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chuỗi <code>&quot;001&quot;</code> là tiền tố của chuỗi <code>&quot;00153&quot;</code>. Vì vậy, kết quả là <code>false</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= numbers.length &lt;= 50</code></li>
	<li><code>1 &lt;= numbers[i].length &lt;= 50</code></li>
	<li>Tất cả các số chỉ chứa các chữ số <code>&#39;0&#39;</code> đến <code>&#39;9&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Kiểm tra tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần xác định liệu có số nào là tiền tố của số khác hay không. Có nhiều nhất $50$ chuỗi với độ dài $50$, nên việc sắp xếp rồi kiểm tra từng cặp là đủ.
>
> Chỉ chuỗi ngắn hơn mới có thể là tiền tố, vì vậy ta sắp xếp theo độ dài và so sánh mỗi $s$ với các chuỗi đứng trước nó.
>
> Nếu có một chuỗi $t$ đứng trước thỏa mãn $s.\textit{startswith}(t)$, ta trả về false.

<!-- thinking:end -->

Trước hết, ta sắp xếp mảng $\textit{numbers}$ theo độ dài của chuỗi. Sau đó, ta duyệt qua từng chuỗi $\textit{s}$ trong mảng và kiểm tra xem có chuỗi $\textit{t}$ nào đứng trước là tiền tố của $\textit{s}$ hay không. Nếu tồn tại chuỗi như vậy, nghĩa là có một chuỗi là tiền tố của chuỗi khác, nên ta trả về $\textit{false}$. Nếu đã kiểm tra tất cả các chuỗi mà không tìm thấy quan hệ tiền tố nào, ta trả về $\textit{true}$.

Độ phức tạp thời gian là $O(n^2 \times m + n \times \log n)$, và độ phức tạp không gian là $O(m + \log n)$, trong đó $n$ là độ dài của mảng $\textit{numbers}$, còn $m$ là độ dài trung bình của các chuỗi trong mảng $\textit{numbers}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def phonePrefix(self, numbers: List[str]) -> bool:
        numbers.sort(key=len)
        for i, s in enumerate(numbers):
            if any(s.startswith(t) for t in numbers[:i]):
                return False
        return True
```

#### Java

```java
class Solution {
    public boolean phonePrefix(String[] numbers) {
        Arrays.sort(numbers, (a, b) -> Integer.compare(a.length(), b.length()));
        for (int i = 0; i < numbers.length; i++) {
            String s = numbers[i];
            for (int j = 0; j < i; j++) {
                if (s.startsWith(numbers[j])) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

#### C++

```cpp
#include <ranges>

class Solution {
public:
    bool phonePrefix(vector<string>& numbers) {
        ranges::sort(numbers, [](const string& a, const string& b) {
            return a.size() < b.size();
        });
        for (int i = 0; i < numbers.size(); i++) {
            if (ranges::any_of(numbers | views::take(i), [&](const string& t) {
                    return numbers[i].starts_with(t);
                })) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func phonePrefix(numbers []string) bool {
	sort.Slice(numbers, func(i, j int) bool {
		return len(numbers[i]) < len(numbers[j])
	})
	for i, s := range numbers {
		for _, t := range numbers[:i] {
			if strings.HasPrefix(s, t) {
				return false
			}
		}
	}
	return true
}
```

#### TypeScript

```ts
function phonePrefix(numbers: string[]): boolean {
    numbers.sort((a, b) => a.length - b.length);
    for (let i = 0; i < numbers.length; i++) {
        for (let j = 0; j < i; j++) {
            if (numbers[i].startsWith(numbers[j])) {
                return false;
            }
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
