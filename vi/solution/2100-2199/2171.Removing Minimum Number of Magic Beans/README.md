---
comments: true
difficulty: Medium
rating: 1748
source: Weekly Contest 280 Q3
tags:
    - Greedy
    - Array
    - Enumeration
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2171. Removing Minimum Number of Magic Beans](https://leetcode.com/problems/removing-minimum-number-of-magic-beans)

[中文文档](/solution/2100-2199/2171.Removing%20Minimum%20Number%20of%20Magic%20Beans/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>dương</strong> <code>beans</code>, trong đó mỗi số nguyên biểu thị số lượng đậu thần trong một chiếc túi thần cụ thể.</p>

<p><strong>Xóa</strong> một số lượng đậu bất kỳ (<strong>có thể không xóa</strong>) khỏi mỗi túi sao cho số đậu trong mỗi túi còn lại <strong>không rỗng</strong> (vẫn chứa <strong>ít nhất một</strong> hạt đậu) là <strong>bằng nhau</strong>. Một khi đậu đã bị xóa khỏi một túi, bạn <strong>không được phép</strong> trả chúng lại vào bất kỳ túi nào.</p>

<p>Hãy trả về <em><strong>số lượng nhỏ nhất</strong> đậu thần mà bạn phải xóa</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> beans = [4,1,6,5]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
- Ta xóa 1 hạt đậu khỏi chiếc túi chỉ có 1 hạt đậu.
  Khi đó, các túi còn lại là: [4,<strong><u>0</u></strong>,6,5]
- Sau đó, ta xóa 2 hạt đậu khỏi chiếc túi có 6 hạt đậu.
  Khi đó, các túi còn lại là: [4,0,<strong><u>4</u></strong>,5]
- Tiếp theo, ta xóa 1 hạt đậu khỏi chiếc túi có 5 hạt đậu.
  Khi đó, các túi còn lại là: [4,0,4,<strong><u>4</u></strong>]
Tổng cộng ta đã xóa 1 + 2 + 1 = 4 hạt đậu để các túi còn lại không rỗng có cùng số hạt đậu.
Không có phương án nào khác xóa được 4 hạt đậu hoặc ít hơn.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> beans = [2,10,3,2]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong>
- Ta xóa 2 hạt đậu khỏi một trong các túi có 2 hạt đậu.
  Khi đó, các túi còn lại là: [<u><strong>0</strong></u>,10,3,2]
- Sau đó, ta xóa 2 hạt đậu khỏi chiếc túi còn lại có 2 hạt đậu.
  Khi đó, các túi còn lại là: [0,10,3,<u><strong>0</strong></u>]
- Tiếp theo, ta xóa 3 hạt đậu khỏi chiếc túi có 3 hạt đậu.
  Khi đó, các túi còn lại là: [0,10,<u><strong>0</strong></u>,0]
Tổng cộng ta đã xóa 2 + 2 + 3 = 7 hạt đậu để các túi còn lại không rỗng có cùng số hạt đậu.
Không có phương án nào khác xóa được 7 hạt đậu hoặc ít hơn.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= beans.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= beans[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Các túi không rỗng phải có cùng số lượng cuối cùng $x$; ta có thể làm rỗng một túi hoặc giảm số đậu trong đó. Các túi có ít hơn $x$ đậu sẽ bị làm rỗng, còn các túi có nhiều hơn $x$ đậu sẽ giảm xuống $x$. Giá trị tối ưu của $x$ bằng một trong các số lượng ban đầu; nếu không, ta có thể tăng $x$ mà không cần làm rỗng thêm túi nào.
>
> Sắp xếp và thử $\textit{beans}[i]$ làm $x$, khi đó còn lại $(n-i)x$ hạt đậu và phải xóa $s-(n-i)x$ hạt.
>
> Đáp án là số hạt bị xóa nhỏ nhất trong các lựa chọn đó.

<!-- thinking:end -->

Ta có thể sắp xếp số đậu trong tất cả các túi theo thứ tự tăng dần, sau đó liệt kê số đậu $beans[i]$ trong mỗi túi làm số đậu cuối cùng trong túi. Tổng số đậu còn lại là $beans[i] \times (n - i)$, vì vậy số đậu cần xóa là $s - beans[i] \times (n - i)$, trong đó $s$ là tổng số đậu trong tất cả các túi. Ta cần tìm số đậu cần xóa nhỏ nhất trong tất cả các phương án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là số lượng túi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumRemoval(self, beans: List[int]) -> int:
        beans.sort()
        s, n = sum(beans), len(beans)
        return min(s - x * (n - i) for i, x in enumerate(beans))
```

#### Java

```java
class Solution {
    public long minimumRemoval(int[] beans) {
        Arrays.sort(beans);
        long s = 0;
        for (int x : beans) {
            s += x;
        }
        long ans = s;
        int n = beans.length;
        for (int i = 0; i < n; ++i) {
            ans = Math.min(ans, s - (long) beans[i] * (n - i));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumRemoval(vector<int>& beans) {
        sort(beans.begin(), beans.end());
        long long s = accumulate(beans.begin(), beans.end(), 0ll);
        long long ans = s;
        int n = beans.size();
        for (int i = 0; i < n; ++i) {
            ans = min(ans, s - 1ll * beans[i] * (n - i));
        }
        return ans;
    }
};
```

#### Go

```go
func minimumRemoval(beans []int) int64 {
	sort.Ints(beans)
	s := 0
	for _, x := range beans {
		s += x
	}
	ans := s
	n := len(beans)
	for i, x := range beans {
		ans = min(ans, s-x*(n-i))
	}
	return int64(ans)
}
```

#### TypeScript

```ts
function minimumRemoval(beans: number[]): number {
    beans.sort((a, b) => a - b);
    const s = beans.reduce((a, b) => a + b, 0);
    const n = beans.length;
    let ans = s;
    for (let i = 0; i < n; ++i) {
        ans = Math.min(ans, s - beans[i] * (n - i));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
