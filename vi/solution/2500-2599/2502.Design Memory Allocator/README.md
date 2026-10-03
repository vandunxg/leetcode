---
comments: true
difficulty: Medium
rating: 1745
source: Weekly Contest 323 Q3
tags:
    - Design
    - Array
    - Hash Table
    - Simulation
---

<!-- problem:start -->

# [2502. Design Memory Allocator](https://leetcode.com/problems/design-memory-allocator)

[Tài liệu tiếng Trung](/solution/2500-2599/2502.Design%20Memory%20Allocator/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị kích thước của một mảng bộ nhớ <strong>đánh chỉ số từ 0</strong>. Ban đầu, tất cả các đơn vị bộ nhớ đều trống.</p>

<p>Bạn có một bộ cấp phát bộ nhớ với các chức năng sau:</p>

<ol>
	<li><strong>Cấp phát </strong>một block gồm <code>size</code> đơn vị bộ nhớ trống liên tiếp và gán cho block đó id <code>mID</code>.</li>
	<li><strong>Giải phóng</strong> tất cả các đơn vị bộ nhớ có id <code>mID</code> được chỉ định.</li>
</ol>

<p><strong>Lưu ý</strong> rằng:</p>

<ul>
	<li>Có thể cấp phát nhiều block cho cùng một <code>mID</code>.</li>
	<li>Bạn phải giải phóng tất cả các đơn vị bộ nhớ có <code>mID</code>, kể cả khi chúng được cấp phát trong các block khác nhau.</li>
</ul>

<p>Hãy cài đặt lớp <code>Allocator</code>:</p>

<ul>
	<li><code>Allocator(int n)</code> Khởi tạo một đối tượng <code>Allocator</code> với mảng bộ nhớ có kích thước <code>n</code>.</li>
	<li><code>int allocate(int size, int mID)</code> Tìm block <strong>ngoài cùng bên trái</strong> gồm <code>size</code> đơn vị bộ nhớ trống <strong>liên tiếp</strong> và cấp phát block đó với id <code>mID</code>. Trả về chỉ số đầu tiên của block. Nếu không tồn tại block như vậy, trả về <code>-1</code>.</li>
	<li><code>int freeMemory(int mID)</code> Giải phóng tất cả các đơn vị bộ nhớ có id <code>mID</code>. Trả về số đơn vị bộ nhớ đã được giải phóng.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Allocator&quot;, &quot;allocate&quot;, &quot;allocate&quot;, &quot;allocate&quot;, &quot;freeMemory&quot;, &quot;allocate&quot;, &quot;allocate&quot;, &quot;allocate&quot;, &quot;freeMemory&quot;, &quot;allocate&quot;, &quot;freeMemory&quot;]
[[10], [1, 1], [1, 2], [1, 3], [2], [3, 4], [1, 1], [1, 1], [1], [10, 2], [7]]
<strong>Đầu ra</strong>
[null, 0, 1, 2, 1, 3, 1, 6, 3, -1, 0]

<strong>Giải thích</strong>
Allocator loc = new Allocator(10); // Initialize a memory array of size 10. All memory units are initially free.
loc.allocate(1, 1); // The leftmost block&#39;s first index is 0. The memory array becomes [<strong>1</strong>,_,_,_,_,_,_,_,_,_]. We return 0.
loc.allocate(1, 2); // The leftmost block&#39;s first index is 1. The memory array becomes [1,<strong>2</strong>,_,_,_,_,_,_,_,_]. We return 1.
loc.allocate(1, 3); // The leftmost block&#39;s first index is 2. The memory array becomes [1,2,<strong>3</strong>,_,_,_,_,_,_,_]. We return 2.
loc.freeMemory(2); // Free all memory units with mID 2. The memory array becomes [1,_, 3,_,_,_,_,_,_,_]. We return 1 since there is only 1 unit with mID 2.
loc.allocate(3, 4); // The leftmost block&#39;s first index is 3. The memory array becomes [1,_,3,<strong>4</strong>,<strong>4</strong>,<strong>4</strong>,_,_,_,_]. We return 3.
loc.allocate(1, 1); // The leftmost block&#39;s first index is 1. The memory array becomes [1,<strong>1</strong>,3,4,4,4,_,_,_,_]. We return 1.
loc.allocate(1, 1); // The leftmost block&#39;s first index is 6. The memory array becomes [1,1,3,4,4,4,<strong>1</strong>,_,_,_]. We return 6.
loc.freeMemory(1); // Free all memory units with mID 1. The memory array becomes [_,_,3,4,4,4,_,_,_,_]. We return 3 since there are 3 units with mID 1.
loc.allocate(10, 2); // We can not find any free block with 10 consecutive free memory units, so we return -1.
loc.freeMemory(7); // Free all memory units with mID 7. The memory array remains the same since there is no memory unit with mID 7. We return 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n, size, mID &lt;= 1000</code></li>
	<li>Sẽ có nhiều nhất <code>1000</code> lời gọi đến <code>allocate</code> và <code>freeMemory</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Việc cấp phát phải chiếm $\textit{size}$ đơn vị trống liên tiếp tại chỉ số khả thi ngoài cùng bên trái; việc giải phóng trả lại mọi đơn vị đang được giữ bởi $\textit{mID}$. Với $n,q\le 10^3$, duyệt toàn bộ mảng sau mỗi lời gọi chỉ tốn $O(nq)$.
