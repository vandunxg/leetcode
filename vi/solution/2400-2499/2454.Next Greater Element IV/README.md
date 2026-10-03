---
comments: true
difficulty: Hard
rating: 2175
source: Biweekly Contest 90 Q4
tags:
    - Stack
    - Array
    - Binary Search
    - Sorting
    - Monotonic Stack
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2454. Next Greater Element IV](https://leetcode.com/problems/next-greater-element-iv)

[中文文档](/solution/2400-2499/2454.Next%20Greater%20Element%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên không âm <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Với mỗi số nguyên trong <code>nums</code>, hãy tìm số nguyên <strong>lớn hơn thứ hai</strong> tương ứng.</p>

<p>Số nguyên <strong>lớn hơn thứ hai</strong> của <code>nums[i]</code> là <code>nums[j]</code> sao cho:</p>

<ul>
	<li><code>j &gt; i</code></li>
	<li><code>nums[j] &gt; nums[i]</code></li>
	<li>Tồn tại <strong>chính xác một</strong> chỉ số <code>k</code> sao cho <code>nums[k] &gt; nums[i]</code> và <code>i &lt; k &lt; j</code>.</li>
</ul>

<p>Nếu không có <code>nums[j]</code> nào như vậy, số nguyên lớn hơn thứ hai được xem là <code>-1</code>.</p>

<ul>
	<li>Ví dụ, trong mảng <code>[1, 2, 4, 3]</code>, số nguyên lớn hơn thứ hai của <code>1</code> là <code>4</code>, của <code>2</code> là <code>3</code>, còn của <code>3</code> và <code>4</code> là <code>-1</code>.</li>
</ul>

<p>Trả về<em> một mảng số nguyên </em><code>answer</code><em>, trong đó </em><code>answer[i]</code><em> là số nguyên lớn hơn thứ hai của </em><code>nums[i]</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,0,9,6]
<strong>Đầu ra:</strong> [9,6,6,-1,-1]
<strong>Giải thích:</strong>
Chỉ số 0: 4 là số nguyên đầu tiên lớn hơn 2, còn 9 là số nguyên thứ hai lớn hơn 2 ở bên phải 2.
Chỉ số 1: 9 là số đầu tiên, còn 6 là số nguyên thứ hai lớn hơn 4 ở bên phải 4.
Chỉ số 2: 9 là số đầu tiên, còn 6 là số nguyên thứ hai lớn hơn 0 ở bên phải 0.
Chỉ số 3: Không có số nguyên nào lớn hơn 9 ở bên phải, nên số nguyên lớn hơn thứ hai được xem là -1.
Chỉ số 4: Không có số nguyên nào lớn hơn 6 ở bên phải, nên số nguyên lớn hơn thứ hai được xem là -1.
Vì vậy, ta trả về [9,6,6,-1,-1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3]
<strong>Đầu ra:</strong> [-1,-1]
<strong>Giải thích:</strong>
Ta trả về [-1,-1] vì không số nào trong hai số có số nguyên nào lớn hơn nó.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Số nguyên lớn hơn thứ hai là giá trị lớn hơn nghiêm ngặt thứ hai ở bên phải. Với $n\le 10^5$, nếu xử lý từ lớn đến nhỏ thì mọi chỉ số đã được lưu đều thuộc về một giá trị lớn hơn. Phần tử thứ hai bên phải của $i$ trong ordered set chính là đáp án.
>
> Sắp xếp $(value,index)$ theo thứ tự giảm dần. Với mỗi chỉ số, dùng phép tìm kiếm nhị phân trên ordered set các chỉ số rồi đọc phần tử ở vị trí sau đó hai phần tử.

<!-- thinking:end -->

Ta có thể chuyển các phần tử trong mảng thành các cặp $(x, i)$, trong đó $x$ là giá trị của phần tử và $i$ là chỉ số của phần tử. Sau đó sắp xếp theo giá trị của các phần tử theo thứ tự giảm dần.

Tiếp theo, ta duyệt qua mảng đã sắp xếp, đồng thời duy trì một ordered set lưu các chỉ số của các phần tử. Khi duyệt đến phần tử $(x, i)$, chỉ số của tất cả phần tử lớn hơn $x$ đã nằm trong ordered set. Ta chỉ cần tìm chỉ số $j$ của phần tử kế tiếp sau $i$ trong ordered set, khi đó phần tử tương ứng với $j$ là phần tử lớn thứ hai của $x$. Sau đó, ta thêm $i$ vào ordered set. Tiếp tục duyệt phần tử kế tiếp.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def secondGreaterElement(self, nums: List[int]) -> List[int]:
        arr = [(x, i) for i, x in enumerate(nums)]
        arr.sort(key=lambda x: -x[0])
        sl = SortedList()
        n = len(nums)
        ans = [-1] * n
        for _, i in arr:
            j = sl.bisect_right(i)
            if j + 1 < len(sl):
                ans[i] = nums[sl[j + 1]]
            sl.add(i)
        return ans
```

#### Java

```java
class Solution {
    public int[] secondGreaterElement(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        Arrays.fill(ans, -1);
        int[][] arr = new int[n][0];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[] {nums[i], i};
        }
        Arrays.sort(arr, (a, b) -> a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]);
        TreeSet<Integer> ts = new TreeSet<>();
        for (int[] pair : arr) {
            int i = pair[1];
            Integer j = ts.higher(i);
            if (j != null && ts.higher(j) != null) {
                ans[i] = nums[ts.higher(j)];
            }
            ts.add(i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> secondGreaterElement(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n, -1);
        vector<pair<int, int>> arr(n);
        for (int i = 0; i < n; ++i) {
            arr[i] = {-nums[i], i};
        }
        sort(arr.begin(), arr.end());
        set<int> ts;
        for (auto& [_, i] : arr) {
            auto it = ts.upper_bound(i);
            if (it != ts.end() && ts.upper_bound(*it) != ts.end()) {
                ans[i] = nums[*ts.upper_bound(*it)];
            }
            ts.insert(i);
        }
        return ans;
    }
};
```

#### TypeScript

```ts
function secondGreaterElement(nums: number[]): number[] {
    const n = nums.length;
    const arr: number[][] = [];
    for (let i = 0; i < n; ++i) {
        arr.push([nums[i], i]);
    }
    arr.sort((a, b) => (a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]));
    const ans = Array(n).fill(-1);
    const ts = new TreeSet<number>();
    for (const [_, i] of arr) {
        let j = ts.higher(i);
        if (j !== undefined) {
            j = ts.higher(j);
            if (j !== undefined) {
                ans[i] = nums[j];
            }
        }
        ts.add(i);
    }
    return ans;
}

