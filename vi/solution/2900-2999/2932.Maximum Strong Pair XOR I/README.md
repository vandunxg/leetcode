---
comments: true
difficulty: Easy
rating: 1246
source: Weekly Contest 371 Q1
tags:
    - Bit Manipulation
    - Trie
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [2932. Maximum Strong Pair XOR I](https://leetcode.com/problems/maximum-strong-pair-xor-i)

[中文文档](/solution/2900-2999/2932.Maximum%20Strong%20Pair%20XOR%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> <strong>được đánh chỉ số từ 0</strong>. Một cặp số nguyên <code>x</code> và <code>y</code> được gọi là một cặp <strong>mạnh</strong> nếu thỏa mãn điều kiện:</p>

<ul>
	<li><code>|x - y| &lt;= min(x, y)</code></li>
</ul>

<p>Chọn hai số nguyên từ <code>nums</code> sao cho chúng tạo thành một cặp mạnh và phép toán bitwise <code>XOR</code> của chúng là <strong>lớn nhất</strong> trong tất cả các cặp mạnh của mảng.</p>

<p>Trả về <em>giá trị <strong>lớn nhất</strong> </em><code>XOR</code><em> trong tất cả các cặp mạnh có thể tạo thành trong mảng</em> <code>nums</code>.</p>

<p><strong>Lưu ý</strong> rằng có thể chọn cùng một số nguyên hai lần để tạo thành một cặp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Có 11 cặp mạnh trong mảng <code>nums</code>: (1, 1), (1, 2), (2, 2), (2, 3), (2, 4), (3, 3), (3, 4), (3, 5), (4, 4), (4, 5) và (5, 5).
Giá trị XOR lớn nhất có thể của các cặp này là 3 XOR 4 = 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,100]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Có 2 cặp mạnh trong mảng <code>nums</code>: (10, 10) và (100, 100).
Giá trị XOR lớn nhất có thể của các cặp mạnh là 10 XOR 10 = 0, vì cặp (100, 100) cũng cho 100 XOR 100 = 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,6,25,30]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Có 6 cặp mạnh trong mảng <code>nums</code>: (5, 5), (5, 6), (6, 6), (25, 25), (25, 30) và (30, 30).
Giá trị XOR lớn nhất có thể của các cặp mạnh là 25 XOR 30 = 7, vì giá trị XOR khác 0 duy nhất còn lại là 5 XOR 6 = 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp mạnh thỏa mãn $|x-y| \le \min(x,y)$. Vì $n \le 50$, ta có thể thử mọi cặp và giữ lại giá trị XOR lớn nhất trong các cặp hợp lệ.
>
> Một biểu thức lồng nhau sẽ lọc theo điều kiện; không cần thêm cấu trúc dữ liệu nào.

<!-- thinking:end -->

Ta có thể duyệt qua từng cặp số $(x, y)$ trong mảng. Nếu $|x - y| \leq \min(x, y)$ thì đây là một cặp mạnh. Ta tính giá trị XOR của cặp này và cập nhật đáp án.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumStrongPairXor(self, nums: List[int]) -> int:
        return max(x ^ y for x in nums for y in nums if abs(x - y) <= min(x, y))
