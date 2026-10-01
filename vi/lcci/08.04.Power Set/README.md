---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [08.04. Power Set](https://leetcode.cn/problems/power-set-lcci)

[中文文档](/lcci/08.04.Power%20Set/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một phương thức trả về tất cả tập con của một tập hợp. Các phần tử trong một tập hợp đôi một khác nhau.</p>

<p>Lưu ý: Tập kết quả không được chứa các tập con trùng lặp.</p>

<p><strong>Ví dụ:</strong></p>

<pre>

<strong> Đầu vào</strong>:  nums = [1,2,3]

<strong> Đầu ra</strong>:

[

  [3],

&nbsp; [1],

&nbsp; [2],

&nbsp; [1,2,3],

&nbsp; [1,3],

&nbsp; [2,3],

&nbsp; [1,2],

&nbsp; []

]

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Tập lũy thừa có kích thước $2^n$ và phải được liệt kê. Việc độc lập chọn lấy hoặc bỏ qua từng phần tử sẽ bao phủ mọi tập con.
>
> $dfs(u,t)$ trước tiên bỏ qua $nums[u]$, sau đó lấy nó và loại phần tử này khi quay lui. Một bản sao của $t$ được lưu tại $u=n$.
>
> Thứ tự bỏ qua rồi lấy phù hợp với việc xây dựng từ tập rỗng; thao tác loại phần tử giữ cho buffer dùng chung luôn sạch giữa các nhánh.

<!-- thinking:end -->

Ta xây dựng một hàm đệ quy $dfs(u, t)$, trong đó $u$ là chỉ số của phần tử hiện đang được liệt kê, còn $t$ là tập con hiện tại.

Với phần tử hiện tại có chỉ số $u$, ta có thể chọn thêm nó vào tập con $t$, hoặc chọn không thêm nó vào tập con $t$. Thực hiện đệ quy hai lựa chọn này sẽ thu được tất cả tập con.

Độ phức tạp thời gian là $O(n \times 2^n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng. Mỗi phần tử trong mảng có hai trạng thái, tức là được chọn hoặc không được chọn, tạo thành tổng cộng $2^n$ trạng thái. Mỗi trạng thái cần $O(n)$ thời gian để tạo tập con.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        def dfs(u, t):
            if u == len(nums):
                ans.append(t[:])
                return
            dfs(u + 1, t)
            t.append(nums[u])
            dfs(u + 1, t)
            t.pop()

        ans = []
        dfs(0, [])
        return ans
```

#### Java

```java
class Solution {
    private List<List<Integer>> ans = new ArrayList<>();
    private int[] nums;

    public List<List<Integer>> subsets(int[] nums) {
        this.nums = nums;
        dfs(0, new ArrayList<>());
        return ans;
    }

    private void dfs(int u, List<Integer> t) {
        if (u == nums.length) {
            ans.add(new ArrayList<>(t));
            return;
        }
        dfs(u + 1, t);
        t.add(nums[u]);
        dfs(u + 1, t);
        t.remove(t.size() - 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> t;
        dfs(0, nums, t, ans);
        return ans;
    }

    void dfs(int u, vector<int>& nums, vector<int>& t, vector<vector<int>>& ans) {
        if (u == nums.size()) {
            ans.push_back(t);
            return;
        }
        dfs(u + 1, nums, t, ans);
        t.push_back(nums[u]);
        dfs(u + 1, nums, t, ans);
        t.pop_back();
    }
};
```

#### Go

```go
func subsets(nums []int) [][]int {
	var ans [][]int
	var dfs func(u int, t []int)
	dfs = func(u int, t []int) {
		if u == len(nums) {
			ans = append(ans, append([]int(nil), t...))
			return
		}
		dfs(u+1, t)
		t = append(t, nums[u])
		dfs(u+1, t)
		t = t[:len(t)-1]
	}
	var t []int
	dfs(0, t)
	return ans
}
```

#### TypeScript

```ts
function subsets(nums: number[]): number[][] {
    const res = [[]];
    nums.forEach(num => {
        res.forEach(item => {
            res.push(item.concat(num));
        });
    });
    return res;
}
```

#### Rust

```rust
impl Solution {
    pub fn subsets(nums: Vec<i32>) -> Vec<Vec<i32>> {
        let n = nums.len();
        let mut res: Vec<Vec<i32>> = vec![vec![]];
        for i in 0..n {
            let m = res.len();
            for j in 0..m {
                let mut subset = res[j].clone();
                subset.push(nums[i]);
                res.push(subset);
            }
        }
        res
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var subsets = function (nums) {
    let prev = [];
    let res = [];
    dfs(nums, 0, prev, res);
    return res;
};

function dfs(nums, depth, prev, res) {
    res.push(prev.slice());
    for (let i = depth; i < nums.length; i++) {
        prev.push(nums[i]);
        depth++;
        dfs(nums, depth, prev, res);
        prev.pop();
    }
}
```

#### Swift

```swift
class Solution {
    private var ans = [[Int]]()
    private var nums: [Int] = []

    func subsets(_ nums: [Int]) -> [[Int]] {
        self.nums = nums
        dfs(0, [])
        return ans.sorted { $0.count < $1.count }
    }

    private func dfs(_ u: Int, _ t: [Int]) {
        if u == nums.count {
            ans.append(t)
            return
        }
        dfs(u + 1, t)
        var tWithCurrent = t
        tWithCurrent.append(nums[u])
        dfs(u + 1, tWithCurrent)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Liệt kê nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Đệ quy là chính xác nhưng phải duy trì call stack và phần code backtracking.
>
> Mỗi tập con là một mask trong $[0,2^n)$; bit thứ $i$ cho biết $nums[i]$ có được đưa vào hay không. Có thể xây dựng cùng một tập các tập con bằng cách lặp.

<!-- thinking:end -->

Ta có thể chuyển quá trình đệ quy trong Lời giải 1 thành dạng lặp, tức là dùng phép liệt kê nhị phân để liệt kê tất cả tập con.

Ta có thể dùng $2^n$ số nhị phân để biểu diễn tất cả tập con của $n$ phần tử. Nếu bit thứ $i$ của một số nhị phân `mask` là $1$, điều đó có nghĩa tập con chứa phần tử thứ $i$ $v$ của mảng; nếu là $0$, điều đó có nghĩa tập con không chứa phần tử thứ $i$ $v$ của mảng.

Độ phức tạp thời gian là $O(n \times 2^n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng. Có tổng cộng $2^n$ tập con, và mỗi tập con cần $O(n)$ thời gian để tạo.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        ans = []
        for mask in range(1 << len(nums)):
            t = []
            for i, v in enumerate(nums):
                if (mask >> i) & 1:
                    t.append(v)
            ans.append(t)
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        int n = nums.length;
        List<List<Integer>> ans = new ArrayList<>();
        for (int mask = 0; mask < 1 << n; ++mask) {
            List<Integer> t = new ArrayList<>();
            for (int i = 0; i < n; ++i) {
                if (((mask >> i) & 1) == 1) {
                    t.add(nums[i]);
                }
            }
            ans.add(t);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> ans;
        vector<int> t;
        int n = nums.size();
        for (int mask = 0; mask < 1 << n; ++mask) {
            t.clear();
            for (int i = 0; i < n; ++i) {
                if ((mask >> i) & 1) {
                    t.push_back(nums[i]);
                }
            }
            ans.push_back(t);
        }
        return ans;
    }
};
```

#### Go

```go
func subsets(nums []int) [][]int {
	var ans [][]int
	n := len(nums)
	for mask := 0; mask < 1<<n; mask++ {
		t := []int{}
		for i, v := range nums {
			if ((mask >> i) & 1) == 1 {
				t = append(t, v)
			}
		}
		ans = append(ans, t)
	}
	return ans
}
```

#### TypeScript

```ts
function subsets(nums: number[]): number[][] {
    const n = nums.length;
    const res = [];
    const list = [];
    const dfs = (i: number) => {
        if (i === n) {
            res.push([...list]);
            return;
        }
        list.push(nums[i]);
        dfs(i + 1);
        list.pop();
        dfs(i + 1);
    };
    dfs(0);
    return res;
}
```

#### Rust

```rust
impl Solution {
    fn dfs(nums: &Vec<i32>, i: usize, res: &mut Vec<Vec<i32>>, list: &mut Vec<i32>) {
        if i == nums.len() {
            res.push(list.clone());
            return;
        }
        list.push(nums[i]);
        Self::dfs(nums, i + 1, res, list);
        list.pop();
        Self::dfs(nums, i + 1, res, list);
    }

    pub fn subsets(nums: Vec<i32>) -> Vec<Vec<i32>> {
        let mut res = vec![];
        Self::dfs(&nums, 0, &mut res, &mut vec![]);
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
