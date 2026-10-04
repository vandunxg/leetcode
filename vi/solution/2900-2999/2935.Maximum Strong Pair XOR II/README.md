---
comments: true
difficulty: Hard
rating: 2348
source: Weekly Contest 371 Q4
tags:
    - Bit Manipulation
    - Trie
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [2935. Maximum Strong Pair XOR II](https://leetcode.com/problems/maximum-strong-pair-xor-ii)

[中文文档](/solution/2900-2999/2935.Maximum%20Strong%20Pair%20XOR%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Một cặp số nguyên <code>x</code> và <code>y</code> được gọi là cặp <strong>mạnh</strong> nếu thỏa mãn điều kiện:</p>

<ul>
	<li><code>|x - y| &lt;= min(x, y)</code></li>
</ul>

<p>Hãy chọn hai số nguyên từ <code>nums</code> sao cho chúng tạo thành một cặp mạnh và phép <code>XOR</code> theo bit của chúng là <strong>lớn nhất</strong> trong tất cả các cặp mạnh của mảng.</p>

<p>Trả về <em>giá trị <strong>lớn nhất</strong> </em><code>XOR</code><em> trong tất cả các cặp mạnh có thể tạo thành từ mảng</em> <code>nums</code>.</p>

<p><strong>Lưu ý</strong> rằng bạn có thể chọn cùng một số nguyên hai lần để tạo thành một cặp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3,4,5]
<strong>Output:</strong> 7
<strong>Giải thích:</strong> Có 11 cặp mạnh trong mảng <code>nums</code>: (1, 1), (1, 2), (2, 2), (2, 3), (2, 4), (3, 3), (3, 4), (3, 5), (4, 4), (4, 5) và (5, 5).
Giá trị XOR lớn nhất có thể tạo từ các cặp này là 3 XOR 4 = 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [10,100]
<strong>Output:</strong> 0
<strong>Giải thích:</strong> Có 2 cặp mạnh trong mảng nums: (10, 10) và (100, 100).
Giá trị XOR lớn nhất có thể tạo từ các cặp này là 10 XOR 10 = 0, vì cặp (100, 100) cũng cho 100 XOR 100 = 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [500,520,2500,3000]
<strong>Output:</strong> 1020
<strong>Giải thích:</strong> Có 6 cặp mạnh trong mảng nums: (500, 500), (500, 520), (520, 520), (2500, 2500), (2500, 3000) và (3000, 3000).
Giá trị XOR lớn nhất có thể tạo từ các cặp này là 500 XOR 520 = 1020, vì giá trị XOR khác 0 duy nhất còn lại là 2500 XOR 3000 = 636.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 2<sup>20</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Binary Trie

<!-- thinking:start -->

> **Tư duy**
>
> Đây chính là phương pháp 2 của phần I, với $n \le 5 \times 10^4$ và các giá trị nhỏ hơn $2^{20}$. Không thể liệt kê tất cả các cặp. Với $x \le y$, một cặp mạnh thỏa mãn $y \le 2x$; vì vậy ta sắp xếp mảng và duy trì cửa sổ này bằng hai con trỏ.
>
> Trie 0-1 chọn bit đối lập để tối đa hóa XOR và dùng $cnt$ để xóa phần tử ở đầu cửa sổ. Có 21 bit nên độ phức tạp là $O(n \log A)$.

<!-- thinking:end -->

Từ bất đẳng thức $|x - y| \leq \min(x, y)$, trong đó có giá trị tuyệt đối và giá trị nhỏ nhất, ta có thể giả sử $x \leq y$. Khi đó, ta có $y - x \leq x$, hay $y \leq 2x$. Ta có thể duyệt $y$ từ nhỏ đến lớn, khi đó $x$ phải thỏa mãn bất đẳng thức $y \leq 2x$.

Vì vậy, ta sắp xếp mảng $nums$, sau đó duyệt $y$ từ nhỏ đến lớn. Ta dùng hai con trỏ để duy trì một cửa sổ sao cho các phần tử $x$ trong cửa sổ thỏa mãn bất đẳng thức $y \leq 2x$. Ta có thể dùng binary trie để duy trì các phần tử trong cửa sổ, nhờ đó tìm được giá trị XOR lớn nhất trong cửa sổ trong thời gian $O(1)$. Mỗi lần ta thêm $y$ vào trie và xóa các phần tử ở đầu cửa sổ không thỏa mãn bất đẳng thức, qua đó bảo đảm các phần tử trong cửa sổ đều thỏa mãn $y \leq 2x$. Sau đó, ta truy vấn giá trị XOR lớn nhất từ trie và cập nhật đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$, còn độ phức tạp không gian là $O(n \times \log M)$. Trong đó, $n$ là độ dài của mảng $nums$, còn $M$ là giá trị lớn nhất trong mảng $nums$. Trong bài toán này, $M = 2^{20}$.

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
        for i in range(20, -1, -1):
            v = x >> i & 1
            if node.children[v] is None:
                node.children[v] = Trie()
            node = node.children[v]
            node.cnt += 1

    def search(self, x: int) -> int:
        node = self
        ans = 0
        for i in range(20, -1, -1):
            v = x >> i & 1
            if node.children[v ^ 1] and node.children[v ^ 1].cnt:
                ans |= 1 << i
                node = node.children[v ^ 1]
            else:
                node = node.children[v]
        return ans

    def remove(self, x: int):
        node = self
        for i in range(20, -1, -1):
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
        for (int i = 20; i >= 0; --i) {
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
        for (int i = 20; i >= 0; --i) {
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
        for (int i = 20; i >= 0; --i) {
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
        for (int i = 20; ~i; --i) {
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
        for (int i = 20; ~i; --i) {
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
        for (int i = 20; ~i; --i) {
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
	for i := 20; i >= 0; i-- {
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
	for i := 20; i >= 0; i-- {
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
	for i := 20; i >= 0; i-- {
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
        for (let i = 20; i >= 0; i--) {
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
        for (let i = 20; i >= 0; i--) {
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
        for (let i = 20; i >= 0; i--) {
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