```

#### Java

```java
class Solution {
    public int maximumStrongPairXor(int[] nums) {
        int ans = 0;
        for (int x : nums) {
            for (int y : nums) {
                if (Math.abs(x - y) <= Math.min(x, y)) {
                    ans = Math.max(ans, x ^ y);
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
    int maximumStrongPairXor(vector<int>& nums) {
        int ans = 0;
        for (int x : nums) {
            for (int y : nums) {
                if (abs(x - y) <= min(x, y)) {
                    ans = max(ans, x ^ y);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximumStrongPairXor(nums []int) (ans int) {
	for _, x := range nums {
		for _, y := range nums {
			if abs(x-y) <= min(x, y) {
				ans = max(ans, x^y)
			}
		}
	}
	return
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
function maximumStrongPairXor(nums: number[]): number {
    let ans = 0;
    for (const x of nums) {
        for (const y of nums) {
            if (Math.abs(x - y) <= Math.min(x, y)) {
                ans = Math.max(ans, x ^ y);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Binary Trie

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đủ dùng khi $n=50$, nhưng phần II tăng $n$ lên $5 \times 10^4$. Khi $x \le y$, bất đẳng thức trở thành $y \le 2x$. Ta sắp xếp, duyệt số lớn hơn $y$ và dùng hai con trỏ để duy trì một cửa sổ các giá trị $x$.
>
> Trie nhị phân giúp tìm XOR lớn nhất với $y$ trong cửa sổ: thêm $y$, loại các $nums[i]$ đã hết hạn, rồi truy vấn. Cùng đoạn code này có thể dùng cho phần II.

<!-- thinking:end -->

Xét bất đẳng thức $|x - y| \leq \min(x, y)$ chứa giá trị tuyệt đối và giá trị nhỏ nhất, ta có thể giả sử $x \leq y$. Khi đó, ta có $y - x \leq x$, hay $y \leq 2x$. Ta có thể duyệt $y$ từ nhỏ đến lớn, khi đó $x$ phải thỏa mãn bất đẳng thức $y \leq 2x$.

Vì vậy, ta sắp xếp mảng $nums$, sau đó duyệt $y$ từ nhỏ đến lớn. Ta dùng hai con trỏ để duy trì một cửa sổ sao cho các phần tử $x$ trong cửa sổ thỏa mãn bất đẳng thức $y \leq 2x$. Ta có thể dùng một binary trie để duy trì các phần tử trong cửa sổ, nhờ đó tìm giá trị XOR lớn nhất trong cửa sổ trong thời gian $O(1)$. Mỗi lần, ta thêm $y$ vào trie và loại các phần tử ở đầu bên trái của cửa sổ không thỏa mãn bất đẳng thức, đảm bảo các phần tử trong cửa sổ thỏa mãn $y \leq 2x$. Sau đó, ta truy vấn giá trị XOR lớn nhất từ trie và cập nhật đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$, và độ phức tạp không gian là $O(n \times \log M)$. Ở đây, $n$ là độ dài của mảng $nums$, còn $M$ là giá trị lớn nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    __slots__ = ("children", "cnt")

    def __init__(self):
        self.children: List[Trie | None] = [None, None]
        self.cnt = 0

    def insert(self, x: int):
        node = self
        for i in range(7, -1, -1):
            v = x >> i & 1
            if node.children[v] is None:
                node.children[v] = Trie()
            node = node.children[v]
            node.cnt += 1

    def search(self, x: int) -> int:
        node = self
        ans = 0
        for i in range(7, -1, -1):
            v = x >> i & 1
            if node.children[v ^ 1] and node.children[v ^ 1].cnt:
                ans |= 1 << i
                node = node.children[v ^ 1]
            else:
                node = node.children[v]
        return ans

    def remove(self, x: int):
        node = self
        for i in range(7, -1, -1):
            v = x >> i & 1
            node = node.children[v]
            node.cnt -= 1


class Solution:
    def maximumStrongPairXor(self, nums: List[int]) -> int:
        nums.sort()
        tree = Trie()
        ans = i = 0
        for y in nums:
            tree.insert(y)
            while y > nums[i] * 2:
                tree.remove(nums[i])
                i += 1
            ans = max(ans, tree.search(y))
        return ans
```

#### Java

```java
class Trie {
    private Trie[] children = new Trie[2];
    private int cnt = 0;

    public Trie() {
    }

    public void insert(int x) {
        Trie node = this;
        for (int i = 7; i >= 0; --i) {
            int v = x >> i & 1;
            if (node.children[v] == null) {
                node.children[v] = new Trie();
            }
            node = node.children[v];
            ++node.cnt;
        }
    }

    public int search(int x) {
        Trie node = this;
        int ans = 0;
        for (int i = 7; i >= 0; --i) {
            int v = x >> i & 1;
            if (node.children[v ^ 1] != null && node.children[v ^ 1].cnt > 0) {
                ans |= 1 << i;
                node = node.children[v ^ 1];
            } else {
                node = node.children[v];
            }
        }
        return ans;
    }

    public void remove(int x) {
        Trie node = this;
        for (int i = 7; i >= 0; --i) {
            int v = x >> i & 1;
            node = node.children[v];
            --node.cnt;
        }
    }
}

class Solution {
    public int maximumStrongPairXor(int[] nums) {
        Arrays.sort(nums);
        Trie tree = new Trie();
        int ans = 0, i = 0;
        for (int y : nums) {
            tree.insert(y);
            while (y > nums[i] * 2) {
                tree.remove(nums[i++]);
            }
            ans = Math.max(ans, tree.search(y));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Trie {
public:
    Trie* children[2];
    int cnt;

    Trie()
        : cnt(0) {
        children[0] = nullptr;
        children[1] = nullptr;
    }

    void insert(int x) {
        Trie* node = this;
        for (int i = 7; ~i; --i) {
            int v = (x >> i) & 1;
            if (node->children[v] == nullptr) {
                node->children[v] = new Trie();
            }
            node = node->children[v];
            ++node->cnt;
        }
    }

    int search(int x) {
        Trie* node = this;
        int ans = 0;
        for (int i = 7; ~i; --i) {
            int v = (x >> i) & 1;
            if (node->children[v ^ 1] != nullptr && node->children[v ^ 1]->cnt > 0) {
                ans |= 1 << i;
                node = node->children[v ^ 1];
            } else {
                node = node->children[v];
            }
        }
        return ans;
    }

    void remove(int x) {
        Trie* node = this;
        for (int i = 7; ~i; --i) {
            int v = (x >> i) & 1;
            node = node->children[v];
            --node->cnt;
        }
    }
};

class Solution {
public:
    int maximumStrongPairXor(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        Trie* tree = new Trie();
        int ans = 0, i = 0;
        for (int y : nums) {
            tree->insert(y);
            while (y > nums[i] * 2) {
                tree->remove(nums[i++]);
            }
            ans = max(ans, tree->search(y));
        }
        return ans;
    }
};
```

#### Go

```go
type Trie struct {
	children [2]*Trie
	cnt      int
}

func newTrie() *Trie {
	return &Trie{}
}

func (t *Trie) insert(x int) {
	node := t
	for i := 7; i >= 0; i-- {
		v := (x >> uint(i)) & 1
		if node.children[v] == nil {
			node.children[v] = newTrie()
		}
		node = node.children[v]
		node.cnt++
	}
}

func (t *Trie) search(x int) int {
	node := t
	ans := 0
	for i := 7; i >= 0; i-- {
		v := (x >> uint(i)) & 1
		if node.children[v^1] != nil && node.children[v^1].cnt > 0 {
			ans |= 1 << uint(i)
			node = node.children[v^1]
		} else {
			node = node.children[v]
		}
	}
	return ans
}

func (t *Trie) remove(x int) {
	node := t
	for i := 7; i >= 0; i-- {
		v := (x >> uint(i)) & 1
		node = node.children[v]
		node.cnt--
	}
}

func maximumStrongPairXor(nums []int) (ans int) {
	sort.Ints(nums)
	tree := newTrie()
	i := 0
	for _, y := range nums {
		tree.insert(y)
		for ; y > nums[i]*2; i++ {
			tree.remove(nums[i])
		}
		ans = max(ans, tree.search(y))
	}
	return ans
}
```

#### TypeScript

```ts
class Trie {
    children: (Trie | null)[];
    cnt: number;

    constructor() {
        this.children = [null, null];
        this.cnt = 0;
    }

    insert(x: number): void {
        let node: Trie | null = this;
        for (let i = 7; i >= 0; i--) {
            const v = (x >> i) & 1;
            if (node.children[v] === null) {
                node.children[v] = new Trie();
            }
            node = node.children[v] as Trie;
            node.cnt++;
        }
    }

    search(x: number): number {
        let node: Trie | null = this;
        let ans = 0;
        for (let i = 7; i >= 0; i--) {
            const v = (x >> i) & 1;
            if (node.children[v ^ 1] !== null && (node.children[v ^ 1] as Trie).cnt > 0) {
                ans |= 1 << i;
                node = node.children[v ^ 1] as Trie;
            } else {
                node = node.children[v] as Trie;
            }
        }
        return ans;
    }

    remove(x: number): void {
        let node: Trie | null = this;
        for (let i = 7; i >= 0; i--) {
            const v = (x >> i) & 1;
            node = node.children[v] as Trie;
            node.cnt--;
        }
    }
}

function maximumStrongPairXor(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const tree = new Trie();
    let ans = 0;
    let i = 0;

    for (const y of nums) {
        tree.insert(y);

        while (y > nums[i] * 2) {
            tree.remove(nums[i++]);
        }

        ans = Math.max(ans, tree.search(y));
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
