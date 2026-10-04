---
comments: true
difficulty: Easy
rating: 1198
source: Biweekly Contest 104 Q1
tags:
    - Array
    - String
---

<!-- problem:start -->

# [2678. Number of Senior Citizens](https://leetcode.com/problems/number-of-senior-citizens)

[中文文档](/solution/2600-2699/2678.Number%20of%20Senior%20Citizens/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng chuỗi <code>details</code> <strong>đánh chỉ số từ 0</strong>. Mỗi phần tử trong <code>details</code> chứa thông tin của một hành khách, được nén thành một chuỗi có độ dài <code>15</code>. Hệ thống quy định:</p>

<ul>
	<li>Mười ký tự đầu tiên là số điện thoại của hành khách.</li>
	<li>Ký tự tiếp theo cho biết giới tính của người đó.</li>
	<li>Hai ký tự sau đó cho biết tuổi của người đó.</li>
	<li>Hai ký tự cuối cùng xác định số ghế được phân cho người đó.</li>
</ul>

<p><em>Trả về số hành khách <strong>trên </strong><strong>60 tuổi</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> details = [&quot;7868190130M7522&quot;,&quot;5303914400F9211&quot;,&quot;9273338290F4010&quot;]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hành khách ở các chỉ số 0, 1 và 2 lần lượt có tuổi là 75, 92 và 40. Vì vậy, có 2 người lớn hơn 60 tuổi.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> details = [&quot;1313579440F2036&quot;,&quot;2921522980M5644&quot;]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có hành khách nào lớn hơn 60 tuổi.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= details.length &lt;= 100</code></li>
	<li><code>details[i].length == 15</code></li>
	<li><code>details[i] consists of digits from &#39;0&#39; to &#39;9&#39;.</code></li>
	<li><code>details[i][10] is either &#39;M&#39; or &#39;F&#39; or &#39;O&#39;.</code></li>
	<li>Số điện thoại và số ghế của các hành khách là khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt và đếm

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bản ghi có độ dài cố định và tuổi nằm ở các ký tự $12$ và $13$. Chỉ cần phân tích hai chữ số đó rồi so sánh với $60$ để đếm số người cao tuổi mà không cần đọc các trường khác.

<!-- thinking:end -->

Ta có thể duyệt qua từng chuỗi $x$ trong `details`, chuyển các ký tự thứ $12$ và $13$ (được đánh chỉ số lần lượt là $11$ và $12$) của $x$ thành số nguyên, rồi kiểm tra xem chúng có lớn hơn $60$ hay không. Nếu có, ta tăng đáp án lên một.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của `details`. Độ phức tạp không gian là $O(1)`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSeniors(self, details: List[str]) -> int:
        return sum(int(x[11:13]) > 60 for x in details)
```

#### Java

```java
class Solution {
    public int countSeniors(String[] details) {
        int ans = 0;
        for (var x : details) {
            int age = Integer.parseInt(x.substring(11, 13));
            if (age > 60) {
                ++ans;
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
    int countSeniors(vector<string>& details) {
        int ans = 0;
        for (auto& x : details) {
            int age = stoi(x.substr(11, 2));
            ans += age > 60;
        }
        return ans;
    }
};
```

#### Go

```go
func countSeniors(details []string) (ans int) {
	for _, x := range details {
		age, _ := strconv.Atoi(x[11:13])
		if age > 60 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function countSeniors(details: string[]): number {
    return details.filter(x => +x.slice(11, 13) > 60).length;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_seniors(details: Vec<String>) -> i32 {
        details
            .iter()
            .filter_map(|s| s[11..13].parse::<i32>().ok())
            .filter(|&age| age > 60)
            .count() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
