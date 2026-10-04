---
comments: true
difficulty: Easy
rating: 1420
source: Weekly Contest 352 Q1
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [2760. Longest Even Odd Subarray With Threshold](https://leetcode.com/problems/longest-even-odd-subarray-with-threshold)

[中文文档](/solution/2700-2799/2760.Longest%20Even%20Odd%20Subarray%20With%20Threshold/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code> và một số nguyên <code>threshold</code>.</p>

<p>Hãy tìm độ dài của <strong>mảng con dài nhất</strong> của <code>nums</code>, bắt đầu tại chỉ số <code>l</code> và kết thúc tại chỉ số <code>r</code> <code>(0 &lt;= l &lt;= r &lt; nums.length)</code>, thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>nums[l] % 2 == 0</code></li>
	<li>Với mọi chỉ số <code>i</code> trong đoạn <code>[l, r - 1]</code>, <code>nums[i] % 2 != nums[i + 1] % 2</code></li>
	<li>Với mọi chỉ số <code>i</code> trong đoạn <code>[l, r]</code>, <code>nums[i] &lt;= threshold</code></li>
</ul>

<p>Trả về <em>một số nguyên biểu thị độ dài của mảng con dài nhất thỏa mãn các điều kiện trên.</em></p>

<p><strong>Lưu ý:</strong> <strong>mảng con</strong> là một dãy phần tử liên tiếp, không rỗng, nằm trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,5,4], threshold = 5
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trong ví dụ này, ta có thể chọn mảng con bắt đầu tại l = 1 và kết thúc tại r = 3 =&gt; [2,5,4]. Mảng con này thỏa mãn các điều kiện.
Do đó, đáp án là độ dài của mảng con, bằng 3. Ta có thể chứng minh rằng 3 là độ dài lớn nhất có thể đạt được.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2], threshold = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Trong ví dụ này, ta có thể chọn mảng con bắt đầu tại l = 1 và kết thúc tại r = 1 =&gt; [2].
Mảng con này thỏa mãn tất cả các điều kiện và ta có thể chứng minh rằng 1 là độ dài lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,4,5], threshold = 4
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trong ví dụ này, ta có thể chọn mảng con bắt đầu tại l = 0 và kết thúc tại r = 2 =&gt; [2,3,4].
Mảng con này thỏa mãn các điều kiện.
Do đó, đáp án là độ dài của mảng con, bằng 3. Ta có thể chứng minh rằng 3 là độ dài lớn nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100 </code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100 </code></li>
	<li><code>1 &lt;= threshold &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Mảng con dài nhất phải bắt đầu bằng một giá trị chẵn, có tính chẵn lẻ xen kẽ và mọi giá trị không vượt quá $threshold$. Với $n\le 100$, ta có thể mở rộng từ từng điểm bắt đầu bên trái.
>
> Với mỗi chỉ số bên trái $l$ hợp lệ, ta duyệt sang phải cho đến khi tính chẵn lẻ lặp lại hoặc một giá trị vượt quá threshold, rồi cập nhật đáp án bằng $r-l$.

<!-- thinking:end -->

Ta duyệt mọi $l$ trong đoạn $[0,..n-1]$. Nếu $nums[l]$ thỏa mãn $nums[l] \bmod 2 = 0$ và $nums[l] \leq threshold$, ta bắt đầu từ $l+1$ để tìm $r$ lớn nhất thỏa mãn điều kiện. Khi đó, độ dài của mảng con chẵn-lẻ dài nhất có $nums[l]$ làm điểm cuối bên trái là $r - l$. Ta lấy giá trị lớn nhất trong tất cả các $r - l$ làm đáp án.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestAlternatingSubarray(self, nums: List[int], threshold: int) -> int:
        ans, n = 0, len(nums)
        for l in range(n):
            if nums[l] % 2 == 0 and nums[l] <= threshold:
                r = l + 1
                while r < n and nums[r] % 2 != nums[r - 1] % 2 and nums[r] <= threshold:
                    r += 1
                ans = max(ans, r - l)
        return ans
