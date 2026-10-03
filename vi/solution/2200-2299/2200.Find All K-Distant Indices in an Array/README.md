---
comments: true
difficulty: Easy
rating: 1266
source: Weekly Contest 284 Q1
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [2200. Find All K-Distant Indices in an Array](https://leetcode.com/problems/find-all-k-distant-indices-in-an-array)

[中文文档](/solution/2200-2299/2200.Find%20All%20K-Distant%20Indices%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> <strong>được đánh số từ 0</strong> và hai số nguyên <code>key</code>, <code>k</code>. Một <strong>chỉ số cách key không quá k</strong> là một chỉ số <code>i</code> của <code>nums</code> sao cho tồn tại ít nhất một chỉ số <code>j</code> thỏa mãn <code>|i - j| &lt;= k</code> và <code>nums[j] == key</code>.</p>

<p>Trả về <em>danh sách tất cả các chỉ số cách key không quá k, được sắp xếp theo <strong>thứ tự tăng dần</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,9,1,3,9,5], key = 9, k = 1
<strong>Đầu ra:</strong> [1,2,3,4,5,6]
<strong>Giải thích:</strong> Ở đây, <code>nums[2] == key</code> và <code>nums[5] == key.
- For index 0, |0 - 2| &gt; k and |0 - 5| &gt; k, so there is no j</code> sao cho <code>|0 - j| &lt;= k</code> và <code>nums[j] == key. Thus, 0 is not a k-distant index.
- For index 1, |1 - 2| &lt;= k and nums[2] == key, so 1 is a k-distant index.
- For index 2, |2 - 2| &lt;= k and nums[2] == key, so 2 is a k-distant index.
- For index 3, |3 - 2| &lt;= k and nums[2] == key, so 3 is a k-distant index.
- For index 4, |4 - 5| &lt;= k and nums[5] == key, so 4 is a k-distant index.
- For index 5, |5 - 5| &lt;= k and nums[5] == key, so 5 is a k-distant index.
- For index 6, |6 - 5| &lt;= k and nums[5] == key, so 6 is a k-distant index.
</code>Do đó, ta trả về [1,2,3,4,5,6], là dãy được sắp xếp theo thứ tự tăng dần.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,2,2,2], key = 2, k = 2
<strong>Đầu ra:</strong> [0,1,2,3,4]
<strong>Giải thích:</strong> Với mọi chỉ số i trong nums, tồn tại một chỉ số j sao cho |i - j| &lt;= k và nums[j] == key, nên mọi chỉ số đều là chỉ số cách key không quá k.
Vì vậy, ta trả về [0,1,2,3,4].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 1000</code></li>
	<li><code>key</code> là một số nguyên trong mảng <code>nums</code>.</li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần mọi chỉ số nằm trong khoảng cách $k$ so với một lần xuất hiện của $key$. Kiểm tra một cửa sổ có độ rộng $2k$ quanh mỗi $i$, hoặc duyệt toàn bộ mảng để tìm một $j$ hợp lệ, đều có độ phức tạp $O(n^2)$. Với $n \le 10^3$, cận này chấp nhận được.
>
> Điều quan trọng chỉ là có tồn tại $key$ trong $[i-k, i+k]$ hay không. Với mỗi $i$ cố định, ta duyệt $j$; ngay khi $|i-j| \le k$ và $nums[j] = key$, ta ghi nhận $i$ rồi thoát vòng lặp trong.

<!-- thinking:end -->

