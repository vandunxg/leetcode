---
comments: true
difficulty: Medium
rating: 1759
source: Weekly Contest 488 Q3
tags:
    - Queue
    - Array
    - Monotonic Queue
---

<!-- problem:start -->

# [3835. Count Subarrays With Cost Less Than or Equal to K](https://leetcode.com/problems/count-subarrays-with-cost-less-than-or-equal-to-k)

[中文文档](/solution/3800-3899/3835.Count%20Subarrays%20With%20Cost%20Less%20Than%20or%20Equal%20to%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Với mọi <span data-keyword="subarray-nonempty">mảng con</span> <code>nums[l..r]</code>, ta định nghĩa <strong>chi phí</strong> của nó là:</p>

<p><code>cost = (max(nums[l..r]) - min(nums[l..r])) * (r - l + 1)</code>.</p>

<p>Hãy trả về một số nguyên biểu thị số lượng mảng con của <code>nums</code> có chi phí <strong>nhỏ hơn hoặc bằng</strong> <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta xét tất cả các mảng con của <code>nums</code>:</p>

<ul>
	<li><code>nums[0..0]</code>: <code>cost = (1 - 1) * 1 = 0</code></li>
	<li><code>nums[0..1]</code>: <code>cost = (3 - 1) * 2 = 4</code></li>
	<li><code>nums[0..2]</code>: <code>cost = (3 - 1) * 3 = 6</code></li>
	<li><code>nums[1..1]</code>: <code>cost = (3 - 3) * 1 = 0</code></li>
	<li><code>nums[1..2]</code>: <code>cost = (3 - 2) * 2 = 2</code></li>
	<li><code>nums[2..2]</code>: <code>cost = (2 - 2) * 1 = 0</code></li>
</ul>

<p>Có 5 mảng con có chi phí nhỏ hơn hoặc bằng 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,5,5,5], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mọi mảng con của <code>nums</code>, giá trị lớn nhất và nhỏ nhất đều bằng nhau, nên chi phí luôn bằng 0.</p>

<p>Do đó, mọi mảng con của <code>nums</code> đều có chi phí nhỏ hơn hoặc bằng 0.</p>

<p>Với một mảng có độ dài 4, tổng số mảng con là <code>(4 * 5) / 2 = 10</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ các mảng con của <code>nums</code> gồm một phần tử mới có chi phí bằng 0, và có 3 mảng con như vậy.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Deque + Enumeration + Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Chi phí của một mảng con bằng $(\max-\min)$ nhân với độ dài của nó; ta cần đếm các mảng con có chi phí $\le k$. Với $n \le 10^5$, không thể liệt kê tất cả các đầu mút.
>
> Khi mở rộng một cửa sổ, $\max-\min$ và độ dài không bao giờ giảm, nên chi phí có tính đơn điệu. Với một đầu phải cố định, các đầu trái hợp lệ tạo thành một prefix.
>
> Hai deque lưu giá trị lớn nhất và nhỏ nhất của cửa sổ. Ta tăng $l$ cho đến khi chi phí hợp lệ; đầu phải đó đóng góp $r-l+1$ mảng con.
>
> Mỗi chỉ số chỉ được thêm vào và loại khỏi một deque đúng một lần.

<!-- thinking:end -->

Ta nhận thấy rằng nếu một mảng con $\text{nums}[l..r]$ có chi phí nhỏ hơn hoặc bằng $k$, thì với mọi $l' \geq l$ và $r' \leq r$, mảng con $\text{nums}[l'..r']$ cũng có chi phí nhỏ hơn hoặc bằng $k$. Vì vậy, ta có thể duyệt chỉ số kết thúc bên phải $r$, dùng two pointers để duy trì chỉ số bắt đầu nhỏ nhất $l$ thỏa mãn điều kiện. Khi đó, số mảng con kết thúc tại $r$ và thỏa mãn điều kiện là $r - l + 1$, rồi ta cộng dồn vào đáp án.

