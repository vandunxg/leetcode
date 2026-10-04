---
comments: true
difficulty: Medium
rating: 1791
source: Weekly Contest 353 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2771. Longest Non-decreasing Subarray From Two Arrays](https://leetcode.com/problems/longest-non-decreasing-subarray-from-two-arrays)

[中文文档](/solution/2700-2799/2771.Longest%20Non-decreasing%20Subarray%20From%20Two%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <strong>được đánh chỉ số từ 0</strong>, <code>nums1</code> và <code>nums2</code>, có cùng độ dài <code>n</code>.</p>

<p>Gọi một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> khác là <code>nums3</code>, có độ dài <code>n</code>. Với mỗi chỉ số <code>i</code> trong khoảng <code>[0, n - 1]</code>, bạn có thể gán <code>nums1[i]</code> hoặc <code>nums2[i]</code> vào <code>nums3[i]</code>.</p>

<p>Nhiệm vụ của bạn là tối đa hóa độ dài của <strong>mảng con không giảm dài nhất</strong> trong <code>nums3</code> bằng cách chọn các giá trị một cách tối ưu.</p>

<p>Trả về <em>một số nguyên biểu diễn độ dài của mảng con <strong>không giảm dài nhất</strong> trong</em> <code>nums3</code>.</p>

<p><strong>Lưu ý: </strong><strong>Mảng con</strong> là một dãy phần tử <strong>liền kề, không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,3,1], nums2 = [1,2,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích: </strong>Một cách để xây dựng nums3 là:
nums3 = [nums1[0], nums2[1], nums2[2]] =&gt; [2,2,1].
Mảng con bắt đầu từ chỉ số 0 và kết thúc ở chỉ số 1, [2,2], tạo thành một mảng con không giảm có độ dài 2.
Ta có thể chứng minh rằng 2 là độ dài lớn nhất có thể đạt được.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,3,2,1], nums2 = [2,2,3,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Một cách để xây dựng nums3 là:
nums3 = [nums1[0], nums2[1], nums2[2], nums2[3]] =&gt; [1,2,3,4].
Toàn bộ mảng tạo thành một mảng con không giảm có độ dài 4, nên đây là độ dài lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,1], nums2 = [2,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Một cách để xây dựng nums3 là:
nums3 = [nums1[0], nums1[1]] =&gt; [1,1].
Toàn bộ mảng tạo thành một mảng con không giảm có độ dài 2, nên đây là độ dài lớn nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length == nums2.length == n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Tại mỗi chỉ số, ta chọn phần tử từ $nums1$ hoặc $nums2$ và cần tìm đoạn liên tiếp không giảm dài nhất. Số chuỗi lựa chọn là cấp số nhân, nhưng đoạn cần tìm là liên tiếp, nên chỉ cần xét cách các cột kề nhau nối tiếp nhau.
>
> Gọi $f$ và $g$ là độ dài lớn nhất kết thúc tại cột hiện tại khi chọn phần tử từ $nums1$ và $nums2$, được chuyển từ hai lựa chọn ở cột trước nếu các giá trị không giảm. Ta cuộn cặp giá trị này và duy trì đáp án lớn nhất.

<!-- thinking:end -->

Ta định nghĩa hai biến $f$ và $g$, lần lượt biểu diễn độ dài của mảng con không giảm dài nhất tại vị trí hiện tại. Trong đó, $f$ là độ dài của mảng con không giảm dài nhất kết thúc bằng một phần tử từ $nums1$, còn $g$ là độ dài của mảng con không giảm dài nhất kết thúc bằng một phần tử từ $nums2$. Ban đầu, $f = g = 1$ và đáp án ban đầu $ans = 1$.

Tiếp theo, ta duyệt các phần tử của mảng trong khoảng $i \in [1, n)$; với mỗi $i$, ta định nghĩa hai biến $ff$ và $gg$, lần lượt biểu diễn độ dài của mảng con không giảm dài nhất kết thúc bằng $nums1[i]$ và $nums2[i]$. Khi khởi tạo, $ff = gg = 1$.

Ta có thể tính các giá trị của $ff$ và $gg$ dựa trên các giá trị của $f$ và $g$:

- Nếu $nums1[i] \ge nums1[i - 1]$, thì $ff = \max(ff, f + 1)$;
- Nếu $nums1[i] \ge nums2[i - 1]$, thì $ff = \max(ff, g + 1)$;
- Nếu $nums2[i] \ge nums1[i - 1]$, thì $gg = \max(gg, f + 1)$;
- Nếu $nums2[i] \ge nums2[i - 1]$, thì $gg = \max(gg, g + 1)$.

Sau đó, ta cập nhật $f = ff$ và $g = gg$, đồng thời cập nhật $ans$ thành $\max(ans, f, g)$.

Sau khi kết thúc vòng lặp, ta trả về $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxNonDecreasingLength(self, nums1: List[int], nums2: List[int]) -> int:
        n = len(nums1)
        f = g = 1
        ans = 1
        for i in range(1, n):
            ff = gg = 1
            if nums1[i] >= nums1[i - 1]:
                ff = max(ff, f + 1)
            if nums1[i] >= nums2[i - 1]:
                ff = max(ff, g + 1)
            if nums2[i] >= nums1[i - 1]:
                gg = max(gg, f + 1)
            if nums2[i] >= nums2[i - 1]:
                gg = max(gg, g + 1)
            f, g = ff, gg
            ans = max(ans, f, g)
        return ans
```

#### Java

```java
class Solution {
    public int maxNonDecreasingLength(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int f = 1, g = 1;
        int ans = 1;
        for (int i = 1; i < n; ++i) {
            int ff = 1, gg = 1;
            if (nums1[i] >= nums1[i - 1]) {
                ff = Math.max(ff, f + 1);
            }
            if (nums1[i] >= nums2[i - 1]) {
                ff = Math.max(ff, g + 1);
            }
            if (nums2[i] >= nums1[i - 1]) {
                gg = Math.max(gg, f + 1);
            }
            if (nums2[i] >= nums2[i - 1]) {
                gg = Math.max(gg, g + 1);
            }
            f = ff;
            g = gg;
            ans = Math.max(ans, Math.max(f, g));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxNonDecreasingLength(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        int f = 1, g = 1;
        int ans = 1;
        for (int i = 1; i < n; ++i) {
            int ff = 1, gg = 1;
            if (nums1[i] >= nums1[i - 1]) {
                ff = max(ff, f + 1);
            }
            if (nums1[i] >= nums2[i - 1]) {
                ff = max(ff, g + 1);
            }
            if (nums2[i] >= nums1[i - 1]) {
                gg = max(gg, f + 1);
            }
            if (nums2[i] >= nums2[i - 1]) {
                gg = max(gg, g + 1);
            }
            f = ff;
            g = gg;
            ans = max(ans, max(f, g));
        }
        return ans;
    }
};
```

#### Go

```go
func maxNonDecreasingLength(nums1 []int, nums2 []int) int {
	n := len(nums1)
	f, g, ans := 1, 1, 1
	for i := 1; i < n; i++ {
		ff, gg := 1, 1
		if nums1[i] >= nums1[i-1] {
			ff = max(ff, f+1)
		}
		if nums1[i] >= nums2[i-1] {
			ff = max(ff, g+1)
		}
		if nums2[i] >= nums1[i-1] {
			gg = max(gg, f+1)
		}
		if nums2[i] >= nums2[i-1] {
			gg = max(gg, g+1)
		}
		f, g = ff, gg
		ans = max(ans, max(f, g))
	}
	return ans
}
```

#### TypeScript

```ts
function maxNonDecreasingLength(nums1: number[], nums2: number[]): number {
    const n = nums1.length;
    let [f, g, ans] = [1, 1, 1];
    for (let i = 1; i < n; ++i) {
        let [ff, gg] = [1, 1];
        if (nums1[i] >= nums1[i - 1]) {
            ff = Math.max(ff, f + 1);
        }
        if (nums1[i] >= nums2[i - 1]) {
            ff = Math.max(ff, g + 1);
        }
        if (nums2[i] >= nums1[i - 1]) {
            gg = Math.max(gg, f + 1);
        }
        if (nums2[i] >= nums2[i - 1]) {
            gg = Math.max(gg, g + 1);
        }
        f = ff;
        g = gg;
        ans = Math.max(ans, f, g);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
