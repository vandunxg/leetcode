---
comments: true
difficulty: Medium
rating: 1397
source: Weekly Contest 356 Q2
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [2799. Count Complete Subarrays in an Array](https://leetcode.com/problems/count-complete-subarrays-in-an-array)

[Tài liệu tiếng Trung](/solution/2700-2799/2799.Count%20Complete%20Subarrays%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên <strong>dương</strong>.</p>

<p>Ta gọi một mảng con là <strong>đầy đủ</strong> nếu thỏa mãn điều kiện sau:</p>

<ul>
	<li>Số lượng phần tử <strong>khác nhau</strong> trong mảng con bằng số lượng phần tử khác nhau trong toàn bộ mảng.</li>
</ul>

<p>Hãy trả về <em>số lượng mảng con <strong>đầy đủ</strong></em>.</p>

<p><strong>Mảng con</strong> là một phần liên tiếp, không rỗng của một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,1,2,2]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các mảng con đầy đủ là: [1,3,1,2], [1,3,1,2,2], [3,1,2] và [3,1,2,2].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,5,5,5]
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Mảng chỉ gồm số nguyên 5, nên mọi mảng con đều đầy đủ. Có thể chọn 10 mảng con.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 2000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con đầy đủ chứa mọi giá trị khác nhau của mảng ban đầu. Với $n\le 1000$, việc tạo một set cho mỗi cặp điểm đầu-cuối vẫn đáp ứng được giới hạn.
>
> Trước hết, tính số lượng giá trị khác nhau toàn cục $cnt$, sau đó với mỗi điểm đầu bên trái, mở rộng set sang phải và đếm một mảng con đầy đủ mỗi khi kích thước set đạt $cnt$.

<!-- thinking:end -->

Đầu tiên, ta dùng một hash table để đếm số lượng phần tử khác nhau trong mảng, ký hiệu là $cnt$.

Tiếp theo, ta liệt kê chỉ số điểm đầu bên trái $i$ của mảng con và duy trì một set $s$ để lưu các phần tử trong mảng con. Mỗi khi di chuyển chỉ số điểm cuối bên phải $j$ sang phải, ta thêm $nums[j]$ vào set $s$ và kiểm tra xem kích thước của set $s$ có bằng $cnt$ hay không. Nếu bằng $cnt$, điều đó có nghĩa là mảng con hiện tại là một mảng con đầy đủ, khi đó ta tăng đáp án lên $1$.

Sau khi kết thúc quá trình liệt kê, ta trả về đáp án.