type Compare<T> = (lhs: T, rhs: T) => number;

class RBTreeNode<T = number> {
    data: T;
    count: number;
    left: RBTreeNode<T> | null;
    right: RBTreeNode<T> | null;
    parent: RBTreeNode<T> | null;
    color: number;
    constructor(data: T) {
        this.data = data;
        this.left = this.right = this.parent = null;
        this.color = 0;
        this.count = 1;
    }

    sibling(): RBTreeNode<T> | null {
        if (!this.parent) return null; // sibling null if no parent
        return this.isOnLeft() ? this.parent.right : this.parent.left;
    }

    isOnLeft(): boolean {
        return this === this.parent!.left;
    }

    hasRedChild(): boolean {
        return (
            Boolean(this.left && this.left.color === 0) ||
            Boolean(this.right && this.right.color === 0)
        );
    }
}

class RBTree<T> {
    root: RBTreeNode<T> | null;
    lt: (l: T, r: T) => boolean;
    constructor(compare: Compare<T> = (l: T, r: T) => (l < r ? -1 : l > r ? 1 : 0)) {
        this.root = null;
        this.lt = (l: T, r: T) => compare(l, r) < 0;
    }

    rotateLeft(pt: RBTreeNode<T>): void {
        const right = pt.right!;
        pt.right = right.left;

        if (pt.right) pt.right.parent = pt;
        right.parent = pt.parent;

        if (!pt.parent) this.root = right;
        else if (pt === pt.parent.left) pt.parent.left = right;
        else pt.parent.right = right;

        right.left = pt;
        pt.parent = right;
    }

    rotateRight(pt: RBTreeNode<T>): void {
        const left = pt.left!;
        pt.left = left.right;

        if (pt.left) pt.left.parent = pt;
        left.parent = pt.parent;

        if (!pt.parent) this.root = left;
        else if (pt === pt.parent.left) pt.parent.left = left;
        else pt.parent.right = left;

        left.right = pt;
        pt.parent = left;
    }

    swapColor(p1: RBTreeNode<T>, p2: RBTreeNode<T>): void {
        const tmp = p1.color;
        p1.color = p2.color;
        p2.color = tmp;
    }

