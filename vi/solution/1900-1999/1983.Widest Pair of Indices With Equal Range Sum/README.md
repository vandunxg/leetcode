---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [1983. Widest Pair of Indices With Equal Range Sum 🔒](https://leetcode.com/problems/widest-pair-of-indices-with-equal-range-sum)

[中文文档](/solution/1900-1999/1983.Widest%20Pair%20of%20Indices%20With%20Equal%20Range%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng nhị phân <strong>đánh chỉ số từ 0</strong> <code>nums1</code> và <code>nums2</code>. Hãy tìm cặp chỉ số <strong>rộng nhất</strong> <code>(i, j)</code> sao cho <code>i &lt;= j</code> và <code>nums1[i] + nums1[i+1] + ... + nums1[j] == nums2[i] + nums2[i+1] + ... + nums2[j]</code>.</p>

<p>Cặp chỉ số <strong>rộng nhất</strong> là cặp có <strong>khoảng cách</strong> <strong>lớn nhất</strong> giữa <code>i</code> và <code>j</code>. <strong>Khoảng cách</strong> giữa một cặp chỉ số được định nghĩa là <code>j - i + 1</code>.</p>

<p>Trả về <em><strong>khoảng cách</strong> của cặp chỉ số <strong>rộng nhất</strong>. Nếu không có cặp chỉ số nào thỏa mãn điều kiện, trả về </em><code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,1,0,1], nums2 = [0,1,1,0]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Nếu i = 1 và j = 3:
nums1[1] + nums1[2] + nums1[3] = 1 + 0 + 1 = 2.
nums2[1] + nums2[2] + nums2[3] = 1 + 1 + 0 = 2.
Khoảng cách giữa i và j là j - i + 1 = 3 - 1 + 1 = 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [0,1], nums2 = [1,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Nếu i = 1 và j = 1:
nums1[1] = 1.
nums2[1] = 1.
Khoảng cách giữa i và j là j - i + 1 = 1 - 1 + 1 = 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [0], nums2 = [1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Không có cặp chỉ số nào thỏa mãn yêu cầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>nums1[i]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>nums2[i]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Hai tổng trên đoạn bằng nhau tương đương với việc tìm một mảng con có tổng bằng 0 trong mảng hiệu theo từng phần tử. Mảng con dài nhất được tìm bằng vị trí xuất hiện đầu tiên của mỗi prefix.
>
> Một hash table lưu chỉ số xuất hiện sớm nhất của mỗi prefix; khi gặp lại một prefix, ta cập nhật độ dài.

<!-- thinking:end -->

Ta nhận thấy rằng với mọi cặp chỉ số $(i, j)$, nếu $nums1[i] + nums1[i+1] + ... + nums1[j] = nums2[i] + nums2[i+1] + ... + nums2[j]$, thì $nums1[i] - nums2[i] + nums1[i+1] - nums2[i+1] + ... + nums1[j] - nums2[j] = 0$. Nếu lấy các phần tử tương ứng của mảng $nums1$ và mảng $nums2$ trừ cho nhau để tạo thành một mảng mới $nums$, bài toán được chuyển thành tìm mảng con dài nhất trong $nums$ sao cho tổng các phần tử của mảng con bằng $0$. Ta có thể giải bài toán này bằng phương pháp prefix sum + hash table.

Ta định nghĩa biến $s$ biểu diễn prefix sum hiện tại của $nums$, đồng thời dùng hash table $d$ để lưu vị trí xuất hiện đầu tiên của mỗi prefix sum. Ban đầu, $s = 0$ và $d[0] = -1$.

Tiếp theo, ta duyệt qua từng phần tử $x$ trong mảng $nums$, tính giá trị của $s$, rồi kiểm tra xem $s$ có tồn tại trong hash table hay không. Nếu $s$ tồn tại trong hash table, điều đó có nghĩa là có một mảng con $nums[d[s]+1,..i]$ có tổng bằng $0$, và ta cập nhật đáp án thành $\max(ans, i - d[s])$. Nếu không, ta thêm giá trị của $s$ vào hash table, cho biết vị trí xuất hiện đầu tiên của $s$ là $i$.

Sau khi duyệt xong, ta thu được đáp án cuối cùng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def widestPairOfIndices(self, nums1: List[int], nums2: List[int]) -> int:
        d = {0: -1}
        ans = s = 0
        for i, (a, b) in enumerate(zip(nums1, nums2)):
            s += a - b
            if s in d:
                ans = max(ans, i - d[s])
            else:
                d[s] = i
        return ans
```

#### Java

```java
class Solution {
    public int widestPairOfIndices(int[] nums1, int[] nums2) {
        Map<Integer, Integer> d = new HashMap<>();
        d.put(0, -1);
        int n = nums1.length;
        int s = 0;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            s += nums1[i] - nums2[i];
            if (d.containsKey(s)) {
                ans = Math.max(ans, i - d.get(s));
            } else {
                d.put(s, i);
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
    int widestPairOfIndices(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> d;
        d[0] = -1;
        int ans = 0, s = 0;
        int n = nums1.size();
        for (int i = 0; i < n; ++i) {
            s += nums1[i] - nums2[i];
            if (d.count(s)) {
                ans = max(ans, i - d[s]);
            } else {
                d[s] = i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func widestPairOfIndices(nums1 []int, nums2 []int) (ans int) {
	d := map[int]int{0: -1}
	s := 0
	for i := range nums1 {
		s += nums1[i] - nums2[i]
		if j, ok := d[s]; ok {
			ans = max(ans, i-j)
		} else {
			d[s] = i
		}
	}
	return
}
```

#### TypeScript

```ts
function widestPairOfIndices(nums1: number[], nums2: number[]): number {
    const d: Map<number, number> = new Map();
    d.set(0, -1);
    const n: number = nums1.length;
    let s: number = 0;
    let ans: number = 0;
    for (let i = 0; i < n; ++i) {
        s += nums1[i] - nums2[i];
        if (d.has(s)) {
            ans = Math.max(ans, i - (d.get(s) as number));
        } else {
            d.set(s, i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
