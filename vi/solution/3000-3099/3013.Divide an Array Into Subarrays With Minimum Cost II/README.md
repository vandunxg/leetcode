---
comments: true
difficulty: Hard
rating: 2540
source: Biweekly Contest 122 Q4
tags:
    - Array
    - Hash Table
    - Sliding Window
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3013. Divide an Array Into Subarrays With Minimum Cost II](https://leetcode.com/problems/divide-an-array-into-subarrays-with-minimum-cost-ii)

[中文文档](/solution/3000-3099/3013.Divide%20an%20Array%20Into%20Subarrays%20With%20Minimum%20Cost%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code> có độ dài <code>n</code>, cùng hai số nguyên <strong>dương</strong> <code>k</code> và <code>dist</code>.</p>

<p><strong>Chi phí</strong> của một mảng là giá trị của phần tử <strong>đầu tiên</strong> trong mảng đó. Ví dụ, chi phí của <code>[1,2,3]</code> là <code>1</code>, còn chi phí của <code>[3,4,1]</code> là <code>3</code>.</p>

<p>Bạn cần chia <code>nums</code> thành <code>k</code> <span data-keyword="subarray-nonempty">mảng con</span> <strong>liên tiếp và không giao nhau</strong>, sao cho hiệu giữa chỉ số bắt đầu của mảng con <strong>thứ hai</strong> và chỉ số bắt đầu của mảng con thứ <code>kth</code> <strong>nhỏ hơn hoặc bằng</strong> <code>dist</code>. Nói cách khác, nếu chia <code>nums</code> thành các mảng con <code>nums[0..(i<sub>1</sub> - 1)], nums[i<sub>1</sub>..(i<sub>2</sub> - 1)], ..., nums[i<sub>k-1</sub>..(n - 1)]</code>, thì <code>i<sub>k-1</sub> - i<sub>1</sub> &lt;= dist</code>.</p>

<p>Trả về <em>tổng <strong>nhỏ nhất</strong> có thể của chi phí các</em> <em>mảng con</em> này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,2,6,4,2], k = 3, dist = 3
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Cách tốt nhất để chia nums thành 3 mảng con là: [1,3], [2,6,4] và [2]. Lựa chọn này hợp lệ vì i<sub>k-1</sub> - i<sub>1</sub> là 5 - 2 = 3, bằng với dist. Tổng chi phí là nums[0] + nums[2] + nums[5] = 1 + 2 + 2 = 5.
Có thể chứng minh rằng không có cách nào chia nums thành 3 mảng con với chi phí nhỏ hơn 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,1,2,2,2,1], k = 4, dist = 3
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Cách tốt nhất để chia nums thành 4 mảng con là: [10], [1], [2] và [2,2,1]. Lựa chọn này hợp lệ vì hiệu giữa i<sub>k-1</sub> và i<sub>1</sub> là 3 - 1 = 2, nhỏ hơn dist. Tổng chi phí là nums[0] + nums[1] + nums[2] + nums[3] = 10 + 1 + 2 + 2 = 15.
Cách chia [10], [1], [2,2,2] và [1] không hợp lệ, vì hiệu giữa i<sub>k-1</sub> và i<sub>1</sub> là 5 - 1 = 4, lớn hơn dist.
Có thể chứng minh rằng không có cách nào chia nums thành 4 mảng con với chi phí nhỏ hơn 15.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,8,18,9], k = 3, dist = 1
<strong>Đầu ra:</strong> 36
<strong>Giải thích:</strong> Cách tốt nhất để chia nums thành 3 mảng con là: [10], [8] và [18,9]. Lựa chọn này hợp lệ vì i<sub>k-1</sub> - i<sub>1</sub> là 2 - 1 = 1, bằng với dist. Tổng chi phí là nums[0] + nums[1] + nums[2] = 10 + 8 + 18 = 36.
Cách chia [10], [8,18] và [9] không hợp lệ, vì hiệu giữa i<sub>k-1</sub> và i<sub>1</sub> là 3 - 1 = 2, lớn hơn dist.
Có thể chứng minh rằng không có cách nào chia nums thành 3 mảng con với chi phí nhỏ hơn 36.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>3 &lt;= k &lt;= n</code></li>
	<li><code>k - 2 &lt;= dist &lt;= n - 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Không giống phần I, $k$ và $\textit{dist}$ thay đổi, còn $n \le 10^5$. Mảng con đầu tiên vẫn có chi phí là $\textit{nums}[0]$; vị trí bắt đầu của $k-1$ mảng con còn lại phải nằm trong một cửa sổ có độ dài $\textit{dist}+1$.
