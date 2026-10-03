---
comments: true
difficulty: Medium
rating: 1280
source: Weekly Contest 274 Q2
tags:
    - Array
    - Math
    - String
    - Matrix
---

<!-- problem:start -->

# [2125. Number of Laser Beams in a Bank](https://leetcode.com/problems/number-of-laser-beams-in-a-bank)

[中文文档](/solution/2100-2199/2125.Number%20of%20Laser%20Beams%20in%20a%20Bank/README.md)

## Mô tả

<!-- description:start -->

<p>Các thiết bị an ninh chống trộm được kích hoạt bên trong một ngân hàng. Bạn được cho một mảng chuỗi nhị phân <code>bank</code> <strong>được đánh số từ 0</strong>, biểu diễn sơ đồ mặt bằng của ngân hàng, là một ma trận 2D <code>m x n</code>. <code>bank[i]</code> biểu diễn hàng thứ <code>i<sup>th</sup></code>, chỉ gồm các ký tự <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>. <code>&#39;0&#39;</code> có nghĩa là ô trống, còn <code>&#39;1&#39;</code> có nghĩa là ô có thiết bị an ninh.</p>

<p>Có <strong>một</strong> tia laser giữa <strong>hai</strong> thiết bị an ninh bất kỳ <strong>nếu cả hai</strong> điều kiện sau được thỏa mãn:</p>

<ul>
	<li>Hai thiết bị nằm trên <strong>hai hàng khác nhau</strong>: <code>r<sub>1</sub></code> và <code>r<sub>2</sub></code>, trong đó <code>r<sub>1</sub> &lt; r<sub>2</sub></code>.</li>
	<li>Với <strong>mỗi</strong> hàng <code>i</code> thỏa mãn <code>r<sub>1</sub> &lt; i &lt; r<sub>2</sub></code>, hàng thứ <code>i<sup>th</sup></code> <strong>không có thiết bị an ninh nào</strong>.</li>
</ul>

<p>Các tia laser độc lập với nhau, tức là một tia không gây ảnh hưởng hoặc hợp nhất với tia khác.</p>

<p>Hãy trả về <em>tổng số tia laser trong ngân hàng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2125.Number%20of%20Laser%20Beams%20in%20a%20Bank/images/laser1.jpg" style="width: 400px; height: 368px;" />
<pre>
<strong>Đầu vào:</strong> bank = [&quot;011001&quot;,&quot;000000&quot;,&quot;010100&quot;,&quot;001000&quot;]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Giữa mỗi cặp thiết bị sau đây có một tia laser. Tổng cộng có 8 tia laser:
 * bank[0][1] -- bank[2][1]
 * bank[0][1] -- bank[2][3]
 * bank[0][2] -- bank[2][1]
 * bank[0][2] -- bank[2][3]
 * bank[0][5] -- bank[2][1]
 * bank[0][5] -- bank[2][3]
 * bank[2][1] -- bank[3][2]
 * bank[2][3] -- bank[3][2]
 Lưu ý rằng không có tia laser nào giữa một thiết bị bất kỳ trên hàng thứ 0<sup>th</sup> và một thiết bị bất kỳ trên hàng thứ 3<sup>rd</sup>.
 Điều này là do hàng thứ 2<sup>nd</sup> có các thiết bị an ninh, vi phạm điều kiện thứ hai.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2100-2199/2125.Number%20of%20Laser%20Beams%20in%20a%20Bank/images/laser2.jpg" style="width: 244px; height: 325px;" />
<pre>
<strong>Đầu vào:</strong> bank = [&quot;000&quot;,&quot;111&quot;,&quot;000&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không tồn tại hai thiết bị nào nằm trên hai hàng khác nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>m == bank.length</code></li>
	<li><code>n == bank[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 500</code></li>
	<li><code>bank[i][j]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm theo từng hàng

<!-- thinking:start -->

> **Tư duy**
>
> Một tia laser tồn tại giữa hai hàng có thiết bị chỉ khi mọi hàng nằm giữa chúng đều trống. Việc liệt kê các cặp hàng và quét các hàng ở giữa có độ phức tạp bậc hai theo số hàng.
>
> Số tia laser giữa hai hàng không trống liên tiếp chính xác là tích số thiết bị của hai hàng; các hàng trống có thể được bỏ qua. Vì vậy, ta đếm số ký tự `'1'` trên mỗi hàng và ghi nhớ số thiết bị của hàng không trống trước đó.
>
> Với mỗi hàng có $\textit{cur}>0$, ta cộng $\textit{pre}\times\textit{cur}$ rồi cập nhật $\textit{pre}$.

<!-- thinking:end -->

Ta có thể đếm số thiết bị an ninh theo từng hàng. Nếu hàng hiện tại không có thiết bị an ninh nào, ta bỏ qua hàng đó. Ngược lại, ta nhân số thiết bị an ninh trong hàng hiện tại với số thiết bị an ninh trong hàng trước đó rồi cộng vào đáp án. Sau đó, ta cập nhật số thiết bị an ninh của hàng trước đó thành số thiết bị an ninh trong hàng hiện tại.

Độ phức tạp thời gian là $O(m \times n)$, trong đó $m$ và $n$ lần lượt là số hàng và số cột. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfBeams(self, bank: List[str]) -> int:
        ans = pre = 0
        for row in bank:
            if (cur := row.count("1")) > 0:
                ans += pre * cur
                pre = cur
        return ans
```

#### Java

```java
class Solution {
    public int numberOfBeams(String[] bank) {
        int ans = 0, pre = 0;
        for (String row : bank) {
            int cur = 0;
            for (int i = 0; i < row.length(); ++i) {
                cur += row.charAt(i) - '0';
            }
            if (cur > 0) {
                ans += pre * cur;
                pre = cur;
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
    int numberOfBeams(vector<string>& bank) {
        int ans = 0, pre = 0;
        for (auto& row : bank) {
            int cur = count(row.begin(), row.end(), '1');
            if (cur) {
                ans += pre * cur;
                pre = cur;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfBeams(bank []string) (ans int) {
	pre := 0
	for _, row := range bank {
		if cur := strings.Count(row, "1"); cur > 0 {
			ans += pre * cur
			pre = cur
		}
	}
	return
}
```

#### TypeScript

```ts
function numberOfBeams(bank: string[]): number {
    let [ans, pre] = [0, 0];
    for (const row of bank) {
        const cur = row.split('1').length - 1;
        if (cur) {
            ans += pre * cur;
            pre = cur;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_beams(bank: Vec<String>) -> i32 {
        let mut ans = 0;
        let mut pre = 0;
        for row in bank {
            let cur = row.chars().filter(|&c| c == '1').count() as i32;
            if cur > 0 {
                ans += pre * cur;
                pre = cur;
            }
        }
        ans
    }
}
```

#### C

```c
int numberOfBeams(char** bank, int bankSize) {
    int ans = 0, pre = 0;
    for (int i = 0; i < bankSize; ++i) {
        int cur = 0;
        for (int j = 0; bank[i][j] != '\0'; ++j) {
            if (bank[i][j] == '1') {
                cur++;
            }
        }
        if (cur) {
            ans += pre * cur;
            pre = cur;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
