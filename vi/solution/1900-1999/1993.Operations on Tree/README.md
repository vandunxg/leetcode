---
comments: true
difficulty: Medium
rating: 1861
source: Biweekly Contest 60 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Design
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1993. Operations on Tree](https://leetcode.com/problems/operations-on-tree)

[中文文档](/solution/1900-1999/1993.Operations%20on%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một cây gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>, được biểu diễn dưới dạng mảng cha <code>parent</code>, trong đó <code>parent[i]</code> là node cha của node thứ <code>i<sup>th</sup></code>. Gốc của cây là node <code>0</code>, nên <code>parent[0] = -1</code> vì nó không có node cha. Bạn cần thiết kế một cấu trúc dữ liệu cho phép người dùng lock, unlock và upgrade các node trong cây.</p>

<p>Cấu trúc dữ liệu phải hỗ trợ các hàm sau:</p>

<ul>
	<li><strong>Lock:</strong> <strong>Lock</strong> node đã cho cho người dùng đã cho và ngăn những người dùng khác lock cùng node đó. Bạn chỉ có thể lock một node bằng hàm này nếu node đang được unlock.</li>
	<li><strong>Unlock: Unlock</strong> node đã cho cho người dùng đã cho. Bạn chỉ có thể unlock node bằng hàm này nếu node hiện đang được lock bởi chính người dùng đó.</li>
	<li><b>Upgrade</b><strong>: Lock</strong> node đã cho cho người dùng đã cho và <strong>unlock</strong> tất cả node hậu duệ của nó, <strong>bất kể</strong> ai đã lock chúng. Bạn chỉ có thể upgrade một node nếu <strong>tất cả</strong> 3 điều kiện sau đều đúng:
	<ul>
		<li>Node đang được unlock,</li>
		<li>Node có ít nhất một node hậu duệ đang được lock (bởi <strong>bất kỳ</strong> người dùng nào), và</li>
		<li>Node không có node tổ tiên nào đang được lock.</li>
	</ul>
	</li>
</ul>

<p>Hãy triển khai class <code>LockingTree</code>:</p>

<ul>
	<li><code>LockingTree(int[] parent)</code> khởi tạo cấu trúc dữ liệu với mảng cha.</li>
	<li><code>lock(int num, int user)</code> trả về <code>true</code> nếu người dùng có mã <code>user</code> có thể lock node <code>num</code>, hoặc <code>false</code> nếu ngược lại. Nếu có thể, node <code>num</code> sẽ trở thành<strong> locked</strong> bởi người dùng có mã <code>user</code>.</li>
	<li><code>unlock(int num, int user)</code> trả về <code>true</code> nếu người dùng có mã <code>user</code> có thể unlock node <code>num</code>, hoặc <code>false</code> nếu ngược lại. Nếu có thể, node <code>num</code> sẽ trở thành <strong>unlocked</strong>.</li>
	<li><code>upgrade(int num, int user)</code> trả về <code>true</code> nếu người dùng có mã <code>user</code> có thể upgrade node <code>num</code>, hoặc <code>false</code> nếu ngược lại. Nếu có thể, node <code>num</code> sẽ được <strong>upgraded</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1993.Operations%20on%20Tree/images/untitled.png" style="width: 375px; height: 246px;" />
<pre>
<strong>Đầu vào</strong>
[&quot;LockingTree&quot;, &quot;lock&quot;, &quot;unlock&quot;, &quot;unlock&quot;, &quot;lock&quot;, &quot;upgrade&quot;, &quot;lock&quot;]
[[[-1, 0, 0, 1, 1, 2, 2]], [2, 2], [2, 3], [2, 2], [4, 5], [0, 1], [0, 1]]
<strong>Đầu ra</strong>
[null, true, false, true, true, true, false]

<strong>Giải thích</strong>
LockingTree lockingTree = new LockingTree([-1, 0, 0, 1, 1, 2, 2]);
lockingTree.lock(2, 2); // return true because node 2 is unlocked.
// Node 2 will now be locked by user 2.
lockingTree.unlock(2, 3); // return false because user 3 cannot unlock a node locked by user 2.
lockingTree.unlock(2, 2); // return true because node 2 was previously locked by user 2.
// Node 2 will now be unlocked.
lockingTree.lock(4, 5); // return true because node 4 is unlocked.
// Node 4 will now be locked by user 5.
lockingTree.upgrade(0, 1); // return true because node 0 is unlocked and has at least one locked descendant (node 4).
// Node 0 will now be locked by user 1 and node 4 will now be unlocked.
lockingTree.lock(0, 1); // return false because node 0 is already locked.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == parent.length</code></li>
	<li><code>2 &lt;= n &lt;= 2000</code></li>
	<li><code>0 &lt;= parent[i] &lt;= n - 1</code> với <code>i != 0</code></li>
	<li><code>parent[0] == -1</code></li>
	<li><code>0 &lt;= num &lt;= n - 1</code></li>
	<li><code>1 &lt;= user &lt;= 10<sup>4</sup></code></li>
	<li><code>parent</code> biểu diễn một cây hợp lệ.</li>
	<li>Có nhiều nhất <code>2000</code> lời gọi đến <code>lock</code>, <code>unlock</code> và <code>upgrade</code> <strong>tổng cộng</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Lock và unlock là các cập nhật tại một node. Upgrade cần node và các node tổ tiên của nó đều được unlock, đồng thời phải có ít nhất một node hậu duệ đang được lock, sau đó unlock cả cây con và lock node hiện tại. Vì $n\le 2000$, ta có thể duyệt theo chuỗi node cha và thực hiện DFS.
>
> Một mảng lưu người dùng đang lock mỗi node, còn danh sách kề lưu các node con. Khi upgrade, trước tiên ta đi ngược lên các node cha, sau đó dùng DFS để unlock các node hậu duệ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class LockingTree:
    def __init__(self, parent: List[int]):
        n = len(parent)
        self.locked = [-1] * n
        self.parent = parent
        self.children = [[] for _ in range(n)]
        for son, fa in enumerate(parent[1:], 1):
            self.children[fa].append(son)

    def lock(self, num: int, user: int) -> bool:
        if self.locked[num] == -1:
            self.locked[num] = user
            return True
        return False

    def unlock(self, num: int, user: int) -> bool:
        if self.locked[num] == user:
            self.locked[num] = -1
            return True
        return False

    def upgrade(self, num: int, user: int) -> bool:
        def dfs(x: int):
            nonlocal find
            for y in self.children[x]:
                if self.locked[y] != -1:
                    self.locked[y] = -1
                    find = True
                dfs(y)

        x = num
        while x != -1:
            if self.locked[x] != -1:
                return False
            x = self.parent[x]

        find = False
        dfs(num)
        if not find:
            return False
        self.locked[num] = user
        return True


# Your LockingTree object will be instantiated and called as such:
# obj = LockingTree(parent)
# param_1 = obj.lock(num,user)
# param_2 = obj.unlock(num,user)
# param_3 = obj.upgrade(num,user)
```

#### Java

```java
class LockingTree {
    private int[] locked;
    private int[] parent;
    private List<Integer>[] children;

    public LockingTree(int[] parent) {
        int n = parent.length;
        locked = new int[n];
        this.parent = parent;
        children = new List[n];
        Arrays.fill(locked, -1);
        Arrays.setAll(children, i -> new ArrayList<>());
        for (int i = 1; i < n; i++) {
            children[parent[i]].add(i);
        }
    }

    public boolean lock(int num, int user) {
        if (locked[num] == -1) {
            locked[num] = user;
            return true;
        }
        return false;
    }

    public boolean unlock(int num, int user) {
        if (locked[num] == user) {
            locked[num] = -1;
            return true;
        }
        return false;
    }

    public boolean upgrade(int num, int user) {
        int x = num;
        while (x != -1) {
            if (locked[x] != -1) {
                return false;
            }
            x = parent[x];
        }
        boolean[] find = new boolean[1];
        dfs(num, find);
        if (!find[0]) {
            return false;
        }
        locked[num] = user;
        return true;
    }

    private void dfs(int x, boolean[] find) {
        for (int y : children[x]) {
            if (locked[y] != -1) {
                locked[y] = -1;
                find[0] = true;
            }
            dfs(y, find);
        }
    }
}

/**
 * Your LockingTree object will be instantiated and called as such:
 * LockingTree obj = new LockingTree(parent);
 * boolean param_1 = obj.lock(num,user);
 * boolean param_2 = obj.unlock(num,user);
 * boolean param_3 = obj.upgrade(num,user);
 */
```

#### C++

```cpp
class LockingTree {
public:
    LockingTree(vector<int>& parent) {
        int n = parent.size();
        locked = vector<int>(n, -1);
        this->parent = parent;
        children.resize(n);
        for (int i = 1; i < n; ++i) {
            children[parent[i]].push_back(i);
        }
    }

    bool lock(int num, int user) {
        if (locked[num] == -1) {
            locked[num] = user;
            return true;
        }
        return false;
    }

    bool unlock(int num, int user) {
        if (locked[num] == user) {
            locked[num] = -1;
            return true;
        }
        return false;
    }

    bool upgrade(int num, int user) {
        int x = num;
        while (x != -1) {
            if (locked[x] != -1) {
                return false;
            }
            x = parent[x];
        }
        bool find = false;
        function<void(int)> dfs = [&](int x) {
            for (int y : children[x]) {
                if (locked[y] != -1) {
                    find = true;
                    locked[y] = -1;
                }
                dfs(y);
            }
        };
        dfs(num);
        if (!find) {
            return false;
        }
        locked[num] = user;
        return true;
    }

private:
    vector<int> locked;
    vector<int> parent;
    vector<vector<int>> children;
};

/**
 * Your LockingTree object will be instantiated and called as such:
 * LockingTree* obj = new LockingTree(parent);
 * bool param_1 = obj->lock(num,user);
 * bool param_2 = obj->unlock(num,user);
 * bool param_3 = obj->upgrade(num,user);
 */
```

#### Go

```go
type LockingTree struct {
	locked   []int
	parent   []int
	children [][]int
}

func Constructor(parent []int) LockingTree {
	n := len(parent)
	locked := make([]int, n)
	for i := range locked {
		locked[i] = -1
	}
	children := make([][]int, n)
	for i := 1; i < n; i++ {
		children[parent[i]] = append(children[parent[i]], i)
	}
	return LockingTree{locked, parent, children}
}

func (this *LockingTree) Lock(num int, user int) bool {
	if this.locked[num] == -1 {
		this.locked[num] = user
		return true
	}
	return false
}

func (this *LockingTree) Unlock(num int, user int) bool {
	if this.locked[num] == user {
		this.locked[num] = -1
		return true
	}
	return false
}

func (this *LockingTree) Upgrade(num int, user int) bool {
	x := num
	for ; x != -1; x = this.parent[x] {
		if this.locked[x] != -1 {
			return false
		}
	}
	find := false
	var dfs func(int)
	dfs = func(x int) {
		for _, y := range this.children[x] {
			if this.locked[y] != -1 {
				find = true
				this.locked[y] = -1
			}
			dfs(y)
		}
	}
	dfs(num)
	if !find {
		return false
	}
	this.locked[num] = user
	return true
}

/**
 * Your LockingTree object will be instantiated and called as such:
 * obj := Constructor(parent);
 * param_1 := obj.Lock(num,user);
 * param_2 := obj.Unlock(num,user);
 * param_3 := obj.Upgrade(num,user);
 */
```

#### TypeScript

```ts
class LockingTree {
    private locked: number[];
    private parent: number[];
    private children: number[][];

    constructor(parent: number[]) {
        const n = parent.length;
        this.locked = Array(n).fill(-1);
        this.parent = parent;
        this.children = Array(n)
            .fill(0)
            .map(() => []);
        for (let i = 1; i < n; i++) {
            this.children[parent[i]].push(i);
        }
    }

    lock(num: number, user: number): boolean {
        if (this.locked[num] === -1) {
            this.locked[num] = user;
            return true;
        }
        return false;
    }

    unlock(num: number, user: number): boolean {
        if (this.locked[num] === user) {
            this.locked[num] = -1;
            return true;
        }
        return false;
    }

    upgrade(num: number, user: number): boolean {
        let x = num;
        for (; x !== -1; x = this.parent[x]) {
            if (this.locked[x] !== -1) {
                return false;
            }
        }
        let find = false;
        const dfs = (x: number) => {
            for (const y of this.children[x]) {
                if (this.locked[y] !== -1) {
                    this.locked[y] = -1;
                    find = true;
                }
                dfs(y);
            }
        };
        dfs(num);
        if (!find) {
            return false;
        }
        this.locked[num] = user;
        return true;
    }
}

/**
 * Your LockingTree object will be instantiated and called as such:
 * var obj = new LockingTree(parent)
 * var param_1 = obj.lock(num,user)
 * var param_2 = obj.unlock(num,user)
 * var param_3 = obj.upgrade(num,user)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
