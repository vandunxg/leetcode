---
comments: true
difficulty: Medium
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Dynamic Programming
    - Longest Increasing Subsequence
---

<!-- problem:start -->

# [673. Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence)

[中文文档](/solution/0600-0699/0673.Number%20of%20Longest%20Increasing%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về <em>số lượng dãy con tăng dài nhất.</em></p>

<p><strong>Lưu ý</strong>, dãy phải tăng <strong>nghiêm ngặt</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,5,4,7]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Hai dãy con tăng dài nhất là [1, 3, 4, 7] và [1, 3, 5, 7].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,2,2,2]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Dãy con tăng dài nhất có độ dài 1 và có 5 dãy con tăng độ dài 1, nên kết quả là 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2000</code></li>
	<li><code>-10<sup>6</sup> &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
	<li>Kết quả được đảm bảo nằm trong phạm vi số nguyên 32-bit.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tính cả độ dài LIS lẫn số dãy đạt độ dài đó. Với $n\le 2000$, độ phức tạp $O(n^2)$ là khả thi.
>
> $f[i]$ và $cnt[i]$ lần lượt là độ dài lớn nhất và số dãy kết thúc tại $i$. Khi $nums[j]<nums[i]$, ta cập nhật độ dài hoặc cộng thêm số cách. Cộng $cnt$ tại các vị trí đạt độ dài lớn nhất toàn cục.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là độ dài của dãy con tăng dài nhất kết thúc tại $nums[i]$, còn $cnt[i]$ là số dãy con tăng dài nhất kết thúc tại $nums[i]$. Ban đầu, $f[i]=1$, $cnt[i]=1$. Ta cũng dùng $mx$ để lưu độ dài dãy con tăng dài nhất và $ans$ để lưu số dãy con tăng dài nhất.

Với mỗi $nums[i]$, ta duyệt tất cả phần tử $nums[j]$ trong $nums[0:i-1]$. Nếu $nums[j] < nums[i]$, có thể nối $nums[i]$ sau $nums[j]$ để tạo dãy con tăng dài hơn. Nếu $f[i] < f[j] + 1$, độ dài dãy con tăng dài nhất kết thúc tại $nums[i]$ đã tăng, nên ta cập nhật $f[i]=f[j]+1$ và $cnt[i]=cnt[j]$. Nếu $f[i]=f[j]+1$, ta tìm được một dãy con tăng có cùng độ dài dài nhất hiện tại, nên cộng $cnt[j]$ vào $cnt[i]$. Tiếp đó, nếu $mx < f[i]$, độ dài dãy con tăng dài nhất đã tăng, nên cập nhật $mx=f[i]$ và $ans=cnt[i]$. Nếu $mx=f[i]$, ta tìm được dãy con tăng dài nhất có cùng độ dài với mức hiện tại, nên cộng $cnt[i]$ vào $ans$.

Cuối cùng, ta trả về $ans$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findNumberOfLIS(self, nums: List[int]) -> int:
        n = len(nums)
        f = [1] * n
        cnt = [1] * n
        mx = 0
        for i in range(n):
            for j in range(i):
                if nums[j] < nums[i]:
                    if f[i] < f[j] + 1:
                        f[i] = f[j] + 1
                        cnt[i] = cnt[j]
                    elif f[i] == f[j] + 1:
                        cnt[i] += cnt[j]
            if mx < f[i]:
                mx = f[i]
                ans = cnt[i]
            elif mx == f[i]:
                ans += cnt[i]
        return ans
