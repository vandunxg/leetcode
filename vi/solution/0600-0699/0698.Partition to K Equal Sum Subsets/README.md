---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Memoization
    - Array
    - Dynamic Programming
    - Backtracking
    - Bitmask
---

<!-- problem:start -->

# [698. Partition to K Equal Sum Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets)

[中文文档](/solution/0600-0699/0698.Partition%20to%20K%20Equal%20Sum%20Subsets/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>. Hãy trả về <code>true</code> nếu có thể chia mảng thành <code>k</code> tập con không rỗng có tổng bằng nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,3,2,3,5,2,1], k = 4
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có thể chia thành 4 tập con (5), (1, 4), (2,3), (2,3) có tổng bằng nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4], k = 3
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 16</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li>Số lần xuất hiện của mỗi phần tử nằm trong khoảng <code>[1, 4]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS + cắt tỉa

<!-- thinking:start -->

> **Tư duy**
>
> Chia thành $k$ tập con có tổng bằng nhau. Nếu tổng tất cả phần tử không chia hết cho $k$, không có lời giải. Vì $n\le 16$, có thể dùng DFS nếu cắt tỉa các cách điền đối xứng.
>
> Gọi tổng cần đạt của mỗi tập con là $s$, mảng $cur$ lưu tổng hiện tại của từng tập. Xét các số từ lớn đến nhỏ: bỏ qua tập con nếu thêm số sẽ làm tổng vượt quá $s$, hoặc nếu tổng của nó trùng với tập trước. Thành công khi đã xếp hết các số.

<!-- thinking:end -->

Theo đề bài, ta cần chia mảng $\textit{nums}$ thành $k$ tập con sao cho tổng mỗi tập bằng nhau. Vì vậy, trước tiên tính tổng tất cả phần tử trong $\textit{nums}$. Nếu tổng này không chia hết cho $k$, ta không thể chia mảng thành $k$ tập con và trả về ngay $\textit{false}$.

Nếu tổng chia hết cho $k$, gọi tổng cần đạt của mỗi tập con là $s$. Sau đó, tạo mảng $\textit{cur}$ có độ dài $k$ để lưu tổng hiện tại của mỗi tập con.

Ta sắp xếp mảng $\textit{nums}$ theo thứ tự giảm dần để giảm số lần tìm kiếm, rồi xét từ phần tử đầu tiên và lần lượt thử thêm nó vào từng tập con trong $\textit{cur}$. Nếu thêm $\textit{nums}[i]$ vào tập $\textit{cur}[j]$ khiến tổng vượt quá $s$, ta bỏ qua tập đó. Ngoài ra, nếu $\textit{cur}[j]$ bằng $\textit{cur}[j - 1]$, nghĩa là ta đã tìm kiếm trường hợp tương ứng với $\textit{cur}[j - 1]$, nên có thể bỏ qua lần thử hiện tại.

Nếu có thể xếp tất cả phần tử vào các tập trong $\textit{cur}$, ta đã chia được mảng thành $k$ tập con và trả về $\textit{true}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
        def dfs(i: int) -> bool:
            if i == len(nums):
                return True
            for j in range(k):
                if j and cur[j] == cur[j - 1]:
                    continue
                cur[j] += nums[i]
                if cur[j] <= s and dfs(i + 1):
                    return True
                cur[j] -= nums[i]
            return False

        s, mod = divmod(sum(nums), k)
        if mod:
            return False
        cur = [0] * k
        nums.sort(reverse=True)
        return dfs(0)
```

#### Java

```java
class Solution {
    private int[] nums;
    private int[] cur;
    private int s;

    public boolean canPartitionKSubsets(int[] nums, int k) {
        for (int v : nums) {
            s += v;
        }
        if (s % k != 0) {
            return false;
        }
        s /= k;
        cur = new int[k];
        Arrays.sort(nums);
        this.nums = nums;
        return dfs(nums.length - 1);
    }