>
> Với mỗi cửa sổ, ta cần tổng của $k-1$ giá trị nhỏ nhất. Việc sắp xếp lại mọi cửa sổ sẽ quá chậm.
>
> Hai danh sách đã sắp xếp lưu $k-1$ giá trị nhỏ nhất trong cửa sổ và các giá trị còn lại, đồng thời duy trì tổng của các giá trị ở danh sách đầu tiên. Khi cửa sổ trượt, ta chèn phần tử vào danh sách trái hoặc phải theo độ lớn rồi cân bằng lại kích thước.

<!-- thinking:end -->

Bài toán yêu cầu chúng ta chia mảng $\textit{nums}$ thành $k$ mảng con liên tiếp và không giao nhau, sao cho khoảng cách giữa phần tử đầu tiên của mảng con thứ hai và phần tử đầu tiên của mảng con thứ $k$ không vượt quá $\textit{dist}$. Điều này tương đương với việc tìm một mảng con có kích thước $\textit{dist}+1$, bắt đầu từ phần tử ở chỉ số $1$ của $\textit{nums}$, rồi tính tổng của $k-1$ phần tử nhỏ nhất trong đó. Ta giảm $k$ đi $1$, nên chỉ cần tìm tổng của $k$ phần tử nhỏ nhất rồi cộng thêm $\textit{nums}[0]$.

Ta có thể dùng hai ordered set $\textit{l}$ và $\textit{r}$ để duy trì các phần tử trong cửa sổ có kích thước $\textit{dist} + 1$. Set $\textit{l}$ lưu $k$ phần tử nhỏ nhất, còn set $\textit{r}$ lưu các phần tử còn lại trong cửa sổ. Ta duy trì biến $\textit{s}$ biểu diễn tổng của $\textit{nums}[0]$ và các phần tử trong $\textit{l}$. Ban đầu, ta cộng tổng $\textit{dist}+2$ phần tử đầu tiên vào $\textit{s}$ và thêm tất cả phần tử có chỉ số $[1, \textit{dist} + 1]$ vào $\textit{l}$. Nếu kích thước của $\textit{l}$ lớn hơn $k$, ta liên tục chuyển phần tử lớn nhất từ $\textit{l}$ sang $\textit{r}$ cho đến khi kích thước của $\textit{l}$ bằng $k$, đồng thời cập nhật $\textit{s}$.

Lúc này, đáp án ban đầu là $\textit{ans} = \textit{s}$.

Tiếp theo, ta duyệt $\textit{nums}$ bắt đầu từ $\textit{dist}+2$. Với mỗi phần tử $\textit{nums}[i]$, ta cần xóa $\textit{nums}[i-\textit{dist}-1]$ khỏi $\textit{l}$ hoặc $\textit{r}$, sau đó thêm $\textit{nums}[i]$ vào $\textit{l}$ hoặc $\textit{r}$. Nếu $\textit{nums}[i]$ nhỏ hơn phần tử lớn nhất trong $\textit{l}$, ta thêm $\textit{nums}[i]$ vào $\textit{l}$; ngược lại, ta thêm nó vào $\textit{r}$. Nếu kích thước của $\textit{l}$ nhỏ hơn $k$, ta chuyển phần tử nhỏ nhất từ $\textit{r}$ sang $\textit{l}$ cho đến khi kích thước của $\textit{l}$ bằng $k$. Nếu kích thước của $\textit{l}$ lớn hơn $k$, ta chuyển phần tử lớn nhất từ $\textit{l}$ sang $\textit{r}$ cho đến khi kích thước của $\textit{l}$ bằng $k$. Trong quá trình này, ta cập nhật $\textit{s}$ và cập nhật $\textit{ans} = \min(\textit{ans}, \textit{s})$.

