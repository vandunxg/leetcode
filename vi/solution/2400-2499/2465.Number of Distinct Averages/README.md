---
comments: true
difficulty: Easy
rating: 1250
source: Biweekly Contest 91 Q1
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [2465. Number of Distinct Averages](https://leetcode.com/problems/number-of-distinct-averages)

[中文文档](/solution/2400-2499/2465.Number%20of%20Distinct%20Averages/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, có độ dài <strong>chẵn</strong>.</p>

<p>Chừng nào <code>nums</code> <strong>chưa rỗng</strong>, bạn phải lặp lại các bước sau:</p>

<ul>
	<li>Tìm số nhỏ nhất trong <code>nums</code> và xóa nó.</li>
	<li>Tìm số lớn nhất trong <code>nums</code> và xóa nó.</li>
	<li>Tính giá trị trung bình của hai số vừa xóa.</li>
</ul>

<p><strong>Giá trị trung bình</strong> của hai số <code>a</code> và <code>b</code> là <code>(a + b) / 2</code>.</p>

<ul>
	<li>Ví dụ, giá trị trung bình của <code>2</code> và <code>3</code> là <code>(2 + 3) / 2 = 2.5</code>.</li>
</ul>

<p>Trả về<em> số lượng giá trị trung bình <strong>khác nhau</strong> được tính theo quy trình trên</em>.</p>

<p><strong>Lưu ý</strong> rằng khi có nhiều số cùng là số nhỏ nhất hoặc số lớn nhất, có thể xóa bất kỳ số nào trong chúng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,1,4,0,3,5]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
1. Xóa 0 và 5, giá trị trung bình là (0 + 5) / 2 = 2.5. Khi đó, nums = [4,1,4,3].
2. Xóa 1 và 4. Giá trị trung bình là (1 + 4) / 2 = 2.5, và nums = [4,3].
3. Xóa 3 và 4, giá trị trung bình là (3 + 4) / 2 = 3.5.
Vì có 2 giá trị khác nhau trong số 2.5, 2.5 và 3.5, ta trả về 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,100]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Chỉ có một giá trị trung bình được tính sau khi xóa 1 và 100, vì vậy ta trả về 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>nums.length</code> là số chẵn.</li>
	<li><code>0 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước ghép số nhỏ nhất và số lớn nhất hiện tại; các giá trị trung bình khác nhau tương đương với các tổng khác nhau (hệ số $1/2$ không ảnh hưởng). Với $n\le 100$, ta sắp xếp rồi ghép các phần tử ở hai đầu vào một set.

<!-- thinking:end -->

Bài toán yêu cầu chúng ta mỗi lần tìm giá trị nhỏ nhất và lớn nhất trong mảng $nums$, xóa chúng rồi tính giá trị trung bình của hai giá trị đã xóa. Vì vậy, trước tiên ta có thể sắp xếp mảng $nums$, sau đó mỗi lần lấy phần tử đầu tiên và phần tử cuối cùng của mảng, tính tổng của chúng, dùng một hash table hoặc mảng $cnt$ để ghi nhận số lần xuất hiện của mỗi tổng, cuối cùng đếm số tổng khác nhau.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distinctAverages(self, nums: List[int]) -> int:
        nums.sort()
        return len(set(nums[i] + nums[-i - 1] for i in range(len(nums) >> 1)))
```

#### Java

```java
class Solution {
    public int distinctAverages(int[] nums) {
        Arrays.sort(nums);
        Set<Integer> s = new HashSet<>();
        int n = nums.length;
        for (int i = 0; i < n >> 1; ++i) {
            s.add(nums[i] + nums[n - i - 1]);
        }
        return s.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int distinctAverages(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        unordered_set<int> s;
        int n = nums.size();
        for (int i = 0; i < n >> 1; ++i) {
            s.insert(nums[i] + nums[n - i - 1]);
        }
        return s.size();
    }
};
```

#### Go

```go
func distinctAverages(nums []int) (ans int) {
	sort.Ints(nums)
	n := len(nums)
	s := map[int]struct{}{}
	for i := 0; i < n>>1; i++ {
		s[nums[i]+nums[n-i-1]] = struct{}{}
	}
	return len(s)
}
```

#### TypeScript

```ts
function distinctAverages(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const s: Set<number> = new Set();
    const n = nums.length;
    for (let i = 0; i < n >> 1; ++i) {
        s.add(nums[i] + nums[n - i - 1]);
    }
    return s.size;
}
```

#### Rust

```rust
impl Solution {
    pub fn distinct_averages(nums: Vec<i32>) -> i32 {
        let mut nums = nums;
        nums.sort();
        let n = nums.len();
        let mut cnt = vec![0; 201];
        let mut ans = 0;

        for i in 0..n >> 1 {
            let x = (nums[i] + nums[n - i - 1]) as usize;
            cnt[x] += 1;

            if cnt[x] == 1 {
                ans += 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng kích thước của set. Một counter tăng đáp án khi một tổng xuất hiện lần đầu cũng đếm được các giá trị khác nhau tương tự.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distinctAverages(self, nums: List[int]) -> int:
        nums.sort()
        ans = 0
        cnt = Counter()
        for i in range(len(nums) >> 1):
            x = nums[i] + nums[-i - 1]
            cnt[x] += 1
            if cnt[x] == 1:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int distinctAverages(int[] nums) {
        Arrays.sort(nums);
        int[] cnt = new int[201];
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n >> 1; ++i) {
            if (++cnt[nums[i] + nums[n - i - 1]] == 1) {
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
    int distinctAverages(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int cnt[201]{};
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n >> 1; ++i) {
            if (++cnt[nums[i] + nums[n - i - 1]] == 1) {
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func distinctAverages(nums []int) (ans int) {
	sort.Ints(nums)
	n := len(nums)
	cnt := [201]int{}
	for i := 0; i < n>>1; i++ {
		x := nums[i] + nums[n-i-1]
		cnt[x]++
		if cnt[x] == 1 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function distinctAverages(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const cnt: number[] = Array(201).fill(0);
    let ans = 0;
    const n = nums.length;
    for (let i = 0; i < n >> 1; ++i) {
        if (++cnt[nums[i] + nums[n - i - 1]] === 1) {
            ++ans;
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn distinct_averages(nums: Vec<i32>) -> i32 {
        let mut h = HashMap::new();
        let mut nums = nums;
        let mut ans = 0;
        let n = nums.len();
        nums.sort();

        for i in 0..n >> 1 {
            let x = nums[i] + nums[n - i - 1];
            *h.entry(x).or_insert(0) += 1;

            if *h.get(&x).unwrap() == 1 {
                ans += 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3

<!-- thinking:start -->

> **Tư duy**
>
> Vẫn là cách ghép các phần tử đã sắp xếp như ở phương pháp 2, nhưng dùng set thay cho counter: chèn phần tử và tăng đáp án khi phần tử mới xuất hiện. Cả ba cách đều gồm sắp xếp rồi loại bỏ phần tử trùng lặp trong thời gian tuyến tính.

<!-- thinking:end -->

<!-- tabs:start -->

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn distinct_averages(nums: Vec<i32>) -> i32 {
        let mut set = HashSet::new();
        let mut ans = 0;
        let n = nums.len();
        let mut nums = nums;
        nums.sort();

        for i in 0..n >> 1 {
            let x = nums[i] + nums[n - i - 1];

            if set.contains(&x) {
                continue;
            }

            set.insert(x);
            ans += 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