>
> Ta đánh dấu trạng thái sử dụng trong một mảng có độ dài $n$: $0$ biểu thị ô trống. Khi cấp phát, ta đếm các số 0 liên tiếp và ghi $\textit{mID}$ khi đạt $\textit{size}$; khi giải phóng, ta duyệt mảng, xóa các ô bằng $\textit{mID}$ và đếm số ô đó.

<!-- thinking:end -->

Do dữ liệu đầu vào của bài toán không lớn, ta có thể dùng trực tiếp một mảng để mô phỏng không gian bộ nhớ.

Khi khởi tạo, đặt mỗi phần tử trong mảng thành $0$, biểu thị rằng ô đó đang trống.

Khi gọi phương thức `allocate`, duyệt mảng, tìm `size` đơn vị bộ nhớ trống liên tiếp, đặt chúng thành `mID`, rồi trả về chỉ số đầu tiên.

Khi gọi phương thức `free`, duyệt mảng, đặt tất cả các đơn vị bộ nhớ bằng `mID` thành $0$, biểu thị rằng chúng đã được giải phóng.

Độ phức tạp thời gian là $O(n \times q)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là kích thước không gian bộ nhớ và $q$ là số lần gọi phương thức.

<!-- tabs:start -->

#### Python3

```python
class Allocator:

    def __init__(self, n: int):
        self.m = [0] * n

    def allocate(self, size: int, mID: int) -> int:
        cnt = 0
        for i, v in enumerate(self.m):
            if v:
                cnt = 0
            else:
                cnt += 1
                if cnt == size:
                    self.m[i - size + 1 : i + 1] = [mID] * size
                    return i - size + 1
        return -1

    def freeMemory(self, mID: int) -> int:
        ans = 0
        for i, v in enumerate(self.m):
            if v == mID:
                self.m[i] = 0
                ans += 1
        return ans


# Your Allocator object will be instantiated and called as such:
# obj = Allocator(n)
# param_1 = obj.allocate(size,mID)
# param_2 = obj.freeMemory(mID)
```

#### Java

```java
class Allocator {
    private int[] m;

    public Allocator(int n) {
        m = new int[n];
    }

    public int allocate(int size, int mID) {
        int cnt = 0;
        for (int i = 0; i < m.length; ++i) {
            if (m[i] > 0) {
                cnt = 0;
            } else if (++cnt == size) {
                Arrays.fill(m, i - size + 1, i + 1, mID);
                return i - size + 1;
            }
        }
        return -1;
    }

    public int freeMemory(int mID) {
        int ans = 0;
        for (int i = 0; i < m.length; ++i) {
            if (m[i] == mID) {
                m[i] = 0;
                ++ans;
            }
        }
        return ans;
    }
}

/**
 * Your Allocator object will be instantiated and called as such:
 * Allocator obj = new Allocator(n);
 * int param_1 = obj.allocate(size,mID);
 * int param_2 = obj.freeMemory(mID);
 */
```

#### C++

