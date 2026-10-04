---
comments: true
difficulty: Medium
rating: 2047
source: Weekly Contest 373 Q3
tags:
    - Union Find
    - Array
    - Sorting
---

<!-- problem:start -->

# [2948. Make Lexicographically Smallest Array by Swapping Elements](https://leetcode.com/problems/make-lexicographically-smallest-array-by-swapping-elements)

[Tài liệu tiếng Trung](/solution/2900-2999/2948.Make%20Lexicographically%20Smallest%20Array%20by%20Swapping%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>dương</strong> <strong>được đánh chỉ số từ 0</strong> <code>nums</code> và một số nguyên <strong>dương</strong> <code>limit</code>.</p>

<p>Trong một thao tác, bạn có thể chọn hai chỉ số bất kỳ <code>i</code> và <code>j</code> rồi hoán đổi <code>nums[i]</code> và <code>nums[j]</code> <strong>nếu</strong> <code>|nums[i] - nums[j]| &lt;= limit</code>.</p>

<p>Hãy trả về <em><strong>mảng nhỏ nhất theo thứ tự từ điển</strong> có thể thu được bằng cách thực hiện thao tác trên một số lần bất kỳ</em>.</p>

<p>Mảng <code>a</code> nhỏ hơn mảng <code>b</code> theo thứ tự từ điển nếu tại vị trí đầu tiên mà <code>a</code> và <code>b</code> khác nhau, phần tử của mảng <code>a</code> nhỏ hơn phần tử tương ứng trong <code>b</code>. Ví dụ, mảng <code>[2,10,3]</code> nhỏ hơn mảng <code>[10,2,3]</code> theo thứ tự từ điển vì chúng khác nhau tại chỉ số <code>0</code> và <code>2 &lt; 10</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,3,9,8], limit = 2
<strong>Đầu ra:</strong> [1,3,5,8,9]
<strong>Giải thích:</strong> Thực hiện thao tác 2 lần:
- Hoán đổi nums[1] với nums[2]. Mảng trở thành [1,3,5,9,8]
- Hoán đổi nums[3] với nums[4]. Mảng trở thành [1,3,5,8,9]
Không thể thu được mảng nhỏ hơn theo thứ tự từ điển bằng cách thực hiện thêm thao tác.
Lưu ý rằng có thể thu được cùng một kết quả bằng các thao tác khác nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,7,6,18,2,1], limit = 3
<strong>Đầu ra:</strong> [1,6,7,18,1,2]
<strong>Giải thích:</strong> Thực hiện thao tác 3 lần:
- Hoán đổi nums[1] với nums[2]. Mảng trở thành [1,6,7,18,2,1]
- Hoán đổi nums[0] với nums[4]. Mảng trở thành [2,6,7,18,1,1]
- Hoán đổi nums[0] với nums[5]. Mảng trở thành [1,6,7,18,1,2]
Không thể thu được mảng nhỏ hơn theo thứ tự từ điển bằng cách thực hiện thêm thao tác.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,7,28,19,10], limit = 3
<strong>Đầu ra:</strong> [1,7,28,19,10]
<strong>Giải thích:</strong> [1,7,28,19,10] là mảng nhỏ nhất theo thứ tự từ điển mà ta có thể thu được vì không thể thực hiện thao tác trên bất kỳ hai chỉ số nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= limit &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị chênh lệch không quá $limit$ có thể được hoán đổi, và tính bắc cầu tạo thành các thành phần liên thông. Sau khi sắp xếp, một đoạn liên tiếp mà khoảng cách giữa mọi cặp phần tử kề nhau là $\le limit$ là một thành phần; các đoạn khác nhau không thể trộn lẫn.
>
> Các giá trị trong một thành phần có thể được gán lại cho các chỉ số ban đầu của thành phần đó. Sắp xếp các cặp $(value,index)$, chia thành các đoạn, sắp xếp các chỉ số rồi ghi các giá trị trở lại theo thứ tự tăng dần. $n \le 10^5$.

<!-- thinking:end -->

Theo mô tả bài toán, mảng đã sắp xếp $\textit{nums}$ có thể được chia thành một số mảng con sao cho hiệu giữa hai phần tử kề nhau trong mỗi mảng con không vượt quá $\textit{limit}$.