Đáp án cuối cùng là $\textit{ans}$.

Độ phức tạp thời gian là $O(n \times \log \textit{dist})$, và độ phức tạp không gian là $O(\textit{dist})$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumCost(self, nums: List[int], k: int, dist: int) -> int:
        def l2r():
            nonlocal s
            x = l.pop()
            s -= x
            r.add(x)

        def r2l():
            nonlocal s
            x = r.pop(0)
            l.add(x)
            s += x

        k -= 1
        s = sum(nums[: dist + 2])
        l = SortedList(nums[1 : dist + 2])
        r = SortedList()
        while len(l) > k:
            l2r()
        ans = s
        for i in range(dist + 2, len(nums)):
            x = nums[i - dist - 1]
            if x in l:
                l.remove(x)
                s -= x
            else:
                r.remove(x)
            y = nums[i]
            if y < l[-1]:
                l.add(y)
                s += y
            else:
                r.add(y)
            while len(l) < k:
                r2l()
            while len(l) > k:
                l2r()
            ans = min(ans, s)
        return ans
```

#### Java

```java
class Solution {
    private final TreeMap<Integer, Integer> l = new TreeMap<>();
    private final TreeMap<Integer, Integer> r = new TreeMap<>();
    private long s;
    private int size;

    public long minimumCost(int[] nums, int k, int dist) {
        --k;
        s = nums[0];
        for (int i = 1; i < dist + 2; ++i) {
            s += nums[i];
            l.merge(nums[i], 1, Integer::sum);
        }
        size = dist + 1;
        while (size > k) {
            l2r();
        }
        long ans = s;
        for (int i = dist + 2; i < nums.length; ++i) {
            int x = nums[i - dist - 1];
            if (l.containsKey(x)) {
                if (l.merge(x, -1, Integer::sum) == 0) {
                    l.remove(x);
                }
                s -= x;
                --size;
            } else if (r.merge(x, -1, Integer::sum) == 0) {
                r.remove(x);
            }
            int y = nums[i];
            if (y < l.lastKey()) {
                l.merge(y, 1, Integer::sum);
                ++size;
                s += y;
            } else {
                r.merge(y, 1, Integer::sum);
            }
            while (size < k) {
                r2l();
            }
            while (size > k) {
                l2r();
            }
            ans = Math.min(ans, s);
        }
        return ans;
    }

    private void l2r() {
        int x = l.lastKey();
        s -= x;
        if (l.merge(x, -1, Integer::sum) == 0) {
            l.remove(x);
        }
        --size;
        r.merge(x, 1, Integer::sum);
    }

