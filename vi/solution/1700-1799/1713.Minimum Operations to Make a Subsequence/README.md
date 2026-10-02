---
comments: true
difficulty: Hard
rating: 2350
source: Weekly Contest 222 Q4
tags:
    - Greedy
    - Array
    - Hash Table
    - Binary Search
    - Longest Increasing Subsequence
---

<!-- problem:start -->

# [1713. Minimum Operations to Make a Subsequence](https://leetcode.com/problems/minimum-operations-to-make-a-subsequence)

[中文文档](/solution/1700-1799/1713.Minimum%20Operations%20to%20Make%20a%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>target</code> gồm các số nguyên <strong>phân biệt</strong> và một mảng số nguyên khác <code>arr</code>, mảng này <strong>có thể</strong> chứa phần tử trùng lặp.</p>

<p>Trong một thao tác, bạn có thể chèn một số nguyên bất kỳ vào bất kỳ vị trí nào trong <code>arr</code>. Ví dụ, nếu <code>arr = [1,4,1,2]</code>, bạn có thể thêm <code>3</code> vào giữa để được <code>[1,4,<u>3</u>,1,2]</code>. Lưu ý rằng có thể chèn số nguyên vào đầu hoặc cuối mảng.</p>

<p>Trả về <em>số thao tác <strong>ít nhất</strong> cần thực hiện để </em><code>target</code><em> trở thành một <strong>mảng con</strong> của </em><code>arr</code><em>.</em></p>

<p><strong>Mảng con</strong> của một mảng là mảng mới tạo từ mảng ban đầu bằng cách xóa một số phần tử (có thể không xóa phần tử nào) mà không thay đổi thứ tự tương đối của các phần tử còn lại. Ví dụ, <code>[2,7,4]</code> là mảng con của <code>[4,<u>2</u>,3,<u>7</u>,2,1,<u>4</u>]</code> (các phần tử được gạch chân), còn <code>[2,4,2]</code> thì không.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> target = [5,1,3], <code>arr</code> = [9,4,2,3,4]
<strong>Output:</strong> 2
<strong>Explanation:</strong> Bạn có thể thêm 5 và 1 để được <code>arr</code> = [<u>5</u>,9,4,<u>1</u>,2,3,4], khi đó target sẽ là mảng con của <code>arr</code>.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> target = [6,4,8,1,3,2], <code>arr</code> = [4,7,6,2,3,8,6,1]
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= target.length, arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= target[i], arr[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>target</code> contains no duplicates.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Longest Increasing Subsequence + Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Số lần chèn ít nhất bằng $|\textit{target}|$ trừ độ dài LCS. LCS thông thường tốn $O(mn)$, trong khi cả hai mảng có thể dài tới $10^5$.
>
> Các giá trị trong $\textit{target}$ phân biệt, nên các phần tử của $arr$ xuất hiện trong $\textit{target}$ có thể ánh xạ thành chỉ số. Khi đó LCS trở thành LIS của dãy chỉ số này.
>
> Fenwick tree lưu độ dài LIS tốt nhất kết thúc trước chỉ số hiện tại: truy vấn $x-1$, rồi cập nhật $x$. Kết quả là $m$ trừ độ dài LIS đó.

<!-- thinking:end -->

Theo đề bài, mảng con chung giữa `target` và `arr` càng dài thì càng ít phần tử cần thêm. Vì vậy, số phần tử cần thêm ít nhất bằng độ dài `target` trừ độ dài mảng con chung dài nhất giữa `target` và `arr`.

Tuy nhiên, độ phức tạp của việc [tìm mảng con chung dài nhất](https://github.com/doocs/leetcode/blob/main/solution/1100-1199/1143.Longest%20Common%20Subsequence/README.md) là $O(m \times n)$, không thể đáp ứng bài này. Ta cần thay đổi cách tiếp cận.

Ta có thể dùng hash table để ghi lại chỉ số của từng phần tử trong mảng `target`, rồi duyệt mảng `arr`. Với mỗi phần tử trong `arr`, nếu hash table chứa phần tử đó, ta thêm chỉ số tương ứng vào một mảng. Ta thu được mảng mới `nums`, biểu diễn các chỉ số trong `target` của những phần tử thuộc `arr` (bỏ qua các phần tử không có trong `target`). Độ dài mảng con tăng dài nhất của `nums` chính là độ dài mảng con chung dài nhất giữa `target` và `arr`.

Do đó, bài toán được chuyển thành tìm độ dài mảng con tăng dài nhất của `nums`. Tham khảo [300. Longest Increasing Subsequence](https://github.com/doocs/leetcode/blob/main/solution/0300-0399/0300.Longest%20Increasing%20Subsequence/README.md).

Độ phức tạp thời gian là $O(n \times \log m)$ và độ phức tạp không gian là $O(m)$. Ở đây, $m$ và $n$ lần lượt là độ dài của `target` và `arr`.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    __slots__ = "n", "c"

    def __init__(self, n: int):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x: int, v: int):
        while x <= self.n:
            self.c[x] = max(self.c[x], v)
            x += x & -x

    def query(self, x: int) -> int:
        res = 0
        while x:
            res = max(res, self.c[x])
            x -= x & -x
        return res


class Solution:
    def minOperations(self, target: List[int], arr: List[int]) -> int:
        d = {x: i for i, x in enumerate(target, 1)}
        nums = [d[x] for x in arr if x in d]
        m = len(target)
        tree = BinaryIndexedTree(m)
        ans = 0
        for x in nums:
            v = tree.query(x - 1) + 1
            ans = max(ans, v)
            tree.update(x, v)
        return len(target) - ans
```

#### Java

```java
class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        this.c = new int[n + 1];
    }

    public void update(int x, int v) {
        for (; x <= n; x += x & -x) {
            c[x] = Math.max(c[x], v);
        }
    }

    public int query(int x) {
        int ans = 0;
        for (; x > 0; x -= x & -x) {
            ans = Math.max(ans, c[x]);
        }
        return ans;
    }
}

class Solution {
    public int minOperations(int[] target, int[] arr) {
        int m = target.length;
        Map<Integer, Integer> d = new HashMap<>(m);
        for (int i = 0; i < m; i++) {
            d.put(target[i], i + 1);
        }
        List<Integer> nums = new ArrayList<>();
        for (int x : arr) {
            if (d.containsKey(x)) {
                nums.add(d.get(x));
            }
        }
        BinaryIndexedTree tree = new BinaryIndexedTree(m);
        int ans = 0;
        for (int x : nums) {
            int v = tree.query(x - 1) + 1;
            ans = Math.max(ans, v);
            tree.update(x, v);
        }
        return m - ans;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
private:
    int n;
    vector<int> c;

public:
    BinaryIndexedTree(int n)
        : n(n)
        , c(n + 1) {}

    void update(int x, int v) {
        for (; x <= n; x += x & -x) {
            c[x] = max(c[x], v);
        }
    }

    int query(int x) {
        int ans = 0;
        for (; x > 0; x -= x & -x) {
            ans = max(ans, c[x]);
        }
        return ans;
    }
};

class Solution {
public:
    int minOperations(vector<int>& target, vector<int>& arr) {
        int m = target.size();
        unordered_map<int, int> d;
        for (int i = 0; i < m; ++i) {
            d[target[i]] = i + 1;
        }
        vector<int> nums;
        for (int x : arr) {
            if (d.contains(x)) {
                nums.push_back(d[x]);
            }
        }
        BinaryIndexedTree tree(m);
        int ans = 0;
        for (int x : nums) {
            int v = tree.query(x - 1) + 1;
            ans = max(ans, v);
            tree.update(x, v);
        }
        return m - ans;
    }
};
```

#### Go

```go
type BinaryIndexedTree struct {
	n int
	c []int
}

func NewBinaryIndexedTree(n int) BinaryIndexedTree {
	return BinaryIndexedTree{n: n, c: make([]int, n+1)}
}

func (bit *BinaryIndexedTree) Update(x, v int) {
	for ; x <= bit.n; x += x & -x {
		if v > bit.c[x] {
			bit.c[x] = v
		}
	}
}

func (bit *BinaryIndexedTree) Query(x int) int {
	ans := 0
	for ; x > 0; x -= x & -x {
		if bit.c[x] > ans {
			ans = bit.c[x]
		}
	}
	return ans
}

func minOperations(target []int, arr []int) int {
	m := len(target)
	d := make(map[int]int)
	for i, x := range target {
		d[x] = i + 1
	}
	var nums []int
	for _, x := range arr {
		if pos, exists := d[x]; exists {
			nums = append(nums, pos)
		}
	}
	tree := NewBinaryIndexedTree(m)
	ans := 0
	for _, x := range nums {
		v := tree.Query(x-1) + 1
		if v > ans {
			ans = v
		}
		tree.Update(x, v)
	}
	return m - ans
}
```

#### TypeScript

```ts
class BinaryIndexedTree {
    private n: number;
    private c: number[];

    constructor(n: number) {
        this.n = n;
        this.c = Array(n + 1).fill(0);
    }

    update(x: number, v: number): void {
        for (; x <= this.n; x += x & -x) {
            this.c[x] = Math.max(this.c[x], v);
        }
    }

    query(x: number): number {
        let ans = 0;
        for (; x > 0; x -= x & -x) {
            ans = Math.max(ans, this.c[x]);
        }
        return ans;
    }
}

function minOperations(target: number[], arr: number[]): number {
    const m = target.length;
    const d: Map<number, number> = new Map();
    target.forEach((x, i) => d.set(x, i + 1));
    const nums: number[] = [];
    arr.forEach(x => {
        if (d.has(x)) {
            nums.push(d.get(x)!);
        }
    });
    const tree = new BinaryIndexedTree(m);
    let ans = 0;
    nums.forEach(x => {
        const v = tree.query(x - 1) + 1;
        ans = Math.max(ans, v);
        tree.update(x, v);
    });
    return m - ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
