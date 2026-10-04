---
comments: true
difficulty: Hard
rating: 2444
source: Weekly Contest 376 Q4
tags:
    - Array
    - Binary Search
    - Prefix Sum
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [2968. Apply Operations to Maximize Frequency Score](https://leetcode.com/problems/apply-operations-to-maximize-frequency-score)

[中文文档](/solution/2900-2999/2968.Apply%20Operations%20to%20Maximize%20Frequency%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Bạn có thể thực hiện thao tác sau trên mảng <strong>nhiều nhất</strong> <code>k</code> lần:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> bất kỳ trong mảng và <strong>tăng</strong> hoặc <strong>giảm</strong> <code>nums[i]</code> đi <code>1</code>.</li>
</ul>

<p>Điểm số của mảng cuối cùng là <strong>tần suất</strong> của phần tử xuất hiện nhiều nhất trong mảng.</p>

<p>Trả về <em><strong>điểm số lớn nhất</strong> mà bạn có thể đạt được</em>.</p>

<p>Tần suất của một phần tử là số lần phần tử đó xuất hiện trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,6,4], k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau trên mảng:
- Chọn i = 0 và tăng giá trị của nums[0] lên 1. Mảng thu được là [2,2,6,4].
- Chọn i = 3 và giảm giá trị của nums[3] đi 1. Mảng thu được là [2,2,6,3].
- Chọn i = 3 và giảm giá trị của nums[3] đi 1. Mảng thu được là [2,2,6,2].
Phần tử 2 xuất hiện nhiều nhất trong mảng cuối cùng, vì vậy điểm số của ta là 3.
Có thể chứng minh rằng ta không thể đạt được điểm số tốt hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,4,2,4], k = 0
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta không thể thực hiện thao tác nào, vì vậy điểm số là tần suất của phần tử xuất hiện nhiều nhất trong mảng ban đầu, bằng 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>14</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tổng tiền tố + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Với nhiều nhất $k$ lần tăng hoặc giảm, ta muốn làm cho nhiều giá trị bằng nhau nhất có thể. Sau khi sắp xếp, phương án tối ưu là đưa một đoạn liên tiếp về cùng giá trị trung vị của đoạn. Tính khả thi đơn điệu theo độ dài, nên ta có thể tìm kiếm nhị phân độ dài đó.
>
> Tổng tiền tố cho phép tính chi phí đưa về trung vị trong $O(1)$. Với $n \le 10^5$, ta có thể kiểm tra trong $O(n \log n)$.

<!-- thinking:end -->

Bài toán yêu cầu tìm tần suất lớn nhất của mốt mà ta có thể đạt được sau khi thực hiện nhiều nhất $k$ thao tác. Nếu sắp xếp mảng $nums$ theo thứ tự tăng dần, tốt nhất là biến một đoạn liên tiếp các số thành cùng một số, từ đó giảm số thao tác cần thiết và tăng tần suất của mốt.

Do đó, trước hết ta chỉ cần sắp xếp mảng $nums$.

Tiếp theo, ta nhận thấy rằng nếu tần suất $x$ là khả thi thì với mọi $y \le x$, tần suất $y$ cũng khả thi, thể hiện tính đơn điệu. Vì vậy, ta có thể dùng tìm kiếm nhị phân để tìm tần suất lớn nhất khả thi.

Ta tìm kiếm nhị phân trên tần suất, đặt biên trái là $l = 0$ và biên phải là $r = n$, trong đó $n$ là độ dài mảng. Ở mỗi bước tìm kiếm nhị phân, ta lấy giá trị giữa $mid = \lfloor \frac{l + r + 1}{2} \rfloor$, sau đó xác định xem có tồn tại một mảng con liên tiếp độ dài $mid$ trong $nums$ sao cho tất cả phần tử trong mảng con này trở thành trung vị của nó và số thao tác không vượt quá $k$ hay không. Nếu có, ta cập nhật biên trái $l$ thành $mid$; ngược lại, ta cập nhật biên phải $r$ thành $mid - 1$.

Để xác định liệu có tồn tại mảng con như vậy hay không, ta có thể dùng tổng tiền tố. Trước hết, ta định nghĩa hai con trỏ $i$ và $j$, ban đầu $i = 0$, $j = i + mid$. Khi đó, tất cả phần tử từ $nums[i]$ đến $nums[j - 1]$ được đổi thành $nums[(i + j) / 2]$, và số thao tác cần thiết là $left + right$, trong đó:

$$
\begin{aligned}
\textit{left} &= \sum_{k = i}^{(i + j) / 2 - 1} (nums[(i + j) / 2] - nums[k]) \\
&= ((i + j) / 2 - i) \times nums[(i + j) / 2] - \sum_{k = i}^{(i + j) / 2 - 1} nums[k]
\end{aligned}
$$

$$
\begin{aligned}
\textit{right} &= \sum_{k = (i + j) / 2 + 1}^{j} (nums[k] - nums[(i + j) / 2]) \\
&= \sum_{k = (i + j) / 2 + 1}^{j} nums[k] - (j - (i + j) / 2) \times nums[(i + j) / 2]
\end{aligned}
$$

Ta có thể dùng mảng tổng tiền tố $s$ để tính $\sum_{k = i}^{j} nums[k]$, từ đó tính $left$ và $right$ trong $O(1)$ thời gian.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxFrequencyScore(self, nums: List[int], k: int) -> int:
        nums.sort()
        s = list(accumulate(nums, initial=0))
        n = len(nums)
        l, r = 0, n
        while l < r:
            mid = (l + r + 1) >> 1
            ok = False
            for i in range(n - mid + 1):
                j = i + mid
                x = nums[(i + j) // 2]
                left = ((i + j) // 2 - i) * x - (s[(i + j) // 2] - s[i])
                right = (s[j] - s[(i + j) // 2]) - ((j - (i + j) // 2) * x)
                if left + right <= k:
                    ok = True
                    break
            if ok:
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    public int maxFrequencyScore(int[] nums, long k) {
        Arrays.sort(nums);
        int n = nums.length;
        long[] s = new long[n + 1];
        for (int i = 1; i <= n; i++) {
            s[i] = s[i - 1] + nums[i - 1];
        }
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            boolean ok = false;

            for (int i = 0; i <= n - mid; i++) {
                int j = i + mid;
                int x = nums[(i + j) / 2];
                long left = ((i + j) / 2 - i) * (long) x - (s[(i + j) / 2] - s[i]);
                long right = (s[j] - s[(i + j) / 2]) - ((j - (i + j) / 2) * (long) x);
                if (left + right <= k) {
                    ok = true;
                    break;
                }
            }

            if (ok) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }

        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxFrequencyScore(vector<int>& nums, long long k) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        vector<long long> s(n + 1, 0);
        for (int i = 1; i <= n; i++) {
            s[i] = s[i - 1] + nums[i - 1];
        }

        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            bool ok = false;

            for (int i = 0; i <= n - mid; i++) {
                int j = i + mid;
                int x = nums[(i + j) / 2];
                long long left = ((i + j) / 2 - i) * (long long) x - (s[(i + j) / 2] - s[i]);
                long long right = (s[j] - s[(i + j) / 2]) - ((j - (i + j) / 2) * (long long) x);

                if (left + right <= k) {
                    ok = true;
                    break;
                }
            }

            if (ok) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }

        return l;
    }
};
```

#### Go

```go
func maxFrequencyScore(nums []int, k int64) int {
	sort.Ints(nums)
	n := len(nums)
	s := make([]int64, n+1)
	for i := 1; i <= n; i++ {
		s[i] = s[i-1] + int64(nums[i-1])
	}

	l, r := 0, n
	for l < r {
		mid := (l + r + 1) >> 1
		ok := false

		for i := 0; i <= n-mid; i++ {
			j := i + mid
			x := int64(nums[(i+j)/2])
			left := (int64((i+j)/2-i) * x) - (s[(i+j)/2] - s[i])
			right := (s[j] - s[(i+j)/2]) - (int64(j-(i+j)/2) * x)

			if left+right <= k {
				ok = true
				break
			}
		}

		if ok {
			l = mid
		} else {
			r = mid - 1
		}
	}

	return l
}
```

#### TypeScript

```ts
function maxFrequencyScore(nums: number[], k: number): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 1; i <= n; i++) {
        s[i] = s[i - 1] + nums[i - 1];
    }

    let l: number = 0;
    let r: number = n;
    while (l < r) {
        const mid: number = (l + r + 1) >> 1;
        let ok: boolean = false;
        for (let i = 0; i <= n - mid; i++) {
            const j = i + mid;
            const x = nums[Math.floor((i + j) / 2)];
            const left = (Math.floor((i + j) / 2) - i) * x - (s[Math.floor((i + j) / 2)] - s[i]);
            const right = s[j] - s[Math.floor((i + j) / 2)] - (j - Math.floor((i + j) / 2)) * x;
            if (left + right <= k) {
                ok = true;
                break;
            }
        }
        if (ok) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }

    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