    swapData(p1: RBTreeNode<T>, p2: RBTreeNode<T>): void {
        const tmp = p1.data;
        p1.data = p2.data;
        p2.data = tmp;
    }

    fixAfterInsert(pt: RBTreeNode<T>): void {
        let parent = null;
        let grandParent = null;

        while (pt !== this.root && pt.color !== 1 && pt.parent?.color === 0) {
            parent = pt.parent;
            grandParent = pt.parent.parent;

            /*  Case : A
                Parent of pt is left child of Grand-parent of pt */
            if (parent === grandParent?.left) {
                const uncle = grandParent.right;

                /* Case : 1
                   The uncle of pt is also red
                   Only Recoloring required */
                if (uncle && uncle.color === 0) {
                    grandParent.color = 0;
                    parent.color = 1;
                    uncle.color = 1;
                    pt = grandParent;
                } else {
                    /* Case : 2
                       pt is right child of its parent
                       Left-rotation required */
                    if (pt === parent.right) {
                        this.rotateLeft(parent);
                        pt = parent;
                        parent = pt.parent;
                    }

                    /* Case : 3
                       pt is left child of its parent
                       Right-rotation required */
                    this.rotateRight(grandParent);
                    this.swapColor(parent!, grandParent);
                    pt = parent!;
                }
            } else {
                /* Case : B
               Parent of pt is right child of Grand-parent of pt */
                const uncle = grandParent!.left;

                /*  Case : 1
                    The uncle of pt is also red
                    Only Recoloring required */
                if (uncle != null && uncle.color === 0) {
                    grandParent!.color = 0;
                    parent.color = 1;
                    uncle.color = 1;
                    pt = grandParent!;
                } else {
                    /* Case : 2
                       pt is left child of its parent
                       Right-rotation required */
                    if (pt === parent.left) {
                        this.rotateRight(parent);
                        pt = parent;
                        parent = pt.parent;
                    }

                    /* Case : 3
                       pt is right child of its parent
                       Left-rotation required */
                    this.rotateLeft(grandParent!);
                    this.swapColor(parent!, grandParent!);
                    pt = parent!;
                }
            }
        }
        this.root!.color = 1;
    }

    delete(val: T): boolean {
        const node = this.find(val);
        if (!node) return false;
        node.count--;
        if (!node.count) this.deleteNode(node);
        return true;
    }

    deleteAll(val: T): boolean {
        const node = this.find(val);
        if (!node) return false;
        this.deleteNode(node);
        return true;
    }

    deleteNode(v: RBTreeNode<T>): void {
        const u = BSTreplace(v);

        // True when u and v are both black
        const uvBlack = (u === null || u.color === 1) && v.color === 1;
        const parent = v.parent!;

        if (!u) {
            // u is null therefore v is leaf
            if (v === this.root) this.root = null;
            // v is root, making root null
            else {
                if (uvBlack) {
                    // u and v both black
                    // v is leaf, fix double black at v
                    this.fixDoubleBlack(v);
                } else {
                    // u or v is red
                    if (v.sibling()) {
                        // sibling is not null, make it red"
                        v.sibling()!.color = 0;
                    }
                }
                // delete v from the tree
                if (v.isOnLeft()) parent.left = null;
                else parent.right = null;
            }
            return;
        }

        if (!v.left || !v.right) {
            // v has 1 child
            if (v === this.root) {
                // v is root, assign the value of u to v, and delete u
                v.data = u.data;
                v.left = v.right = null;
            } else {
                // Detach v from tree and move u up
                if (v.isOnLeft()) parent.left = u;
                else parent.right = u;
                u.parent = parent;
                if (uvBlack) this.fixDoubleBlack(u);
                // u and v both black, fix double black at u
                else u.color = 1; // u or v red, color u black
            }
            return;
        }

        // v has 2 children, swap data with successor and recurse
        this.swapData(u, v);
        this.deleteNode(u);

        // find node that replaces a deleted node in BST
        function BSTreplace(x: RBTreeNode<T>): RBTreeNode<T> | null {
            // when node have 2 children
            if (x.left && x.right) return successor(x.right);
            // when leaf
            if (!x.left && !x.right) return null;
            // when single child
            return x.left ?? x.right;
        }
        // find node that do not have a left child
        // in the subtree of the given node
        function successor(x: RBTreeNode<T>): RBTreeNode<T> {
            let temp = x;
            while (temp.left) temp = temp.left;
            return temp;
        }
    }

