---
comments: true
difficulty: Easy
rating: 1217
source: Weekly Contest 474 Q1
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [3731. Find Missing Elements](https://leetcode.com/problems/find-missing-elements)

[中文文档](/solution/3700-3799/3731.Find%20Missing%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> gồm các số nguyên <strong>không trùng nhau</strong>.</p>

<p>Ban đầu, <code>nums</code> chứa <strong>mọi số nguyên</strong> trong một khoảng nhất định. Tuy nhiên, một số số nguyên có thể đã bị <strong>thiếu</strong> khỏi mảng.</p>

<p>Số nguyên <strong>nhỏ nhất</strong> và <strong>lớn nhất</strong> của khoảng ban đầu vẫn còn xuất hiện trong <code>nums</code>.</p>

<p>Hãy trả về một danh sách <strong>đã sắp xếp</strong> gồm tất cả các số nguyên bị thiếu trong khoảng này. Nếu không có số nguyên nào bị thiếu, hãy trả về một danh sách <strong>rỗng</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,2,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số nguyên nhỏ nhất là 1 và lớn nhất là 5, nên khoảng đầy đủ phải là <code>[1,2,3,4,5]</code>. Trong các số này, chỉ có 3 bị thiếu.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,8,6,9]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số nguyên nhỏ nhất là 6 và lớn nhất là 9, nên khoảng đầy đủ là <code>[6,7,8,9]</code>. Tất cả các số nguyên đã xuất hiện, nên không có số nguyên nào bị thiếu.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,3,4]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số nguyên nhỏ nhất là 1 và lớn nhất là 5, nên khoảng đầy đủ phải là <code>[1,2,3,4,5]</code>. Các số nguyên bị thiếu là 2, 3 và 4.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Khoảng được xác định bởi giá trị nhỏ nhất và lớn nhất của mảng; các giá trị bị thiếu là những số nguyên nằm trong khoảng đó nhưng không xuất hiện. Với $n\le 100$, một set kết hợp với việc duyệt qua $(\textit{mn},\textit{mx})$ sẽ liệt kê chúng theo đúng thứ tự.

<!-- thinking:end -->

Trước tiên, ta tìm giá trị nhỏ nhất và lớn nhất trong mảng $\textit{nums}$, lần lượt ký hiệu là $\textit{mn}$ và $\textit{mx}$. Sau đó, ta dùng một hash table để lưu tất cả phần tử trong mảng $\textit{nums}$.

Tiếp theo, ta duyệt qua khoảng $[\textit{mn} + 1, \textit{mx} - 1]$. Với mỗi số nguyên $x$, nếu $x$ không có trong hash table, ta thêm nó vào danh sách kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMissingElements(self, nums: List[int]) -> List[int]:
        mn, mx = min(nums), max(nums)
        s = set(nums)
        return [x for x in range(mn + 1, mx) if x not in s]
```

#### Java

```java
class Solution {
    public List<Integer> findMissingElements(int[] nums) {
        int mn = 100, mx = 0;
        Set<Integer> s = new HashSet<>();
        for (int x : nums) {
            mn = Math.min(mn, x);
            mx = Math.max(mx, x);
            s.add(x);
        }
        List<Integer> ans = new ArrayList<>();
        for (int x = mn + 1; x < mx; ++x) {
            if (!s.contains(x)) {
                ans.add(x);
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
    vector<int> findMissingElements(vector<int>& nums) {
        int mn = 100, mx = 0;
        unordered_set<int> s;
        for (int x : nums) {
            mn = min(mn, x);
            mx = max(mx, x);
            s.insert(x);
        }
        vector<int> ans;
        for (int x = mn + 1; x < mx; ++x) {
            if (!s.count(x)) {
                ans.push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findMissingElements(nums []int) (ans []int) {
	mn, mx := 100, 0
	s := make(map[int]bool)
	for _, x := range nums {
        mn = min(mn, x)
        mx = max(mx, x)
		s[x] = true
	}
	for x := mn + 1; x < mx; x++ {
		if !s[x] {
			ans = append(ans, x)
		}
	}
	return
}
```

#### TypeScript

```ts
function findMissingElements(nums: number[]): number[] {
    let [mn, mx] = [100, 0];
    const s = new Set<number>();
    for (const x of nums) {
        mn = Math.min(mn, x);
        mx = Math.max(mx, x);
        s.add(x);
    }
    const ans: number[] = [];
    for (let x = mn + 1; x < mx; ++x) {
        if (!s.has(x)) {
            ans.push(x);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
