---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [3874. Valid Subarrays With Exactly One Peak 🔒](https://leetcode.com/problems/valid-subarrays-with-exactly-one-peak)

[中文文档](/solution/3800-3899/3874.Valid%20Subarrays%20With%20Exactly%20One%20Peak/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Một chỉ số <code>i</code> là một <strong>đỉnh</strong> nếu:</p>

<ul>
	<li><code>0 &lt; i &lt; n - 1</code></li>
	<li><code>nums[i] &gt; nums[i - 1]</code> và <code>nums[i] &gt; nums[i + 1]</code></li>
</ul>

<p>Một mảng con <code>[l, r]</code> là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li>Nó chứa <strong>đúng một</strong> đỉnh tại chỉ số <code>i</code> của <code>nums</code></li>
	<li><code>i - l &lt;= k</code> và <code>r - i &lt;= k</code></li>
</ul>

<p>Hãy trả về một số nguyên biểu thị số lượng <strong>mảng con hợp lệ</strong> trong <code>nums</code>.</p>
Một <strong>mảng con</strong> là một dãy phần tử liên tiếp <b>không rỗng</b> trong một mảng.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chỉ số <code>i = 1</code> là một đỉnh vì <code>nums[1] = 3</code> lớn hơn <code>nums[0] = 1</code> và <code>nums[2] = 2</code>.</li>
	<li>Mọi mảng con hợp lệ đều phải chứa chỉ số 1, và khoảng cách từ đỉnh đến cả hai đầu của mảng con không được vượt quá <code>k = 1</code>.</li>
	<li>Các mảng con hợp lệ là <code>[3]</code>, <code>[1, 3]</code>, <code>[3, 2]</code> và <code>[1, 3, 2]</code>, nên đáp án là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,8,9], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Không có chỉ số <code>i</code> nào sao cho <code>nums[i]</code> lớn hơn cả <code>nums[i - 1]</code> và <code>nums[i + 1]</code>.</li>
	<li>Do đó, mảng không chứa đỉnh nào. Vì vậy, số lượng mảng con hợp lệ là 0.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,5,1], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chỉ số <code>i = 2</code> là một đỉnh vì <code>nums[2] = 5</code> lớn hơn <code>nums[1] = 3</code> và <code>nums[3] = 1</code>.</li>
	<li>Mọi mảng con hợp lệ đều phải chứa đỉnh này, và khoảng cách từ đỉnh đến cả hai đầu của mảng con không được vượt quá <code>k = 2</code>.</li>
	<li>Các mảng con hợp lệ là <code>[5]</code>, <code>[3, 5]</code>, <code>[5, 1]</code>, <code>[3, 5, 1]</code>, <code>[4, 3, 5]</code> và <code>[4, 3, 5, 1]</code>, nên đáp án là 6.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con hợp lệ chứa đúng một đỉnh, và đỉnh đó cách không quá $k$ phần tử so với cả hai đầu. Vì $n \le 10^5$, không thể liệt kê mọi đoạn.
>
> Các đỉnh ngăn cách nhau. Với một đỉnh duy nhất $p$, đầu trái không được chạm tới đỉnh trước, đầu phải không được chạm tới đỉnh sau, và cả hai đầu phải nằm trong $[p-k,p+k]$.
>
> Thu thập tất cả các đỉnh, sau đó với mỗi đỉnh nhân số vị trí đầu trái hợp lệ với số vị trí đầu phải hợp lệ.
>
> Các giới hạn theo đỉnh lân cận đảm bảo tính duy nhất.

<!-- thinking:end -->

Trước tiên, ta duyệt mảng để tìm tất cả vị trí đỉnh và lưu chúng vào danh sách $\textit{peaks}$.