    fixDoubleBlack(x: RBTreeNode<T>): void {
        if (x === this.root) return; // Reached root

        const sibling = x.sibling();
        const parent = x.parent!;
        if (!sibling) {
            // No sibiling, double black pushed up
            this.fixDoubleBlack(parent);
        } else {
            if (sibling.color === 0) {
                // Sibling red
                parent.color = 0;
                sibling.color = 1;
                if (sibling.isOnLeft()) this.rotateRight(parent);
                // left case
                else this.rotateLeft(parent); // right case
                this.fixDoubleBlack(x);
            } else {
                // Sibling black
                if (sibling.hasRedChild()) {
                    // at least 1 red children
                    if (sibling.left && sibling.left.color === 0) {
                        if (sibling.isOnLeft()) {
                            // left left
                            sibling.left.color = sibling.color;
                            sibling.color = parent.color;
                            this.rotateRight(parent);
                        } else {
                            // right left
                            sibling.left.color = parent.color;
                            this.rotateRight(sibling);
                            this.rotateLeft(parent);
                        }
                    } else {
                        if (sibling.isOnLeft()) {
                            // left right
                            sibling.right!.color = parent.color;
                            this.rotateLeft(sibling);
                            this.rotateRight(parent);
                        } else {
                            // right right
                            sibling.right!.color = sibling.color;
                            sibling.color = parent.color;
                            this.rotateLeft(parent);
                        }
                    }
                    parent.color = 1;
                } else {
                    // 2 black children
                    sibling.color = 0;
                    if (parent.color === 1) this.fixDoubleBlack(parent);
                    else parent.color = 1;
                }
            }
        }
    }

    insert(data: T): boolean {
        // search for a position to insert
        let parent = this.root;
        while (parent) {
            if (this.lt(data, parent.data)) {
                if (!parent.left) break;
                else parent = parent.left;
            } else if (this.lt(parent.data, data)) {
                if (!parent.right) break;
                else parent = parent.right;
            } else break;
        }

        // insert node into parent
        const node = new RBTreeNode(data);
        if (!parent) this.root = node;
        else if (this.lt(node.data, parent.data)) parent.left = node;
        else if (this.lt(parent.data, node.data)) parent.right = node;
        else {
            parent.count++;
            return false;
        }
        node.parent = parent;
        this.fixAfterInsert(node);
        return true;
    }

    find(data: T): RBTreeNode<T> | null {
        let p = this.root;
        while (p) {
            if (this.lt(data, p.data)) {
                p = p.left;
            } else if (this.lt(p.data, data)) {
                p = p.right;
            } else break;
        }
        return p ?? null;
    }

    *inOrder(root: RBTreeNode<T> = this.root!): Generator<T, undefined, void> {
        if (!root) return;
        for (const v of this.inOrder(root.left!)) yield v;
        yield root.data;
        for (const v of this.inOrder(root.right!)) yield v;
    }

    *reverseInOrder(root: RBTreeNode<T> = this.root!): Generator<T, undefined, void> {
        if (!root) return;
        for (const v of this.reverseInOrder(root.right!)) yield v;
        yield root.data;
        for (const v of this.reverseInOrder(root.left!)) yield v;
    }
}

class TreeSet<T = number> {
    _size: number;
    tree: RBTree<T>;
    compare: Compare<T>;
    constructor(
        collection: T[] | Compare<T> = [],
        compare: Compare<T> = (l: T, r: T) => (l < r ? -1 : l > r ? 1 : 0),
    ) {
        if (typeof collection === 'function') {
            compare = collection;
            collection = [];
        }
        this._size = 0;
        this.compare = compare;
        this.tree = new RBTree(compare);
        for (const val of collection) this.add(val);
    }

    size(): number {
        return this._size;
    }

    has(val: T): boolean {
        return !!this.tree.find(val);
    }

    add(val: T): boolean {
        const successful = this.tree.insert(val);
        this._size += successful ? 1 : 0;
        return successful;
    }

    delete(val: T): boolean {
        const deleted = this.tree.deleteAll(val);
        this._size -= deleted ? 1 : 0;
        return deleted;
    }

    ceil(val: T): T | undefined {
        let p = this.tree.root;
        let higher = null;
        while (p) {
            if (this.compare(p.data, val) >= 0) {
                higher = p;
                p = p.left;
            } else {
                p = p.right;
            }
        }
        return higher?.data;
    }