Ta liệt kê chỉ số $i$ trong đoạn $[0, n)$, với mỗi chỉ số $i$, ta lại liệt kê chỉ số $j$ trong đoạn $[0, n)$. Nếu $|i - j| \leq k$ và $nums[j] = key$, thì $i$ là một chỉ số cách key không quá k. Ta thêm $i$ vào mảng kết quả, sau đó thoát vòng lặp trong và chuyển sang liệt kê chỉ số $i$ tiếp theo.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findKDistantIndices(self, nums: List[int], key: int, k: int) -> List[int]:
        ans = []
        n = len(nums)
        for i in range(n):
            if any(abs(i - j) <= k and nums[j] == key for j in range(n)):
                ans.append(i)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> findKDistantIndices(int[] nums, int key, int k) {
        int n = nums.length;
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (Math.abs(i - j) <= k && nums[j] == key) {
                    ans.add(i);
                    break;
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
    vector<int> findKDistantIndices(vector<int>& nums, int key, int k) {
        int n = nums.size();
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (abs(i - j) <= k && nums[j] == key) {
                    ans.push_back(i);
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findKDistantIndices(nums []int, key int, k int) (ans []int) {
	for i := range nums {
		for j, x := range nums {
			if abs(i-j) <= k && x == key {
				ans = append(ans, i)
				break
			}
		}
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function findKDistantIndices(nums: number[], key: number, k: number): number[] {
    const n = nums.length;
    const ans: number[] = [];
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < n; ++j) {
            if (Math.abs(i - j) <= k && nums[j] === key) {
                ans.push(i);
                break;
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_k_distant_indices(nums: Vec<i32>, key: i32, k: i32) -> Vec<i32> {
        let n = nums.len();
        let mut ans = Vec::new();
        for i in 0..n {
            for j in 0..n {
                if (i as i32 - j as i32).abs() <= k && nums[j] == key {
                    ans.push(i as i32);
                    break;
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

### Lời giải 2: Tiền xử lý + tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt lại mảng với mỗi $i$ và không tận dụng các vị trí của $key$ đã biết. Ta có thể thu thập các vị trí đó một lần rồi truy vấn trong thời gian logarit.
>
> Lưu mọi chỉ số của $key$ vào một danh sách đã sắp xếp $idx$. Với mỗi $i$, dùng tìm kiếm nhị phân trên $idx$ để tìm một giá trị trong $[i-k, i+k]$ thông qua $\textit{bisect\_left}$ và $\textit{bisect\_right}$. Nếu $l \le r$ thì $i$ hợp lệ. Độ phức tạp thời gian giảm còn $O(n \log n)$.

<!-- thinking:end -->

Ta có thể tiền xử lý để lấy các chỉ số của mọi phần tử bằng $key$, lưu chúng vào mảng $idx$. Tất cả các chỉ số trong mảng $idx$ đều được sắp xếp theo thứ tự tăng dần.

Tiếp theo, ta liệt kê chỉ số $i$. Với mỗi chỉ số $i$, ta có thể dùng tìm kiếm nhị phân để tìm các phần tử trong đoạn $[i - k, i + k]$ của mảng $idx$. Nếu có phần tử, thì $i$ là một chỉ số cách key không quá k. Ta thêm $i$ vào mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findKDistantIndices(self, nums: List[int], key: int, k: int) -> List[int]:
        idx = [i for i, x in enumerate(nums) if x == key]
        ans = []
        for i in range(len(nums)):
            l = bisect_left(idx, i - k)
            r = bisect_right(idx, i + k) - 1
            if l <= r:
                ans.append(i)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> findKDistantIndices(int[] nums, int key, int k) {
        List<Integer> idx = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == key) {
                idx.add(i);
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < nums.length; ++i) {
            int l = Collections.binarySearch(idx, i - k);
            int r = Collections.binarySearch(idx, i + k + 1);
            l = l < 0 ? -l - 1 : l;
            r = r < 0 ? -r - 2 : r - 1;
            if (l <= r) {
                ans.add(i);
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
    vector<int> findKDistantIndices(vector<int>& nums, int key, int k) {
        vector<int> idx;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            if (nums[i] == key) {
                idx.push_back(i);
            }
        }
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            auto it1 = lower_bound(idx.begin(), idx.end(), i - k);
            auto it2 = upper_bound(idx.begin(), idx.end(), i + k) - 1;
            if (it1 <= it2) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findKDistantIndices(nums []int, key int, k int) (ans []int) {
	idx := []int{}
	for i, x := range nums {
		if x == key {
			idx = append(idx, i)
		}
	}
	for i := range nums {
		l := sort.SearchInts(idx, i-k)
		r := sort.SearchInts(idx, i+k+1) - 1
		if l <= r {
			ans = append(ans, i)
		}
	}
	return
}
```

#### TypeScript

```ts
function findKDistantIndices(nums: number[], key: number, k: number): number[] {
    const n = nums.length;
    const idx: number[] = [];
    for (let i = 0; i < n; i++) {
        if (nums[i] === key) {
            idx.push(i);
        }
    }
    const search = (x: number): number => {
        let [l, r] = [0, idx.length];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (idx[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    const ans: number[] = [];
    for (let i = 0; i < n; ++i) {
        const l = search(i - k);
        const r = search(i + k + 1) - 1;
        if (l <= r) {
            ans.push(i);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_k_distant_indices(nums: Vec<i32>, key: i32, k: i32) -> Vec<i32> {
        let n = nums.len();
        let mut idx = Vec::new();
        for i in 0..n {
            if nums[i] == key {
                idx.push(i as i32);
            }
        }

        let search = |x: i32| -> usize {
            let (mut l, mut r) = (0, idx.len());
            while l < r {
                let mid = (l + r) >> 1;
                if idx[mid] >= x {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            l
        };

        let mut ans = Vec::new();
        for i in 0..n {
            let l = search(i as i32 - k);
            let r = search(i as i32 + k + 1) as i32 - 1;
            if l as i32 <= r {
                ans.push(i as i32);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 2 đã tránh việc duyệt lại các vị trí của $key$, nhưng vẫn lưu một mảng chỉ số và thực hiện hai lần tìm kiếm nhị phân cho mỗi $i$. Khi $i$ tăng, $j$ nhỏ nhất thỏa mãn $j \ge i-k$ và $nums[j] = key$ chỉ có thể dịch sang phải.
>
> Ta tăng một con trỏ $j$ đơn điệu: tăng nó khi $j < i-k$ hoặc ô hiện tại không phải là $key$. Sau đó, nếu $j \le i+k$ thì $i$ là một chỉ số cách key không quá k. Chỉ cần một lượt duyệt với $O(1)$ không gian bổ sung.

<!-- thinking:end -->

Ta liệt kê chỉ số $i$ và dùng một con trỏ $j$ trỏ đến chỉ số nhỏ nhất thỏa mãn $j \geq i - k$ và $nums[j] = key$. Nếu $j$ tồn tại và $j \leq i + k$, thì $i$ là một chỉ số cách key không quá k. Ta thêm $i$ vào mảng kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findKDistantIndices(self, nums: List[int], key: int, k: int) -> List[int]:
        ans = []
        j, n = 0, len(nums)
        for i in range(n):
            while j < i - k or (j < n and nums[j] != key):
                j += 1
            if j < n and j <= (i + k):
                ans.append(i)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> findKDistantIndices(int[] nums, int key, int k) {
        int n = nums.length;
        List<Integer> ans = new ArrayList<>();
        for (int i = 0, j = 0; i < n; ++i) {
            while (j < i - k || (j < n && nums[j] != key)) {
                ++j;
            }
            if (j < n && j <= i + k) {
                ans.add(i);
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
    vector<int> findKDistantIndices(vector<int>& nums, int key, int k) {
        int n = nums.size();
        vector<int> ans;
        for (int i = 0, j = 0; i < n; ++i) {
            while (j < i - k || (j < n && nums[j] != key)) {
                ++j;
            }
            if (j < n && j <= i + k) {
                ans.push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findKDistantIndices(nums []int, key int, k int) (ans []int) {
	n := len(nums)
	for i, j := 0, 0; i < n; i++ {
		for j < i-k || (j < n && nums[j] != key) {
			j++
		}
		if j < n && j <= i+k {
			ans = append(ans, i)
		}
	}
	return
}
```

#### TypeScript

```ts
function findKDistantIndices(nums: number[], key: number, k: number): number[] {
    const n = nums.length;
    const ans: number[] = [];
    for (let i = 0, j = 0; i < n; ++i) {
        while (j < i - k || (j < n && nums[j] !== key)) {
            ++j;
        }
        if (j < n && j <= i + k) {
            ans.push(i);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_k_distant_indices(nums: Vec<i32>, key: i32, k: i32) -> Vec<i32> {
        let n = nums.len();
        let mut ans = Vec::new();
        let mut j = 0;
        for i in 0..n {
            while j < i.saturating_sub(k as usize) || (j < n && nums[j] != key) {
                j += 1;
            }
            if j < n && j <= i + k as usize {
                ans.push(i as i32);
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
