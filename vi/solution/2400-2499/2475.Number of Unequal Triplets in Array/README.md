---
comments: true
difficulty: Easy
rating: 1255
source: Weekly Contest 320 Q1
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [2475. Number of Unequal Triplets in Array](https://leetcode.com/problems/number-of-unequal-triplets-in-array)

[中文文档](/solution/2400-2499/2475.Number%20of%20Unequal%20Triplets%20in%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên dương <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Hãy tìm số lượng bộ ba <code>(i, j, k)</code> thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>0 &lt;= i &lt; j &lt; k &lt; nums.length</code></li>
	<li><code>nums[i]</code>, <code>nums[j]</code> và <code>nums[k]</code> <strong>đôi một khác nhau</strong>.
	<ul>
		<li>Nói cách khác, <code>nums[i] != nums[j]</code>, <code>nums[i] != nums[k]</code> và <code>nums[j] != nums[k]</code>.</li>
	</ul>
	</li>
</ul>

<p>Trả về <em>số lượng bộ ba thỏa mãn các điều kiện trên.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,4,2,4,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các bộ ba sau thỏa mãn các điều kiện:
- (0, 2, 4) vì 4 != 2 != 3
- (1, 2, 4) vì 4 != 2 != 3
- (2, 3, 4) vì 2 != 4 != 3
Vì có 3 bộ ba nên ta trả về 3.
Lưu ý rằng (2, 0, 4) không phải là bộ ba hợp lệ vì 2 &gt; 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,1,1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có bộ ba nào thỏa mãn các điều kiện nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê bằng brute force

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 100$, số bộ ba có thứ tự phù hợp với ba vòng lặp lồng nhau kiểm tra điều kiện đôi một khác nhau.

<!-- thinking:end -->

Ta có thể trực tiếp liệt kê tất cả các bộ ba $(i, j, k)$ và đếm những bộ ba thỏa mãn các điều kiện.

Độ phức tạp thời gian là $O(n^3)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def unequalTriplets(self, nums: List[int]) -> int:
        n = len(nums)
        ans = 0
        for i in range(n):
            for j in range(i + 1, n):
                for k in range(j + 1, n):
                    ans += (
                        nums[i] != nums[j] and nums[j] != nums[k] and nums[i] != nums[k]
                    )
        return ans
```

#### Java

```java
class Solution {
    public int unequalTriplets(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                for (int k = j + 1; k < n; ++k) {
                    if (nums[i] != nums[j] && nums[j] != nums[k] && nums[i] != nums[k]) {
                        ++ans;
                    }
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
    int unequalTriplets(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                for (int k = j + 1; k < n; ++k) {
                    if (nums[i] != nums[j] && nums[j] != nums[k] && nums[i] != nums[k]) {
                        ++ans;
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func unequalTriplets(nums []int) (ans int) {
	n := len(nums)
	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			for k := j + 1; k < n; k++ {
				if nums[i] != nums[j] && nums[j] != nums[k] && nums[i] != nums[k] {
					ans++
				}
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function unequalTriplets(nums: number[]): number {
    const n = nums.length;
    let ans = 0;
    for (let i = 0; i < n - 2; i++) {
        for (let j = i + 1; j < n - 1; j++) {
            for (let k = j + 1; k < n; k++) {
                if (nums[i] !== nums[j] && nums[j] !== nums[k] && nums[i] !== nums[k]) {
                    ans++;
                }
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn unequal_triplets(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut ans = 0;
        for i in 0..n - 2 {
            for j in i + 1..n - 1 {
                for k in j + 1..n {
                    if nums[i] != nums[j] && nums[j] != nums[k] && nums[i] != nums[k] {
                        ans += 1;
                    }
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

### Lời giải 2: Sắp xếp + liệt kê phần tử ở giữa + tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 có độ phức tạp $O(n^3)$. Sau khi sắp xếp, các phần tử bằng nhau nằm liền nhau. Cố định chỉ số giữa $j$; tích của số lượng giá trị nhỏ hơn ở bên trái và lớn hơn ở bên phải chính là số đóng góp. Hai lần tìm kiếm nhị phân xác định hai ranh giới.

<!-- thinking:end -->

Ta cũng có thể sắp xếp mảng $nums$ trước.

Sau đó, ta duyệt qua $nums$, liệt kê phần tử ở giữa $nums[j]$, rồi dùng tìm kiếm nhị phân để tìm chỉ số gần nhất $i$ ở bên trái của $nums[j]$ sao cho $nums[i] < nums[j]$; đồng thời tìm chỉ số gần nhất $k$ ở bên phải của $nums[j]$ sao cho $nums[k] > nums[j]$. Khi đó, số bộ ba có $nums[j]$ là phần tử ở giữa và thỏa mãn các điều kiện là $(i + 1) \times (n - k)$, được cộng vào đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def unequalTriplets(self, nums: List[int]) -> int:
        nums.sort()
        ans, n = 0, len(nums)
        for j in range(1, n - 1):
            i = bisect_left(nums, nums[j], hi=j) - 1
            k = bisect_right(nums, nums[j], lo=j + 1)
            ans += (i >= 0 and k < n) * (i + 1) * (n - k)
        return ans
```

#### Java

```java
class Solution {
    public int unequalTriplets(int[] nums) {
        Arrays.sort(nums);
        int ans = 0, n = nums.length;
        for (int j = 1; j < n - 1; ++j) {
            int i = search(nums, nums[j], 0, j) - 1;
            int k = search(nums, nums[j] + 1, j + 1, n);
            if (i >= 0 && k < n) {
                ans += (i + 1) * (n - k);
            }
        }
        return ans;
    }

    private int search(int[] nums, int x, int left, int right) {
        while (left < right) {
            int mid = (left + right) >> 1;
            if (nums[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int unequalTriplets(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int ans = 0, n = nums.size();
        for (int j = 1; j < n - 1; ++j) {
            int i = lower_bound(nums.begin(), nums.begin() + j, nums[j]) - nums.begin() - 1;
            int k = upper_bound(nums.begin() + j + 1, nums.end(), nums[j]) - nums.begin();
            if (i >= 0 && k < n) {
                ans += (i + 1) * (n - k);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func unequalTriplets(nums []int) (ans int) {
	sort.Ints(nums)
	n := len(nums)
	for j := 1; j < n-1; j++ {
		i := sort.Search(j, func(h int) bool { return nums[h] >= nums[j] }) - 1
		k := sort.Search(n, func(h int) bool { return h > j && nums[h] > nums[j] })
		if i >= 0 && k < n {
			ans += (i + 1) * (n - k)
		}
	}
	return
}
```

#### TypeScript

```ts
function unequalTriplets(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    let ans = 0;
    const search = (x: number, left: number, right: number): number => {
        while (left < right) {
            const mid = (left + right) >> 1;
            if (nums[mid] >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    };
    for (let j = 1; j < n - 1; ++j) {
        const i = search(nums[j], 0, j) - 1;
        const k = search(nums[j] + 1, j + 1, n);
        if (i >= 0 && k < n) {
            ans += (i + 1) * (n - k);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn unequal_triplets(mut nums: Vec<i32>) -> i32 {
        nums.sort_unstable();
        let n = nums.len();
        let mut ans = 0;
        for j in 1..n - 1 {
            let i = nums[..j].partition_point(|&x| x < nums[j]) as i32 - 1;
            let k = j + 1 + nums[j + 1..].partition_point(|&x| x <= nums[j]);
            if i >= 0 && k < n {
                ans += (i + 1) * (n - k) as i32;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Hash table

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 2 vẫn phải sắp xếp. Chỉ các giá trị khác nhau mới quan trọng, không phải các chỉ số, nên ta đếm tần suất: với số lượng phần tử ở giữa là $b$, khi đã thấy $a$ phần tử, ta có $c=n-a-b$ phần tử còn lại và cộng $a\cdot b\cdot c$. Độ phức tạp tuyến tính.

<!-- thinking:end -->

Ta cũng có thể dùng một hash table $cnt$ để đếm số lần xuất hiện của mỗi phần tử trong mảng $nums$.

Sau đó, ta duyệt qua hash table $cnt$, liệt kê số lượng phần tử ở giữa là $b$, và gọi số lượng phần tử ở bên trái là $a$. Khi đó, số lượng phần tử ở bên phải là $c = n - a - b$. Số bộ ba thỏa mãn các điều kiện lúc này là $a \times b \times c$, được cộng vào đáp án. Tiếp theo, cập nhật $a = a + b$ và tiếp tục liệt kê số lượng phần tử ở giữa $b$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def unequalTriplets(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        n = len(nums)
        ans = a = 0
        for b in cnt.values():
            c = n - a - b
            ans += a * b * c
            a += b
        return ans
```

#### Java

```java
class Solution {
    public int unequalTriplets(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int v : nums) {
            cnt.merge(v, 1, Integer::sum);
        }
        int ans = 0, a = 0;
        int n = nums.length;
        for (int b : cnt.values()) {
            int c = n - a - b;
            ans += a * b * c;
            a += b;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int unequalTriplets(vector<int>& nums) {
        unordered_map<int, int> cnt;
        for (int& v : nums) {
            ++cnt[v];
        }
        int ans = 0, a = 0;
        int n = nums.size();
        for (auto& [_, b] : cnt) {
            int c = n - a - b;
            ans += a * b * c;
            a += b;
        }
        return ans;
    }
};
```

#### Go

```go
func unequalTriplets(nums []int) (ans int) {
	cnt := map[int]int{}
	for _, v := range nums {
		cnt[v]++
	}
	a, n := 0, len(nums)
	for _, b := range cnt {
		c := n - a - b
		ans += a * b * c
		a += b
	}
	return
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn unequal_triplets(nums: Vec<i32>) -> i32 {
        let cnt = nums.iter().fold(HashMap::new(), |mut map, &n| {
            *map.entry(n).or_insert(0) += 1;
            map
        });

        let mut ans = 0;
        let n = nums.len();
        let mut a = 0;
        for &b in cnt.values() {
            let c = n - a - b;
            ans += a * b * c;
            a += b;
        }

        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