    floor(val: T): T | undefined {
        let p = this.tree.root;
        let lower = null;
        while (p) {
            if (this.compare(val, p.data) >= 0) {
                lower = p;
                p = p.right;
            } else {
                p = p.left;
            }
        }
        return lower?.data;
    }

    higher(val: T): T | undefined {
        let p = this.tree.root;
        let higher = null;
        while (p) {
            if (this.compare(val, p.data) < 0) {
                higher = p;
                p = p.left;
            } else {
                p = p.right;
            }
        }
        return higher?.data;
    }

    lower(val: T): T | undefined {
        let p = this.tree.root;
        let lower = null;
        while (p) {
            if (this.compare(p.data, val) < 0) {
                lower = p;
                p = p.right;
            } else {
                p = p.left;
            }
        }
        return lower?.data;
    }

    first(): T | undefined {
        return this.tree.inOrder().next().value;
    }

    last(): T | undefined {
        return this.tree.reverseInOrder().next().value;
    }

    shift(): T | undefined {
        const first = this.first();
        if (first === undefined) return undefined;
        this.delete(first);
        return first;
    }

    pop(): T | undefined {
        const last = this.last();
        if (last === undefined) return undefined;
        this.delete(last);
        return last;
    }

    *[Symbol.iterator](): Generator<T, void, void> {
        for (const val of this.values()) yield val;
    }

    *keys(): Generator<T, void, void> {
        for (const val of this.values()) yield val;
    }

    *values(): Generator<T, undefined, void> {
        for (const val of this.tree.inOrder()) yield val;
        return undefined;
    }

    /**
     * Return a generator for reverse order traversing the set
     */
    *rvalues(): Generator<T, undefined, void> {
        for (const val of this.tree.reverseInOrder()) yield val;
        return undefined;
    }
}

class TreeMultiSet<T = number> {
    _size: number;
    tree: RBTree<T>;
    compare: Compare<T>;
    constructor(
        collection: T[] | Compare<T> = [],
        compare: Compare<T> = (l: T, r: T) => (l < r ? -1 : l > r ? 1 : 0),
    ) {
        if (typeof collection === 'function') {
            compare = collection;
            collection = [];
        }
        this._size = 0;
        this.compare = compare;
        this.tree = new RBTree(compare);
        for (const val of collection) this.add(val);
    }

    size(): number {
        return this._size;
    }

    has(val: T): boolean {
        return !!this.tree.find(val);
    }

    add(val: T): boolean {
        const successful = this.tree.insert(val);
        this._size++;
        return successful;
    }

    delete(val: T): boolean {
        const successful = this.tree.delete(val);
        if (!successful) return false;
        this._size--;
        return true;
    }

    count(val: T): number {
        const node = this.tree.find(val);
        return node ? node.count : 0;
    }

    ceil(val: T): T | undefined {
        let p = this.tree.root;
        let higher = null;
        while (p) {
            if (this.compare(p.data, val) >= 0) {
                higher = p;
                p = p.left;
            } else {
                p = p.right;
            }
        }
        return higher?.data;
    }

    floor(val: T): T | undefined {
        let p = this.tree.root;
        let lower = null;
        while (p) {
            if (this.compare(val, p.data) >= 0) {
                lower = p;
                p = p.right;
            } else {
                p = p.left;
            }
        }
        return lower?.data;
    }

    higher(val: T): T | undefined {
        let p = this.tree.root;
        let higher = null;
        while (p) {
            if (this.compare(val, p.data) < 0) {
                higher = p;
                p = p.left;
            } else {
                p = p.right;
            }
        }
        return higher?.data;
    }

    lower(val: T): T | undefined {
        let p = this.tree.root;
        let lower = null;
        while (p) {
            if (this.compare(p.data, val) < 0) {
                lower = p;
                p = p.right;
            } else {
                p = p.left;
            }
        }
        return lower?.data;
    }

    first(): T | undefined {
        return this.tree.inOrder().next().value;
    }

    last(): T | undefined {
        return this.tree.reverseInOrder().next().value;
    }

    shift(): T | undefined {
        const first = this.first();
        if (first === undefined) return undefined;
        this.delete(first);
        return first;
    }

    pop(): T | undefined {
        const last = this.last();
        if (last === undefined) return undefined;
        this.delete(last);
        return last;
    }

    *[Symbol.iterator](): Generator<T, void, void> {
        yield* this.values();
    }

    *keys(): Generator<T, void, void> {
        for (const val of this.values()) yield val;
    }

