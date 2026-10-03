---
comments: true
difficulty: Easy
rating: 1322
source: Weekly Contest 316 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [2446. Determine if Two Events Have Conflict](https://leetcode.com/problems/determine-if-two-events-have-conflict)

[中文文档](/solution/2400-2499/2446.Determine%20if%20Two%20Events%20Have%20Conflict/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng chuỗi biểu diễn hai sự kiện bao gồm xảy ra <strong>trong cùng một ngày</strong>, <code>event1</code> và <code>event2</code>, trong đó:</p>

<ul>
	<li><code>event1 = [startTime<sub>1</sub>, endTime<sub>1</sub>]</code> và</li>
	<li><code>event2 = [startTime<sub>2</sub>, endTime<sub>2</sub>]</code>.</li>
</ul>

<p>Thời gian của các sự kiện là thời gian hợp lệ theo định dạng 24 giờ, có dạng <code>HH:MM</code>.</p>

<p>Hai sự kiện <strong>xung đột</strong> khi chúng có phần giao không rỗng (nghĩa là có một thời điểm cùng thuộc cả hai sự kiện).</p>

<p>Hãy trả về <code>true</code><em> nếu hai sự kiện xung đột. Nếu không, hãy trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> event1 = [&quot;01:15&quot;,&quot;02:00&quot;], event2 = [&quot;02:00&quot;,&quot;03:00&quot;]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hai sự kiện giao nhau tại thời điểm 2:00.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> event1 = [&quot;01:00&quot;,&quot;02:00&quot;], event2 = [&quot;01:20&quot;,&quot;03:00&quot;]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Hai sự kiện giao nhau trong khoảng từ 01:20 đến 02:00.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> event1 = [&quot;10:00&quot;,&quot;11:00&quot;], event2 = [&quot;14:00&quot;,&quot;15:00&quot;]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Hai sự kiện không giao nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>event1.length == event2.length == 2</code></li>
	<li><code>event1[i].length == event2[i].length == 5</code></li>
	<li><code>startTime<sub>1</sub> &lt;= endTime<sub>1</sub></code></li>
	<li><code>startTime<sub>2</sub> &lt;= endTime<sub>2</sub></code></li>
	<li>Tất cả thời gian của các sự kiện đều theo định dạng <code>HH:MM</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: So sánh chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Các chuỗi $\texttt{HH:MM}$ có thể được so sánh theo thứ tự thời gian. Hai khoảng không giao nhau khi một khoảng nằm hoàn toàn trước khoảng còn lại: $event1[0]>event2[1]$ hoặc $event1[1]<event2[0]$. Phủ định của điều kiện này chính là có xung đột.

<!-- thinking:end -->

Nếu thời gian bắt đầu của $event1$ muộn hơn thời gian kết thúc của $event2$, hoặc thời gian kết thúc của $event1$ sớm hơn thời gian bắt đầu của $event2$, thì hai sự kiện không xung đột. Ngược lại, hai sự kiện sẽ xung đột.

<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2446.Determine%20if%20Two%20Events%20Have%20Conflict/images/event.png" />

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def haveConflict(self, event1: List[str], event2: List[str]) -> bool:
        return not (event1[0] > event2[1] or event1[1] < event2[0])
```

#### Java

```java
class Solution {
    public boolean haveConflict(String[] event1, String[] event2) {
        return !(event1[0].compareTo(event2[1]) > 0 || event1[1].compareTo(event2[0]) < 0);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool haveConflict(vector<string>& event1, vector<string>& event2) {
        return !(event1[0] > event2[1] || event1[1] < event2[0]);
    }
};
```

#### Go

```go
func haveConflict(event1 []string, event2 []string) bool {
	return !(event1[0] > event2[1] || event1[1] < event2[0])
}
```

#### TypeScript

```ts
function haveConflict(event1: string[], event2: string[]): boolean {
    return !(event1[0] > event2[1] || event1[1] < event2[0]);
}
```

#### Rust

```rust
impl Solution {
    pub fn have_conflict(event1: Vec<String>, event2: Vec<String>) -> bool {
        !(event1[1] < event2[0] || event1[0] > event2[1])
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
