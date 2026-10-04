---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Sorting
    - Monotonic Stack
---

<!-- problem:start -->

# [2863. Maximum Length of Semi-Decreasing Subarrays 🔒](https://leetcode.com/problems/maximum-length-of-semi-decreasing-subarrays)

[中文文档](/solution/2800-2899/2863.Maximum%20Length%20of%20Semi-Decreasing%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Hãy trả về <em>độ dài của mảng con <strong>semi-decreasing</strong> dài nhất của </em><code>nums</code><em>, và trả về </em><code>0</code><em> nếu không có mảng con nào như vậy.</em></p>

<ul>
	<li><b>Mảng con</b> là một dãy phần tử liên tiếp, không rỗng, nằm trong một mảng.</li>
	<li>Một mảng không rỗng là <strong>semi-decreasing</strong> nếu phần tử đầu tiên của nó <strong>lớn hơn nghiêm ngặt</strong> phần tử cuối cùng.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,6,5,4,3,2,1,6,10,11]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Chọn mảng con [7,6,5,4,3,2,1,6].
Phần tử đầu tiên là 7 và phần tử cuối cùng là 6, nên điều kiện được thỏa mãn.
Do đó, đáp án là độ dài của mảng con, bằng 8.
Có thể chứng minh rằng không có mảng con nào thỏa mãn điều kiện đã cho có độ dài lớn hơn 8.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [57,55,50,60,61,58,63,59,64,60,63]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Chọn mảng con [61,58,63,59,64,60].
Phần tử đầu tiên là 61 và phần tử cuối cùng là 60, nên điều kiện được thỏa mãn.
Do đó, đáp án là độ dài của mảng con, bằng 6.
Có thể chứng minh rằng không có mảng con nào thỏa mãn điều kiện đã cho có độ dài lớn hơn 6.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Vì không có mảng con semi-decreasing nào trong mảng đã cho, đáp án là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con semi-decreasing chỉ cần có phần tử đầu tiên lớn hơn phần tử cuối cùng. Duyệt các giá trị từ lớn đến nhỏ và duy trì chỉ số nhỏ nhất $k$ đã gặp, ta có được một độ dài ứng viên khi ghép lần xuất hiện ngoài cùng bên phải của giá trị hiện tại với $k$.

<!-- thinking:end -->

Bài toán về cơ bản là tìm cặp nghịch thế có độ dài lớn nhất. Ta có thể dùng một hash table $d$ để ghi lại chỉ số $i$ tương ứng với mỗi số $x$ trong mảng.

Tiếp theo, ta duyệt các key của hash table theo thứ tự giảm dần của các số. Ta duy trì một số $k$ để theo dõi chỉ số nhỏ nhất đã xuất hiện cho đến thời điểm hiện tại. Với số hiện tại $x$, ta có thể nhận được độ dài cặp nghịch thế lớn nhất là $d[x][|d[x]|-1]-k + 1$, trong đó $|d[x]|$ là độ dài của mảng $d[x]$, tức là số lần số $x$ xuất hiện trong mảng ban đầu. Ta cập nhật đáp án tương ứng. Sau đó, ta cập nhật $k$ thành $d[x][0]$, là chỉ số mà số $x$ xuất hiện lần đầu trong mảng ban đầu. Ta tiếp tục duyệt các key của hash table cho đến khi duyệt hết tất cả các key.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubarrayLength(self, nums: List[int]) -> int:
        d = defaultdict(list)
        for i, x in enumerate(nums):
            d[x].append(i)
        ans, k = 0, inf
        for x in sorted(d, reverse=True):
            ans = max(ans, d[x][-1] - k + 1)
            k = min(k, d[x][0])
        return ans
```

#### Java

```java
class Solution {
    public int maxSubarrayLength(int[] nums) {
        TreeMap<Integer, List<Integer>> d = new TreeMap<>(Comparator.reverseOrder());
        for (int i = 0; i < nums.length; ++i) {
            d.computeIfAbsent(nums[i], k -> new ArrayList<>()).add(i);
        }
        int ans = 0, k = 1 << 30;
        for (List<Integer> idx : d.values()) {
            ans = Math.max(ans, idx.get(idx.size() - 1) - k + 1);
            k = Math.min(k, idx.get(0));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSubarrayLength(vector<int>& nums) {
        map<int, vector<int>, greater<int>> d;
        for (int i = 0; i < nums.size(); ++i) {
            d[nums[i]].push_back(i);
        }
        int ans = 0, k = 1 << 30;
        for (auto& [_, idx] : d) {
            ans = max(ans, idx.back() - k + 1);
            k = min(k, idx[0]);
        }
        return ans;
    }
};
```

#### Go

```go
func maxSubarrayLength(nums []int) (ans int) {
	d := map[int][]int{}
	for i, x := range nums {
		d[x] = append(d[x], i)
	}
	keys := []int{}
	for x := range d {
		keys = append(keys, x)
	}
	sort.Slice(keys, func(i, j int) bool { return keys[i] > keys[j] })
	k := 1 << 30
	for _, x := range keys {
		idx := d[x]
		ans = max(ans, idx[len(idx)-1]-k+1)
		k = min(k, idx[0])
	}
	return ans
}
```

#### TypeScript

```ts
function maxSubarrayLength(nums: number[]): number {
    const d: Map<number, number[]> = new Map();
    for (let i = 0; i < nums.length; ++i) {
        if (!d.has(nums[i])) {
            d.set(nums[i], []);
        }
        d.get(nums[i])!.push(i);
    }
    const keys = Array.from(d.keys()).sort((a, b) => b - a);
    let ans = 0;
    let k = Infinity;
    for (const x of keys) {
        const idx = d.get(x)!;
        ans = Math.max(ans, idx.at(-1) - k + 1);
        k = Math.min(k, idx[0]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
