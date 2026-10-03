---
comments: true
difficulty: Medium
rating: 1567
source: Weekly Contest 263 Q3
tags:
    - Bit Manipulation
    - Array
    - Backtracking
    - Enumeration
---

<!-- problem:start -->

# [2044. Count Number of Maximum Bitwise-OR Subsets](https://leetcode.com/problems/count-number-of-maximum-bitwise-or-subsets)

[中文文档](/solution/2000-2099/2044.Count%20Number%20of%20Maximum%20Bitwise-OR%20Subsets/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, hãy tìm <strong>phép OR bitwise</strong> <strong>lớn nhất</strong> có thể của một tập con của <code>nums</code> và trả về <em><strong>số lượng các tập con khác rỗng khác nhau</strong> có phép OR bitwise lớn nhất</em>.</p>

<p>Mảng <code>a</code> là <strong>tập con</strong> của mảng <code>b</code> nếu có thể thu được <code>a</code> từ <code>b</code> bằng cách xóa một số phần tử (có thể không xóa phần tử nào) của <code>b</code>. Hai tập con được xem là <strong>khác nhau</strong> nếu các chỉ số của những phần tử được chọn khác nhau.</p>

<p>Phép OR bitwise của một mảng <code>a</code> bằng <code>a[0] <strong>OR</strong> a[1] <strong>OR</strong> ... <strong>OR</strong> a[a.length - 1]</code> (<strong>đánh chỉ số từ 0</strong>).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Phép OR bitwise lớn nhất có thể của một tập con là 3. Có 2 tập con có phép OR bitwise bằng 3:
- [3]
- [3,1]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,2]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Mọi tập con khác rỗng của [2,2,2] đều có phép OR bitwise bằng 2. Có tổng cộng 2<sup>3</sup> - 1 = 7 tập con.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,1,5]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Phép OR bitwise lớn nhất có thể của một tập con là 7. Có 6 tập con có phép OR bitwise bằng 7:
- [3,5]
- [3,1,5]
- [3,2,5]
- [3,2,1,5]
- [2,5]
- [2,1,5]</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 16</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Với $n \le 16$, có $2^{16}$ tập con. OR của tất cả phần tử là $mx$ lớn nhất; ta đếm các tập con có OR bằng $mx$.
>
> DFS chọn hoặc bỏ qua từng chỉ số và so sánh OR hiện tại ở các nút lá. Độ sâu là $n$, không cần tạo danh sách mask riêng.

<!-- thinking:end -->

Giá trị OR bitwise lớn nhất $\textit{mx}$ trong mảng $\textit{nums}$ có thể được tính bằng cách thực hiện phép OR bitwise trên tất cả phần tử trong mảng.

Sau đó, ta có thể dùng tìm kiếm theo chiều sâu để liệt kê tất cả tập con và đếm số tập con có OR bitwise bằng $\textit{mx}$. Ta xây dựng hàm $\text{dfs(i, t)}$, biểu diễn số tập con bắt đầu từ chỉ số $\textit{i}$ khi giá trị OR bitwise hiện tại là $\textit{t}$. Ban đầu, $\textit{i} = 0$ và $\textit{t} = 0$.

