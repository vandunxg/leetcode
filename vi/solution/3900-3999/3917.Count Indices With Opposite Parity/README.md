---
comments: true
difficulty: Easy
rating: 1198
source: Weekly Contest 500 Q1
tags:
    - Array
---

<!-- problem:start -->

# [3917. Count Indices With Opposite Parity](https://leetcode.com/problems/count-indices-with-opposite-parity)

[中文文档](/solution/3900-3999/3917.Count%20Indices%20With%20Opposite%20Parity/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code>.</p>

<p><strong>Điểm số</strong> của một chỉ số <code>i</code> được định nghĩa là số lượng chỉ số <code>j</code> thỏa mãn:</p>

<ul>
	<li><code>i &lt; j &lt; n</code>, và</li>
	<li><code>nums[i]</code> và <code>nums[j]</code> có parity khác nhau (một số chẵn và số còn lại là số lẻ).</li>
</ul>

<p>Trả về một mảng số nguyên <code>answer</code> có độ dài <code>n</code>, trong đó <code>answer[i]</code> là điểm số của chỉ số <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,1,1,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>nums[0] = 1</code> là số lẻ. Do đó, các chỉ số <code>j = 1</code> và <code>j = 3</code> thỏa mãn điều kiện, nên điểm số của chỉ số 0 là 2.</li>
	<li><code>nums[1] = 2</code> là số chẵn. Do đó, chỉ số <code>j = 2</code> thỏa mãn điều kiện, nên điểm số của chỉ số 1 là 1.</li>
	<li><code>nums[2] = 3</code> là số lẻ. Do đó, chỉ số <code>j = 3</code> thỏa mãn điều kiện, nên điểm số của chỉ số 2 là 1.</li>
	<li><code>nums[3] = 4</code> là số chẵn. Do đó, không có chỉ số nào thỏa mãn điều kiện, nên điểm số của chỉ số 3 là 0.</li>
</ul>

<p>Vậy <code>answer = [2, 1, 1, 0]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0]</span></p>

<p><strong>Giải thích:</strong></p>

<p><code>nums</code> chỉ có một phần tử. Do đó, điểm số của chỉ số 0 là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Việc quét lại phần còn lại của mảng để tìm các phần tử có parity đối lập tại mỗi chỉ số có độ phức tạp $O(n^2)$. Độ phức tạp này vẫn phù hợp với $n\le 100$, nhưng đáp án chỉ phụ thuộc vào tổng số phần tử chẵn và lẻ.
>
> Trước tiên, đếm số phần tử chẵn và lẻ vào $\textit{cnt}[0]$ và $\textit{cnt}[1]$. Khi duyệt đến $x$, giảm bucket tương ứng của nó trước, sau đó ghi lại số phần tử còn lại có parity đối lập.
>
> Nhờ đó, có thể tính đáp án cho mỗi chỉ số trong thời gian hằng số mà không cần quét lồng thêm lần nữa.

<!-- thinking:end -->

Trước tiên, ta đếm số phần tử chẵn và lẻ trong mảng $\textit{nums}$, lần lượt ký hiệu là $cnt[0]$ và $cnt[1]$.

Sau đó, ta duyệt mảng $\textit{nums}$ từ trái sang phải. Với chỉ số $i$, trước tiên ta giảm $cnt[\textit{nums}[i] \bmod 2]$ đi 1, rồi gán $cnt[\textit{nums}[i] \bmod 2 \oplus 1]$ cho $ans[i]$.

Sau khi duyệt xong, trả về mảng đáp án $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Không tính không gian của mảng đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOppositeParity(self, nums: list[int]) -> list[int]:
        cnt = [0, 0]
        for x in nums:
            cnt[x & 1] += 1
        ans = [0] * len(nums)
        for i, x in enumerate(nums):
            cnt[x & 1] -= 1
            ans[i] = cnt[x & 1 ^ 1]
        return ans
```

#### Java

```java
class Solution {
    public int[] countOppositeParity(int[] nums) {
        int[] cnt = new int[2];
        for (int x : nums) {
            cnt[x & 1]++;
        }
        int n = nums.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            cnt[nums[i] & 1]--;
            ans[i] = cnt[nums[i] & 1 ^ 1];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countOppositeParity(vector<int>& nums) {
        int cnt[2] = {0, 0};
        for (int x : nums) {
            cnt[x & 1]++;
        }
        int n = nums.size();
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            cnt[nums[i] & 1]--;
            ans[i] = cnt[(nums[i] & 1) ^ 1];
        }
        return ans;
    }
};
```

#### Go

```go
func countOppositeParity(nums []int) []int {
    cnt := [2]int{}
    for _, x := range nums {
        cnt[x&1]++
    }
    n := len(nums)
    ans := make([]int, n)
    for i, x := range nums {
        cnt[x&1]--
        ans[i] = cnt[x&1^1]
    }
    return ans
}
```

#### TypeScript

```ts
function countOppositeParity(nums: number[]): number[] {
    const cnt = Array<number>(2).fill(0);
    for (const x of nums) {
        ++cnt[x & 1];
    }
    const n = nums.length;
    const ans = Array<number>(n).fill(0);
    for (let i = 0; i < n; ++i) {
        --cnt[nums[i] & 1];
        ans[i] = cnt[(nums[i] & 1) ^ 1];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
