---
comments: true
difficulty: Medium
rating: 1622
source: Biweekly Contest 66 Q2
tags:
    - Greedy
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2086. Minimum Number of Food Buckets to Feed the Hamsters](https://leetcode.com/problems/minimum-number-of-food-buckets-to-feed-the-hamsters)

[中文文档](/solution/2000-2099/2086.Minimum%20Number%20of%20Food%20Buckets%20to%20Feed%20the%20Hamsters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <strong>được đánh chỉ số từ 0</strong> <code>hamsters</code>, trong đó <code>hamsters[i]</code> là một trong các giá trị sau:</p>

<ul>
	<li><code>&#39;H&#39;</code> cho biết có một hamster ở chỉ số <code>i</code>, hoặc</li>
	<li><code>&#39;.&#39;</code> cho biết chỉ số <code>i</code> đang trống.</li>
</ul>

<p>Bạn sẽ đặt một số xô thức ăn tại các chỉ số trống để cho hamster ăn. Một hamster có thể ăn nếu có ít nhất một xô thức ăn ở bên trái hoặc bên phải của nó. Cụ thể hơn, hamster ở chỉ số <code>i</code> có thể ăn nếu bạn đặt một xô thức ăn tại chỉ số <code>i - 1</code> <strong>và/hoặc</strong> tại chỉ số <code>i + 1</code>.</p>

<p>Trả về <em>số lượng xô thức ăn nhỏ nhất bạn cần <strong>đặt tại các chỉ số trống</strong> để cho tất cả hamster ăn, hoặc </em><code>-1</code><em> nếu không thể cho tất cả hamster ăn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2086.Minimum%20Number%20of%20Food%20Buckets%20to%20Feed%20the%20Hamsters/images/example1.png" style="width: 482px; height: 162px;" />
<pre>
<strong>Đầu vào:</strong> hamsters = &quot;H..H&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Đặt hai xô thức ăn tại các chỉ số 1 và 2.
Có thể chứng minh rằng nếu chỉ đặt một xô thức ăn, một trong các hamster sẽ không được ăn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2086.Minimum%20Number%20of%20Food%20Buckets%20to%20Feed%20the%20Hamsters/images/example2.png" style="width: 602px; height: 162px;" />
<pre>
<strong>Đầu vào:</strong> hamsters = &quot;.H.H.&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Đặt một xô thức ăn tại chỉ số 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2086.Minimum%20Number%20of%20Food%20Buckets%20to%20Feed%20the%20Hamsters/images/example3.png" style="width: 602px; height: 162px;" />
<pre>
<strong>Đầu vào:</strong> hamsters = &quot;.HHH.&quot;
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Nếu đặt một xô thức ăn tại mọi chỉ số trống như hình, hamster ở chỉ số 2 sẽ không thể ăn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= hamsters.length &lt;= 10<sup>5</sup></code></li>
	<li><code>hamsters[i]</code> là <code>&#39;H&#39;</code> hoặc <code>&#39;.&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hamster cần một xô thức ăn kề bên tại một ô trống. Chiến lược tham lam từ trái sang phải ưu tiên ô trống bên phải vì xô ở đó có thể cho hamster tiếp theo ăn; nếu không thì chọn bên trái; nếu cả hai đều không được thì thất bại.
>
> Khi đặt xô bên phải, ta bỏ qua thêm một chỉ số để không sử dụng lại xô đó sai cách. Duyệt tuyến tính một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumBuckets(self, street: str) -> int:
        ans = 0
        i, n = 0, len(street)
        while i < n:
            if street[i] == 'H':
                if i + 1 < n and street[i + 1] == '.':
                    i += 2
                    ans += 1
                elif i and street[i - 1] == '.':
                    ans += 1
                else:
                    return -1
            i += 1
        return ans
```

#### Java

```java
class Solution {
    public int minimumBuckets(String street) {
        int n = street.length();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (street.charAt(i) == 'H') {
                if (i + 1 < n && street.charAt(i + 1) == '.') {
                    ++ans;
                    i += 2;
                } else if (i > 0 && street.charAt(i - 1) == '.') {
                    ++ans;
                } else {
                    return -1;
                }
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
    int minimumBuckets(string street) {
        int n = street.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (street[i] == 'H') {
                if (i + 1 < n && street[i + 1] == '.') {
                    ++ans;
                    i += 2;
                } else if (i && street[i - 1] == '.') {
                    ++ans;
                } else {
                    return -1;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumBuckets(street string) int {
	ans, n := 0, len(street)
	for i := 0; i < n; i++ {
		if street[i] == 'H' {
			if i+1 < n && street[i+1] == '.' {
				ans++
				i += 2
			} else if i > 0 && street[i-1] == '.' {
				ans++
			} else {
				return -1
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