    private void r2l() {
        int x = r.firstKey();
        if (r.merge(x, -1, Integer::sum) == 0) {
            r.remove(x);
        }
        l.merge(x, 1, Integer::sum);
        s += x;
        ++size;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimumCost(vector<int>& nums, int k, int dist) {
        --k;
        multiset<int> l(nums.begin() + 1, nums.begin() + dist + 2), r;
        long long s = accumulate(nums.begin(), nums.begin() + dist + 2, 0LL);
        while (l.size() > k) {
            int x = *l.rbegin();
            l.erase(l.find(x));
            s -= x;
            r.insert(x);
        }
        long long ans = s;
        for (int i = dist + 2; i < nums.size(); ++i) {
            int x = nums[i - dist - 1];
            auto it = l.find(x);
            if (it != l.end()) {
                l.erase(it);
                s -= x;
            } else {
                r.erase(r.find(x));
            }
            int y = nums[i];
            if (y < *l.rbegin()) {
                l.insert(y);
                s += y;
            } else {
                r.insert(y);
            }
            while (l.size() == k - 1) {
                int x = *r.begin();
                r.erase(r.find(x));
                l.insert(x);
                s += x;
            }
            while (l.size() == k + 1) {
                int x = *l.rbegin();
                l.erase(l.find(x));
                s -= x;
                r.insert(x);
            }
            ans = min(ans, s);
        }
        return ans;
    }
};
```

#### Go

```go
func minimumCost(nums []int, k int, dist int) int64 {
	k--
	l := redblacktree.New[int, int]()
	r := redblacktree.New[int, int]()
	merge := func(st *redblacktree.Tree[int, int], x, v int) {
		c, _ := st.Get(x)
		if c+v == 0 {
			st.Remove(x)
		} else {
			st.Put(x, c+v)
		}
	}

	s := nums[0]
	for _, x := range nums[1 : dist+2] {
		s += x
		merge(l, x, 1)
	}
	size := dist + 1

	l2r := func() {
		x := l.Right().Key
		merge(l, x, -1)
		s -= x
		size--
		merge(r, x, 1)
	}
	r2l := func() {
		x := r.Left().Key
		merge(r, x, -1)
		merge(l, x, 1)
		s += x
		size++
	}

	for size > k {
		l2r()
	}

	ans := s
	for i := dist + 2; i < len(nums); i++ {
		x := nums[i-dist-1]
		if _, ok := l.Get(x); ok {
			merge(l, x, -1)
			s -= x
			size--
		} else {
			merge(r, x, -1)
		}
		y := nums[i]
		if y < l.Right().Key {
			merge(l, y, 1)
			s += y
			size++
		} else {
			merge(r, y, 1)
		}
		for size < k {
			r2l()
		}
		for size > k {
			l2r()
		}
		ans = min(ans, s)

	}
	return int64(ans)
}
```

#### TypeScript

```ts
function minimumCost(nums: number[], k: number, dist: number): number {
    --k;
    const l = new TreapMultiSet<number>((a, b) => a - b);
    const r = new TreapMultiSet<number>((a, b) => a - b);
    let s = nums[0];
    for (let i = 1; i < dist + 2; ++i) {
        s += nums[i];
        l.add(nums[i]);
    }
    const l2r = () => {
        const x = l.pop()!;
        s -= x;
        r.add(x);
    };
    const r2l = () => {
        const x = r.shift()!;
        l.add(x);
        s += x;
    };
    while (l.size > k) {
        l2r();
    }
    let ans = s;
    for (let i = dist + 2; i < nums.length; ++i) {
        const x = nums[i - dist - 1];
        if (l.has(x)) {
            l.delete(x);
            s -= x;
        } else {
            r.delete(x);
        }
        const y = nums[i];
        if (y < l.last()!) {
            l.add(y);
            s += y;
        } else {
            r.add(y);
        }
        while (l.size < k) {
            r2l();
        }
        while (l.size > k) {
            l2r();
        }
        ans = Math.min(ans, s);
    }
    return ans;
}

type CompareFunction<T, R extends 'number' | 'boolean'> = (
    a: T,
    b: T,
) => R extends 'number' ? number : boolean;

interface ITreapMultiSet<T> extends Iterable<T> {
    add: (...value: T[]) => this;
    has: (value: T) => boolean;
    delete: (value: T) => void;

    bisectLeft: (value: T) => number;
    bisectRight: (value: T) => number;

    indexOf: (value: T) => number;
    lastIndexOf: (value: T) => number;

    at: (index: number) => T | undefined;
    first: () => T | undefined;
    last: () => T | undefined;

    lower: (value: T) => T | undefined;
    higher: (value: T) => T | undefined;
    floor: (value: T) => T | undefined;
    ceil: (value: T) => T | undefined;

    shift: () => T | undefined;
    pop: (index?: number) => T | undefined;

    count: (value: T) => number;

    keys: () => IterableIterator<T>;
    values: () => IterableIterator<T>;
    rvalues: () => IterableIterator<T>;
    entries: () => IterableIterator<[number, T]>;

    readonly size: number;
}

class TreapNode<T = number> {
    value: T;
    count: number;
    size: number;
    priority: number;
    left: TreapNode<T> | null;
    right: TreapNode<T> | null;

    constructor(value: T) {
        this.value = value;
        this.count = 1;
        this.size = 1;
        this.priority = Math.random();
        this.left = null;
        this.right = null;
    }

    static getSize(node: TreapNode<any> | null): number {
        return node?.size ?? 0;
    }

    static getFac(node: TreapNode<any> | null): number {
        return node?.priority ?? 0;
    }

    pushUp(): void {
        let tmp = this.count;
        tmp += TreapNode.getSize(this.left);
        tmp += TreapNode.getSize(this.right);
        this.size = tmp;
    }