```cpp
class Allocator {
public:
    vector<int> m;

    Allocator(int n) {
        m = vector<int>(n, 0);
    }

    int allocate(int size, int mID) {
        int cnt = 0;
        for (int i = 0; i < m.size(); ++i) {
            if (m[i] > 0) {
                cnt = 0;
            } else if (++cnt == size) {
                fill(m.begin() + i - size + 1, m.begin() + i + 1, mID);
                return i - size + 1;
            }
        }
        return -1;
    }

    int freeMemory(int mID) {
        int ans = 0;
        for (int i = 0; i < m.size(); ++i) {
            if (m[i] == mID) {
                m[i] = 0;
                ++ans;
            }
        }
        return ans;
    }
};

/**
 * Your Allocator object will be instantiated and called as such:
 * Allocator* obj = new Allocator(n);
 * int param_1 = obj->allocate(size,mID);
 * int param_2 = obj->freeMemory(mID);
 */
```

#### Go

```go
type Allocator struct {
	m []int
}

func Constructor(n int) Allocator {
	return Allocator{m: make([]int, n)}
}

func (this *Allocator) Allocate(size int, mID int) int {
	cnt := 0
	for i := 0; i < len(this.m); i++ {
		if this.m[i] > 0 {
			cnt = 0
		} else if cnt++; cnt == size {
			for j := i - size + 1; j <= i; j++ {
				this.m[j] = mID
			}
			return i - size + 1
		}
	}
	return -1
}

func (this *Allocator) FreeMemory(mID int) int {
	ans := 0
	for i := 0; i < len(this.m); i++ {
		if this.m[i] == mID {
			this.m[i] = 0
			ans++
		}
	}
	return ans
}

/**
 * Your Allocator object will be instantiated and called as such:
 * obj := Constructor(n);
 * param_1 = obj.Allocate(size,mID);
 * param_2 = obj.FreeMemory(mID);
 */
```

#### TypeScript

```ts
class Allocator {
    private m: number[];

    constructor(n: number) {
        this.m = Array(n).fill(0);
    }

    allocate(size: number, mID: number): number {
        let cnt = 0;
        for (let i = 0; i < this.m.length; i++) {
            if (this.m[i] > 0) {
                cnt = 0;
            } else if (++cnt === size) {
                for (let j = i - size + 1; j <= i; j++) {
                    this.m[j] = mID;
                }
                return i - size + 1;
            }
        }
        return -1;
    }

    freeMemory(mID: number): number {
        let ans = 0;
        for (let i = 0; i < this.m.length; i++) {
            if (this.m[i] === mID) {
                this.m[i] = 0;
                ans++;
            }
        }
        return ans;
    }
}

/**
 * Your Allocator object will be instantiated and called as such:
 * var obj = new Allocator(n)
 * var param_1 = obj.allocate(size,mID)
 * var param_2 = obj.freeMemory(mID)
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hash Table + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt toàn bộ bộ nhớ sau mỗi lời gọi. Nếu chỉ lưu các đoạn đang được sử dụng, các đoạn trống sẽ là khoảng cách giữa hai block kề nhau.
>
> Ta lưu các đoạn đã cấp phát trong một danh sách được sắp xếp, cùng hai phần tử canh $(-1,-1)$ và $(n,n)$. Khi cấp phát, chọn khoảng trống đầu tiên có độ dài đủ lớn; một hash map ghi lại các interval của từng $\textit{mID}$ để việc giải phóng có thể xóa các block đó. Mỗi lời gọi có độ phức tạp logarithmic theo số block đang được sử dụng.

<!-- thinking:end -->

Ta có thể dùng một ordered set để duy trì chỉ số bắt đầu và kết thúc của tất cả các đơn vị bộ nhớ đã cấp phát, trong đó chỉ số bắt đầu là key và chỉ số kết thúc là value. Ngoài ra, ta dùng một hash table để lưu `mID` và chỉ số bắt đầu tương ứng của đơn vị bộ nhớ.

Khi gọi phương thức `allocate`, duyệt ordered set, tìm interval trống đầu tiên có độ dài lớn hơn hoặc bằng `size`, cấp phát interval đó cho `mID`, rồi cập nhật ordered set. Sau đó, thêm `mID` và chỉ số bắt đầu tương ứng của đơn vị bộ nhớ vào hash table.

Khi gọi phương thức `free`, tìm chỉ số bắt đầu của đơn vị bộ nhớ tương ứng với `mID` từ hash table, sau đó xóa đơn vị đó khỏi ordered set và xóa `mID` khỏi hash table.

Độ phức tạp thời gian là $O(q \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là kích thước không gian bộ nhớ và $q$ là số lần gọi phương thức.

<!-- tabs:start -->

#### Python3

```python
class Allocator:

    def __init__(self, n: int):
        self.sl = SortedList([(-1, -1), (n, n)])
        self.d = defaultdict(list)

    def allocate(self, size: int, mID: int) -> int:
        for (_, s), (e, _) in pairwise(self.sl):
            s, e = s + 1, e - 1
            if e - s + 1 >= size:
                self.sl.add((s, s + size - 1))
                self.d[mID].append((s, s + size - 1))
                return s
        return -1

    def freeMemory(self, mID: int) -> int:
        ans = 0
        for block in self.d[mID]:
            self.sl.remove(block)
            ans += block[1] - block[0] + 1
        del self.d[mID]
        return ans


