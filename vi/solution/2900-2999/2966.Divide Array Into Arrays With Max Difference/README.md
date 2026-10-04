---
comments: true
difficulty: Medium
rating: 1395
source: Weekly Contest 376 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2966. Divide Array Into Arrays With Max Difference](https://leetcode.com/problems/divide-array-into-arrays-with-max-difference)

[中文文档](/solution/2900-2999/2966.Divide%20Array%20Into%20Arrays%20With%20Max%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có kích thước <code>n</code>, trong đó <code>n</code> là bội số của 3, và một số nguyên dương <code>k</code>.</p>

<p>Chia mảng <code>nums</code> thành <code>n / 3</code> mảng có kích thước <strong>3</strong>, thỏa mãn điều kiện sau:</p>

<ul>
	<li>Hiệu giữa <strong>bất kỳ</strong> hai phần tử nào trong cùng một mảng <strong>nhỏ hơn hoặc bằng</strong> <code>k</code>.</li>
</ul>

<p>Trả về một mảng <strong>2 chiều</strong> chứa các mảng đó. Nếu không thể thỏa mãn các điều kiện, trả về một mảng rỗng. Nếu có nhiều đáp án, trả về <strong>bất kỳ</strong> đáp án nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,4,8,7,9,3,5,1], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1,1,3],[3,4,5],[7,8,9]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hiệu giữa bất kỳ hai phần tử nào trong mỗi mảng đều nhỏ hơn hoặc bằng 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4,2,2,5,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cách khác nhau để chia <code>nums</code> thành 2 mảng có kích thước 3 là:</p>

<ul>
	<li>[[2,2,2],[2,4,5]] (và các hoán vị của nó)</li>
	<li>[[2,2,4],[2,2,5]] (và các hoán vị của nó)</li>
</ul>

<p>Vì có bốn số 2 nên bất kể chia thế nào, sẽ luôn có một mảng chứa các phần tử 2 và 5. Vì <code>5 - 2 = 3 &gt; k</code>, điều kiện không được thỏa mãn, do đó không có cách chia hợp lệ.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,2,9,8,2,12,7,12,10,5,8,5,5,7,9,2,5,11], k = 14</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[2,2,2],[4,5,5],[5,5,7],[7,8,8],[9,9,10],[11,12,12]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hiệu giữa bất kỳ hai phần tử nào trong mỗi mảng đều nhỏ hơn hoặc bằng 14.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>n </code>là bội số của 3</li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Chia thành các bộ ba sao cho hiệu giữa phần tử lớn nhất và nhỏ nhất không vượt quá $k$. Sau khi sắp xếp, các bộ ba liền kề có độ rộng nhỏ nhất; lấy các phần tử ở xa hơn chỉ làm độ rộng tăng.
>
> Với mỗi ba phần tử, kiểm tra $t[2]-t[0]$ và trả về kết quả thất bại nếu giá trị này vượt quá $k$. Vì $n$ là bội số của $3$ và có thể lên tới $10^5$, ta sắp xếp rồi duyệt để nhóm các phần tử.

<!-- thinking:end -->

Đầu tiên, ta sắp xếp mảng. Sau đó, mỗi lần ta lấy ra ba phần tử. Nếu hiệu giữa giá trị lớn nhất và nhỏ nhất trong ba phần tử này lớn hơn $k$, điều kiện không thể được thỏa mãn, nên ta trả về một mảng rỗng. Ngược lại, ta thêm mảng gồm ba phần tử này vào mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def divideArray(self, nums: List[int], k: int) -> List[List[int]]:
        nums.sort()
        ans = []
        n = len(nums)
        for i in range(0, n, 3):
            t = nums[i : i + 3]
            if t[2] - t[0] > k:
                return []
            ans.append(t)
        return ans
```

#### Java

```java
class Solution {
    public int[][] divideArray(int[] nums, int k) {
        Arrays.sort(nums);
        int n = nums.length;
        int[][] ans = new int[n / 3][];
        for (int i = 0; i < n; i += 3) {
            int[] t = Arrays.copyOfRange(nums, i, i + 3);
            if (t[2] - t[0] > k) {
                return new int[][] {};
            }
            ans[i / 3] = t;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> divideArray(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        int n = nums.size();
        for (int i = 0; i < n; i += 3) {
            vector<int> t = {nums[i], nums[i + 1], nums[i + 2]};
            if (t[2] - t[0] > k) {
                return {};
            }
            ans.emplace_back(t);
        }
        return ans;
    }
};
```

#### Go

```go
func divideArray(nums []int, k int) [][]int {
	sort.Ints(nums)
	ans := [][]int{}
	for i := 0; i < len(nums); i += 3 {
		t := slices.Clone(nums[i : i+3])
		if t[2]-t[0] > k {
			return [][]int{}
		}
		ans = append(ans, t)
	}
	return ans
}
```

#### TypeScript

```ts
function divideArray(nums: number[], k: number): number[][] {
    nums.sort((a, b) => a - b);
    const ans: number[][] = [];
    for (let i = 0; i < nums.length; i += 3) {
        const t = nums.slice(i, i + 3);
        if (t[2] - t[0] > k) {
            return [];
        }
        ans.push(t);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn divide_array(mut nums: Vec<i32>, k: i32) -> Vec<Vec<i32>> {
        nums.sort();
        let mut ans = Vec::new();
        let n = nums.len();

        for i in (0..n).step_by(3) {
            if i + 2 >= n {
                return vec![];
            }

            let t = &nums[i..i+3];
            if t[2] - t[0] > k {
                return vec![];
            }

            ans.push(t.to_vec());
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int[][] DivideArray(int[] nums, int k) {
        Array.Sort(nums);
        List<int[]> ans = new List<int[]>();

        for (int i = 0; i < nums.Length; i += 3) {
            if (i + 2 >= nums.Length) {
                return new int[0][];
            }

            int[] t = new int[] { nums[i], nums[i + 1], nums[i + 2] };
            if (t[2] - t[0] > k) {
                return new int[0][];
            }

            ans.Add(t);
        }

        return ans.ToArray();
    }
}
```

#### Swift

```swift
class Solution {
    func divideArray(_ nums: [Int], _ k: Int) -> [[Int]] {
        var sortedNums = nums.sorted()
        var ans: [[Int]] = []

        for i in stride(from: 0, to: sortedNums.count, by: 3) {
            if i + 2 >= sortedNums.count {
                return []
            }

            let t = Array(sortedNums[i..<i+3])
            if t[2] - t[0] > k {
                return []
            }

            ans.append(t)
        }

        return ans
    }
}
```

#### Dart

```dart
class Solution {
  List<List<int>> divideArray(List<int> nums, int k) {
    nums.sort();
    List<List<int>> ans = [];

    for (int i = 0; i < nums.length; i += 3) {
      if (i + 2 >= nums.length) {
        return [];
      }

      List<int> t = nums.sublist(i, i + 3);
      if (t[2] - t[0] > k) {
        return [];
      }

      ans.add(t);
    }

    return ans;
  }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