Với mỗi vị trí đỉnh, ta tính các biên trái và phải có tâm là đỉnh đó, sao cho khoảng cách không vượt quá $k$. Nếu có nhiều đỉnh, ta cần đảm bảo mảng con được tính không chứa các đỉnh khác. Sau đó, dựa trên các biên trái và phải, ta tính số mảng con hợp lệ có tâm tại mỗi đỉnh rồi cộng vào đáp án.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validSubarrays(self, nums: list[int], k: int) -> int:
        n = len(nums)
        peaks = []
        for i in range(1, n - 1):
            if nums[i] > nums[i - 1] and nums[i] > nums[i + 1]:
                peaks.append(i)

        ans = 0
        for j, p in enumerate(peaks):
            left_min = max(p - k, 0)
            if j > 0:
                left_min = max(left_min, peaks[j - 1] + 1)

            right_max = min(p + k, n - 1)
            if j < len(peaks) - 1:
                right_max = min(right_max, peaks[j + 1] - 1)

            ans += (p - left_min + 1) * (right_max - p + 1)
        return ans
```

#### Java

```java
class Solution {
    public long validSubarrays(int[] nums, int k) {
        int n = nums.length;
        List<Integer> peaks = new ArrayList<>();

        for (int i = 1; i < n - 1; i++) {
            if (nums[i] > nums[i - 1] && nums[i] > nums[i + 1]) {
                peaks.add(i);
            }
        }

        long ans = 0;
        for (int j = 0; j < peaks.size(); j++) {
            int p = peaks.get(j);

            int leftMin = Math.max(p - k, 0);
            if (j > 0) {
                leftMin = Math.max(leftMin, peaks.get(j - 1) + 1);
            }

            int rightMax = Math.min(p + k, n - 1);
            if (j < peaks.size() - 1) {
                rightMax = Math.min(rightMax, peaks.get(j + 1) - 1);
            }

            ans += (long) (p - leftMin + 1) * (rightMax - p + 1);
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long validSubarrays(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> peaks;

        for (int i = 1; i < n - 1; ++i) {
            if (nums[i] > nums[i - 1] && nums[i] > nums[i + 1]) {
                peaks.push_back(i);
            }
        }

        long long ans = 0;
        for (int j = 0; j < peaks.size(); ++j) {
            int p = peaks[j];

            int leftMin = max(p - k, 0);
            if (j > 0) {
                leftMin = max(leftMin, peaks[j - 1] + 1);
            }

            int rightMax = min(p + k, n - 1);
            if (j < peaks.size() - 1) {
                rightMax = min(rightMax, peaks[j + 1] - 1);
            }

            ans += 1LL * (p - leftMin + 1) * (rightMax - p + 1);
        }

        return ans;
    }
};
```

#### Go

```go
func validSubarrays(nums []int, k int) int64 {
	n := len(nums)
	peaks := []int{}

	for i := 1; i < n-1; i++ {
		if nums[i] > nums[i-1] && nums[i] > nums[i+1] {
			peaks = append(peaks, i)
		}
	}

	var ans int64
	for j, p := range peaks {
		leftMin := max(p-k, 0)
		if j > 0 {
			leftMin = max(leftMin, peaks[j-1]+1)
		}

		rightMax := min(p+k, n-1)
		if j < len(peaks)-1 {
			rightMax = min(rightMax, peaks[j+1]-1)
		}

		ans += int64(p-leftMin+1) * int64(rightMax-p+1)
	}

	return ans
}
```

#### TypeScript

```ts
function validSubarrays(nums: number[], k: number): number {
    const n = nums.length;
    const peaks: number[] = [];

    for (let i = 1; i < n - 1; i++) {
        if (nums[i] > nums[i - 1] && nums[i] > nums[i + 1]) {
            peaks.push(i);
        }
    }

    let ans = 0;
    for (let j = 0; j < peaks.length; j++) {
        const p = peaks[j];

        let leftMin = Math.max(p - k, 0);
        if (j > 0) {
            leftMin = Math.max(leftMin, peaks[j - 1] + 1);
        }

        let rightMax = Math.min(p + k, n - 1);
        if (j < peaks.length - 1) {
            rightMax = Math.min(rightMax, peaks[j + 1] - 1);
        }

        ans += (p - leftMin + 1) * (rightMax - p + 1);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
