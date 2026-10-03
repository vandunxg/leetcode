---
comments: true
difficulty: Hard
rating: 1960
source: Biweekly Contest 83 Q4
tags:
    - Greedy
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2350. Shortest Impossible Sequence of Rolls](https://leetcode.com/problems/shortest-impossible-sequence-of-rolls)

[中文文档](/solution/2300-2399/2350.Shortest%20Impossible%20Sequence%20of%20Rolls/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>rolls</code> có độ dài <code>n</code> và một số nguyên <code>k</code>. Bạn tung một viên xúc xắc <code>k</code> mặt được đánh số từ <code>1</code> đến <code>k</code> <code>n</code> lần, trong đó kết quả của lần tung <code>i<sup>th</sup></code> là <code>rolls[i]</code>.</p>

<p>Trả về<em> độ dài của chuỗi lần tung <strong>ngắn nhất</strong> sao cho không tồn tại <span data-keyword="subsequence-array">dãy con</span> như vậy trong </em><code>rolls</code>.</p>

<p>Một <strong>chuỗi lần tung</strong> có độ dài <code>len</code> là kết quả của việc tung một viên xúc xắc <code>k</code> mặt <code>len</code> lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> rolls = [4,2,1,2,3,3,2,4,1], k = 4
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Mọi chuỗi lần tung có độ dài 1, [1], [2], [3], [4], đều có thể được lấy từ rolls.
Mọi chuỗi lần tung có độ dài 2, [1, 1], [1, 2], ..., [4, 4], đều có thể được lấy từ rolls.
Chuỗi [1, 4, 2] không thể được lấy từ rolls, vì vậy ta trả về 3.
Lưu ý rằng còn có những chuỗi khác không thể được lấy từ rolls.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> rolls = [1,1,2,2], k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Mọi chuỗi lần tung có độ dài 1, [1], [2], đều có thể được lấy từ rolls.
Chuỗi [2, 1] không thể được lấy từ rolls, vì vậy ta trả về 2.
Lưu ý rằng còn có những chuỗi khác không thể được lấy từ rolls, nhưng [2, 1] là chuỗi ngắn nhất.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> rolls = [1,1,3,2,2,2,3,3], k = 4
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chuỗi [4] không thể được lấy từ rolls, vì vậy ta trả về 1.
Lưu ý rằng còn có những chuỗi khác không thể được lấy từ rolls, nhưng [4] là chuỗi ngắn nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == rolls.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= rolls[i] &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi có độ dài $L$ là dãy con của $rolls$ khi và chỉ khi ta có thể chọn được $L$ lượt liên tiếp, mỗi lượt bao phủ đủ các mặt từ $1..k$. Với $n \le 10^5$, việc liệt kê các chuỗi là không thể.
>
> Duyệt từ trái sang phải và thu thập các mặt chưa xuất hiện. Khi tập hợp đạt kích thước $k$, ta đã hoàn thành một đoạn chứa đủ mọi mặt: tăng đáp án rồi xóa tập hợp. Đáp án là số đoạn hoàn chỉnh cộng một, chính là độ dài nhỏ nhất mà ta không thể hoàn thành.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestSequence(self, rolls: List[int], k: int) -> int:
        ans = 1
        s = set()
        for v in rolls:
            s.add(v)
            if len(s) == k:
                ans += 1
                s.clear()
        return ans
```

#### Java

```java
class Solution {
    public int shortestSequence(int[] rolls, int k) {
        Set<Integer> s = new HashSet<>();
        int ans = 1;
        for (int v : rolls) {
            s.add(v);
            if (s.size() == k) {
                s.clear();
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
    int shortestSequence(vector<int>& rolls, int k) {
        unordered_set<int> s;
        int ans = 1;
        for (int v : rolls) {
            s.insert(v);
            if (s.size() == k) {
                s.clear();
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func shortestSequence(rolls []int, k int) int {
	s := map[int]bool{}
	ans := 1
	for _, v := range rolls {
		s[v] = true
		if len(s) == k {
			ans++
			s = map[int]bool{}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