Do đó, mảng nhỏ nhất theo thứ tự từ điển có thể thu được bằng cách hoán đổi là mảng trong đó các phần tử thuộc mỗi mảng con được sắp xếp rồi đặt lại vào các vị trí ban đầu theo thứ tự.

Trước tiên, ta ghép mỗi phần tử trong $\textit{nums}$ với chỉ số của nó để tạo thành một mảng các tuple, sau đó sắp xếp theo giá trị phần tử. Tiếp theo, ta duyệt mảng tuple đã sắp xếp, xác định phạm vi của từng mảng con, sắp xếp các phần tử trong mỗi mảng con theo chỉ số ban đầu, rồi điền chúng trở lại các vị trí tương ứng để thu được kết quả cuối cùng.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lexicographicallySmallestArray(self, nums: List[int], limit: int) -> List[int]:
        n = len(nums)
        arr = sorted(zip(nums, range(n)))
        ans = [0] * n
        i = 0
        while i < n:
            j = i + 1
            while j < n and arr[j][0] - arr[j - 1][0] <= limit:
                j += 1
            idx = sorted(k for _, k in arr[i:j])
            for k, (x, _) in zip(idx, arr[i:j]):
                ans[k] = x
            i = j
        return ans
```

#### Java

```java
class Solution {
    public int[] lexicographicallySmallestArray(int[] nums, int limit) {
        int n = nums.length;
        Integer[] idx = new Integer[n];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, (i, j) -> nums[i] - nums[j]);
        int[] ans = new int[n];
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && nums[idx[j]] - nums[idx[j - 1]] <= limit) {
                ++j;
            }
            Integer[] t = Arrays.copyOfRange(idx, i, j);
            Arrays.sort(t, (x, y) -> x - y);
            for (int k = i; k < j; ++k) {
                ans[t[k - i]] = nums[idx[k]];
            }
            i = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> lexicographicallySmallestArray(vector<int>& nums, int limit) {
        int n = nums.size();
        vector<int> idx(n);
        iota(idx.begin(), idx.end(), 0);
        sort(idx.begin(), idx.end(), [&](int i, int j) {
            return nums[i] < nums[j];
        });
        vector<int> ans(n);
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && nums[idx[j]] - nums[idx[j - 1]] <= limit) {
                ++j;
            }
            vector<int> t(idx.begin() + i, idx.begin() + j);
            sort(t.begin(), t.end());
            for (int k = i; k < j; ++k) {
                ans[t[k - i]] = nums[idx[k]];
            }
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func lexicographicallySmallestArray(nums []int, limit int) []int {
	n := len(nums)
	idx := make([]int, n)
	for i := range idx {
		idx[i] = i
	}
	slices.SortFunc(idx, func(i, j int) int { return nums[i] - nums[j] })
	ans := make([]int, n)
	for i := 0; i < n; {
		j := i + 1
		for j < n && nums[idx[j]]-nums[idx[j-1]] <= limit {
			j++
		}
		t := slices.Clone(idx[i:j])
		slices.Sort(t)
		for k := i; k < j; k++ {
			ans[t[k-i]] = nums[idx[k]]
		}
		i = j
	}
	return ans
}
```

#### TypeScript

```ts
function lexicographicallySmallestArray(nums: number[], limit: number): number[] {
    const n: number = nums.length;
    const idx: number[] = Array.from({ length: n }, (_, i) => i);
    idx.sort((i, j) => nums[i] - nums[j]);
    const ans: number[] = Array(n).fill(0);
    for (let i = 0; i < n;) {
        let j = i + 1;
        while (j < n && nums[idx[j]] - nums[idx[j - 1]] <= limit) {
            j++;
        }
        const t: number[] = idx.slice(i, j).sort((a, b) => a - b);
        for (let k: number = i; k < j; k++) {
            ans[t[k - i]] = nums[idx[k]];
        }
        i = j;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn lexicographically_smallest_array(nums: Vec<i32>, limit: i32) -> Vec<i32> {
        let n = nums.len();
        let mut idx: Vec<usize> = (0..n).collect();

        idx.sort_by_key(|&i| nums[i]);

        let mut ans = vec![0; n];

        let mut i = 0;
        while i < n {
            let mut j = i + 1;
            while j < n && nums[idx[j]] - nums[idx[j - 1]] <= limit {
                j += 1;
            }

            let mut t = idx[i..j].to_vec();
            t.sort();

            for k in i..j {
                ans[t[k - i]] = nums[idx[k]];
            }

            i = j;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