```

#### Java

```java
class Solution {
    public int longestAlternatingSubarray(int[] nums, int threshold) {
        int ans = 0, n = nums.length;
        for (int l = 0; l < n; ++l) {
            if (nums[l] % 2 == 0 && nums[l] <= threshold) {
                int r = l + 1;
                while (r < n && nums[r] % 2 != nums[r - 1] % 2 && nums[r] <= threshold) {
                    ++r;
                }
                ans = Math.max(ans, r - l);
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
    int longestAlternatingSubarray(vector<int>& nums, int threshold) {
        int ans = 0, n = nums.size();
        for (int l = 0; l < n; ++l) {
            if (nums[l] % 2 == 0 && nums[l] <= threshold) {
                int r = l + 1;
                while (r < n && nums[r] % 2 != nums[r - 1] % 2 && nums[r] <= threshold) {
                    ++r;
                }
                ans = max(ans, r - l);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestAlternatingSubarray(nums []int, threshold int) (ans int) {
	n := len(nums)
	for l := range nums {
		if nums[l]%2 == 0 && nums[l] <= threshold {
			r := l + 1
			for r < n && nums[r]%2 != nums[r-1]%2 && nums[r] <= threshold {
				r++
			}
			ans = max(ans, r-l)
		}
	}
	return
}
```

#### TypeScript

```ts
function longestAlternatingSubarray(nums: number[], threshold: number): number {
    const n = nums.length;
    let ans = 0;
    for (let l = 0; l < n; ++l) {
        if (nums[l] % 2 === 0 && nums[l] <= threshold) {
            let r = l + 1;
            while (r < n && nums[r] % 2 !== nums[r - 1] % 2 && nums[r] <= threshold) {
                ++r;
            }
            ans = Math.max(ans, r - l);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duyệt tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 khởi động lại bên trong một đoạn đã thất bại, nên quét lại cùng một đoạn nhiều lần. Các đoạn hợp lệ không giao nhau, vì vậy sau mỗi đoạn, ta có thể nhảy điểm bắt đầu sang $r$ và biến việc duyệt thành tuyến tính.

<!-- thinking:end -->

Ta nhận thấy bài toán thực tế chia mảng thành một số mảng con rời nhau thỏa mãn điều kiện. Ta chỉ cần tìm mảng con dài nhất trong số đó. Vì vậy, khi duyệt $l$ và $r$, ta không cần quay lui mà chỉ cần duyệt từ trái sang phải một lần.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestAlternatingSubarray(self, nums: List[int], threshold: int) -> int:
        ans, l, n = 0, 0, len(nums)
        while l < n:
            if nums[l] % 2 == 0 and nums[l] <= threshold:
                r = l + 1
                while r < n and nums[r] % 2 != nums[r - 1] % 2 and nums[r] <= threshold:
                    r += 1
                ans = max(ans, r - l)
                l = r
            else:
                l += 1
        return ans
```

#### Java

```java
class Solution {
    public int longestAlternatingSubarray(int[] nums, int threshold) {
        int ans = 0;
        for (int l = 0, n = nums.length; l < n;) {
            if (nums[l] % 2 == 0 && nums[l] <= threshold) {
                int r = l + 1;
                while (r < n && nums[r] % 2 != nums[r - 1] % 2 && nums[r] <= threshold) {
                    ++r;
                }
                ans = Math.max(ans, r - l);
                l = r;
            } else {
                ++l;
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
    int longestAlternatingSubarray(vector<int>& nums, int threshold) {
        int ans = 0;
        for (int l = 0, n = nums.size(); l < n;) {
            if (nums[l] % 2 == 0 && nums[l] <= threshold) {
                int r = l + 1;
                while (r < n && nums[r] % 2 != nums[r - 1] % 2 && nums[r] <= threshold) {
                    ++r;
                }
                ans = max(ans, r - l);
                l = r;
            } else {
                ++l;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestAlternatingSubarray(nums []int, threshold int) (ans int) {
	for l, n := 0, len(nums); l < n; {
		if nums[l]%2 == 0 && nums[l] <= threshold {
			r := l + 1
			for r < n && nums[r]%2 != nums[r-1]%2 && nums[r] <= threshold {
				r++
			}
			ans = max(ans, r-l)
			l = r
		} else {
			l++
		}
	}
	return
}
```

#### TypeScript

```ts
function longestAlternatingSubarray(nums: number[], threshold: number): number {
    const n = nums.length;
    let ans = 0;
    for (let l = 0; l < n;) {
        if (nums[l] % 2 === 0 && nums[l] <= threshold) {
            let r = l + 1;
            while (r < n && nums[r] % 2 !== nums[r - 1] % 2 && nums[r] <= threshold) {
                ++r;
            }
            ans = Math.max(ans, r - l);
            l = r;
        } else {
            ++l;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
