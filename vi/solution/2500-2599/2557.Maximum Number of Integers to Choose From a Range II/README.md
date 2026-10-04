---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2557. Maximum Number of Integers to Choose From a Range II 🔒](https://leetcode.com/problems/maximum-number-of-integers-to-choose-from-a-range-ii)

[中文文档](/solution/2500-2599/2557.Maximum%20Number%20of%20Integers%20to%20Choose%20From%20a%20Range%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>banned</code> và hai số nguyên <code>n</code> và <code>maxSum</code>. Bạn cần chọn một số lượng số nguyên theo các quy tắc sau:</p>

<ul>
	<li>Các số nguyên được chọn phải nằm trong khoảng <code>[1, n]</code>.</li>
	<li>Mỗi số nguyên chỉ được chọn <strong>nhiều nhất một lần</strong>.</li>
	<li>Các số nguyên được chọn không được xuất hiện trong mảng <code>banned</code>.</li>
	<li>Tổng các số nguyên được chọn không được vượt quá <code>maxSum</code>.</li>
</ul>

<p>Trả về <em>số lượng số nguyên <strong>lớn nhất</strong> mà bạn có thể chọn theo các quy tắc trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> banned = [1,4,6], n = 6, maxSum = 4
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Bạn có thể chọn số nguyên 3.
3 nằm trong khoảng [1, 6] và không xuất hiện trong banned. Tổng các số nguyên được chọn là 3, không vượt quá maxSum.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> banned = [4,3,5,6], n = 7, maxSum = 18
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Bạn có thể chọn các số nguyên 1, 2 và 7.
Tất cả các số này đều nằm trong khoảng [1, 7], không xuất hiện trong banned và tổng của chúng là 10, không vượt quá maxSum.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= banned.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= banned[i] &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= maxSum &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Loại bỏ phần tử trùng lặp + Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Quy tắc chọn giống với Range I, nhưng $n$ và $\textit{maxSum}$ quá lớn để duyệt từ $1$ đến $n$.
>
> Thêm $0$ và $n+1$ vào tập các phần tử bị cấm rồi sắp xếp. Mỗi khoảng trống là một đoạn liên tiếp; tổng của $t$ số nguyên đầu tiên trong đoạn là một cấp số cộng, nên tìm kiếm nhị phân sẽ tìm được $t$ lớn nhất có thể chọn. Lấp đầy các khoảng trống từ trái sang phải cho đến khi hết ngân sách.

<!-- thinking:end -->

Ta có thể thêm $0$ và $n + 1$ vào mảng `banned`, sau đó loại bỏ các phần tử trùng lặp và sắp xếp mảng `banned`.

Tiếp theo, ta duyệt qua mọi cặp phần tử liền kề $i$ và $j$ trong mảng `banned`. Khoảng các số nguyên có thể chọn là $[i + 1, j - 1]$. Ta dùng tìm kiếm nhị phân để xác định số phần tử có thể chọn trong khoảng này, tìm số lượng phần tử có thể chọn lớn nhất rồi cộng vào $ans$. Đồng thời, ta trừ tổng các phần tử này khỏi `maxSum`. Nếu `maxSum` nhỏ hơn $0$, ta dừng vòng lặp. Trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng `banned`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxCount(self, banned: List[int], n: int, maxSum: int) -> int:
        banned.extend([0, n + 1])
        ban = sorted(set(banned))
        ans = 0
        for i, j in pairwise(ban):
            left, right = 0, j - i - 1
            while left < right:
                mid = (left + right + 1) >> 1
                if (i + 1 + i + mid) * mid // 2 <= maxSum:
                    left = mid
                else:
                    right = mid - 1
            ans += left
            maxSum -= (i + 1 + i + left) * left // 2
            if maxSum <= 0:
                break
        return ans
```

#### Java

```java
class Solution {
    public int maxCount(int[] banned, int n, long maxSum) {
        Set<Integer> black = new HashSet<>();
        black.add(0);
        black.add(n + 1);
        for (int x : banned) {
            black.add(x);
        }
        List<Integer> ban = new ArrayList<>(black);
        Collections.sort(ban);
        int ans = 0;
        for (int k = 1; k < ban.size(); ++k) {
            int i = ban.get(k - 1), j = ban.get(k);
            int left = 0, right = j - i - 1;
            while (left < right) {
                int mid = (left + right + 1) >>> 1;
                if ((i + 1 + i + mid) * 1L * mid / 2 <= maxSum) {
                    left = mid;
                } else {
                    right = mid - 1;
                }
            }
            ans += left;
            maxSum -= (i + 1 + i + left) * 1L * left / 2;
            if (maxSum <= 0) {
                break;
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
    int maxCount(vector<int>& banned, int n, long long maxSum) {
        banned.push_back(0);
        banned.push_back(n + 1);
        sort(banned.begin(), banned.end());
        banned.erase(unique(banned.begin(), banned.end()), banned.end());
        int ans = 0;
        for (int k = 1; k < banned.size(); ++k) {
            int i = banned[k - 1], j = banned[k];
            int left = 0, right = j - i - 1;
            while (left < right) {
                int mid = left + ((right - left + 1) / 2);
                if ((i + 1 + i + mid) * 1LL * mid / 2 <= maxSum) {
                    left = mid;
                } else {
                    right = mid - 1;
                }
            }
            ans += left;
            maxSum -= (i + 1 + i + left) * 1LL * left / 2;
            if (maxSum <= 0) {
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxCount(banned []int, n int, maxSum int64) (ans int) {
	banned = append(banned, []int{0, n + 1}...)
	sort.Ints(banned)
	ban := []int{}
	for i, x := range banned {
		if i > 0 && x == banned[i-1] {
			continue
		}
		ban = append(ban, x)
	}
	for k := 1; k < len(ban); k++ {
		i, j := ban[k-1], ban[k]
		left, right := 0, j-i-1
		for left < right {
			mid := (left + right + 1) >> 1
			if int64((i+1+i+mid)*mid/2) <= maxSum {
				left = mid
			} else {
				right = mid - 1
			}
		}
		ans += left
		maxSum -= int64((i + 1 + i + left) * left / 2)
		if maxSum <= 0 {
			break
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