Ta có thể dùng hai deque để lần lượt duy trì giá trị lớn nhất và nhỏ nhất trong cửa sổ hiện tại.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\text{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubarrays(self, nums: List[int], k: int) -> int:
        ans = 0
        q1 = deque()
        q2 = deque()
        l = 0
        for r, x in enumerate(nums):
            while q1 and nums[q1[-1]] <= x:
                q1.pop()
            while q2 and nums[q2[-1]] >= x:
                q2.pop()
            q1.append(r)
            q2.append(r)
            while l < r and (nums[q1[0]] - nums[q2[0]]) * (r - l + 1) > k:
                l += 1
                if q1[0] < l:
                    q1.popleft()
                if q2[0] < l:
                    q2.popleft()
            ans += r - l + 1
        return ans
```

#### Java

```java
class Solution {
    public long countSubarrays(int[] nums, long k) {
        long ans = 0;
        Deque<Integer> q1 = new ArrayDeque<>();
        Deque<Integer> q2 = new ArrayDeque<>();
        int l = 0;

        for (int r = 0; r < nums.length; r++) {
            int x = nums[r];

            while (!q1.isEmpty() && nums[q1.peekLast()] <= x) {
                q1.pollLast();
            }
            while (!q2.isEmpty() && nums[q2.peekLast()] >= x) {
                q2.pollLast();
            }
            q1.addLast(r);
            q2.addLast(r);

            while (
                l < r && (long) (nums[q1.peekFirst()] - nums[q2.peekFirst()]) * (r - l + 1) > k) {
                l++;
                if (q1.peekFirst() < l) {
                    q1.pollFirst();
                }
                if (q2.peekFirst() < l) {
                    q2.pollFirst();
                }
            }

            ans += r - l + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countSubarrays(vector<int>& nums, long long k) {
        long long ans = 0;
        deque<int> q1, q2;
        int l = 0;

        for (int r = 0; r < nums.size(); r++) {
            int x = nums[r];

            while (!q1.empty() && nums[q1.back()] <= x) {
                q1.pop_back();
            }
            while (!q2.empty() && nums[q2.back()] >= x) {
                q2.pop_back();
            }
            q1.push_back(r);
            q2.push_back(r);

            while (l < r && (long long) (nums[q1.front()] - nums[q2.front()]) * (r - l + 1) > k) {
                l++;
                if (q1.front() < l) {
                    q1.pop_front();
                }
                if (q2.front() < l) {
                    q2.pop_front();
                }
            }

            ans += r - l + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func countSubarrays(nums []int, k int64) int64 {
	var ans int64 = 0
	q1 := make([]int, 0)
	q2 := make([]int, 0)
	l := 0

	for r, x := range nums {
		for len(q1) > 0 && nums[q1[len(q1)-1]] <= x {
			q1 = q1[:len(q1)-1]
		}
		for len(q2) > 0 && nums[q2[len(q2)-1]] >= x {
			q2 = q2[:len(q2)-1]
		}
		q1 = append(q1, r)
		q2 = append(q2, r)

		for l < r &&
			int64(nums[q1[0]]-nums[q2[0]])*int64(r-l+1) > k {
			l++
			if q1[0] < l {
				q1 = q1[1:]
			}
			if q2[0] < l {
				q2 = q2[1:]
			}
		}
		ans += int64(r - l + 1)
	}
	return ans
}
```

#### TypeScript

```ts
function countSubarrays(nums: number[], k: number): number {
    let ans = 0;
    const q1: number[] = [];
    const q2: number[] = [];
    let h1 = 0,
        t1 = 0;
    let h2 = 0,
        t2 = 0;
    let l = 0;
    for (let r = 0; r < nums.length; r++) {
        const x = nums[r];
        while (h1 < t1 && nums[q1[t1 - 1]] <= x) {
            t1--;
        }
        while (h2 < t2 && nums[q2[t2 - 1]] >= x) {
            t2--;
        }
        q1[t1++] = r;
        q2[t2++] = r;
        while (l < r && (nums[q1[h1]] - nums[q2[h2]]) * (r - l + 1) > k) {
            l++;
            if (q1[h1] < l) {
                h1++;
            }
            if (q2[h2] < l) {
                h2++;
            }
        }
        ans += r - l + 1;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
