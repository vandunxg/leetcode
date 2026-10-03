---
comments: true
difficulty: Easy
rating: 1295
source: Weekly Contest 287 Q1
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [2224. Minimum Number of Operations to Convert Time](https://leetcode.com/problems/minimum-number-of-operations-to-convert-time)

[中文文档](/solution/2200-2299/2224.Minimum%20Number%20of%20Operations%20to%20Convert%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi <code>current</code> và <code>correct</code>, biểu diễn hai thời điểm theo định dạng <strong>24 giờ</strong>.</p>

<p>Thời gian 24 giờ có định dạng <code>&quot;HH:MM&quot;</code>, trong đó <code>HH</code> nằm trong khoảng từ <code>00</code> đến <code>23</code>, còn <code>MM</code> nằm trong khoảng từ <code>00</code> đến <code>59</code>. Thời điểm sớm nhất là <code>00:00</code>, còn thời điểm muộn nhất là <code>23:59</code>.</p>

<p>Trong một thao tác, bạn có thể tăng thời gian <code>current</code> thêm <code>1</code>, <code>5</code>, <code>15</code> hoặc <code>60</code> phút. Bạn có thể thực hiện thao tác này <strong>bất kỳ</strong> số lần nào.</p>

<p>Trả về <em><strong>số thao tác ít nhất</strong> cần thiết để chuyển </em><code>current</code><em> thành </em><code>correct</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> current = &quot;02:30&quot;, correct = &quot;04:35&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:
</strong>Ta có thể chuyển current thành correct qua 3 thao tác như sau:
- Cộng thêm 60 phút vào current. current trở thành &quot;03:30&quot;.
- Cộng thêm 60 phút vào current. current trở thành &quot;04:30&quot;.
- Cộng thêm 5 phút vào current. current trở thành &quot;04:35&quot;.
Có thể chứng minh rằng không thể chuyển current thành correct với ít hơn 3 thao tác.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> current = &quot;11:00&quot;, correct = &quot;11:01&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta chỉ cần cộng thêm một phút vào current, nên số thao tác ít nhất cần thiết là 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>current</code> và <code>correct</code> có định dạng <code>&quot;HH:MM&quot;</code></li>
	<li><code>current &lt;= correct</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể cộng thêm $1$, $5$, $15$ hoặc $60$ phút và cần số lần cộng ít nhất để chuyển từ $current$ đến $correct$. Khoảng cách này nhỏ hơn một ngày, nên có thể dùng tìm kiếm knapsack, nhưng các bước tăng tạo thành một hệ mệnh giá chuẩn: ưu tiên giá trị lớn hơn không bao giờ bất lợi.
>
> Chuyển cả hai thời điểm thành số phút tính từ nửa đêm và gọi $d$ là hiệu dương của chúng. Lấy nhiều bước $60$ nhất có thể, sau đó lần lượt đến $15$, $5$ và $1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def convertTime(self, current: str, correct: str) -> int:
        a = int(current[:2]) * 60 + int(current[3:])
        b = int(correct[:2]) * 60 + int(correct[3:])
        ans, d = 0, b - a
        for i in [60, 15, 5, 1]:
            ans += d // i
            d %= i
        return ans
```

#### Java

```java
class Solution {
    public int convertTime(String current, String correct) {
        int a = Integer.parseInt(current.substring(0, 2)) * 60
            + Integer.parseInt(current.substring(3));
        int b = Integer.parseInt(correct.substring(0, 2)) * 60
            + Integer.parseInt(correct.substring(3));
        int ans = 0, d = b - a;
        for (int i : Arrays.asList(60, 15, 5, 1)) {
            ans += d / i;
            d %= i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int convertTime(string current, string correct) {
        int a = stoi(current.substr(0, 2)) * 60 + stoi(current.substr(3, 2));
        int b = stoi(correct.substr(0, 2)) * 60 + stoi(correct.substr(3, 2));
        int ans = 0, d = b - a;
        vector<int> inc = {60, 15, 5, 1};
        for (int i : inc) {
            ans += d / i;
            d %= i;
        }
        return ans;
    }
};
```

#### Go

```go
func convertTime(current string, correct string) int {
	parse := func(s string) int {
		h := int(s[0]-'0')*10 + int(s[1]-'0')
		m := int(s[3]-'0')*10 + int(s[4]-'0')
		return h*60 + m
	}
	a, b := parse(current), parse(correct)
	ans, d := 0, b-a
	for _, i := range []int{60, 15, 5, 1} {
		ans += d / i
		d %= i
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