    *values(): Generator<T, undefined, void> {
        for (const val of this.tree.inOrder()) {
            let count = this.count(val);
            while (count--) yield val;
        }
        return undefined;
    }

    /**
     * Return a generator for reverse order traversing the multi-set
     */
    *rvalues(): Generator<T, undefined, void> {
        for (const val of this.tree.reverseInOrder()) {
            let count = this.count(val);
            while (count--) yield val;
        }
        return undefined;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

### Lời giải 2: Hai Stack

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 phải trả thêm một thừa số log cho ordered set. Chỉ cần hai stack giảm dần: stack một chờ phần tử lớn hơn đầu tiên, stack hai chờ phần tử lớn hơn thứ hai. Giá trị hiện tại lấy các phần tử trong stack hai ra để cập nhật đáp án, đồng thời chuyển các phần tử được lấy ra từ stack một sang stack hai; mỗi chỉ số chỉ được xử lý một số lần hằng số.

<!-- thinking:end -->

Ta duy trì hai stack đơn điệu giảm:

`stackOne`: lưu các phần tử chưa gặp phần tử nào lớn hơn ở bên phải.

`stackTwo`: lưu các phần tử đã gặp chính xác một phần tử lớn hơn ở bên phải.

Thuật toán:

Khi duyệt qua mảng `nums`, với phần tử hiện tại $nums[k]$, ta thực hiện các bước sau:

1. Trong khi `stackTwo` không rỗng và phần tử ở đỉnh của nó nhỏ hơn $nums[k]$, các phần tử ở đỉnh này đã tìm thấy phần tử lớn hơn tiếp theo thứ hai. Lấy chúng ra và ghi nhận $nums[k]$ là đáp án cho các chỉ số tương ứng.

2. Trong khi `stackOne` không rỗng và phần tử ở đỉnh của nó nhỏ hơn $nums[k]$, các phần tử ở đỉnh này đã tìm thấy phần tử lớn hơn tiếp theo đầu tiên. Lấy chúng ra để chuyển vào một danh sách/vector tạm thời có tên `transporter`.

3. Lấy tất cả phần tử từ cuối `transporter` và đưa chúng vào `stackTwo`. Các phần tử này sẽ tự duy trì thứ tự giảm dần trong `stackTwo`.

4. Đưa phần tử hiện tại $nums[k]$ vào `stackOne` để so sánh trong tương lai.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng nums.

Điều này là do mỗi phần tử được push và pop qua hai stack nhiều nhất tổng cộng 4 lần.

Độ phức tạp không gian là $O(n)$, vì tất cả phần tử phải được lưu trong hai stack.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def secondGreaterElement(self, nums: list[int]) -> list[int]:
        second_next_greater = [-1] * len(nums)

        stack_1: list[tuple[int, int]] = []  # Decreasing monotonic stacks: (num, idx).
        stack_2: list[tuple[int, int]] = []

        # Transport tuples from stack 1 to stack 2.
        transporter: list[tuple[int, int]] = []

        for idx, num in enumerate(nums):
            while stack_2 and stack_2[-1][0] < num:
                _, past_idx = stack_2.pop(-1)
                second_next_greater[past_idx] = num

            while stack_1 and stack_1[-1][0] < num:
                transporter.append(stack_1.pop(-1))

            while transporter:
                stack_2.append(transporter.pop(-1))  # Ensure decreasing monotonicity.
            stack_1.append((num, idx))

        return second_next_greater
```

#### C++

```cpp
class Solution {
public:
    vector<int> secondGreaterElement(vector<int>& nums) {
        vector<int> secondNextGreater(nums.size(), -1);

        // Decreasing monotonic stacks: {num, idx}.
        stack<pair<int, int>> stackOne, stackTwo;

        vector<pair<int, int>> transporter; // Format: {num, idx}.

        for (int idx = 0; idx < nums.size(); idx++) {
            int num = nums[idx];

            while (!stackTwo.empty() && stackTwo.top().first < num) {
                int pastIdx = stackTwo.top().second;
                secondNextGreater[pastIdx] = num;
                stackTwo.pop();
            }

            while (!stackOne.empty() && stackOne.top().first < num) {
                transporter.push_back(stackOne.top()); // Keep decreasing monotonicity.
                stackOne.pop();
            }

            while (!transporter.empty()) {
                stackTwo.push(transporter.back());
                transporter.pop_back();
            }

            stackOne.push({num, idx});
        }

        return secondNextGreater;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
