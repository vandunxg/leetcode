---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [645. Set Mismatch](https://leetcode.com/problems/set-mismatch)

[中文文档](/solution/0600-0699/0645.Set%20Mismatch/README.md)

## Mô tả

<!-- description:start -->

<p>Một tập hợp số nguyên <code>s</code> ban đầu chứa tất cả các số từ <code>1</code> đến <code>n</code>. Không may, do lỗi dữ liệu, một số trong <code>s</code> bị thay bằng một số khác trong tập hợp, khiến <strong>một số xuất hiện hai lần</strong> và <strong>một số khác bị thiếu</strong>.</p>

<p>Cho mảng số nguyên <code>nums</code> biểu thị trạng thái của tập hợp sau khi xảy ra lỗi.</p>

<p>Hãy tìm số xuất hiện hai lần và số bị thiếu, rồi trả về <em>chúng dưới dạng một mảng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Input:</strong> nums = [1,2,2,4]
<strong>Output:</strong> [2,3]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Input:</strong> nums = [1,1]
<strong>Output:</strong> [1,2]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
 > Trong khoảng $[1,n]$, có một số bị lặp và một số bị thiếu. Có thể tìm cả hai cùng lúc bằng cách xét tổng.
>
> Gọi $s$ là tổng mảng, $s_2$ là tổng các giá trị phân biệt và $s_1$ là tổng các số từ $1$ đến $n$. Số bị lặp là $s-s_2$, còn số bị thiếu là $s_1-s_2$.

<!-- thinking:end -->

Gọi $s_1$ là tổng các số từ $1$ đến $n$, $s_2$ là tổng các giá trị phân biệt trong mảng $nums$, và $s$ là tổng các phần tử của mảng $nums$.

Khi đó, $s - s_2$ là số bị lặp, còn $s_1 - s_2$ là số bị thiếu.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài của mảng $nums$. Cần thêm bộ nhớ để lưu các giá trị sau khi loại bỏ phần tử trùng lặp.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findErrorNums(self, nums: List[int]) -> List[int]:
        n = len(nums)
        s1 = (1 + n) * n // 2
        s2 = sum(set(nums))
        s = sum(nums)
        return [s - s2, s1 - s2]
```

#### Java

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int n = nums.length;
        int s1 = (1 + n) * n / 2;
        int s2 = 0;
        Set<Integer> set = new HashSet<>();
        int s = 0;
        for (int x : nums) {
            if (set.add(x)) {
                s2 += x;
            }
            s += x;
        }
        return new int[] {s - s2, s1 - s2};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        int n = nums.size();
        int s1 = (1 + n) * n / 2;
        int s2 = 0;
        unordered_set<int> set(nums.begin(), nums.end());
        for (int x : set) {
            s2 += x;
        }
        int s = accumulate(nums.begin(), nums.end(), 0);
        return {s - s2, s1 - s2};
    }
};
```

#### Go

```go
func findErrorNums(nums []int) []int {
	n := len(nums)
	s1 := (1 + n) * n / 2
	s2, s := 0, 0
	set := map[int]bool{}
	for _, x := range nums {
		if !set[x] {
			set[x] = true
			s2 += x
		}
		s += x
	}
	return []int{s - s2, s1 - s2}
}
```

#### TypeScript

```ts
function findErrorNums(nums: number[]): number[] {
    const n = nums.length;
    const s1 = (n * (n + 1)) >> 1;
    const s2 = [...new Set(nums)].reduce((a, b) => a + b);
    const s = nums.reduce((a, b) => a + b);
    return [s - s2, s1 - s2];
}
```

#### Rust

```rust
use std::collections::HashSet;
impl Solution {
    pub fn find_error_nums(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len() as i32;
        let s1 = ((1 + n) * n) / 2;
        let s2 = nums
            .iter()
            .cloned()
            .collect::<HashSet<i32>>()
            .iter()
            .sum::<i32>();
        let s: i32 = nums.iter().sum();
        vec![s - s2, s1 - s2]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Cách tính tổng cần một set. Frequency map cho biết trực tiếp số bị lặp có tần suất $2$ và số bị thiếu có tần suất $0$.

<!-- thinking:end -->

Ta cũng có thể dùng cách trực quan hơn: sử dụng hash table $cnt$ để đếm số lần xuất hiện của từng số trong mảng $nums$.

Tiếp theo, duyệt $x \in [1, n]$: nếu $cnt[x] = 2$ thì $x$ là số bị lặp; nếu $cnt[x] = 0$ thì $x$ là số bị thiếu.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findErrorNums(self, nums: List[int]) -> List[int]:
        cnt = Counter(nums)
        n = len(nums)
        ans = [0] * 2
        for x in range(1, n + 1):
            if cnt[x] == 2:
                ans[0] = x
            if cnt[x] == 0:
                ans[1] = x
        return ans
```

#### Java

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int n = nums.length;
        Map<Integer, Integer> cnt = new HashMap<>(n);
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum);
        }
        int[] ans = new int[2];
        for (int x = 1; x <= n; ++x) {
            int t = cnt.getOrDefault(x, 0);
            if (t == 2) {
                ans[0] = x;
            } else if (t == 0) {
                ans[1] = x;
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
    vector<int> findErrorNums(vector<int>& nums) {
        int n = nums.size();
        unordered_map<int, int> cnt;
        for (int x : nums) {
            ++cnt[x];
        }
        vector<int> ans(2);
        for (int x = 1; x <= n; ++x) {
            if (cnt[x] == 2) {
                ans[0] = x;
            } else if (cnt[x] == 0) {
                ans[1] = x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findErrorNums(nums []int) []int {
	n := len(nums)
	cnt := map[int]int{}
	for _, x := range nums {
		cnt[x]++
	}
	ans := make([]int, 2)
	for x := 1; x <= n; x++ {
		if cnt[x] == 2 {
			ans[0] = x
		} else if cnt[x] == 0 {
			ans[1] = x
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findErrorNums(nums: number[]): number[] {
    const n = nums.length;
    const cnt: Map<number, number> = new Map();
    for (const x of nums) {
        cnt.set(x, (cnt.get(x) || 0) + 1);
    }
    const ans: number[] = new Array(2).fill(0);
    for (let x = 1; x <= n; ++x) {
        const t = cnt.get(x) || 0;
        if (t === 2) {
            ans[0] = x;
        } else if (t === 0) {
            ans[1] = x;
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn find_error_nums(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len() as i32;
        let mut cnt: HashMap<i32, i32> = HashMap::new();

        for &x in &nums {
            *cnt.entry(x).or_insert(0) += 1;
        }

        let mut ans = vec![0; 2];

        for x in 1..=n {
            let c = *cnt.get(&x).unwrap_or(&0);
            if c == 2 {
                ans[0] = x;
            } else if c == 0 {
                ans[1] = x;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Thao tác Bit

<!-- thinking:start -->

> **Tư duy**
>
> Hai cách đầu cần bộ nhớ phụ tuyến tính. XOR các phần tử trong $nums$ với các số từ $1$ đến $n$ cho kết quả $a\oplus b$; tách theo bit 1 thấp nhất để tìm lại $a$ và $b$, rồi xác định số nào xuất hiện trong mảng.

<!-- thinking:end -->

Theo tính chất của phép XOR, với số nguyên $x$, ta có $x \oplus x = 0$ và $x \oplus 0 = x$. Vì vậy, khi XOR tất cả phần tử trong mảng $nums$ với mọi số $i \in [1, n]$, các số xuất hiện hai lần sẽ triệt tiêu nhau, chỉ còn XOR giữa số bị thiếu và số bị lặp, tức $xs = a \oplus b$.

Vì hai số này khác nhau nên kết quả XOR có ít nhất một bit bằng $1$. Ta dùng phép toán $lowbit$ để tìm bit 1 thấp nhất trong kết quả, rồi chia các số trong mảng $nums$ và các số $i \in [1, n]$ thành hai nhóm tùy bit đó có bằng $1$ hay không. Hai số cần tìm sẽ nằm ở hai nhóm khác nhau. XOR các số trong một nhóm thu được $a$, còn XOR các số trong nhóm kia thu được $b$.

Tiếp theo, ta chỉ cần xác định trong $a$ và $b$, số nào bị lặp và số nào bị thiếu. Duyệt mảng $nums$: nếu gặp $x=a$ thì $a$ là số bị lặp và trả về $[a, b]$; nếu không, sau khi duyệt hết mảng, trả về $[b, a]$.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$ vì chỉ dùng thêm lượng bộ nhớ hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findErrorNums(self, nums: List[int]) -> List[int]:
        xs = 0
        for i, x in enumerate(nums, 1):
            xs ^= i ^ x
        a = 0
        lb = xs & -xs
        for i, x in enumerate(nums, 1):
            if i & lb:
                a ^= i
            if x & lb:
                a ^= x
        b = xs ^ a
        for x in nums:
            if x == a:
                return [a, b]
        return [b, a]
```

#### Java

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int n = nums.length;
        int xs = 0;
        for (int i = 1; i <= n; ++i) {
            xs ^= i ^ nums[i - 1];
        }
        int lb = xs & -xs;
        int a = 0;
        for (int i = 1; i <= n; ++i) {
            if ((i & lb) > 0) {
                a ^= i;
            }
            if ((nums[i - 1] & lb) > 0) {
                a ^= nums[i - 1];
            }
        }
        int b = xs ^ a;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == a) {
                return new int[] {a, b};
            }
        }
        return new int[] {b, a};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findErrorNums(vector<int>& nums) {
        int n = nums.size();
        int xs = 0;
        for (int i = 1; i <= n; ++i) {
            xs ^= i ^ nums[i - 1];
        }
        int lb = xs & -xs;
        int a = 0;
        for (int i = 1; i <= n; ++i) {
            if (i & lb) {
                a ^= i;
            }
            if (nums[i - 1] & lb) {
                a ^= nums[i - 1];
            }
        }
        int b = xs ^ a;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == a) {
                return {a, b};
            }
        }
        return {b, a};
    }
};
```

#### Go

```go
func findErrorNums(nums []int) []int {
	xs := 0
	for i, x := range nums {
		xs ^= x ^ (i + 1)
	}
	lb := xs & -xs
	a := 0
	for i, x := range nums {
		if (i+1)&lb != 0 {
			a ^= (i + 1)
		}
		if x&lb != 0 {
			a ^= x
		}
	}
	b := xs ^ a
	for _, x := range nums {
		if x == a {
			return []int{a, b}
		}
	}
	return []int{b, a}
}
```

#### TypeScript

```ts
function findErrorNums(nums: number[]): number[] {
    const n = nums.length;
    let xs = 0;
    for (let i = 1; i <= n; ++i) {
        xs ^= i ^ nums[i - 1];
    }
    const lb = xs & -xs;
    let a = 0;
    for (let i = 1; i <= n; ++i) {
        if (i & lb) {
            a ^= i;
        }
        if (nums[i - 1] & lb) {
            a ^= nums[i - 1];
        }
    }
    const b = xs ^ a;
    return nums.includes(a) ? [a, b] : [b, a];
}
```

#### Rust

```rust
impl Solution {
    pub fn find_error_nums(nums: Vec<i32>) -> Vec<i32> {
        let mut xs = 0;
        for (i, x) in nums.iter().enumerate() {
            xs ^= ((i + 1) as i32) ^ x;
        }
        let mut a = 0;
        let lb = xs & -xs;
        for (i, x) in nums.iter().enumerate() {
            if (((i + 1) as i32) & lb) != 0 {
                a ^= (i + 1) as i32;
            }
            if (*x & lb) != 0 {
                a ^= *x;
            }
        }
        let b = xs ^ a;
        for x in nums.iter() {
            if *x == a {
                return vec![a, b];
            }
        }
        vec![b, a]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
