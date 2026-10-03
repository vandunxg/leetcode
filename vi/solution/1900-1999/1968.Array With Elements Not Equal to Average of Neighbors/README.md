---
comments: true
difficulty: Medium
rating: 1499
source: Weekly Contest 254 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1968. Array With Elements Not Equal to Average of Neighbors](https://leetcode.com/problems/array-with-elements-not-equal-to-average-of-neighbors)

[中文文档](/solution/1900-1999/1968.Array%20With%20Elements%20Not%20Equal%20to%20Average%20of%20Neighbors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> <strong>được đánh chỉ số từ 0</strong>, trong đó các phần tử <strong>phân biệt</strong>. Hãy sắp xếp lại các phần tử trong mảng sao cho mọi phần tử trong mảng sau khi sắp xếp lại đều <strong>không</strong> bằng <strong>giá trị trung bình</strong> của hai phần tử kề nó.</p>

<p>Cụ thể hơn, mảng sau khi sắp xếp lại cần thỏa mãn với mọi <code>i</code> thuộc đoạn <code>1 &lt;= i &lt; nums.length - 1</code>, <code>(nums[i-1] + nums[i+1]) / 2</code> <strong>không</strong> bằng <code>nums[i]</code>.</p>

<p>Trả về <em><strong>bất kỳ</strong> cách sắp xếp lại nào của </em><code>nums</code><em> thỏa mãn yêu cầu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> [1,2,4,5,3]
<strong>Giải thích:</strong>
Khi i=1, nums[i] = 2, và giá trị trung bình của hai phần tử kề là (1+4) / 2 = 2.5.
Khi i=2, nums[i] = 4, và giá trị trung bình của hai phần tử kề là (2+5) / 2 = 3.5.
Khi i=3, nums[i] = 5, và giá trị trung bình của hai phần tử kề là (4+3) / 2 = 3.5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,2,0,9,7]
<strong>Đầu ra:</strong> [9,7,6,2,0]
<strong>Giải thích:</strong>
Khi i=1, nums[i] = 7, và giá trị trung bình của hai phần tử kề là (9+6) / 2 = 7.5.
Khi i=2, nums[i] = 6, và giá trị trung bình của hai phần tử kề là (7+2) / 2 = 4.5.
Khi i=3, nums[i] = 2, và giá trị trung bình của hai phần tử kề là (6+0) / 2 = 3.
Lưu ý rằng mảng ban đầu [6,2,0,9,7] cũng thỏa mãn các điều kiện.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tránh trường hợp $2\cdot a[i]=a[i-1]+a[i+1]$. Vì các giá trị phân biệt, việc sắp xếp rồi đan xen nửa nhỏ với nửa lớn sẽ cho kết quả phù hợp.
>
> Các chỉ số chẵn nhận nửa đầu, còn các chỉ số lẻ nhận nửa sau; khi đó hai phần tử kề mỗi phần tử ở giữa sẽ đến từ hai đầu đối lập và không thể có giá trị trung bình bằng phần tử đó.

<!-- thinking:end -->

Vì các phần tử trong mảng phân biệt, trước tiên ta có thể sắp xếp mảng, sau đó chia mảng thành hai phần. Đặt nửa đầu các phần tử vào các vị trí chẵn của mảng kết quả, và nửa sau vào các vị trí lẻ của mảng kết quả. Nhờ đó, với mỗi phần tử, hai phần tử kề nó sẽ không có giá trị bằng giá trị trung bình của phần tử đó.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rearrangeArray(self, nums: List[int]) -> List[int]:
        nums.sort()
        n = len(nums)
        m = (n + 1) // 2
        ans = []
        for i in range(m):
            ans.append(nums[i])
            if i + m < n:
                ans.append(nums[i + m])
        return ans
```

#### Java

```java
class Solution {
    public int[] rearrangeArray(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        int m = (n + 1) >> 1;
        int[] ans = new int[n];
        for (int i = 0, j = 0; i < n; i += 2, j++) {
            ans[i] = nums[j];
            if (j + m < n) {
                ans[i + 1] = nums[j + m];
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
    vector<int> rearrangeArray(vector<int>& nums) {
        ranges::sort(nums);
        vector<int> ans;
        int n = nums.size();
        int m = (n + 1) >> 1;
        for (int i = 0; i < m; ++i) {
            ans.push_back(nums[i]);
            if (i + m < n) {
                ans.push_back(nums[i + m]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func rearrangeArray(nums []int) (ans []int) {
	sort.Ints(nums)
	n := len(nums)
	m := (n + 1) >> 1
	for i := 0; i < m; i++ {
		ans = append(ans, nums[i])
		if i+m < n {
			ans = append(ans, nums[i+m])
		}
	}
	return
}
```

#### TypeScript

```ts
function rearrangeArray(nums: number[]): number[] {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    const m = (n + 1) >> 1;
    const ans: number[] = [];
    for (let i = 0; i < m; i++) {
        ans.push(nums[i]);
        if (i + m < n) {
            ans.push(nums[i + m]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