Trong hàm $\text{dfs(i, t)}$, nếu $\textit{i}$ bằng độ dài mảng thì nghĩa là ta đã liệt kê xong tất cả phần tử. Khi đó, nếu $\textit{t}$ bằng $\textit{mx}$, ta tăng đáp án lên một. Ngược lại, ta có thể chọn loại bỏ phần tử hiện tại $\textit{nums[i]}$ hoặc chọn phần tử hiện tại $\textit{nums[i]}$, do đó lần lượt gọi đệ quy $\text{dfs(i + 1, t)}$ và $\text{dfs(i + 1, t | nums[i])}$.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(2^n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countMaxOrSubsets(self, nums: List[int]) -> int:
        def dfs(i, t):
            nonlocal ans, mx
            if i == len(nums):
                if t == mx:
                    ans += 1
                return
            dfs(i + 1, t)
            dfs(i + 1, t | nums[i])

        ans = 0
        mx = reduce(lambda x, y: x | y, nums)
        dfs(0, 0)
        return ans
```

#### Java

```java
class Solution {
    private int mx;
    private int ans;
    private int[] nums;

    public int countMaxOrSubsets(int[] nums) {
        mx = 0;
        for (int x : nums) {
            mx |= x;
        }
        this.nums = nums;
        dfs(0, 0);
        return ans;
    }

    private void dfs(int i, int t) {
        if (i == nums.length) {
            if (t == mx) {
                ++ans;
            }
            return;
        }
        dfs(i + 1, t);
        dfs(i + 1, t | nums[i]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countMaxOrSubsets(vector<int>& nums) {
        int ans = 0;
        int mx = accumulate(nums.begin(), nums.end(), 0, bit_or<int>());
        auto dfs = [&](this auto&& dfs, int i, int t) {
            if (i == nums.size()) {
                if (t == mx) {
                    ans++;
                }
                return;
            }
            dfs(i + 1, t);
            dfs(i + 1, t | nums[i]);
        };
        dfs(0, 0);
        return ans;
    }
};
```

#### Go

```go
func countMaxOrSubsets(nums []int) (ans int) {
	mx := 0
	for _, x := range nums {
		mx |= x
	}

	var dfs func(i, t int)
	dfs = func(i, t int) {
		if i == len(nums) {
			if t == mx {
				ans++
			}
			return
		}
		dfs(i+1, t)
		dfs(i+1, t|nums[i])
	}

	dfs(0, 0)
	return
}
```

#### TypeScript

```ts
function countMaxOrSubsets(nums: number[]): number {
    let ans = 0;
    const mx = nums.reduce((x, y) => x | y, 0);

    const dfs = (i: number, t: number) => {
        if (i === nums.length) {
            if (t === mx) {
                ans++;
            }
            return;
        }
        dfs(i + 1, t);
        dfs(i + 1, t | nums[i]);
    };

    dfs(0, 0);
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_max_or_subsets(nums: Vec<i32>) -> i32 {
        let mut ans = 0;
        let mx = nums.iter().fold(0, |x, &y| x | y);

        fn dfs(i: usize, t: i32, nums: &Vec<i32>, mx: i32, ans: &mut i32) {
            if i == nums.len() {
                if t == mx {
                    *ans += 1;
                }
                return;
            }
            dfs(i + 1, t, nums, mx, ans);
            dfs(i + 1, t | nums[i], nums, mx, ans);
        }

        dfs(0, 0, &nums, mx, &mut ans);
        ans
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
> Lời giải 1 dùng đệ quy. Liệt kê nhị phân thực hiện OR các bit của từng mask, đồng thời cập nhật giá trị lớn nhất hiện tại và số lượng tương ứng, nên không cần biết trước $mx$.
>
> Vòng lặp bên trong tốn thêm $n$; bộ nhớ bổ sung giảm xuống còn $O(1)$.

<!-- thinking:end -->

Ta có thể dùng phương pháp liệt kê nhị phân để đếm các kết quả OR bitwise của tất cả tập con. Với mảng $\textit{nums}$ có độ dài $n$, ta dùng một số nguyên $\textit{mask}$ để biểu diễn một tập con, trong đó bit thứ $i$ của $\textit{mask}$ bằng 1 nghĩa là chọn phần tử $\textit{nums[i]}$, còn bằng 0 nghĩa là không chọn phần tử đó.

Ta có thể duyệt qua mọi giá trị $\textit{mask}$ từ $0$ đến $2^n - 1$. Với mỗi $\textit{mask}$, ta tính kết quả OR bitwise của tập con tương ứng, sau đó cập nhật giá trị lớn nhất $\textit{mx}$ và đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(2^n \cdot n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countMaxOrSubsets(self, nums: List[int]) -> int:
        n = len(nums)
        ans = 0
        mx = 0
        for mask in range(1 << n):
            t = 0
            for i, v in enumerate(nums):
                if (mask >> i) & 1:
                    t |= v
            if mx < t:
                mx = t
                ans = 1
            elif mx == t:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countMaxOrSubsets(int[] nums) {
        int n = nums.length;
        int ans = 0;
        int mx = 0;
        for (int mask = 1; mask < 1 << n; ++mask) {
            int t = 0;
            for (int i = 0; i < n; ++i) {
                if (((mask >> i) & 1) == 1) {
                    t |= nums[i];
                }
            }
            if (mx < t) {
                mx = t;
                ans = 1;
            } else if (mx == t) {
                ++ans;
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
    int countMaxOrSubsets(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        int mx = 0;
        for (int mask = 1; mask < 1 << n; ++mask) {
            int t = 0;
            for (int i = 0; i < n; ++i) {
                if ((mask >> i) & 1) {
                    t |= nums[i];
                }
            }
            if (mx < t) {
                mx = t;
                ans = 1;
            } else if (mx == t)
                ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func countMaxOrSubsets(nums []int) (ans int) {
	n := len(nums)
	mx := 0

	for mask := 0; mask < (1 << n); mask++ {
		t := 0
		for i, v := range nums {
			if (mask>>i)&1 == 1 {
				t |= v
			}
		}
		if mx < t {
			mx = t
			ans = 1
		} else if mx == t {
			ans++
		}
	}

	return
}
```

#### TypeScript

```ts
function countMaxOrSubsets(nums: number[]): number {
    const n = nums.length;
    let ans = 0;
    let mx = 0;

    for (let mask = 0; mask < 1 << n; mask++) {
        let t = 0;
        for (let i = 0; i < n; i++) {
            if ((mask >> i) & 1) {
                t |= nums[i];
            }
        }
        if (mx < t) {
            mx = t;
            ans = 1;
        } else if (mx === t) {
            ans++;
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_max_or_subsets(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut ans = 0;
        let mut mx = 0;

        for mask in 0..(1 << n) {
            let mut t = 0;
            for i in 0..n {
                if (mask >> i) & 1 == 1 {
                    t |= nums[i];
                }
            }
            if mx < t {
                mx = t;
                ans = 1;
            } else if mx == t {
                ans += 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
