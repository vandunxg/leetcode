---
comments: true
difficulty: Medium
rating: 1496
source: Biweekly Contest 73 Q2
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [2191. Sort the Jumbled Numbers](https://leetcode.com/problems/sort-the-jumbled-numbers)

[中文文档](/solution/2100-2199/2191.Sort%20the%20Jumbled%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>mapping</code> đánh chỉ số từ <strong>0</strong>, biểu diễn quy tắc ánh xạ của một hệ thập phân bị xáo trộn. <code>mapping[i] = j</code> nghĩa là chữ số <code>i</code> được ánh xạ thành chữ số <code>j</code> trong hệ này.</p>

<p><strong>Giá trị ánh xạ</strong> của một số nguyên là số nguyên mới thu được bằng cách thay mỗi lần xuất hiện của chữ số <code>i</code> trong số nguyên đó bằng <code>mapping[i]</code> với mọi <code>0 &lt;= i &lt;= 9</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên khác là <code>nums</code>. Hãy trả về <em>mảng </em><code>nums</code><em> được sắp xếp theo thứ tự <strong>không giảm</strong> dựa trên <strong>giá trị ánh xạ</strong> của các phần tử.</em></p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Các phần tử có cùng giá trị ánh xạ phải xuất hiện theo <strong>cùng thứ tự tương đối</strong> như trong dữ liệu đầu vào.</li>
	<li>Các phần tử của <code>nums</code> chỉ được sắp xếp dựa trên giá trị ánh xạ của chúng và <strong>không được thay thế</strong> bằng các giá trị đó.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> mapping = [8,9,4,0,2,1,3,5,7,6], nums = [991,338,38]
<strong>Đầu ra:</strong> [338,38,991]
<strong>Giải thích:</strong>
Ánh xạ số 991 như sau:
1. mapping[9] = 6, nên mọi lần xuất hiện của chữ số 9 sẽ trở thành 6.
2. mapping[1] = 9, nên mọi lần xuất hiện của chữ số 1 sẽ trở thành 9.
Do đó, giá trị ánh xạ của 991 là 669.
338 được ánh xạ thành 007, hay 7 sau khi bỏ các số 0 ở đầu.
38 được ánh xạ thành 07, cũng là 7 sau khi bỏ các số 0 ở đầu.
Vì 338 và 38 có cùng giá trị ánh xạ, chúng phải giữ nguyên thứ tự tương đối, nên 338 đứng trước 38.
Vậy mảng sau khi sắp xếp là [338,38,991].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> mapping = [0,1,2,3,4,5,6,7,8,9], nums = [789,456,123]
<strong>Đầu ra:</strong> [123,456,789]
<strong>Giải thích:</strong> 789 được ánh xạ thành 789, 456 được ánh xạ thành 456 và 123 được ánh xạ thành 123. Vì vậy, mảng sau khi sắp xếp là [123,456,789].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>mapping.length == 10</code></li>
	<li><code>0 &lt;= mapping[i] &lt;= 9</code></li>
	<li>Tất cả giá trị của <code>mapping[i]</code> đều <strong>khác nhau</strong>.</li>
	<li><code>1 &lt;= nums.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt; 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp tùy chỉnh

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp theo giá trị thập phân sau ánh xạ, đồng thời giữ nguyên thứ tự ban đầu khi các giá trị bằng nhau. Ta cần một phép so sánh ổn định trên khóa ánh xạ đó.
>
> Ánh xạ từng chữ số của mỗi số thành $y$, sắp xếp các cặp $(y,i)$, rồi lấy các phần tử $\textit{nums}[i]$. Số 0 được ánh xạ riêng để vòng lặp không bị bỏ qua.
>
> Trọng số vị trí $k$ dựng lại $y$ từ chữ số ít quan trọng nhất.

<!-- thinking:end -->

Ta duyệt từng phần tử $nums[i]$ trong mảng $nums$, lưu giá trị ánh xạ $y$ và chỉ số $i$ vào mảng $arr$, sau đó sắp xếp mảng $arr$. Cuối cùng, ta lấy chỉ số $i$ từ mảng $arr$ đã sắp xếp và chuyển nó thành phần tử $nums[i]$ tương ứng trong mảng ban đầu $nums$.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sortJumbled(self, mapping: List[int], nums: List[int]) -> List[int]:
        def f(x: int) -> int:
            if x == 0:
                return mapping[0]
            y, k = 0, 1
            while x:
                x, v = divmod(x, 10)
                v = mapping[v]
                y = k * v + y
                k *= 10
            return y

        arr = sorted((f(x), i) for i, x in enumerate(nums))
        return [nums[i] for _, i in arr]
```

#### Java

```java
class Solution {
    private int[] mapping;

    public int[] sortJumbled(int[] mapping, int[] nums) {
        this.mapping = mapping;
        int n = nums.length;
        int[][] arr = new int[n][0];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[] {f(nums[i]), i};
        }
        Arrays.sort(arr, (a, b) -> a[0] == b[0] ? a[1] - b[1] : a[0] - b[0]);
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = nums[arr[i][1]];
        }
        return ans;
    }

    private int f(int x) {
        if (x == 0) {
            return mapping[0];
        }
        int y = 0;
        for (int k = 1; x > 0; x /= 10) {
            int v = mapping[x % 10];
            y = k * v + y;
            k *= 10;
        }
        return y;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sortJumbled(vector<int>& mapping, vector<int>& nums) {
        auto f = [&](int x) {
            if (x == 0) {
                return mapping[0];
            }
            int y = 0;
            for (int k = 1; x; x /= 10) {
                int v = mapping[x % 10];
                y = k * v + y;
                k *= 10;
            }
            return y;
        };
        const int n = nums.size();
        vector<pair<int, int>> arr;
        for (int i = 0; i < n; ++i) {
            arr.emplace_back(f(nums[i]), i);
        }
        sort(arr.begin(), arr.end());
        vector<int> ans;
        for (const auto& [_, i] : arr) {
            ans.push_back(nums[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func sortJumbled(mapping []int, nums []int) (ans []int) {
	n := len(nums)
	f := func(x int) (y int) {
		if x == 0 {
			return mapping[0]
		}
		for k := 1; x > 0; x /= 10 {
			v := mapping[x%10]
			y = k*v + y
			k *= 10
		}
		return
	}
	arr := make([][2]int, n)
	for i, x := range nums {
		arr[i] = [2]int{f(x), i}
	}
	sort.Slice(arr, func(i, j int) bool { return arr[i][0] < arr[j][0] || arr[i][0] == arr[j][0] && arr[i][1] < arr[j][1] })
	for _, p := range arr {
		ans = append(ans, nums[p[1]])
	}
	return
}
```

#### TypeScript

```ts
function sortJumbled(mapping: number[], nums: number[]): number[] {
    const n = nums.length;
    const f = (x: number): number => {
        if (x === 0) {
            return mapping[0];
        }
        let y = 0;
        for (let k = 1; x; x = (x / 10) | 0) {
            const v = mapping[x % 10];
            y += v * k;
            k *= 10;
        }
        return y;
    };
    const arr: number[][] = nums.map((x, i) => [f(x), i]);
    arr.sort((a, b) => (a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]));
    return arr.map(x => nums[x[1]]);
}
```

#### JavaScript

```js
/**
 * @param {number[]} mapping
 * @param {number[]} nums
 * @return {number[]}
 */
var sortJumbled = function (mapping, nums) {
    const n = nums.length;
    const f = x => {
        if (x === 0) {
            return mapping[0];
        }
        let y = 0;
        for (let k = 1; x; x = (x / 10) | 0) {
            const v = mapping[x % 10];
            y += v * k;
            k *= 10;
        }
        return y;
    };
    const arr = nums.map((x, i) => [f(x), i]);
    arr.sort((a, b) => (a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]));
    return arr.map(x => nums[x[1]]);
};
```

#### Rust

```rust
impl Solution {
    pub fn sort_jumbled(mapping: Vec<i32>, nums: Vec<i32>) -> Vec<i32> {
        let f = |x: i32| -> i32 {
            if x == 0 {
                return mapping[0];
            }
            let mut y = 0;
            let mut k = 1;
            let mut num = x;
            while num != 0 {
                let v = mapping[(num % 10) as usize];
                y = k * v + y;
                k *= 10;
                num /= 10;
            }
            y
        };

        let n = nums.len();
        let mut arr: Vec<(i32, usize)> = Vec::with_capacity(n);
        for i in 0..n {
            arr.push((f(nums[i]), i));
        }
        arr.sort();

        let mut ans: Vec<i32> = Vec::with_capacity(n);
        for (_, i) in arr {
            ans.push(nums[i]);
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int[] SortJumbled(int[] mapping, int[] nums) {
        Func<int, int> f = (int x) => {
            if (x == 0) {
                return mapping[0];
            }
            int y = 0;
            int k = 1;
            int num = x;
            while (num != 0) {
                int v = mapping[num % 10];
                y = k * v + y;
                k *= 10;
                num /= 10;
            }
            return y;
        };

        int n = nums.Length;
        List<(int, int)> arr = new List<(int, int)>();
        for (int i = 0; i < n; ++i) {
            arr.Add((f(nums[i]), i));
        }
        arr.Sort();

        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = nums[arr[i].Item2];
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
