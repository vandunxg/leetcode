---
comments: true
difficulty: Medium
rating: 1908
source: Weekly Contest 145 Q3
tags:
    - Stack
    - Array
    - Hash Table
    - Prefix Sum
    - Monotonic Stack
---

<!-- problem:start -->

# [1124. Longest Well-Performing Interval](https://leetcode.com/problems/longest-well-performing-interval)

[中文文档](/solution/1100-1199/1124.Longest%20Well-Performing%20Interval/README.md)

## Mô tả

<!-- description:start -->

<p>Cho <code>hours</code>, danh sách số giờ làm việc mỗi ngày của một nhân viên.</p>

<p>Một ngày được xem là <em>ngày làm việc mệt mỏi</em> khi và chỉ khi số giờ làm việc lớn hơn nghiêm ngặt <code>8</code>.</p>

<p><em>Khoảng thời gian làm việc hiệu quả</em> là một đoạn ngày mà số ngày làm việc mệt mỏi lớn hơn nghiêm ngặt số ngày không mệt mỏi.</p>

<p>Hãy trả về độ dài của khoảng thời gian làm việc hiệu quả dài nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> hours = [9,9,6,0,6,6,9]
<strong>Đầu ra:</strong> 3
<strong>Giải thích: </strong>Khoảng thời gian làm việc hiệu quả dài nhất là [9,9,6].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> hours = [6,6,6]
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= hours.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= hours[i] &lt;= 16</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ánh xạ ngày làm việc mệt mỏi thành $+1$ và ngày còn lại thành $-1$. Một khoảng làm việc hiệu quả tương ứng với cặp prefix sum thỏa $s_j-s_i>0$. Thử mọi cặp sẽ có độ phức tạp bậc hai.
>
> Nếu $s>0$, toàn bộ prefix hiện tại thỏa mãn. Nếu không, ta chỉ cần tìm một giá trị $s-1$ xuất hiện trước đó, khi ấy tổng của đoạn bằng $1$. Một map lưu chỉ số xuất hiện đầu tiên của mỗi prefix sum để đoạn đó dài nhất có thể.

<!-- thinking:end -->

Ta dùng ý tưởng prefix sum và duy trì biến $s$, biểu thị chênh lệch giữa số "ngày làm việc mệt mỏi" và "ngày không mệt mỏi" từ chỉ số $0$ đến chỉ số hiện tại. Nếu $s > 0$, đoạn từ chỉ số $0$ đến chỉ số hiện tại là một "khoảng thời gian làm việc hiệu quả". Ngoài ra, ta dùng hash table $pos$ để ghi lại chỉ số xuất hiện đầu tiên của mỗi giá trị $s$.

Tiếp theo, ta duyệt mảng `hours`; với mỗi chỉ số $i$:

- Nếu $hours[i] > 8$, tăng $s$ thêm $1$; nếu không, giảm $s$ đi $1$.
- Nếu $s > 0$, đoạn từ chỉ số $0$ đến chỉ số hiện tại $i$ là một "khoảng thời gian làm việc hiệu quả", ta cập nhật kết quả $ans = i + 1$. Nếu không, nhưng $s - 1$ có trong hash table $pos$, đặt $j = pos[s - 1]$. Khi đó, đoạn từ chỉ số $j + 1$ đến chỉ số $i$ là một "khoảng thời gian làm việc hiệu quả", nên ta cập nhật kết quả $ans = \max(ans, i - j)$.
- Sau đó, nếu $s$ chưa có trong hash table $pos$, ta ghi nhận $pos[s] = i$.

Sau khi duyệt xong, trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng `hours`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestWPI(self, hours: List[int]) -> int:
        ans = s = 0
        pos = {}
        for i, x in enumerate(hours):
            s += 1 if x > 8 else -1
            if s > 0:
                ans = i + 1
            elif s - 1 in pos:
                ans = max(ans, i - pos[s - 1])
            if s not in pos:
                pos[s] = i
        return ans
```

#### Java

```java
class Solution {
    public int longestWPI(int[] hours) {
        int ans = 0, s = 0;
        Map<Integer, Integer> pos = new HashMap<>();
        for (int i = 0; i < hours.length; ++i) {
            s += hours[i] > 8 ? 1 : -1;
            if (s > 0) {
                ans = i + 1;
            } else if (pos.containsKey(s - 1)) {
                ans = Math.max(ans, i - pos.get(s - 1));
            }
            pos.putIfAbsent(s, i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestWPI(vector<int>& hours) {
        int ans = 0, s = 0;
        unordered_map<int, int> pos;
        for (int i = 0; i < hours.size(); ++i) {
            s += hours[i] > 8 ? 1 : -1;
            if (s > 0) {
                ans = i + 1;
            } else if (pos.count(s - 1)) {
                ans = max(ans, i - pos[s - 1]);
            }
            if (!pos.count(s)) {
                pos[s] = i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestWPI(hours []int) (ans int) {
	s := 0
	pos := map[int]int{}
	for i, x := range hours {
		if x > 8 {
			s++
		} else {
			s--
		}
		if s > 0 {
			ans = i + 1
		} else if j, ok := pos[s-1]; ok {
			ans = max(ans, i-j)
		}
		if _, ok := pos[s]; !ok {
			pos[s] = i
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
