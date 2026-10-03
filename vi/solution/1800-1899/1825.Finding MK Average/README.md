---
comments: true
difficulty: Hard
rating: 2395
source: Weekly Contest 236 Q4
tags:
    - Design
    - Queue
    - Data Stream
    - Ordered Set
    - Treap
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1825. Finding MK Average](https://leetcode.com/problems/finding-mk-average)

[中文文档](/solution/1800-1899/1825.Finding%20MK%20Average/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>m</code> và <code>k</code>, cùng một stream số nguyên. Nhiệm vụ của bạn là triển khai một cấu trúc dữ liệu tính <strong>MKAverage</strong> cho stream này.</p>

<p>Có thể tính <strong>MKAverage</strong> theo các bước sau:</p>

<ol>
	<li>Nếu số phần tử trong stream nhỏ hơn <code>m</code>, hãy coi <strong>MKAverage</strong> bằng <code>-1</code>. Nếu không, sao chép <code>m</code> phần tử cuối của stream vào một container riêng.</li>
	<li>Xóa <code>k</code> phần tử nhỏ nhất và <code>k</code> phần tử lớn nhất khỏi container.</li>
	<li>Tính giá trị trung bình của các phần tử còn lại, <strong>làm tròn xuống số nguyên gần nhất</strong>.</li>
</ol>

<p>Triển khai lớp <code>MKAverage</code>:</p>

<ul>
	<li><code>MKAverage(int m, int k)</code> Khởi tạo đối tượng <strong>MKAverage</strong> với một stream rỗng và hai số nguyên <code>m</code>, <code>k</code>.</li>
	<li><code>void addElement(int num)</code> Chèn phần tử mới <code>num</code> vào stream.</li>
	<li><code>int calculateMKAverage()</code> Tính và trả về <strong>MKAverage</strong> của stream hiện tại, <strong>làm tròn xuống số nguyên gần nhất</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;MKAverage&quot;, &quot;addElement&quot;, &quot;addElement&quot;, &quot;calculateMKAverage&quot;, &quot;addElement&quot;, &quot;calculateMKAverage&quot;, &quot;addElement&quot;, &quot;addElement&quot;, &quot;addElement&quot;, &quot;calculateMKAverage&quot;]
[[3, 1], [3], [1], [], [10], [], [5], [5], [5], []]
<strong>Đầu ra</strong>
[null, null, null, -1, null, 3, null, null, null, 5]

<strong>Giải thích</strong>
<code>MKAverage obj = new MKAverage(3, 1);
obj.addElement(3);        // current elements are [3]
obj.addElement(1);        // current elements are [3,1]
obj.calculateMKAverage(); // return -1, because m = 3 and only 2 elements exist.
obj.addElement(10);       // current elements are [3,1,10]
obj.calculateMKAverage(); // The last 3 elements are [3,1,10].
                          // After removing smallest and largest 1 element the container will be [3].
                          // The average of [3] equals 3/1 = 3, return 3
obj.addElement(5);        // current elements are [3,1,10,5]
obj.addElement(5);        // current elements are [3,1,10,5,5]
obj.addElement(5);        // current elements are [3,1,10,5,5,5]
obj.calculateMKAverage(); // The last 3 elements are [5,5,5].
                          // After removing smallest and largest 1 element the container will be [5].
                          // The average of [5] equals 5/1 = 5, return 5
</code></pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= m &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt; k*2 &lt; m</code></li>
	<li><code>1 &lt;= num &lt;= 10<sup>5</sup></code></li>
	<li>Sẽ có nhiều nhất <code>10<sup>5</sup></code> lần gọi đến <code>addElement</code> và <code>calculateMKAverage</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ordered Set + Queue

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần trung bình của một cửa sổ độ dài $m$ sau khi bỏ $k$ giá trị nhỏ nhất và $k$ giá trị lớn nhất, trong khi phải chèn phần tử thường xuyên. Sắp xếp lại cửa sổ mỗi lần có độ phức tạp $O(m\log m)$ và quá chậm.
>
> Một queue lưu thứ tự chèn. Ba ordered multiset chứa lần lượt $k$ giá trị nhỏ nhất, đoạn giữa và $k$ giá trị lớn nhất, cùng với tổng đoạn giữa $s$. Sau mỗi lần chèn hoặc lấy ra, ta chuyển các phần tử dư để hai đầu đều có kích thước $k$. Giá trị trung bình là $s/(m-2k)$.

<!-- thinking:end -->

Ta có thể duy trì các cấu trúc dữ liệu hoặc biến sau:

- Một queue $q$ có độ dài $m$, trong đó đầu queue là phần tử được thêm sớm nhất và cuối queue là phần tử mới được thêm gần đây nhất;
- Ba ordered set là $lo$, $mid$, $hi$, trong đó $lo$ và $hi$ lần lượt lưu $k$ phần tử nhỏ nhất và $k$ phần tử lớn nhất, còn $mid$ lưu các phần tử còn lại;
- Một biến $s$ lưu tổng tất cả phần tử trong $mid$;
- Một số ngôn ngữ lập trình (chẳng hạn Java, Go) duy trì thêm hai biến $size1$ và $size3$, lần lượt biểu diễn số phần tử trong $lo$ và $hi$.

Khi gọi hàm $addElement(num)$, thực hiện lần lượt các thao tác sau:

1. Nếu $lo$ rỗng hoặc $num \leq max(lo)$, thêm $num$ vào $lo$; nếu không, nếu $hi$ rỗng hoặc $num \geq min(hi)$, thêm $num$ vào $hi$; còn lại thêm $num$ vào $mid$ và cộng giá trị $num$ vào $s$.
1. Tiếp theo, thêm $num$ vào queue $q$. Nếu lúc này độ dài queue $q$ lớn hơn $m$, lấy phần tử đầu $x$ khỏi queue $q$, sau đó tìm một trong $lo$, $mid$ hoặc $hi$ đang chứa $x$ và xóa $x$ khỏi tập đó. Nếu tập là $mid$, trừ giá trị $x$ khỏi $s$.
1. Nếu độ dài $lo$ lớn hơn $k$, liên tục lấy giá trị lớn nhất $max(lo)$ khỏi $lo$, thêm $max(lo)$ vào $mid$ và cộng giá trị $max(lo)$ vào $s$.
1. Nếu độ dài $hi$ lớn hơn $k$, liên tục lấy giá trị nhỏ nhất $min(hi)$ khỏi $hi$, thêm $min(hi)$ vào $mid$ và cộng giá trị $min(hi)$ vào $s$.
1. Nếu độ dài $lo$ nhỏ hơn $k$ và $mid$ không rỗng, liên tục lấy giá trị nhỏ nhất $min(mid)$ khỏi $mid$, thêm $min(mid)$ vào $lo$ và trừ giá trị $min(mid)$ khỏi $s$.
1. Nếu độ dài $hi$ nhỏ hơn $k$ và $mid$ không rỗng, liên tục lấy giá trị lớn nhất $max(mid)$ khỏi $mid$, thêm $max(mid)$ vào $hi$ và trừ giá trị $max(mid)$ khỏi $s$.

Khi gọi hàm $calculateMKAverage()$, nếu độ dài $q$ nhỏ hơn $m$, trả về $-1$; nếu không, trả về $\frac{s}{m - 2k}$.

Về độ phức tạp thời gian, mỗi lần gọi hàm $addElement(num)$ có độ phức tạp $O(\log m)$, còn mỗi lần gọi hàm $calculateMKAverage()$ có độ phức tạp $O(1)$. Độ phức tạp không gian là $O(m)$.

<!-- tabs:start -->

#### Python3

```python
class MKAverage:
    def __init__(self, m: int, k: int):
        self.m = m
        self.k = k
        self.s = 0
        self.q = deque()
        self.lo = SortedList()
        self.mid = SortedList()
        self.hi = SortedList()

    def addElement(self, num: int) -> None:
        if not self.lo or num <= self.lo[-1]:
            self.lo.add(num)
        elif not self.hi or num >= self.hi[0]:
            self.hi.add(num)
        else:
            self.mid.add(num)
            self.s += num
        self.q.append(num)
        if len(self.q) > self.m:
            x = self.q.popleft()
            if x in self.lo:
                self.lo.remove(x)
            elif x in self.hi:
                self.hi.remove(x)
            else:
                self.mid.remove(x)
                self.s -= x
        while len(self.lo) > self.k:
            x = self.lo.pop()
            self.mid.add(x)
            self.s += x
        while len(self.hi) > self.k:
            x = self.hi.pop(0)
            self.mid.add(x)
            self.s += x
        while len(self.lo) < self.k and self.mid:
            x = self.mid.pop(0)
            self.lo.add(x)
            self.s -= x
        while len(self.hi) < self.k and self.mid:
            x = self.mid.pop()
            self.hi.add(x)
            self.s -= x

    def calculateMKAverage(self) -> int:
        return -1 if len(self.q) < self.m else self.s // (self.m - 2 * self.k)


# Your MKAverage object will be instantiated and called as such:
# obj = MKAverage(m, k)
# obj.addElement(num)
# param_2 = obj.calculateMKAverage()
```

#### Java

```java
class MKAverage {

    private int m, k;
    private long s;
    private int size1, size3;
    private Deque<Integer> q = new ArrayDeque<>();
    private TreeMap<Integer, Integer> lo = new TreeMap<>();
    private TreeMap<Integer, Integer> mid = new TreeMap<>();
    private TreeMap<Integer, Integer> hi = new TreeMap<>();

    public MKAverage(int m, int k) {
        this.m = m;
        this.k = k;
    }

    public void addElement(int num) {
        if (lo.isEmpty() || num <= lo.lastKey()) {
            lo.merge(num, 1, Integer::sum);
            ++size1;
        } else if (hi.isEmpty() || num >= hi.firstKey()) {
            hi.merge(num, 1, Integer::sum);
            ++size3;
        } else {
            mid.merge(num, 1, Integer::sum);
            s += num;
        }
        q.offer(num);
        if (q.size() > m) {
            int x = q.poll();
            if (lo.containsKey(x)) {
                if (lo.merge(x, -1, Integer::sum) == 0) {
                    lo.remove(x);
                }
                --size1;
            } else if (hi.containsKey(x)) {
                if (hi.merge(x, -1, Integer::sum) == 0) {
                    hi.remove(x);
                }
                --size3;
            } else {
                if (mid.merge(x, -1, Integer::sum) == 0) {
                    mid.remove(x);
                }
                s -= x;
            }
        }
        for (; size1 > k; --size1) {
            int x = lo.lastKey();
            if (lo.merge(x, -1, Integer::sum) == 0) {
                lo.remove(x);
            }
            mid.merge(x, 1, Integer::sum);
            s += x;
        }
        for (; size3 > k; --size3) {
            int x = hi.firstKey();
            if (hi.merge(x, -1, Integer::sum) == 0) {
                hi.remove(x);
            }
            mid.merge(x, 1, Integer::sum);
            s += x;
        }
        for (; size1 < k && !mid.isEmpty(); ++size1) {
            int x = mid.firstKey();
            if (mid.merge(x, -1, Integer::sum) == 0) {
                mid.remove(x);
            }
            s -= x;
            lo.merge(x, 1, Integer::sum);
        }
        for (; size3 < k && !mid.isEmpty(); ++size3) {
            int x = mid.lastKey();
            if (mid.merge(x, -1, Integer::sum) == 0) {
                mid.remove(x);
            }
            s -= x;
            hi.merge(x, 1, Integer::sum);
        }
    }

    public int calculateMKAverage() {
        return q.size() < m ? -1 : (int) (s / (q.size() - k * 2));
    }
}

/**
 * Your MKAverage object will be instantiated and called as such:
 * MKAverage obj = new MKAverage(m, k);
 * obj.addElement(num);
 * int param_2 = obj.calculateMKAverage();
 */
```

#### C++

```cpp
class MKAverage {
public:
    MKAverage(int m, int k) {
        this->m = m;
        this->k = k;
    }

    void addElement(int num) {
        if (lo.empty() || num <= *lo.rbegin()) {
            lo.insert(num);
        } else if (hi.empty() || num >= *hi.begin()) {
            hi.insert(num);
        } else {
            mid.insert(num);
            s += num;
        }

        q.push(num);
        if (q.size() > m) {
            int x = q.front();
            q.pop();
            if (lo.find(x) != lo.end()) {
                lo.erase(lo.find(x));
            } else if (hi.find(x) != hi.end()) {
                hi.erase(hi.find(x));
            } else {
                mid.erase(mid.find(x));
                s -= x;
            }
        }
        while (lo.size() > k) {
            int x = *lo.rbegin();
            lo.erase(prev(lo.end()));
            mid.insert(x);
            s += x;
        }
        while (hi.size() > k) {
            int x = *hi.begin();
            hi.erase(hi.begin());
            mid.insert(x);
            s += x;
        }
        while (lo.size() < k && mid.size()) {
            int x = *mid.begin();
            mid.erase(mid.begin());
            s -= x;
            lo.insert(x);
        }
        while (hi.size() < k && mid.size()) {
            int x = *mid.rbegin();
            mid.erase(prev(mid.end()));
            s -= x;
            hi.insert(x);
        }
    }

    int calculateMKAverage() {
        return q.size() < m ? -1 : s / (q.size() - k * 2);
    }

private:
    int m, k;
    long long s = 0;
    queue<int> q;
    multiset<int> lo, mid, hi;
};

/**
 * Your MKAverage object will be instantiated and called as such:
 * MKAverage* obj = new MKAverage(m, k);
 * obj->addElement(num);
 * int param_2 = obj->calculateMKAverage();
 */
```

#### Go

```go
type MKAverage struct {
	lo, mid, hi  *redblacktree.Tree
	q            []int
	m, k, s      int
	size1, size3 int
}

func Constructor(m int, k int) MKAverage {
	lo := redblacktree.NewWithIntComparator()
	mid := redblacktree.NewWithIntComparator()
	hi := redblacktree.NewWithIntComparator()
	return MKAverage{lo, mid, hi, []int{}, m, k, 0, 0, 0}
}

func (this *MKAverage) AddElement(num int) {
	merge := func(rbt *redblacktree.Tree, key, value int) {
		if v, ok := rbt.Get(key); ok {
			nxt := v.(int) + value
			if nxt == 0 {
				rbt.Remove(key)
			} else {
				rbt.Put(key, nxt)
			}
		} else {
			rbt.Put(key, value)
		}
	}

	if this.lo.Empty() || num <= this.lo.Right().Key.(int) {
		merge(this.lo, num, 1)
		this.size1++
	} else if this.hi.Empty() || num >= this.hi.Left().Key.(int) {
		merge(this.hi, num, 1)
		this.size3++
	} else {
		merge(this.mid, num, 1)
		this.s += num
	}
	this.q = append(this.q, num)
	if len(this.q) > this.m {
		x := this.q[0]
		this.q = this.q[1:]
		if _, ok := this.lo.Get(x); ok {
			merge(this.lo, x, -1)
			this.size1--
		} else if _, ok := this.hi.Get(x); ok {
			merge(this.hi, x, -1)
			this.size3--
		} else {
			merge(this.mid, x, -1)
			this.s -= x
		}
	}
	for ; this.size1 > this.k; this.size1-- {
		x := this.lo.Right().Key.(int)
		merge(this.lo, x, -1)
		merge(this.mid, x, 1)
		this.s += x
	}
	for ; this.size3 > this.k; this.size3-- {
		x := this.hi.Left().Key.(int)
		merge(this.hi, x, -1)
		merge(this.mid, x, 1)
		this.s += x
	}
	for ; this.size1 < this.k && !this.mid.Empty(); this.size1++ {
		x := this.mid.Left().Key.(int)
		merge(this.mid, x, -1)
		this.s -= x
		merge(this.lo, x, 1)
	}
	for ; this.size3 < this.k && !this.mid.Empty(); this.size3++ {
		x := this.mid.Right().Key.(int)
		merge(this.mid, x, -1)
		this.s -= x
		merge(this.hi, x, 1)
	}
}

func (this *MKAverage) CalculateMKAverage() int {
	if len(this.q) < this.m {
		return -1
	}
	return this.s / (this.m - 2*this.k)
}

/**
 * Your MKAverage object will be instantiated and called as such:
 * obj := Constructor(m, k);
 * obj.AddElement(num);
 * param_2 := obj.CalculateMKAverage();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Một Ordered Set + Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 phải quản lý ba cây và các bất biến kích thước của chúng. Chỉ một danh sách có thứ tự của cửa sổ là đủ: xây dựng nó khi cửa sổ đầy lần đầu, sau đó điều chỉnh $s$ theo thứ hạng của giá trị được chèn và xóa. Cấu trúc đơn giản hơn và mỗi lần cập nhật vẫn có độ phức tạp logarit.

<!-- thinking:end -->

Dùng một queue để lưu thứ tự chèn và một ordered set cho cửa sổ hiện tại có độ dài $m$. Khi cửa sổ đầy lần đầu, xây dựng set và tính tổng đoạn giữa sau khi bỏ k phần tử nhỏ nhất và k phần tử lớn nhất. Các lần chèn và xóa sau đó cập nhật tổng đoạn giữa dựa trên thứ hạng của phần tử trong set.

Không giống phương pháp 1, vốn chia cửa sổ thành ba tập, cách này chỉ duy trì một dãy có thứ tự.

Mỗi lần gọi `addElement` mất $O(\log m)$ thời gian, còn mỗi lần gọi `calculateMKAverage` mất $O(1)$ thời gian. Độ phức tạp không gian là $O(m)$.

Trong Java, C++ và Go, $num\le 10^5$, nên Fenwick tree tần suất hỗ trợ các query về thứ hạng và phần tử thứ $k$ trong thời gian logarit, tương ứng với các thao tác của ordered set.

<!-- tabs:start -->

#### Python3

```python
class MKAverage:
    def __init__(self, m: int, k: int):
        self.m = m
        self.k = k
        self.sl = SortedList()
        self.q = deque()
        self.s = 0

    def addElement(self, num: int) -> None:
        self.q.append(num)
        if len(self.q) == self.m:
            self.sl = SortedList(self.q)
            self.s = sum(self.sl[self.k : -self.k])
        elif len(self.q) > self.m:
            i = self.sl.bisect_left(num)
            if i < self.k:
                self.s += self.sl[self.k - 1]
            elif self.k <= i <= self.m - self.k:
                self.s += num
            else:
                self.s += self.sl[self.m - self.k]
            self.sl.add(num)

            x = self.q.popleft()
            i = self.sl.bisect_left(x)
            if i < self.k:
                self.s -= self.sl[self.k]
            elif self.k <= i <= self.m - self.k:
                self.s -= x
            else:
                self.s -= self.sl[self.m - self.k]
            self.sl.remove(x)

    def calculateMKAverage(self) -> int:
        return -1 if len(self.sl) < self.m else self.s // (self.m - self.k * 2)


# Your MKAverage object will be instantiated and called as such:
# obj = MKAverage(m, k)
# obj.addElement(num)
# param_2 = obj.calculateMKAverage()
```

#### Java

```java
class MKAverage {
    private static final int N = 100001;
    private final int m, k;
    private long s;
    private final Deque<Integer> q = new ArrayDeque<>();
    private final int[] c = new int[N];

    public MKAverage(int m, int k) {
        this.m = m;
        this.k = k;
    }

    public void addElement(int num) {
        q.offer(num);
        if (q.size() == m) {
            for (int x : q) {
                update(x, 1);
            }
            for (int i = k; i < m - k; ++i) {
                s += kth(i);
            }
        } else if (q.size() > m) {
            int i = rank(num);
            if (i < k) {
                s += kth(k - 1);
            } else if (i <= m - k) {
                s += num;
            } else {
                s += kth(m - k);
            }
            update(num, 1);

            int x = q.poll();
            i = rank(x);
            if (i < k) {
                s -= kth(k);
            } else if (i <= m - k) {
                s -= x;
            } else {
                s -= kth(m - k);
            }
            update(x, -1);
        }
    }

    public int calculateMKAverage() {
        return q.size() < m ? -1 : (int) (s / (m - k * 2));
    }

    private void update(int x, int d) {
        for (; x < N; x += x & -x) {
            c[x] += d;
        }
    }

    private int query(int x) {
        int ans = 0;
        for (; x > 0; x -= x & -x) {
            ans += c[x];
        }
        return ans;
    }

    private int rank(int x) {
        return query(x - 1);
    }

    private int kth(int k) {
        int need = k + 1;
        int idx = 0;
        for (int p = 1 << 16; p > 0; p >>= 1) {
            int nxt = idx + p;
            if (nxt < N && c[nxt] < need) {
                need -= c[nxt];
                idx = nxt;
            }
        }
        return idx + 1;
    }
}
```

#### C++

```cpp
class MKAverage {
public:
    MKAverage(int m, int k) {
        this->m = m;
        this->k = k;
    }

    void addElement(int num) {
        q.push_back(num);
        if ((int) q.size() == m) {
            for (int x : q) {
                update(x, 1);
            }
            for (int i = k; i < m - k; ++i) {
                s += kth(i);
            }
        } else if ((int) q.size() > m) {
            int i = rank(num);
            if (i < k) {
                s += kth(k - 1);
            } else if (i <= m - k) {
                s += num;
            } else {
                s += kth(m - k);
            }
            update(num, 1);

            int x = q.front();
            q.pop_front();
            i = rank(x);
            if (i < k) {
                s -= kth(k);
            } else if (i <= m - k) {
                s -= x;
            } else {
                s -= kth(m - k);
            }
            update(x, -1);
        }
    }

    int calculateMKAverage() {
        return (int) q.size() < m ? -1 : s / (m - k * 2);
    }

private:
    static const int N = 100001;
    int m, k;
    long long s = 0;
    deque<int> q;
    int c[N]{};

    void update(int x, int d) {
        for (; x < N; x += x & -x) {
            c[x] += d;
        }
    }

    int query(int x) {
        int ans = 0;
        for (; x > 0; x -= x & -x) {
            ans += c[x];
        }
        return ans;
    }

    int rank(int x) {
        return query(x - 1);
    }

    int kth(int k) {
        int need = k + 1, idx = 0;
        for (int p = 1 << 16; p > 0; p >>= 1) {
            int nxt = idx + p;
            if (nxt < N && c[nxt] < need) {
                need -= c[nxt];
                idx = nxt;
            }
        }
        return idx + 1;
    }
};
```

#### Go

```go
type MKAverage struct {
	m, k int
	s    int
	q    []int
	c    []int
}

func Constructor(m int, k int) MKAverage {
	return MKAverage{m: m, k: k, c: make([]int, 100001)}
}

func (this *MKAverage) AddElement(num int) {
	this.q = append(this.q, num)
	if len(this.q) == this.m {
		for _, x := range this.q {
			this.update(x, 1)
		}
		for i := this.k; i < this.m-this.k; i++ {
			this.s += this.kth(i)
		}
	} else if len(this.q) > this.m {
		i := this.rank(num)
		if i < this.k {
			this.s += this.kth(this.k - 1)
		} else if i <= this.m-this.k {
			this.s += num
		} else {
			this.s += this.kth(this.m - this.k)
		}
		this.update(num, 1)

		x := this.q[0]
		this.q = this.q[1:]
		i = this.rank(x)
		if i < this.k {
			this.s -= this.kth(this.k)
		} else if i <= this.m-this.k {
			this.s -= x
		} else {
			this.s -= this.kth(this.m - this.k)
		}
		this.update(x, -1)
	}
}

func (this *MKAverage) CalculateMKAverage() int {
	if len(this.q) < this.m {
		return -1
	}
	return this.s / (this.m - this.k*2)
}

func (this *MKAverage) update(x, d int) {
	for ; x < len(this.c); x += x & -x {
		this.c[x] += d
	}
}

func (this *MKAverage) query(x int) (ans int) {
	for ; x > 0; x -= x & -x {
		ans += this.c[x]
	}
	return
}

func (this *MKAverage) rank(x int) int {
	return this.query(x - 1)
}

func (this *MKAverage) kth(k int) int {
	need, idx := k+1, 0
	for p := 1 << 16; p > 0; p >>= 1 {
		nxt := idx + p
		if nxt < len(this.c) && this.c[nxt] < need {
			need -= this.c[nxt]
			idx = nxt
		}
	}
	return idx + 1
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
