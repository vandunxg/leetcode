---
comments: true
difficulty: Hard
rating: 2215
source: Weekly Contest 391 Q4
tags:
    - Geometry
    - Array
    - Math
    - Ordered Set
    - Sorting
---

<!-- problem:start -->

# [3102. Minimize Manhattan Distances](https://leetcode.com/problems/minimize-manhattan-distances)

[Tài liệu tiếng Trung](/solution/3100-3199/3102.Minimize%20Manhattan%20Distances/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>points</code> biểu diễn tọa độ nguyên của một số điểm trên mặt phẳng 2D, trong đó <code>points[i] = [x<sub>i</sub>, y<sub>i</sub>]</code>.</p>

<p>Khoảng cách giữa hai điểm được định nghĩa là <span data-keyword="manhattan-distance">khoảng cách Manhattan</span> của chúng.</p>

<p>Hãy trả về <em>giá trị <strong>nhỏ nhất</strong> có thể của <strong>khoảng cách lớn nhất</strong> giữa bất kỳ hai điểm nào sau khi loại bỏ đúng một điểm</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[3,10],[5,15],[10,2],[4,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Khoảng cách lớn nhất sau khi loại bỏ từng điểm lần lượt là:</p>

<ul>
	<li>Sau khi loại bỏ điểm thứ 0<sup>th</sup>, khoảng cách lớn nhất là giữa các điểm (5, 15) và (10, 2), bằng <code>|5 - 10| + |15 - 2| = 18</code>.</li>
	<li>Sau khi loại bỏ điểm thứ 1<sup>st</sup>, khoảng cách lớn nhất là giữa các điểm (3, 10) và (10, 2), bằng <code>|3 - 10| + |10 - 2| = 15</code>.</li>
	<li>Sau khi loại bỏ điểm thứ 2<sup>nd</sup>, khoảng cách lớn nhất là giữa các điểm (5, 15) và (4, 4), bằng <code>|5 - 4| + |15 - 4| = 12</code>.</li>
	<li>Sau khi loại bỏ điểm thứ 3<sup>rd</sup>, khoảng cách lớn nhất là giữa các điểm (5, 15) và (10, 2), bằng <code>|5 - 10| + |15 - 2| = 18</code>.</li>
</ul>

<p>12 là khoảng cách lớn nhất nhỏ nhất có thể giữa bất kỳ hai điểm nào sau khi loại bỏ đúng một điểm.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">points = [[1,1],[1,1],[1,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Loại bỏ bất kỳ điểm nào cũng cho khoảng cách lớn nhất giữa hai điểm bất kỳ là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= points.length &lt;= 10<sup>5</sup></code></li>
	<li><code>points[i].length == 2</code></li>
	<li><code>1 &lt;= points[i][0], points[i][1] &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi loại bỏ một điểm, ta phải tính lại khoảng cách Manhattan lớn nhất giữa các điểm còn lại. Nếu duyệt tất cả các cặp cho mỗi lần loại bỏ, độ phức tạp là $O(n^3)$ và không thể đáp ứng khi $n$ lên đến vài nghìn.
>
> $|x_1-x_2|+|y_1-y_2|$ bằng giá trị lớn hơn trong độ chênh lệch của $x+y$ và $x-y$. Vì vậy, khoảng cách lớn nhất trên toàn bộ các điểm chỉ phụ thuộc vào các giá trị cực trị của hai tọa độ biến đổi này.
>
> Lưu mọi giá trị $x+y$ và $x-y$ trong các ordered set, tạm thời xóa từng điểm, lấy các giá trị cực trị mới rồi chèn điểm đó trở lại. Mỗi lần cập nhật mất thời gian logarit, nên có thể tìm được lần loại bỏ tốt nhất trong $O(n\log n)$.

<!-- thinking:end -->

Với hai điểm $(x_1, y_1)$ và $(x_2, y_2)$, khoảng cách Manhattan giữa chúng là $|x_1 - x_2| + |y_1 - y_2|$. Ta có thể biến đổi nó thành $\max(x_1 - x_2, x_2 - x_1) + \max(y_1 - y_2, y_2 - y_1)$, tức là:

$$
|x_1 - x_2| + |y_1 - y_2| = \max \begin{cases}
x_1 - x_2 + y_1 - y_2 \\
x_2 - x_1 + y_2 - y_1 \\
x_1 - x_2 + y_2 - y_1 \\
x_2 - x_1 + y_1 - y_2
\end{cases}
$$

Rút gọn được:

$$
|x_1 - x_2| + |y_1 - y_2| = \max \begin{cases}
(x_1 + y_1) - (x_2 + y_2) \\
(x_2 + y_2) - (x_1 + y_1) \\
(x_1 - y_1) - (x_2 - y_2) \\
(x_2 - y_2) - (x_1 - y_1)
\end{cases}
$$

Trong đó, hai trường hợp đầu có thể được biểu diễn là $\max(\max(x_1 + y_1, x_2 + y_2) - \min(x_1 + y_1, x_2 + y_2))$, còn hai trường hợp cuối có thể được biểu diễn là $\max(\max(x_1 - y_1, x_2 - y_2) - \min(x_1 - y_1, x_2 - y_2))$.

Do đó, ta có thể lưu tất cả các điểm theo các giá trị $x + y$ và $x - y$ vào hai ordered set tương ứng. Sau đó, với mỗi điểm, ta xóa điểm đó, cập nhật các giá trị trong ordered set, tính hiệu giữa giá trị lớn nhất và nhỏ nhất, rồi lấy giá trị nhỏ nhất.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng điểm.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDistance(self, points: List[List[int]]) -> int:
        sl1 = SortedList()
        sl2 = SortedList()
        for x, y in points:
            sl1.add(x + y)
            sl2.add(x - y)
        ans = inf
        for x, y in points:
            sl1.remove(x + y)
            sl2.remove(x - y)
            ans = min(ans, max(sl1[-1] - sl1[0], sl2[-1] - sl2[0]))
            sl1.add(x + y)
            sl2.add(x - y)
        return ans
```

#### Java

```java
class Solution {
    public int minimumDistance(int[][] points) {
        TreeMap<Integer, Integer> tm1 = new TreeMap<>();
        TreeMap<Integer, Integer> tm2 = new TreeMap<>();
        for (int[] p : points) {
            int x = p[0], y = p[1];
            tm1.merge(x + y, 1, Integer::sum);
            tm2.merge(x - y, 1, Integer::sum);
        }
        int ans = Integer.MAX_VALUE;
        for (int[] p : points) {
            int x = p[0], y = p[1];
            if (tm1.merge(x + y, -1, Integer::sum) == 0) {
                tm1.remove(x + y);
            }
            if (tm2.merge(x - y, -1, Integer::sum) == 0) {
                tm2.remove(x - y);
            }
            ans = Math.min(
                ans, Math.max(tm1.lastKey() - tm1.firstKey(), tm2.lastKey() - tm2.firstKey()));
            tm1.merge(x + y, 1, Integer::sum);
            tm2.merge(x - y, 1, Integer::sum);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumDistance(vector<vector<int>>& points) {
        multiset<int> st1;
        multiset<int> st2;
        for (auto& p : points) {
            int x = p[0], y = p[1];
            st1.insert(x + y);
            st2.insert(x - y);
        }
        int ans = INT_MAX;
        for (auto& p : points) {
            int x = p[0], y = p[1];
            st1.erase(st1.find(x + y));
            st2.erase(st2.find(x - y));
            ans = min(ans, max(*st1.rbegin() - *st1.begin(), *st2.rbegin() - *st2.begin()));
            st1.insert(x + y);
            st2.insert(x - y);
        }
        return ans;
    }
};
```

#### Go

```go
func minimumDistance(points [][]int) int {
	st1 := redblacktree.New[int, int]()
	st2 := redblacktree.New[int, int]()
	merge := func(st *redblacktree.Tree[int, int], x, v int) {
		c, _ := st.Get(x)
		if c+v == 0 {
			st.Remove(x)
		} else {
			st.Put(x, c+v)
		}
	}
	for _, p := range points {
		x, y := p[0], p[1]
		merge(st1, x+y, 1)
		merge(st2, x-y, 1)
	}
	ans := math.MaxInt
	for _, p := range points {
		x, y := p[0], p[1]
		merge(st1, x+y, -1)
		merge(st2, x-y, -1)
		ans = min(ans, max(st1.Right().Key-st1.Left().Key, st2.Right().Key-st2.Left().Key))
		merge(st1, x+y, 1)
		merge(st2, x-y, 1)
	}
	return ans
}
```

#### TypeScript

```ts
function minimumDistance(points: number[][]): number {
    const st1 = new TreapMultiSet<number>();
    const st2 = new TreapMultiSet<number>();
    for (const [x, y] of points) {
        st1.add(x + y);
        st2.add(x - y);
    }
    let ans = Infinity;
    for (const [x, y] of points) {
        st1.delete(x + y);
        st2.delete(x - y);
        ans = Math.min(ans, Math.max(st1.last() - st1.first(), st2.last() - st2.first()));
        st1.add(x + y);
        st2.add(x - y);
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

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