# Your Allocator object will be instantiated and called as such:
# obj = Allocator(n)
# param_1 = obj.allocate(size,mID)
# param_2 = obj.freeMemory(mID)
```

#### Java

```java
class Allocator {
    private TreeMap<Integer, Integer> tm = new TreeMap<>();
    private Map<Integer, List<Integer>> d = new HashMap<>();

    public Allocator(int n) {
        tm.put(-1, -1);
        tm.put(n, n);
    }

    public int allocate(int size, int mID) {
        int s = -1;
        for (var entry : tm.entrySet()) {
            int v = entry.getKey();
            if (s != -1) {
                int e = v - 1;
                if (e - s + 1 >= size) {
                    tm.put(s, s + size - 1);
                    d.computeIfAbsent(mID, k -> new ArrayList<>()).add(s);
                    return s;
                }
            }
            s = entry.getValue() + 1;
        }
        return -1;
    }

    public int freeMemory(int mID) {
        int ans = 0;
        for (int s : d.getOrDefault(mID, List.of())) {
            int e = tm.remove(s);
            ans += e - s + 1;
        }
        d.remove(mID);
        return ans;
    }
}

/**
 * Your Allocator object will be instantiated and called as such:
 * Allocator obj = new Allocator(n);
 * int param_1 = obj.allocate(size,mID);
 * int param_2 = obj.freeMemory(mID);
 */
```

#### C++

```cpp
class Allocator {
public:
    Allocator(int n) {
        tm[-1] = -1;
        tm[n] = n;
    }

    int allocate(int size, int mID) {
        int s = -1;
        for (auto& [v, c] : tm) {
            if (s != -1) {
                int e = v - 1;
                if (e - s + 1 >= size) {
                    tm[s] = s + size - 1;
                    d[mID].emplace_back(s);
                    return s;
                }
            }
            s = c + 1;
        }
        return -1;
    }

    int freeMemory(int mID) {
        int ans = 0;
        for (int& s : d[mID]) {
            int e = tm[s];
            tm.erase(s);
            ans += e - s + 1;
        }
        d.erase(mID);
        return ans;
    }

private:
    map<int, int> tm;
    unordered_map<int, vector<int>> d;
};

/**
 * Your Allocator object will be instantiated and called as such:
 * Allocator* obj = new Allocator(n);
 * int param_1 = obj->allocate(size,mID);
 * int param_2 = obj->freeMemory(mID);
 */
```

#### Go

```go
type Allocator struct {
	rbt *redblacktree.Tree
	d   map[int][]int
}

func Constructor(n int) Allocator {
	rbt := redblacktree.NewWithIntComparator()
	rbt.Put(-1, -1)
	rbt.Put(n, n)
	return Allocator{rbt, map[int][]int{}}
}

func (this *Allocator) Allocate(size int, mID int) int {
	s := -1
	it := this.rbt.Iterator()
	for it.Next() {
		v := it.Key().(int)
		if s != -1 {
			e := v - 1
			if e-s+1 >= size {
				this.rbt.Put(s, s+size-1)
				this.d[mID] = append(this.d[mID], s)
				return s
			}
		}
		s = it.Value().(int) + 1
	}
	return -1
}

func (this *Allocator) FreeMemory(mID int) int {
	ans := 0
	for _, s := range this.d[mID] {
		if e, ok := this.rbt.Get(s); ok {
			this.rbt.Remove(s)
			ans += e.(int) - s + 1
		}
	}
	this.d[mID] = []int{}
	return ans
}

/**
 * Your Allocator object will be instantiated and called as such:
 * obj := Constructor(n);
 * param_1 = obj.Allocate(size,mID);
 * param_2 = obj.FreeMemory(mID);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