Độ phức tạp thời gian: $O(n^2)$, độ phức tạp không gian: $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countCompleteSubarrays(self, nums: List[int]) -> int:
        cnt = len(set(nums))
        ans, n = 0, len(nums)
        for i in range(n):
            s = set()
            for x in nums[i:]:
                s.add(x)
                if len(s) == cnt:
                    ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int countCompleteSubarrays(int[] nums) {
        Set<Integer> s = new HashSet<>();
        for (int x : nums) {
            s.add(x);
        }
        int cnt = s.size();
        int ans = 0, n = nums.length;
        for (int i = 0; i < n; ++i) {
            s.clear();
            for (int j = i; j < n; ++j) {
                s.add(nums[j]);
                if (s.size() == cnt) {
                    ++ans;
                }
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
    int countCompleteSubarrays(vector<int>& nums) {
        unordered_set<int> s(nums.begin(), nums.end());
        int cnt = s.size();
        int ans = 0, n = nums.size();
        for (int i = 0; i < n; ++i) {
            s.clear();
            for (int j = i; j < n; ++j) {
                s.insert(nums[j]);
                if (s.size() == cnt) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countCompleteSubarrays(nums []int) (ans int) {
	s := map[int]bool{}
	for _, x := range nums {
		s[x] = true
	}
	cnt := len(s)
	for i := range nums {
		s = map[int]bool{}
		for _, x := range nums[i:] {
			s[x] = true
			if len(s) == cnt {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countCompleteSubarrays(nums: number[]): number {
    const s: Set<number> = new Set(nums);
    const cnt = s.size;
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        s.clear();
        for (let j = i; j < n; ++j) {
            s.add(nums[j]);
            if (s.size === cnt) {
                ++ans;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn count_complete_subarrays(nums: Vec<i32>) -> i32 {
        let mut s = HashSet::new();
        for &x in &nums {
            s.insert(x);
        }
        let cnt = s.len();
        let n = nums.len();
        let mut ans = 0;

        for i in 0..n {
            s.clear();
            for j in i..n {
                s.insert(nums[j]);
                if s.len() == cnt {
                    ans += 1;
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hash Table + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 xây dựng lại một set từ mỗi điểm đầu bên trái. Số lượng giá trị khác nhau không giảm khi điểm cuối bên phải dịch chuyển: một khi cửa sổ đã đầy đủ, mọi mảng con giữ nguyên điểm cuối này và có điểm đầu không vượt quá $i$ đều đầy đủ, đóng góp $n-j$. Sau đó, ta dịch điểm đầu sang phải và cập nhật số lần xuất hiện.

<!-- thinking:end -->

Tương tự Lời giải 1, ta có thể dùng một hash table để đếm số lượng phần tử khác nhau trong mảng, ký hiệu là $cnt$.

Tiếp theo, ta dùng hai con trỏ để duy trì một sliding window, trong đó chỉ số điểm cuối bên phải là $j$ và chỉ số điểm đầu bên trái là $i$.

Mỗi khi cố định chỉ số điểm đầu bên trái $i$, ta di chuyển chỉ số điểm cuối bên phải $j$ sang phải. Khi số lượng phần tử khác nhau trong sliding window bằng $cnt$, điều đó có nghĩa là mọi mảng con bắt đầu từ chỉ số điểm đầu bên trái $i$ đến chỉ số điểm cuối bên phải $j$ và xa hơn đều là mảng con đầy đủ. Khi đó, ta tăng đáp án lên $n - j$, trong đó $n$ là độ dài của mảng. Sau đó, ta di chuyển chỉ số điểm đầu bên trái $i$ một bước sang phải và lặp lại quá trình.

Độ phức tạp thời gian: $O(n)$, độ phức tạp không gian: $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countCompleteSubarrays(self, nums: List[int]) -> int:
        cnt = len(set(nums))
        d = Counter()
        ans, n = 0, len(nums)
        i = 0
        for j, x in enumerate(nums):
            d[x] += 1
            while len(d) == cnt:
                ans += n - j
                d[nums[i]] -= 1
                if d[nums[i]] == 0:
                    d.pop(nums[i])
                i += 1
        return ans
```

#### Java

```java
class Solution {
    public int countCompleteSubarrays(int[] nums) {
        Map<Integer, Integer> d = new HashMap<>();
        for (int x : nums) {
            d.put(x, 1);
        }
        int cnt = d.size();
        int ans = 0, n = nums.length;
        d.clear();
        for (int i = 0, j = 0; j < n; ++j) {
            d.merge(nums[j], 1, Integer::sum);
            while (d.size() == cnt) {
                ans += n - j;
                if (d.merge(nums[i], -1, Integer::sum) == 0) {
                    d.remove(nums[i]);
                }
                ++i;
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
    int countCompleteSubarrays(vector<int>& nums) {
        unordered_map<int, int> d;
        for (int x : nums) {
            d[x] = 1;
        }
        int cnt = d.size();
        d.clear();
        int ans = 0, n = nums.size();
        for (int i = 0, j = 0; j < n; ++j) {
            d[nums[j]]++;
            while (d.size() == cnt) {
                ans += n - j;
                if (--d[nums[i]] == 0) {
                    d.erase(nums[i]);
                }
                ++i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countCompleteSubarrays(nums []int) (ans int) {
	d := map[int]int{}
	for _, x := range nums {
		d[x] = 1
	}
	cnt := len(d)
	i, n := 0, len(nums)
	d = map[int]int{}
	for j, x := range nums {
		d[x]++
		for len(d) == cnt {
			ans += n - j
			d[nums[i]]--
			if d[nums[i]] == 0 {
				delete(d, nums[i])
			}
			i++
		}
	}
	return
}
```

#### TypeScript

```ts
function countCompleteSubarrays(nums: number[]): number {
    const d: Map<number, number> = new Map();
    for (const x of nums) {
        d.set(x, (d.get(x) ?? 0) + 1);
    }
    const cnt = d.size;
    d.clear();
    const n = nums.length;
    let ans = 0;
    let i = 0;
    for (let j = 0; j < n; ++j) {
        d.set(nums[j], (d.get(nums[j]) ?? 0) + 1);
        while (d.size === cnt) {
            ans += n - j;
            d.set(nums[i], d.get(nums[i])! - 1);
            if (d.get(nums[i]) === 0) {
                d.delete(nums[i]);
            }
            ++i;
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn count_complete_subarrays(nums: Vec<i32>) -> i32 {
        let mut d = HashMap::new();
        for &x in &nums {
            d.insert(x, 1);
        }
        let cnt = d.len();
        let mut ans = 0;
        let n = nums.len();
        d.clear();

        let (mut i, mut j) = (0, 0);
        while j < n {
            *d.entry(nums[j]).or_insert(0) += 1;
            while d.len() == cnt {
                ans += (n - j) as i32;
                let e = d.get_mut(&nums[i]).unwrap();
                *e -= 1;
                if *e == 0 {
                    d.remove(&nums[i]);
                }
                i += 1;
            }
            j += 1;
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