    rotateRight(): TreapNode<T> {
        // eslint-disable-next-line @typescript-eslint/no-this-alias
        let node: TreapNode<T> = this;
        const left = node.left;
        node.left = left?.right ?? null;
        left && (left.right = node);
        left && (node = left);
        node.right?.pushUp();
        node.pushUp();
        return node;
    }

    rotateLeft(): TreapNode<T> {
        // eslint-disable-next-line @typescript-eslint/no-this-alias
        let node: TreapNode<T> = this;
        const right = node.right;
        node.right = right?.left ?? null;
        right && (right.left = node);
        right && (node = right);
        node.left?.pushUp();
        node.pushUp();
        return node;
    }
}

class TreapMultiSet<T = number> implements ITreapMultiSet<T> {
    private readonly root: TreapNode<T>;
    private readonly compareFn: CompareFunction<T, 'number'>;
    private readonly leftBound: T;
    private readonly rightBound: T;

    constructor(compareFn?: CompareFunction<T, 'number'>);
    constructor(compareFn: CompareFunction<T, 'number'>, leftBound: T, rightBound: T);
    constructor(
        compareFn: CompareFunction<T, any> = (a: any, b: any) => a - b,
        leftBound: any = -Infinity,
        rightBound: any = Infinity,
    ) {
        this.root = new TreapNode<T>(rightBound);
        this.root.priority = Infinity;
        this.root.left = new TreapNode<T>(leftBound);
        this.root.left.priority = -Infinity;
        this.root.pushUp();

        this.leftBound = leftBound;
        this.rightBound = rightBound;
        this.compareFn = compareFn;
    }

    get size(): number {
        return this.root.size - 2;
    }