    private boolean dfs(int i) {
        if (i < 0) {
            return true;
        }
        for (int j = 0; j < cur.length; ++j) {
            if (j > 0 && cur[j] == cur[j - 1]) {
                continue;
            }
            cur[j] += nums[i];
            if (cur[j] <= s && dfs(i - 1)) {
                return true;
            }
            cur[j] -= nums[i];
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int s = accumulate(nums.begin(), nums.end(), 0);
        if (s % k) {
            return false;
        }
        s /= k;
        int n = nums.size();
        vector<int> cur(k);
        function<bool(int)> dfs = [&](int i) {
            if (i == n) {
                return true;
            }
            for (int j = 0; j < k; ++j) {
                if (j && cur[j] == cur[j - 1]) {
                    continue;
                }
                cur[j] += nums[i];
                if (cur[j] <= s && dfs(i + 1)) {
                    return true;
                }
                cur[j] -= nums[i];
            }
            return false;
        };
        sort(nums.begin(), nums.end(), greater<int>());
        return dfs(0);
    }
};
```

#### Go

```go
func canPartitionKSubsets(nums []int, k int) bool {
	s := 0
	for _, v := range nums {
		s += v
	}
	if s%k != 0 {
		return false
	}
	s /= k
	cur := make([]int, k)
	n := len(nums)

	var dfs func(int) bool
	dfs = func(i int) bool {
		if i == n {
			return true
		}
		for j := 0; j < k; j++ {
			if j > 0 && cur[j] == cur[j-1] {
				continue
			}
			cur[j] += nums[i]
			if cur[j] <= s && dfs(i+1) {
				return true
			}
			cur[j] -= nums[i]
		}
		return false
	}

	sort.Sort(sort.Reverse(sort.IntSlice(nums)))
	return dfs(0)
}
```

#### TypeScript

```ts
function canPartitionKSubsets(nums: number[], k: number): boolean {
    const dfs = (i: number): boolean => {
        if (i === nums.length) {
            return true;
        }
        for (let j = 0; j < k; j++) {
            if (j > 0 && cur[j] === cur[j - 1]) {
                continue;
            }
            cur[j] += nums[i];
            if (cur[j] <= s && dfs(i + 1)) {
                return true;
            }
            cur[j] -= nums[i];
        }
        return false;
    };

    let s = nums.reduce((a, b) => a + b, 0);
    const mod = s % k;
    if (mod !== 0) {
        return false;
    }
    s = Math.floor(s / k);
    const cur = Array(k).fill(0);
    nums.sort((a, b) => b - a);
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Nén trạng thái + ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> DFS theo từng tập con vẫn có thể gặp lại cùng một tập các phần tử đã dùng. Ta ghi nhớ trạng thái tìm kiếm bằng mask $\textit{state}$ cùng tổng hiện tại $t$ của tập con đang xét. Sắp xếp tăng dần cho phép thoát vòng lặp ngay khi $t+v>s$.

<!-- thinking:end -->

Tương tự Lời giải 1, trước tiên ta kiểm tra tổng mảng $\textit{nums}$ có chia hết cho $k$ hay không. Nếu không, trả về ngay $\textit{false}$.

Gọi $s$ là tổng cần đạt của mỗi tập con, còn $\textit{state}$ biểu diễn trạng thái phân nhóm hiện tại của các phần tử. Với số thứ $i$, nếu bit thứ $i$ của $\textit{state}$ bằng $0$, nghĩa là phần tử thứ $i$ chưa được xếp vào tập nào.

Mục tiêu là chia các phần tử thành $k$ tập con có tổng bằng $s$. Gọi $t$ là tổng hiện tại của tập con đang xét. Khi phần tử thứ $i$ chưa được xếp:

- Nếu $t + \textit{nums}[i] \gt s$, phần tử thứ $i$ không thể được thêm vào tập hiện tại. Vì mảng $\textit{nums}$ đã được sắp xếp tăng dần, các phần tử từ vị trí $i$ trở đi đều không thể thêm vào tập này, nên ta trả về ngay $\textit{false}$.
- Nếu không, thêm phần tử thứ $i$ vào tập hiện tại, cập nhật trạng thái thành $\textit{state} | 2^i$ rồi tiếp tục tìm phần tử chưa được xếp. Nếu $t + \textit{nums}[i] = s$, ta đã tạo được một tập con có tổng bằng $s$. Khi đó, đặt lại $t$ về $0$ (có thể thực hiện bằng $(t + \textit{nums}[i]) \bmod s$) rồi tiếp tục chia tập con tiếp theo.

Để tránh tìm kiếm lặp lại, ta dùng mảng $\textit{f}$ có độ dài $2^n$ để lưu kết quả tìm kiếm cho từng trạng thái. Mảng $\textit{f}$ có ba giá trị có thể có:

- `0` cho biết trạng thái hiện tại chưa được tìm kiếm;
- `-1`: cho biết không thể chia trạng thái hiện tại thành $k$ tập con;
- `1`: cho biết có thể chia trạng thái hiện tại thành $k$ tập con.

Độ phức tạp thời gian là $O(n \times 2^n)$ và độ phức tạp không gian là $O(2^n)$. Ở đây, $n$ là độ dài mảng $\textit{nums}$. Với mỗi trạng thái, ta cần duyệt mảng $\textit{nums}$, mất $O(n)$ thời gian. Có tổng cộng $2^n$ trạng thái nên độ phức tạp thời gian là $O(n \times 2^n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
        @cache
        def dfs(state, t):
            if state == mask:
                return True
            for i, v in enumerate(nums):
                if (state >> i) & 1:
                    continue
                if t + v > s:
                    break
                if dfs(state | 1 << i, (t + v) % s):
                    return True
            return False

        s, mod = divmod(sum(nums), k)
        if mod:
            return False
        nums.sort()
        mask = (1 << len(nums)) - 1
        return dfs(0, 0)
```

#### Java

```java
class Solution {
    private int[] f;
    private int[] nums;
    private int n;
    private int s;

    public boolean canPartitionKSubsets(int[] nums, int k) {
        for (int v : nums) {
            s += v;
        }
        if (s % k != 0) {
            return false;
        }
        s /= k;
        Arrays.sort(nums);
        this.nums = nums;
        n = nums.length;
        f = new int[1 << n];
        return dfs(0, 0);
    }

    private boolean dfs(int state, int t) {
        if (state == (1 << n) - 1) {
            return true;
        }
        if (f[state] != 0) {
            return f[state] == 1;
        }
        for (int i = 0; i < n; ++i) {
            if (((state >> i) & 1) == 1) {
                continue;
            }
            if (t + nums[i] > s) {
                break;
            }
            if (dfs(state | 1 << i, (t + nums[i]) % s)) {
                f[state] = 1;
                return true;
            }
        }
        f[state] = -1;
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int s = accumulate(nums.begin(), nums.end(), 0);
        if (s % k) {
            return false;
        }
        s /= k;
        sort(nums.begin(), nums.end());
        int n = nums.size();
        int mask = (1 << n) - 1;
        vector<int> f(1 << n);
        function<bool(int, int)> dfs = [&](int state, int t) {
            if (state == mask) {
                return true;
            }
            if (f[state]) {
                return f[state] == 1;
            }
            for (int i = 0; i < n; ++i) {
                if (state >> i & 1) {
                    continue;
                }
                if (t + nums[i] > s) {
                    break;
                }
                if (dfs(state | 1 << i, (t + nums[i]) % s)) {
                    f[state] = 1;
                    return true;
                }
            }
            f[state] = -1;
            return false;
        };
        return dfs(0, 0);
    }
};
```

#### Go

```go
func canPartitionKSubsets(nums []int, k int) bool {
	s := 0
	for _, v := range nums {
		s += v
	}
	if s%k != 0 {
		return false
	}
	s /= k
	n := len(nums)
	f := make([]int, 1<<n)
	mask := (1 << n) - 1

	var dfs func(int, int) bool
	dfs = func(state, t int) bool {
		if state == mask {
			return true
		}
		if f[state] != 0 {
			return f[state] == 1
		}
		for i, v := range nums {
			if (state >> i & 1) == 1 {
				continue
			}
			if t+v > s {
				break
			}
			if dfs(state|1<<i, (t+v)%s) {
				f[state] = 1
				return true
			}
		}
		f[state] = -1
		return false
	}

	sort.Ints(nums)
	return dfs(0, 0)
}
```

#### TypeScript

```ts
function canPartitionKSubsets(nums: number[], k: number): boolean {
    let s = nums.reduce((a, b) => a + b, 0);
    if (s % k !== 0) {
        return false;
    }
    s = Math.floor(s / k);
    nums.sort((a, b) => a - b);
    const n = nums.length;
    const mask = (1 << n) - 1;
    const f = Array(1 << n).fill(0);

    const dfs = (state: number, t: number): boolean => {
        if (state === mask) {
            return true;
        }
        if (f[state] !== 0) {
            return f[state] === 1;
        }
        for (let i = 0; i < n; ++i) {
            if ((state >> i) & 1) {
                continue;
            }
            if (t + nums[i] > s) {
                break;
            }
            if (dfs(state | (1 << i), (t + nums[i]) % s)) {
                f[state] = 1;
                return true;
            }
        }
        f[state] = -1;
        return false;
    };

    return dfs(0, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Memoization là cách tiếp cận từ trên xuống. Theo hướng từ dưới lên, $f[i]$ cho biết mask $i$ có thể đạt được hay không, còn $cur[i]$ là tổng hiện tại của tập con đang điền. Thử thêm từng phần tử $j$ chưa dùng để cập nhật $f[i|2^j]$ mà không cần đệ quy.

<!-- thinking:end -->

Ta có thể dùng quy hoạch động để giải bài toán này.

Định nghĩa $f[i]$ cho biết có thể chia các số đã chọn ở trạng thái $i$ thành $k$ tập con thỏa mãn yêu cầu hay không. Ban đầu, $f[0] = \text{true}$; đáp án là $f[2^n - 1]$, trong đó $n$ là độ dài mảng $\textit{nums}$. Ngoài ra, $cur[i]$ lưu tổng của tập con cuối cùng ở trạng thái chọn số $i$.

Duyệt các trạng thái $i$ trong đoạn $[0, 2^n]$. Với mỗi trạng thái, nếu $f[i]$ là $\text{false}$ thì bỏ qua. Nếu không, lần lượt xét từng số $\textit{nums}[j]$ trong mảng. Nếu $cur[i] + \textit{nums}[j] > s$, thoát vòng lặp vì các số phía sau lớn hơn nên cũng không thể thêm vào tập con hiện tại. Nếu bit thứ $j$ trong biểu diễn nhị phân của $i$ bằng $0$, nghĩa là $\textit{nums}[j]$ chưa được chọn. Ta có thể thêm số này vào tập hiện tại, chuyển trạng thái thành $i | 2^j$, cập nhật $cur[i | 2^j] = (cur[i] + \textit{nums}[j]) \bmod s$ và đặt $f[i | 2^j] = \text{true}$.

Cuối cùng, trả về $f[2^n - 1]$.

Độ phức tạp thời gian là $O(n \times 2^n)$ và độ phức tạp không gian là $O(2^n)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canPartitionKSubsets(self, nums: List[int], k: int) -> bool:
        s = sum(nums)
        if s % k:
            return False
        s //= k
        nums.sort()
        n = len(nums)
        f = [False] * (1 << n)
        cur = [0] * (1 << n)
        f[0] = True
        for i in range(1 << n):
            if not f[i]:
                continue
            for j in range(n):
                if cur[i] + nums[j] > s:
                    break
                if (i >> j & 1) == 0:
                    if not f[i | 1 << j]:
                        cur[i | 1 << j] = (cur[i] + nums[j]) % s
                        f[i | 1 << j] = True
        return f[-1]
```

#### Java

```java
class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {
        int s = 0;
        for (int x : nums) {
            s += x;
        }
        if (s % k != 0) {
            return false;
        }
        s /= k;
        Arrays.sort(nums);
        int n = nums.length;
        boolean[] f = new boolean[1 << n];
        f[0] = true;
        int[] cur = new int[1 << n];
        for (int i = 0; i < 1 << n; ++i) {
            if (!f[i]) {
                continue;
            }
            for (int j = 0; j < n; ++j) {
                if (cur[i] + nums[j] > s) {
                    break;
                }
                if ((i >> j & 1) == 0) {
                    cur[i | 1 << j] = (cur[i] + nums[j]) % s;
                    f[i | 1 << j] = true;
                }
            }
        }
        return f[(1 << n) - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int s = accumulate(nums.begin(), nums.end(), 0);
        if (s % k) {
            return false;
        }
        s /= k;
        sort(nums.begin(), nums.end());
        int n = nums.size();
        bool f[1 << n];
        int cur[1 << n];
        memset(f, false, sizeof(f));
        memset(cur, 0, sizeof(cur));
        f[0] = 1;
        for (int i = 0; i < 1 << n; ++i) {
            if (!f[i]) {
                continue;
            }
            for (int j = 0; j < n; ++j) {
                if (cur[i] + nums[j] > s) {
                    break;
                }
                if ((i >> j & 1) == 0) {
                    f[i | 1 << j] = true;
                    cur[i | 1 << j] = (cur[i] + nums[j]) % s;
                }
            }
        }
        return f[(1 << n) - 1];
    }
};
```

#### Go

```go
func canPartitionKSubsets(nums []int, k int) bool {
	s := 0
	for _, x := range nums {
		s += x
	}
	if s%k != 0 {
		return false
	}
	s /= k
	sort.Ints(nums)
	n := len(nums)
	f := make([]bool, 1<<n)
	cur := make([]int, 1<<n)
	f[0] = true
	for i := 0; i < 1<<n; i++ {
		if !f[i] {
			continue
		}
		for j := 0; j < n; j++ {
			if cur[i]+nums[j] > s {
				break
			}
			if i>>j&1 == 0 {
				f[i|1<<j] = true
				cur[i|1<<j] = (cur[i] + nums[j]) % s
			}
		}
	}
	return f[(1<<n)-1]
}
```

#### TypeScript

```ts
function canPartitionKSubsets(nums: number[], k: number): boolean {
    let s = nums.reduce((a, b) => a + b);
    if (s % k !== 0) {
        return false;
    }
    s /= k;
    nums.sort((a, b) => a - b);
    const n = nums.length;
    const f: boolean[] = Array(1 << n).fill(false);
    f[0] = true;
    const cur: number[] = Array(n).fill(0);
    for (let i = 0; i < 1 << n; ++i) {
        if (!f[i]) {
            continue;
        }
        for (let j = 0; j < n; ++j) {
            if (cur[i] + nums[j] > s) {
                break;
            }
            if (((i >> j) & 1) === 0) {
                f[i | (1 << j)] = true;
                cur[i | (1 << j)] = (cur[i] + nums[j]) % s;
            }
        }
    }
    return f[(1 << n) - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
