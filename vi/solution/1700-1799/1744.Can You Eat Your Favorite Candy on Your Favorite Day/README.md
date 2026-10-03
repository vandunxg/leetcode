---
comments: true
difficulty: Medium
rating: 1858
source: Weekly Contest 226 Q3
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [1744. Can You Eat Your Favorite Candy on Your Favorite Day](https://leetcode.com/problems/can-you-eat-your-favorite-candy-on-your-favorite-day)

[中文文档](/solution/1700-1799/1744.Can%20You%20Eat%20Your%20Favorite%20Candy%20on%20Your%20Favorite%20Day/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên dương <strong>đánh chỉ số từ 0</strong> <code>candiesCount</code>, trong đó <code>candiesCount[i]</code> là số kẹo loại <code>i<sup>th</sup></code> mà bạn có. Ngoài ra, cho mảng 2 chiều <code>queries</code>, trong đó <code>queries[i] = [favoriteType<sub>i</sub>, favoriteDay<sub>i</sub>, dailyCap<sub>i</sub>]</code>.</p>

<p>Bạn chơi một trò chơi với các quy tắc sau:</p>

<ul>
<li>Bạn bắt đầu ăn kẹo vào ngày <code><strong>0</strong></code>.</li>
<li>Bạn <b>không thể</b> ăn <strong>bất kỳ</strong> kẹo loại <code>i</code> nào nếu chưa ăn <strong>tất cả</strong> kẹo loại <code>i - 1</code>.</li>
<li>Bạn phải ăn <strong>ít nhất</strong> <strong>một</strong> viên kẹo mỗi ngày cho đến khi ăn hết kẹo.</li>
</ul>

<p>Hãy tạo mảng boolean <code>answer</code> sao cho <code>answer.length == queries.length</code>, và <code>answer[i]</code> là <code>true</code> nếu bạn có thể ăn kẹo loại <code>favoriteType<sub>i</sub></code> vào ngày <code>favoriteDay<sub>i</sub></code> mà không ăn <strong>quá</strong> <code>dailyCap<sub>i</sub></code> viên trong <strong>bất kỳ</strong> ngày nào, ngược lại là <code>false</code>. Bạn có thể ăn các loại kẹo khác nhau trong cùng một ngày nếu tuân theo quy tắc 2.</p>

<p>Trả về <em>mảng đã tạo </em><code>answer</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> candiesCount = [7,4,5,3,8], queries = [[0,2,2],[4,2,4],[2,13,1000000000]]
<strong>Đầu ra:</strong> [true,false,true]
<strong>Giải thích:</strong>
1- Nếu ăn 2 viên kẹo loại 0 vào ngày 0 và 2 viên kẹo loại 0 vào ngày 1, bạn sẽ ăn một viên kẹo loại 0 vào ngày 2.
2- Bạn có thể ăn nhiều nhất 4 viên kẹo mỗi ngày.
   Nếu ăn 4 viên mỗi ngày, bạn sẽ ăn 4 viên loại 0 vào ngày 0 và 4 viên loại 0 và loại 1 vào ngày 1.
   Vào ngày 2, bạn chỉ có thể ăn 4 viên loại 1 và loại 2, nên không thể ăn kẹo loại 4 vào ngày 2.
3- Nếu ăn 1 viên kẹo mỗi ngày, bạn sẽ ăn một viên kẹo loại 2 vào ngày 13.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> candiesCount = [5,2,6,4,1], queries = [[3,1,2],[4,10,3],[3,10,100],[4,100,30],[1,3,1]]
<strong>Đầu ra:</strong> [false,true,true,false,false]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= candiesCount.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= candiesCount[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 3</code></li>
	<li><code>0 &lt;= favoriteType<sub>i</sub> &lt; candiesCount.length</code></li>
	<li><code>0 &lt;= favoriteDay<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= dailyCap<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các loại kẹo phải được ăn theo thứ tự, mỗi ngày ăn ít nhất một và nhiều nhất $\textit{dailyCap}$ viên. Có quá nhiều query nên không thể mô phỏng từng ngày.
>
> Tổng tiền tố $s[t]$ đếm số kẹo trước loại $t$. Để ăn đến loại $t$ vào ngày $\textit{day}$, lịch ăn chậm nhất không được ăn hết $s[t+1]$ trước ngày đó, còn lịch nhanh nhất không được ăn xong $s[t]$ trước đó.
>
> Do đó, điều kiện là $\textit{day}<s[t+1]$ và $(\textit{day}+1)\cdot mx>s[t]$. Mỗi query được xử lý trong $O(1)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canEat(self, candiesCount: List[int], queries: List[List[int]]) -> List[bool]:
        s = list(accumulate(candiesCount, initial=0))
        ans = []
        for t, day, mx in queries:
            least, most = day, (day + 1) * mx
            ans.append(least < s[t + 1] and most > s[t])
        return ans
```

#### Java

```java
class Solution {
    public boolean[] canEat(int[] candiesCount, int[][] queries) {
        int n = candiesCount.length;
        long[] s = new long[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + candiesCount[i];
        }
        int m = queries.length;
        boolean[] ans = new boolean[m];
        for (int i = 0; i < m; ++i) {
            int t = queries[i][0], day = queries[i][1], mx = queries[i][2];
            long least = day, most = (long) (day + 1) * mx;
            ans[i] = least < s[t + 1] && most > s[t];
        }
        return ans;
    }
}
```

#### C++

```cpp
using ll = long long;

class Solution {
public:
    vector<bool> canEat(vector<int>& candiesCount, vector<vector<int>>& queries) {
        int n = candiesCount.size();
        vector<ll> s(n + 1);
        for (int i = 0; i < n; ++i) s[i + 1] = s[i] + candiesCount[i];
        vector<bool> ans;
        for (auto& q : queries) {
            int t = q[0], day = q[1], mx = q[2];
            ll least = day, most = 1ll * (day + 1) * mx;
            ans.emplace_back(least < s[t + 1] && most > s[t]);
        }
        return ans;
    }
};
```

#### Go

```go
func canEat(candiesCount []int, queries [][]int) (ans []bool) {
	n := len(candiesCount)
	s := make([]int, n+1)
	for i, v := range candiesCount {
		s[i+1] = s[i] + v
	}
	for _, q := range queries {
		t, day, mx := q[0], q[1], q[2]
		least, most := day, (day+1)*mx
		ans = append(ans, least < s[t+1] && most > s[t])
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
