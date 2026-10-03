---
comments: true
difficulty: Medium
rating: 1494
source: Biweekly Contest 92 Q3
tags:
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [2483. Minimum Penalty for a Shop](https://leetcode.com/problems/minimum-penalty-for-a-shop)

[中文文档](/solution/2400-2499/2483.Minimum%20Penalty%20for%20a%20Shop/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp log khách đến một cửa hàng, được biểu diễn bằng một chuỗi <code>customers</code> <strong>đánh chỉ số từ 0</strong>, chỉ gồm các ký tự <code>&#39;N&#39;</code> và <code>&#39;Y&#39;</code>:</p>

<ul>
	<li>nếu ký tự <code>i<sup>th</sup></code> là <code>&#39;Y&#39;</code>, nghĩa là có khách đến vào giờ thứ <code>i<sup>th</sup></code></li>
	<li>ngược lại, <code>&#39;N&#39;</code> cho biết không có khách đến vào giờ thứ <code>i<sup>th</sup></code>.</li>
</ul>

<p>Nếu cửa hàng đóng cửa vào giờ <code>j<sup>th</sup></code> (<code>0 &lt;= j &lt;= n</code>), <strong>mức phạt</strong> được tính như sau:</p>

<ul>
	<li>Với mỗi giờ cửa hàng mở cửa nhưng không có khách đến, mức phạt tăng thêm <code>1</code>.</li>
	<li>Với mỗi giờ cửa hàng đóng cửa nhưng có khách đến, mức phạt tăng thêm <code>1</code>.</li>
</ul>

<p>Hãy trả về <em>giờ <strong>sớm nhất</strong> mà cửa hàng phải đóng cửa để nhận <strong>mức phạt nhỏ nhất</strong>.</em></p>

<p><strong>Lưu ý</strong> rằng nếu cửa hàng đóng cửa vào giờ <code>j<sup>th</sup></code>, điều đó có nghĩa là cửa hàng đóng cửa trong giờ <code>j</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> customers = &quot;YYNY&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- Đóng cửa vào giờ thứ 0<sup>th</sup> có mức phạt 1+1+0+1 = 3.
- Đóng cửa vào giờ thứ 1<sup>st</sup> có mức phạt 0+1+0+1 = 2.
- Đóng cửa vào giờ thứ 2<sup>nd</sup> có mức phạt 0+0+0+1 = 1.
- Đóng cửa vào giờ thứ 3<sup>rd</sup> có mức phạt 0+0+1+1 = 2.
- Đóng cửa vào giờ thứ 4<sup>th</sup> có mức phạt 0+0+1+0 = 1.
Đóng cửa vào giờ thứ 2<sup>nd</sup> hoặc giờ thứ 4<sup>th</sup> đều cho mức phạt nhỏ nhất. Vì 2 sớm hơn, thời điểm đóng cửa tối ưu là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> customers = &quot;NNNNN&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Tốt nhất là đóng cửa vào giờ thứ 0<sup>th</sup> vì không có khách nào đến.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> customers = &quot;YYYY&quot;
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Tốt nhất là đóng cửa vào giờ thứ 4<sup>th</sup> vì giờ nào cũng có khách đến.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= customers.length &lt;= 10<sup>5</sup></code></li>
	<li><code>customers</code> chỉ gồm các ký tự <code>&#39;Y&#39;</code> và <code>&#39;N&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Đóng cửa vào giờ $j$ tốn một đơn vị cho mỗi `N` ở trước và một đơn vị cho mỗi `Y` ở sau. Vì $n\le 10^5$, ta bắt đầu từ việc đóng cửa vào giờ $0$ (tất cả `Y`). Mỗi khi dời thời điểm đóng cửa thêm một giờ, mức phạt tăng $1$ nếu ký tự đó là `N` và giảm $1$ nếu là `Y`. Ta giữ lại $j$ sớm nhất có mức phạt nhỏ nhất.

<!-- thinking:end -->

Nếu cửa hàng đóng cửa vào giờ $0$, chi phí là số ký tự `'Y'` trong $\textit{customers}$. Ta khởi tạo biến đáp án $\textit{ans}$ bằng $0$ và biến chi phí $\textit{cost}$ bằng số ký tự `'Y'` trong $\textit{customers}$.

Tiếp theo, ta liệt kê thời điểm cửa hàng đóng cửa vào giờ $j$ ($1 \leq j \leq n$). Nếu $\textit{customers}[j - 1]$ là `'N'`, điều đó có nghĩa là không có khách đến trong khoảng thời gian cửa hàng mở cửa, nên chi phí tăng $1$; ngược lại, có khách đến trong khoảng thời gian cửa hàng đóng cửa, nên chi phí giảm $1$. Nếu chi phí hiện tại $\textit{cost}$ nhỏ hơn chi phí nhỏ nhất $\textit{mn}$, ta cập nhật biến đáp án $\textit{ans}$ thành $j$ và cập nhật chi phí nhỏ nhất $\textit{mn}$ thành chi phí hiện tại $\textit{cost}$.

Sau khi duyệt xong, ta trả về biến đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $\textit{customers}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def bestClosingTime(self, customers: str) -> int:
        ans = 0
        mn = cost = customers.count("Y")
        for j, c in enumerate(customers, 1):
            cost += 1 if c == "N" else -1
            if cost < mn:
                ans, mn = j, cost
        return ans
```

#### Java

```java
class Solution {
    public int bestClosingTime(String customers) {
        int n = customers.length();
        int ans = 0, cost = 0;
        for (int i = 0; i < n; i++) {
            if (customers.charAt(i) == 'Y') {
                cost++;
            }
        }
        int mn = cost;
        for (int j = 1; j <= n; j++) {
            cost += customers.charAt(j - 1) == 'N' ? 1 : -1;
            if (cost < mn) {
                ans = j;
                mn = cost;
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
    int bestClosingTime(string customers) {
        int ans = 0;
        int cost = 0;
        for (char c : customers) {
            if (c == 'Y') {
                cost++;
            }
        }
        int mn = cost;
        for (int j = 1; j <= customers.size(); ++j) {
            cost += customers[j - 1] == 'N' ? 1 : -1;
            if (cost < mn) {
                ans = j;
                mn = cost;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func bestClosingTime(customers string) int {
	ans := 0
	cost := strings.Count(customers, "Y")
	mn := cost
	for j := 1; j <= len(customers); j++ {
		c := customers[j-1]
		if c == 'N' {
			cost++
		} else {
			cost--
		}
		if cost < mn {
			ans = j
			mn = cost
		}
	}
	return ans
}
```

#### TypeScript

```ts
function bestClosingTime(customers: string): number {
    let ans = 0;
    let cost = 0;
    for (const ch of customers) {
        if (ch === 'Y') {
            cost++;
        }
    }
    let mn = cost;

    for (let j = 1; j <= customers.length; j++) {
        const c = customers[j - 1];
        cost += c === 'N' ? 1 : -1;
        if (cost < mn) {
            mn = cost;
            ans = j;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn best_closing_time(customers: String) -> i32 {
        let bytes = customers.as_bytes();

        let mut cost: i32 = bytes.iter().filter(|&&c| c == b'Y').count() as i32;
        let mut mn = cost;
        let mut ans: i32 = 0;

        for j in 1..=bytes.len() {
            let c = bytes[j - 1];
            if c == b'N' {
                cost += 1;
            } else {
                cost -= 1;
            }
            if cost < mn {
                mn = cost;
                ans = j as i32;
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int BestClosingTime(string customers) {
        int n = customers.Length;
        int ans = 0, cost = 0;
        for (int i = 0; i < n; i++) {
            if (customers[i] == 'Y') {
                cost++;
            }
        }
        int mn = cost;
        for (int j = 1; j <= n; j++) {
            cost += customers[j - 1] == 'N' ? 1 : -1;
            if (cost < mn) {
                ans = j;
                mn = cost;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