    get height(): number {
        const getHeight = (node: TreapNode<T> | null): number => {
            if (node == null) return 0;
            return 1 + Math.max(getHeight(node.left), getHeight(node.right));
        };

        return getHeight(this.root);
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Returns true if value is a member.
     */
    has(value: T): boolean {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): boolean => {
            if (node == null) return false;
            if (compare(node.value, value) === 0) return true;
            if (compare(node.value, value) < 0) return dfs(node.right, value);
            return dfs(node.left, value);
        };

        return dfs(this.root, value);
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Add value to sorted set.
     */
    add(...values: T[]): this {
        const compare = this.compareFn;
        const dfs = (
            node: TreapNode<T> | null,
            value: T,
            parent: TreapNode<T>,
            direction: 'left' | 'right',
        ): void => {
            if (node == null) return;
            if (compare(node.value, value) === 0) {
                node.count++;
                node.pushUp();
            } else if (compare(node.value, value) > 0) {
                if (node.left) {
                    dfs(node.left, value, node, 'left');
                } else {
                    node.left = new TreapNode(value);
                    node.pushUp();
                }

                if (TreapNode.getFac(node.left) > node.priority) {
                    parent[direction] = node.rotateRight();
                }
            } else if (compare(node.value, value) < 0) {
                if (node.right) {
                    dfs(node.right, value, node, 'right');
                } else {
                    node.right = new TreapNode(value);
                    node.pushUp();
                }

                if (TreapNode.getFac(node.right) > node.priority) {
                    parent[direction] = node.rotateLeft();
                }
            }
            parent.pushUp();
        };

        values.forEach(value => dfs(this.root.left, value, this.root, 'left'));
        return this;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Remove value from sorted set if it is a member.
     * If value is not a member, do nothing.
     */
    delete(value: T): void {
        const compare = this.compareFn;
        const dfs = (
            node: TreapNode<T> | null,
            value: T,
            parent: TreapNode<T>,
            direction: 'left' | 'right',
        ): void => {
            if (node == null) return;

            if (compare(node.value, value) === 0) {
                if (node.count > 1) {
                    node.count--;
                    node?.pushUp();
                } else if (node.left == null && node.right == null) {
                    parent[direction] = null;
                } else {
                    // 旋到根节点
                    if (
                        node.right == null ||
                        TreapNode.getFac(node.left) > TreapNode.getFac(node.right)
                    ) {
                        parent[direction] = node.rotateRight();
                        dfs(parent[direction]?.right ?? null, value, parent[direction]!, 'right');
                    } else {
                        parent[direction] = node.rotateLeft();
                        dfs(parent[direction]?.left ?? null, value, parent[direction]!, 'left');
                    }
                }
            } else if (compare(node.value, value) > 0) {
                dfs(node.left, value, node, 'left');
            } else if (compare(node.value, value) < 0) {
                dfs(node.right, value, node, 'right');
            }

            parent?.pushUp();
        };

        dfs(this.root.left, value, this.root, 'left');
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Returns an index to insert value in the sorted set.
     * If the value is already present, the insertion point will be before (to the left of) any existing values.
     */
    bisectLeft(value: T): number {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): number => {
            if (node == null) return 0;

            if (compare(node.value, value) === 0) {
                return TreapNode.getSize(node.left);
            } else if (compare(node.value, value) > 0) {
                return dfs(node.left, value);
            } else if (compare(node.value, value) < 0) {
                return dfs(node.right, value) + TreapNode.getSize(node.left) + node.count;
            }

            return 0;
        };

        return dfs(this.root, value) - 1;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Returns an index to insert value in the sorted set.
     * If the value is already present, the insertion point will be before (to the right of) any existing values.
     */
    bisectRight(value: T): number {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): number => {
            if (node == null) return 0;

            if (compare(node.value, value) === 0) {
                return TreapNode.getSize(node.left) + node.count;
            } else if (compare(node.value, value) > 0) {
                return dfs(node.left, value);
            } else if (compare(node.value, value) < 0) {
                return dfs(node.right, value) + TreapNode.getSize(node.left) + node.count;
            }

            return 0;
        };
        return dfs(this.root, value) - 1;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Returns the index of the first occurrence of a value in the set, or -1 if it is not present.
     */
    indexOf(value: T): number {
        const compare = this.compareFn;
        let isExist = false;

        const dfs = (node: TreapNode<T> | null, value: T): number => {
            if (node == null) return 0;

            if (compare(node.value, value) === 0) {
                isExist = true;
                return TreapNode.getSize(node.left);
            } else if (compare(node.value, value) > 0) {
                return dfs(node.left, value);
            } else if (compare(node.value, value) < 0) {
                return dfs(node.right, value) + TreapNode.getSize(node.left) + node.count;
            }

            return 0;
        };
        const res = dfs(this.root, value) - 1;
        return isExist ? res : -1;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Returns the index of the last occurrence of a value in the set, or -1 if it is not present.
     */
    lastIndexOf(value: T): number {
        const compare = this.compareFn;
        let isExist = false;

        const dfs = (node: TreapNode<T> | null, value: T): number => {
            if (node == null) return 0;

            if (compare(node.value, value) === 0) {
                isExist = true;
                return TreapNode.getSize(node.left) + node.count - 1;
            } else if (compare(node.value, value) > 0) {
                return dfs(node.left, value);
            } else if (compare(node.value, value) < 0) {
                return dfs(node.right, value) + TreapNode.getSize(node.left) + node.count;
            }

            return 0;
        };

        const res = dfs(this.root, value) - 1;
        return isExist ? res : -1;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Returns the item located at the specified index.
     * @param index The zero-based index of the desired code unit. A negative index will count back from the last item.
     */
    at(index: number): T | undefined {
        if (index < 0) index += this.size;
        if (index < 0 || index >= this.size) return undefined;

        const dfs = (node: TreapNode<T> | null, rank: number): T | undefined => {
            if (node == null) return undefined;

            if (TreapNode.getSize(node.left) >= rank) {
                return dfs(node.left, rank);
            } else if (TreapNode.getSize(node.left) + node.count >= rank) {
                return node.value;
            } else {
                return dfs(node.right, rank - TreapNode.getSize(node.left) - node.count);
            }
        };

        const res = dfs(this.root, index + 2);
        return ([this.leftBound, this.rightBound] as any[]).includes(res) ? undefined : res;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Find and return the element less than `val`, return `undefined` if no such element found.
     */
    lower(value: T): T | undefined {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): T | undefined => {
            if (node == null) return undefined;
            if (compare(node.value, value) >= 0) return dfs(node.left, value);

            const tmp = dfs(node.right, value);
            if (tmp == null || compare(node.value, tmp) > 0) {
                return node.value;
            } else {
                return tmp;
            }
        };

        const res = dfs(this.root, value) as any;
        return res === this.leftBound ? undefined : res;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Find and return the element greater than `val`, return `undefined` if no such element found.
     */
    higher(value: T): T | undefined {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): T | undefined => {
            if (node == null) return undefined;
            if (compare(node.value, value) <= 0) return dfs(node.right, value);

            const tmp = dfs(node.left, value);

            if (tmp == null || compare(node.value, tmp) < 0) {
                return node.value;
            } else {
                return tmp;
            }
        };

        const res = dfs(this.root, value) as any;
        return res === this.rightBound ? undefined : res;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Find and return the element less than or equal to `val`, return `undefined` if no such element found.
     */
    floor(value: T): T | undefined {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): T | undefined => {
            if (node == null) return undefined;
            if (compare(node.value, value) === 0) return node.value;
            if (compare(node.value, value) >= 0) return dfs(node.left, value);

            const tmp = dfs(node.right, value);
            if (tmp == null || compare(node.value, tmp) > 0) {
                return node.value;
            } else {
                return tmp;
            }
        };

        const res = dfs(this.root, value) as any;
        return res === this.leftBound ? undefined : res;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description Find and return the element greater than or equal to `val`, return `undefined` if no such element found.
     */
    ceil(value: T): T | undefined {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): T | undefined => {
            if (node == null) return undefined;
            if (compare(node.value, value) === 0) return node.value;
            if (compare(node.value, value) <= 0) return dfs(node.right, value);

            const tmp = dfs(node.left, value);

            if (tmp == null || compare(node.value, tmp) < 0) {
                return node.value;
            } else {
                return tmp;
            }
        };

        const res = dfs(this.root, value) as any;
        return res === this.rightBound ? undefined : res;
    }

    /**
     * @complexity `O(logn)`
     * @description
     * Returns the last element from set.
     * If the set is empty, undefined is returned.
     */
    first(): T | undefined {
        const iter = this.inOrder();
        iter.next();
        const res = iter.next().value;
        return res === this.rightBound ? undefined : res;
    }

    /**
     * @complexity `O(logn)`
     * @description
     * Returns the last element from set.
     * If the set is empty, undefined is returned .
     */
    last(): T | undefined {
        const iter = this.reverseInOrder();
        iter.next();
        const res = iter.next().value;
        return res === this.leftBound ? undefined : res;
    }

    /**
     * @complexity `O(logn)`
     * @description
     * Removes the first element from an set and returns it.
     * If the set is empty, undefined is returned and the set is not modified.
     */
    shift(): T | undefined {
        const first = this.first();
        if (first === undefined) return undefined;
        this.delete(first);
        return first;
    }

    /**
     * @complexity `O(logn)`
     * @description
     * Removes the last element from an set and returns it.
     * If the set is empty, undefined is returned and the set is not modified.
     */
    pop(index?: number): T | undefined {
        if (index == null) {
            const last = this.last();
            if (last === undefined) return undefined;
            this.delete(last);
            return last;
        }

        const toDelete = this.at(index);
        if (toDelete == null) return;
        this.delete(toDelete);
        return toDelete;
    }

    /**
     *
     * @complexity `O(logn)`
     * @description
     * Returns number of occurrences of value in the sorted set.
     */
    count(value: T): number {
        const compare = this.compareFn;
        const dfs = (node: TreapNode<T> | null, value: T): number => {
            if (node == null) return 0;
            if (compare(node.value, value) === 0) return node.count;
            if (compare(node.value, value) < 0) return dfs(node.right, value);
            return dfs(node.left, value);
        };

        return dfs(this.root, value);
    }

    *[Symbol.iterator](): Generator<T, any, any> {
        yield* this.values();
    }

    /**
     * @description
     * Returns an iterable of keys in the set.
     */
    *keys(): Generator<T, any, any> {
        yield* this.values();
    }

    /**
     * @description
     * Returns an iterable of values in the set.
     */
    *values(): Generator<T, any, any> {
        const iter = this.inOrder();
        iter.next();
        const steps = this.size;
        for (let _ = 0; _ < steps; _++) {
            yield iter.next().value;
        }
    }

    /**
     * @description
     * Returns a generator for reversed order traversing the set.
     */
    *rvalues(): Generator<T, any, any> {
        const iter = this.reverseInOrder();
        iter.next();
        const steps = this.size;
        for (let _ = 0; _ < steps; _++) {
            yield iter.next().value;
        }
    }

    /**
     * @description
     * Returns an iterable of key, value pairs for every entry in the set.
     */
    *entries(): IterableIterator<[number, T]> {
        const iter = this.inOrder();
        iter.next();
        const steps = this.size;
        for (let i = 0; i < steps; i++) {
            yield [i, iter.next().value];
        }
    }

    private *inOrder(root: TreapNode<T> | null = this.root): Generator<T, any, any> {
        if (root == null) return;
        yield* this.inOrder(root.left);
        const count = root.count;
        for (let _ = 0; _ < count; _++) {
            yield root.value;
        }
        yield* this.inOrder(root.right);
    }

    private *reverseInOrder(root: TreapNode<T> | null = this.root): Generator<T, any, any> {
        if (root == null) return;
        yield* this.reverseInOrder(root.right);
        const count = root.count;
        for (let _ = 0; _ < count; _++) {
            yield root.value;
        }
        yield* this.reverseInOrder(root.left);
    }
}
```

#### Rust

```rust
use std::collections::BTreeMap;

impl Solution {
    pub fn minimum_cost(nums: Vec<i32>, k: i32, dist: i32) -> i64 {
        let n = nums.len();
        let k = (k - 1) as usize;
        let dist = dist as usize;

        let mut l: BTreeMap<i32, i32> = BTreeMap::new();
        let mut r: BTreeMap<i32, i32> = BTreeMap::new();

        let mut s: i64 = nums[0] as i64;
        let mut size: usize = 0;

        for i in 1..=dist + 1 {
            s += nums[i] as i64;
            *l.entry(nums[i]).or_insert(0) += 1;
        }
        size = dist + 1;

        while size > k {
            l2r(&mut l, &mut r, &mut s, &mut size);
        }

        let mut ans = s;

        for i in dist + 2..n {
            let x = nums[i - dist - 1];
            if let Some(v) = l.get_mut(&x) {
                *v -= 1;
                if *v == 0 {
                    l.remove(&x);
                }
                s -= x as i64;
                size -= 1;
            } else {
                let v = r.get_mut(&x).unwrap();
                *v -= 1;
                if *v == 0 {
                    r.remove(&x);
                }
            }

            let y = nums[i];
            let l_max = *l.iter().next_back().unwrap().0;
            if y < l_max {
                *l.entry(y).or_insert(0) += 1;
                size += 1;
                s += y as i64;
            } else {
                *r.entry(y).or_insert(0) += 1;
            }

            while size < k {
                r2l(&mut l, &mut r, &mut s, &mut size);
            }
            while size > k {
                l2r(&mut l, &mut r, &mut s, &mut size);
            }

            ans = ans.min(s);
        }

        ans
    }
}

fn l2r(
    l: &mut BTreeMap<i32, i32>,
    r: &mut BTreeMap<i32, i32>,
    s: &mut i64,
    size: &mut usize,
) {
    let x = *l.iter().next_back().unwrap().0;
    *s -= x as i64;

    if let Some(v) = l.get_mut(&x) {
        *v -= 1;
        if *v == 0 {
            l.remove(&x);
        }
    }

    *size -= 1;
    *r.entry(x).or_insert(0) += 1;
}

fn r2l(
    l: &mut BTreeMap<i32, i32>,
    r: &mut BTreeMap<i32, i32>,
    s: &mut i64,
    size: &mut usize,
) {
    let x = *r.iter().next().unwrap().0;

    if let Some(v) = r.get_mut(&x) {
        *v -= 1;
        if *v == 0 {
            r.remove(&x);
        }
    }

    *l.entry(x).or_insert(0) += 1;
    *s += x as i64;
    *size += 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
