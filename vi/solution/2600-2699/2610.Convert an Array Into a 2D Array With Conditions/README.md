---
comments: true
difficulty: Medium
rating: 1373
source: Weekly Contest 339 Q2
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2610. Convert an Array Into a 2D Array With Conditions](https://leetcode.com/problems/convert-an-array-into-a-2d-array-with-conditions)

[中文文档](/solution/2600-2699/2610.Convert%20an%20Array%20Into%20a%202D%20Array%20With%20Conditions/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Hãy tạo một mảng 2D từ <code>nums</code> sao cho thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Mảng 2D <strong>chỉ</strong> được chứa các phần tử của mảng <code>nums</code>.</li>
	<li>Mỗi hàng trong mảng 2D chứa các số nguyên <strong>khác nhau</strong>.</li>
	<li>Số hàng trong mảng 2D phải là <strong>ít nhất</strong>.</li>
</ul>

<p>Trả về <em>mảng kết quả</em>. Nếu có nhiều đáp án, hãy trả về bất kỳ đáp án nào.</p>

<p><strong>Lưu ý</strong> rằng mỗi hàng trong mảng 2D có thể chứa số lượng phần tử khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,4,1,2,3,1]
<strong>Đầu ra:</strong> [[1,3,4,2],[1,3],[1]]
<strong>Giải thích:</strong> Ta có thể tạo một mảng 2D gồm các hàng sau:
- 1,3,4,2
- 1,3
- 1
Tất cả phần tử của nums đều được sử dụng và mỗi hàng của mảng 2D chứa các số nguyên khác nhau, nên đây là một đáp án hợp lệ.
Có thể chứng minh rằng không thể có một mảng hợp lệ với ít hơn 3 hàng.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> [[4,3,2,1]]
<strong>Giải thích:</strong> Tất cả phần tử của mảng đều khác nhau, nên ta có thể giữ tất cả chúng trong hàng đầu tiên của mảng 2D.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 200</code></li>
	<li><code>1 &lt;= nums[i] &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hoặc Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi hàng không được lặp lại một giá trị và ta muốn số hàng là ít nhất. Tần suất lớn nhất chính là số hàng nhỏ nhất. Cách tham lam xây dựng từng hàng sẽ phải kiểm tra lại trạng thái đã dùng; tuy $n \le 200$ cho phép làm vậy, tần suất đã cho biết trực tiếp cách sắp xếp.
>
> Một giá trị $x$ xuất hiện $v$ lần phải được đặt vào $v$ hàng đầu tiên, mỗi hàng một lần. Đặt $x$ vào các hàng $0,\ldots,v-1$ vừa thỏa mãn cả hai điều kiện vừa không cần tìm kiếm.

<!-- thinking:end -->

Trước tiên, ta dùng một mảng hoặc hash table $\textit{cnt}$ để đếm tần suất của từng phần tử trong mảng $\textit{nums}$.

Sau đó, ta duyệt qua $\textit{cnt}$. Với mỗi phần tử $x$, ta thêm nó vào hàng thứ 0, hàng thứ 1, hàng thứ 2, ..., và hàng thứ $(cnt[x]-1)$ của danh sách kết quả.

Cuối cùng, ta trả về danh sách kết quả.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMatrix(self, nums: List[int]) -> List[List[int]]:
        cnt = Counter(nums)
        ans = []
        for x, v in cnt.items():
            for i in range(v):
                if len(ans) <= i:
                    ans.append([])
                ans[i].append(x)
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> findMatrix(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        int n = nums.length;
        int[] cnt = new int[n + 1];
        for (int x : nums) {
            ++cnt[x];
        }
        for (int x = 1; x <= n; ++x) {
            int v = cnt[x];
            for (int j = 0; j < v; ++j) {
                if (ans.size() <= j) {
                    ans.add(new ArrayList<>());
                }
                ans.get(j).add(x);
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
    vector<vector<int>> findMatrix(vector<int>& nums) {
        vector<vector<int>> ans;
        int n = nums.size();
        vector<int> cnt(n + 1);
        for (int& x : nums) {
            ++cnt[x];
        }
        for (int x = 1; x <= n; ++x) {
            int v = cnt[x];
            for (int j = 0; j < v; ++j) {
                if (ans.size() <= j) {
                    ans.push_back({});
                }
                ans[j].push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findMatrix(nums []int) (ans [][]int) {
	n := len(nums)
	cnt := make([]int, n+1)
	for _, x := range nums {
		cnt[x]++
	}
	for x, v := range cnt {
		for j := 0; j < v; j++ {
			if len(ans) <= j {
				ans = append(ans, []int{})
			}
			ans[j] = append(ans[j], x)
		}
	}
	return
}
```

#### TypeScript

```ts
function findMatrix(nums: number[]): number[][] {
    const ans: number[][] = [];
    const n = nums.length;
    const cnt: number[] = Array(n + 1).fill(0);
    for (const x of nums) {
        ++cnt[x];
    }
    for (let x = 1; x <= n; ++x) {
        for (let j = 0; j < cnt[x]; ++j) {
            if (ans.length <= j) {
                ans.push([]);
            }
            ans[j].push(x);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_matrix(nums: Vec<i32>) -> Vec<Vec<i32>> {
        let n = nums.len();
        let mut cnt = vec![0; n + 1];
        let mut ans: Vec<Vec<i32>> = Vec::new();

        for &x in &nums {
            cnt[x as usize] += 1;
        }

        for x in 1..=n as i32 {
            for j in 0..cnt[x as usize] {
                if ans.len() <= j {
                    ans.push(Vec::new());
                }
                ans[j].push(x);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