```

#### Java

```java
class Solution {
    public int findNumberOfLIS(int[] nums) {
        int n = nums.length;
        int[] f = new int[n];
        int[] cnt = new int[n];
        int mx = 0, ans = 0;
        for (int i = 0; i < n; ++i) {
            f[i] = 1;
            cnt[i] = 1;
            for (int j = 0; j < i; ++j) {
                if (nums[j] < nums[i]) {
                    if (f[i] < f[j] + 1) {
                        f[i] = f[j] + 1;
                        cnt[i] = cnt[j];
                    } else if (f[i] == f[j] + 1) {
                        cnt[i] += cnt[j];
                    }
                }
            }
            if (mx < f[i]) {
                mx = f[i];
                ans = cnt[i];
            } else if (mx == f[i]) {
                ans += cnt[i];
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
    int findNumberOfLIS(vector<int>& nums) {
        int n = nums.size();
        int mx = 0, ans = 0;
        vector<int> f(n, 1);
        vector<int> cnt(n, 1);
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[j] < nums[i]) {
                    if (f[i] < f[j] + 1) {
                        f[i] = f[j] + 1;
                        cnt[i] = cnt[j];
                    } else if (f[i] == f[j] + 1) {
                        cnt[i] += cnt[j];
                    }
                }
            }
            if (mx < f[i]) {
                mx = f[i];
                ans = cnt[i];
            } else if (mx == f[i]) {
                ans += cnt[i];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findNumberOfLIS(nums []int) (ans int) {
	n, mx := len(nums), 0
	f := make([]int, n)
	cnt := make([]int, n)
	for i, x := range nums {
		for j, y := range nums[:i] {
			if y < x {
				if f[i] < f[j]+1 {
					f[i] = f[j] + 1
					cnt[i] = cnt[j]
				} else if f[i] == f[j]+1 {
					cnt[i] += cnt[j]
				}
			}
		}
		if mx < f[i] {
			mx = f[i]
			ans = cnt[i]
		} else if mx == f[i] {
			ans += cnt[i]
		}
	}
	return
}
```

#### TypeScript

```ts
function findNumberOfLIS(nums: number[]): number {
    const n = nums.length;
    let [ans, mx] = [0, 0];
    const f: number[] = Array(n).fill(1);
    const cnt: number[] = Array(n).fill(1);
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            if (nums[j] < nums[i]) {
                if (f[i] < f[j] + 1) {
                    f[i] = f[j] + 1;
                    cnt[i] = cnt[j];
                } else if (f[i] === f[j] + 1) {
                    cnt[i] += cnt[j];
                }
            }
        }
        if (mx < f[i]) {
            mx = f[i];
            ans = cnt[i];
        } else if (mx === f[i]) {
            ans += cnt[i];
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_number_of_lis(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut ans = 0;
        let mut mx = 0;
        let mut f = vec![1; n];
        let mut cnt = vec![1; n];
        for i in 0..n {
            for j in 0..i {
                if nums[j] < nums[i] {
                    if f[i] < f[j] + 1 {
                        f[i] = f[j] + 1;
                        cnt[i] = cnt[j];
                    } else if f[i] == f[j] + 1 {
                        cnt[i] += cnt[j];
                    }
                }
            }
            if mx < f[i] {
                mx = f[i];
                ans = cnt[i];
            } else if mx == f[i] {
                ans += cnt[i];
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt mọi chỉ số trước đó là nút thắt hiệu năng. Sau khi nén giá trị, Fenwick tree lưu độ dài lớn nhất và số dãy tương ứng trong các khóa nhỏ hơn, giúp mỗi bước chỉ mất $O(\log n)$.

<!-- thinking:end -->

Ta có thể dùng binary indexed tree để duy trì độ dài và số lượng dãy con tăng dài nhất trong một prefix. Ta loại phần tử trùng khỏi mảng $nums$ rồi sắp xếp để tạo mảng $arr$. Sau đó, với mỗi phần tử $x$ trong $nums$, dùng binary search tìm vị trí $i$ của $x$ trong $arr$, rồi query độ dài và số lượng dãy con tăng dài nhất trong đoạn $[1,i-1]$, lần lượt ký hiệu là $v$ và $cnt$. Tiếp theo, cập nhật độ dài và số lượng dãy con tăng dài nhất tại $[i]$ thành $v+1$ và $\max(cnt,1)$. Cuối cùng, query độ dài và số lượng dãy con tăng dài nhất trong $[1,m]$, với $m$ là độ dài mảng $arr$; kết quả query này là đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    __slots__ = ["n", "c", "d"]

    def __init__(self, n):
        self.n = n
        self.c = [0] * (n + 1)
        self.d = [0] * (n + 1)

    def update(self, x, v, cnt):
        while x <= self.n:
            if self.c[x] < v:
                self.c[x] = v
                self.d[x] = cnt
            elif self.c[x] == v:
                self.d[x] += cnt
            x += x & -x

    def query(self, x):
        v = cnt = 0
        while x:
            if self.c[x] > v:
                v = self.c[x]
                cnt = self.d[x]
            elif self.c[x] == v:
                cnt += self.d[x]
            x -= x & -x
        return v, cnt


class Solution:
    def findNumberOfLIS(self, nums: List[int]) -> int:
        arr = sorted(set(nums))
        m = len(arr)
        tree = BinaryIndexedTree(m)
        for x in nums:
            i = bisect_left(arr, x) + 1
            v, cnt = tree.query(i - 1)
            tree.update(i, v + 1, max(cnt, 1))
        return tree.query(m)[1]
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;
    private int[] d;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
        d = new int[n + 1];
    }

    public void update(int x, int v, int cnt) {
        while (x <= n) {
            if (c[x] < v) {
                c[x] = v;
                d[x] = cnt;
            } else if (c[x] == v) {
                d[x] += cnt;
            }
            x += x & -x;
        }
    }

    public int[] query(int x) {
        int v = 0, cnt = 0;
        while (x > 0) {
            if (c[x] > v) {
                v = c[x];
                cnt = d[x];
            } else if (c[x] == v) {
                cnt += d[x];
            }
            x -= x & -x;
        }
        return new int[] {v, cnt};
    }
}

public class Solution {
    public int findNumberOfLIS(int[] nums) {
        int[] arr = Arrays.stream(nums).distinct().sorted().toArray();
        int m = arr.length;
        BinaryIndexedTree tree = new BinaryIndexedTree(m);
        for (int x : nums) {
            int i = Arrays.binarySearch(arr, x) + 1;
            int[] t = tree.query(i - 1);
            int v = t[0];
            int cnt = t[1];
            tree.update(i, v + 1, Math.max(cnt, 1));
        }
        return tree.query(m)[1];
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
private:
    int n;
    vector<int> c;
    vector<int> d;

public:
    BinaryIndexedTree(int n)
        : n(n)
        , c(n + 1, 0)
        , d(n + 1, 0) {}

    void update(int x, int v, int cnt) {
        while (x <= n) {
            if (c[x] < v) {
                c[x] = v;
                d[x] = cnt;
            } else if (c[x] == v) {
                d[x] += cnt;
            }
            x += x & -x;
        }
    }

    pair<int, int> query(int x) {
        int v = 0, cnt = 0;
        while (x > 0) {
            if (c[x] > v) {
                v = c[x];
                cnt = d[x];
            } else if (c[x] == v) {
                cnt += d[x];
            }
            x -= x & -x;
        }
        return {v, cnt};
    }
};

class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        vector<int> arr = nums;
        sort(arr.begin(), arr.end());
        arr.erase(unique(arr.begin(), arr.end()), arr.end());
        int m = arr.size();
        BinaryIndexedTree tree(m);
        for (int x : nums) {
            auto it = lower_bound(arr.begin(), arr.end(), x);
            int i = distance(arr.begin(), it) + 1;
            auto [v, cnt] = tree.query(i - 1);
            tree.update(i, v + 1, max(cnt, 1));
        }
        return tree.query(m).second;
    }
};
```

#### Go

```go
type BinaryIndexedTree struct {
	n int
	c []int
	d []int
}

func newBinaryIndexedTree(n int) BinaryIndexedTree {
	return BinaryIndexedTree{
		n: n,
		c: make([]int, n+1),
		d: make([]int, n+1),
	}
}

func (bit *BinaryIndexedTree) update(x, v, cnt int) {
	for x <= bit.n {
		if bit.c[x] < v {
			bit.c[x] = v
			bit.d[x] = cnt
		} else if bit.c[x] == v {
			bit.d[x] += cnt
		}
		x += x & -x
	}
}

func (bit *BinaryIndexedTree) query(x int) (int, int) {
	v, cnt := 0, 0
	for x > 0 {
		if bit.c[x] > v {
			v = bit.c[x]
			cnt = bit.d[x]
		} else if bit.c[x] == v {
			cnt += bit.d[x]
		}
		x -= x & -x
	}
	return v, cnt
}

func findNumberOfLIS(nums []int) int {
	arr := append([]int(nil), nums...)
	sort.Ints(arr)
	k := 0
	for _, x := range arr {
		if k == 0 || arr[k-1] != x {
			arr[k] = x
			k++
		}
	}
	arr = arr[:k]
	m := len(arr)
	tree := newBinaryIndexedTree(m)
	for _, x := range nums {
		i := sort.SearchInts(arr, x) + 1
		v, cnt := tree.query(i - 1)
		tree.update(i, v+1, max(cnt, 1))
	}
	_, ans := tree.query(m)
	return ans
}
```

#### TypeScript

```ts
class BinaryIndexedTree {
    private n: number;
    private c: number[];
    private d: number[];

    constructor(n: number) {
        this.n = n;
        this.c = Array(n + 1).fill(0);
        this.d = Array(n + 1).fill(0);
    }

    update(x: number, v: number, cnt: number): void {
        while (x <= this.n) {
            if (this.c[x] < v) {
                this.c[x] = v;
                this.d[x] = cnt;
            } else if (this.c[x] === v) {
                this.d[x] += cnt;
            }
            x += x & -x;
        }
    }

    query(x: number): [number, number] {
        let v = 0;
        let cnt = 0;
        while (x > 0) {
            if (this.c[x] > v) {
                v = this.c[x];
                cnt = this.d[x];
            } else if (this.c[x] === v) {
                cnt += this.d[x];
            }
            x -= x & -x;
        }
        return [v, cnt];
    }
}

function findNumberOfLIS(nums: number[]): number {
    const arr: number[] = [...new Set(nums)].sort((a, b) => a - b);
    const m: number = arr.length;
    const tree: BinaryIndexedTree = new BinaryIndexedTree(m);
    const search = (x: number): number => {
        let l = 0,
            r = arr.length;
        while (l < r) {
            const mid = (l + r) >> 1;
            if (arr[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l + 1;
    };
    for (const x of nums) {
        const i: number = search(x);
        const [v, cnt]: [number, number] = tree.query(i - 1);
        tree.update(i, v + 1, Math.max(cnt, 1));
    }
    return tree.query(m)[1];
}
```

#### Rust

```rust
struct BinaryIndexedTree {
    n: usize,
    c: Vec<i32>,
    d: Vec<i32>,
}

impl BinaryIndexedTree {
    fn new(n: usize) -> BinaryIndexedTree {
        BinaryIndexedTree {
            n,
            c: vec![0; n + 1],
            d: vec![0; n + 1],
        }
    }

    fn update(&mut self, x: usize, v: i32, cnt: i32) {
        let mut x = x as usize;
        while x <= self.n {
            if self.c[x] < v {
                self.c[x] = v;
                self.d[x] = cnt;
            } else if self.c[x] == v {
                self.d[x] += cnt;
            }
            x += x & x.wrapping_neg();
        }
    }

    fn query(&self, mut x: usize) -> (i32, i32) {
        let (mut v, mut cnt) = (0, 0);
        while x > 0 {
            if self.c[x] > v {
                v = self.c[x];
                cnt = self.d[x];
            } else if self.c[x] == v {
                cnt += self.d[x];
            }
            x -= x & x.wrapping_neg();
        }
        (v, cnt)
    }
}

impl Solution {
    pub fn find_number_of_lis(nums: Vec<i32>) -> i32 {
        let mut arr: Vec<i32> = nums.iter().cloned().collect();
        arr.sort();
        arr.dedup();
        let m = arr.len();
        let mut tree = BinaryIndexedTree::new(m);
        for x in nums.iter() {
            if let Ok(i) = arr.binary_search(x) {
                let (v, cnt) = tree.query(i);
                tree.update(i + 1, v + 1, cnt.max(1));
            }
        }
        let (_, ans) = tree.query(m);
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
