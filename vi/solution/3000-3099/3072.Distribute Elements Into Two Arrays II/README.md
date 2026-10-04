---
comments: true
difficulty: Hard
rating: 2052
source: Weekly Contest 387 Q4
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Simulation
---

<!-- problem:start -->

# [3072. Distribute Elements Into Two Arrays II](https://leetcode.com/problems/distribute-elements-into-two-arrays-ii)

[中文文档](/solution/3000-3099/3072.Distribute%20Elements%20Into%20Two%20Arrays%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>1</strong>, có độ dài <code>n</code>.</p>

<p>Ta định nghĩa hàm <code>greaterCount</code> sao cho <code>greaterCount(arr, val)</code> trả về số phần tử trong <code>arr</code> <strong>lớn hơn nghiêm ngặt</strong> <code>val</code>.</p>

<p>Bạn cần phân phối tất cả phần tử của <code>nums</code> vào hai mảng <code>arr1</code> và <code>arr2</code> bằng <code>n</code> thao tác. Ở thao tác đầu tiên, thêm <code>nums[1]</code> vào <code>arr1</code>. Ở thao tác thứ hai, thêm <code>nums[2]</code> vào <code>arr2</code>. Sau đó, ở thao tác thứ <code>i<sup>th</sup></code>:</p>

<ul>
	<li>Nếu <code>greaterCount(arr1, nums[i]) &gt; greaterCount(arr2, nums[i])</code>, thêm <code>nums[i]</code> vào <code>arr1</code>.</li>
	<li>Nếu <code>greaterCount(arr1, nums[i]) &lt; greaterCount(arr2, nums[i])</code>, thêm <code>nums[i]</code> vào <code>arr2</code>.</li>
	<li>Nếu <code>greaterCount(arr1, nums[i]) == greaterCount(arr2, nums[i])</code>, thêm <code>nums[i]</code> vào mảng có số phần tử <strong>ít hơn</strong>.</li>
	<li>Nếu vẫn hòa, thêm <code>nums[i]</code> vào <code>arr1</code>.</li>
</ul>

<p>Mảng <code>result</code> được tạo thành bằng cách nối hai mảng <code>arr1</code> và <code>arr2</code>. Ví dụ, nếu <code>arr1 == [1,2,3]</code> và <code>arr2 == [4,5,6]</code>, thì <code>result = [1,2,3,4,5,6]</code>.</p>

<p>Hãy trả về <em>mảng số nguyên</em> <code>result</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,3,3]
<strong>Đầu ra:</strong> [2,3,1,3]
<strong>Giải thích:</strong> Sau 2 thao tác đầu tiên, arr1 = [2] và arr2 = [1].
Ở thao tác thứ 3<sup>rd</sup>, số phần tử lớn hơn 3 trong cả hai mảng đều bằng 0. Ngoài ra, độ dài hai mảng bằng nhau, nên thêm nums[3] vào arr1.
Ở thao tác thứ 4<sup>th</sup>, số phần tử lớn hơn 3 trong cả hai mảng đều bằng 0. Vì arr2 có độ dài nhỏ hơn, nên thêm nums[4] vào arr2.
Sau 4 thao tác, arr1 = [2,3] và arr2 = [1,3].
Do đó, mảng result được tạo thành bằng cách nối hai mảng là [2,3,1,3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,14,3,1,2]
<strong>Đầu ra:</strong> [5,3,1,2,14]
<strong>Giải thích:</strong> Sau 2 thao tác đầu tiên, arr1 = [5] và arr2 = [14].
Ở thao tác thứ 3<sup>rd</sup>, số phần tử lớn hơn 3 trong cả hai mảng đều bằng một. Ngoài ra, độ dài hai mảng bằng nhau, nên thêm nums[3] vào arr1.
Ở thao tác thứ 4<sup>th</sup>, số phần tử lớn hơn 1 trong arr1 nhiều hơn arr2 (2 &gt; 1). Do đó, thêm nums[4] vào arr1.
Ở thao tác thứ 5<sup>th</sup>, số phần tử lớn hơn 2 trong arr1 nhiều hơn arr2 (2 &gt; 1). Do đó, thêm nums[5] vào arr1.
Sau 5 thao tác, arr1 = [5,3,1,2] và arr2 = [14].
Do đó, mảng result được tạo thành bằng cách nối hai mảng là [5,3,1,2,14].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,3,3]
<strong>Đầu ra:</strong> [3,3,3,3]
<strong>Giải thích:</strong> Sau 4 thao tác, arr1 = [3,3] và arr2 = [3,3].
Do đó, mảng result được tạo thành bằng cách nối hai mảng là [3,3,3,3].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Rời rạc hóa + Binary Indexed Tree

<!-- thinking:start -->

> **Tư duy**
>
> Quy tắc hiện tại so sánh số lượng giá trị hiện có lớn hơn nghiêm ngặt, trong khi $n \le 10^5$, nên việc duyệt từng mảng sẽ quá chậm.
>
> Số lượng phần tử lớn hơn bằng độ dài mảng trừ đi số lượng giá trị $\le x$, và Fenwick tree có thể duy trì số lượng này sau khi nén tọa độ.
>
> Ta dùng hai Fenwick tree cho hai mảng; chèn $x$ theo kết quả so sánh, và dùng độ dài mảng để phá hòa.

<!-- thinking:end -->

Ta có thể dùng hai binary indexed tree `tree1` và `tree2` để duy trì số phần tử trong `arr1` và `arr2` nhỏ hơn hoặc bằng một giá trị nhất định. Mỗi lần, ta truy vấn số phần tử nhỏ hơn hoặc bằng giá trị hiện tại trong binary indexed tree, sau đó số phần tử lớn hơn giá trị hiện tại bằng độ dài mảng hiện tại trừ đi kết quả truy vấn. Từ hiệu này, ta quyết định thêm giá trị hiện tại vào mảng nào.

Vì miền giá trị trong đề bài rất lớn, ta cần rời rạc hóa các giá trị này. Ta có thể sắp xếp các giá trị và loại bỏ phần tử trùng lặp, sau đó dùng tìm kiếm nhị phân để tìm vị trí của mỗi giá trị trong mảng đã sắp xếp.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    __slots__ = "n", "c"

    def __init__(self, n: int):
        self.n = n
        self.c = [0] * (n + 1)

    def update(self, x: int, delta: int) -> None:
        while x <= self.n:
            self.c[x] += delta
            x += x & -x

    def query(self, x: int) -> int:
        s = 0
        while x:
            s += self.c[x]
            x -= x & -x
        return s


class Solution:
    def resultArray(self, nums: List[int]) -> List[int]:
        st = sorted(set(nums))
        m = len(st)
        tree1 = BinaryIndexedTree(m + 1)
        tree2 = BinaryIndexedTree(m + 1)
        tree1.update(bisect_left(st, nums[0]) + 1, 1)
        tree2.update(bisect_left(st, nums[1]) + 1, 1)
        arr1 = [nums[0]]
        arr2 = [nums[1]]
        for x in nums[2:]:
            i = bisect_left(st, x) + 1
            a = len(arr1) - tree1.query(i)
            b = len(arr2) - tree2.query(i)
            if a > b:
                arr1.append(x)
                tree1.update(i, 1)
            elif a < b:
                arr2.append(x)
                tree2.update(i, 1)
            elif len(arr1) <= len(arr2):
                arr1.append(x)
                tree1.update(i, 1)
            else:
                arr2.append(x)
                tree2.update(i, 1)
        return arr1 + arr2
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

    public void update(int x, int delta) {
        for (; x <= n; x += x & -x) {
            c[x] += delta;
        }
    }

    public int query(int x) {
        int s = 0;
        for (; x > 0; x -= x & -x) {
            s += c[x];
        }
        return s;
    }
}

class Solution {
    public int[] resultArray(int[] nums) {
        int[] st = nums.clone();
        Arrays.sort(st);
        int n = st.length;
        BinaryIndexedTree tree1 = new BinaryIndexedTree(n + 1);
        BinaryIndexedTree tree2 = new BinaryIndexedTree(n + 1);
        tree1.update(Arrays.binarySearch(st, nums[0]) + 1, 1);
        tree2.update(Arrays.binarySearch(st, nums[1]) + 1, 1);
        int[] arr1 = new int[n];
        int[] arr2 = new int[n];
        arr1[0] = nums[0];
        arr2[0] = nums[1];
        int i = 1, j = 1;
        for (int k = 2; k < n; ++k) {
            int x = Arrays.binarySearch(st, nums[k]) + 1;
            int a = i - tree1.query(x);
            int b = j - tree2.query(x);
            if (a > b) {
                arr1[i++] = nums[k];
                tree1.update(x, 1);
            } else if (a < b) {
                arr2[j++] = nums[k];
                tree2.update(x, 1);
            } else if (i <= j) {
                arr1[i++] = nums[k];
                tree1.update(x, 1);
            } else {
                arr2[j++] = nums[k];
                tree2.update(x, 1);
            }
        }
        for (int k = 0; k < j; ++k) {
            arr1[i++] = arr2[k];
        }
        return arr1;
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

    void update(int x, int delta) {
        for (; x <= n; x += x & -x) {
            c[x] += delta;
        }
    }

    int query(int x) {
        int s = 0;
        for (; x > 0; x -= x & -x) {
            s += c[x];
        }
        return s;
    }
};

class Solution {
public:
    vector<int> resultArray(vector<int>& nums) {
        vector<int> st = nums;
        sort(st.begin(), st.end());
        int n = st.size();
        BinaryIndexedTree tree1(n + 1);
        BinaryIndexedTree tree2(n + 1);
        tree1.update(distance(st.begin(), lower_bound(st.begin(), st.end(), nums[0])) + 1, 1);
        tree2.update(distance(st.begin(), lower_bound(st.begin(), st.end(), nums[1])) + 1, 1);
        vector<int> arr1 = {nums[0]};
        vector<int> arr2 = {nums[1]};
        for (int k = 2; k < n; ++k) {
            int x = distance(st.begin(), lower_bound(st.begin(), st.end(), nums[k])) + 1;
            int a = arr1.size() - tree1.query(x);
            int b = arr2.size() - tree2.query(x);
            if (a > b) {
                arr1.push_back(nums[k]);
                tree1.update(x, 1);
            } else if (a < b) {
                arr2.push_back(nums[k]);
                tree2.update(x, 1);
            } else if (arr1.size() <= arr2.size()) {
                arr1.push_back(nums[k]);
                tree1.update(x, 1);
            } else {
                arr2.push_back(nums[k]);
                tree2.update(x, 1);
            }
        }
        arr1.insert(arr1.end(), arr2.begin(), arr2.end());
        return arr1;
    }
};
```

#### Go

```go
type BinaryIndexedTree struct {
	n int
	c []int
}

func NewBinaryIndexedTree(n int) *BinaryIndexedTree {
	return &BinaryIndexedTree{n: n, c: make([]int, n+1)}
}

func (bit *BinaryIndexedTree) update(x, delta int) {
	for ; x <= bit.n; x += x & -x {
		bit.c[x] += delta
	}
}

func (bit *BinaryIndexedTree) query(x int) int {
	s := 0
	for ; x > 0; x -= x & -x {
		s += bit.c[x]
	}
	return s
}

func resultArray(nums []int) []int {
	st := make([]int, len(nums))
	copy(st, nums)
	sort.Ints(st)
	n := len(st)
	tree1 := NewBinaryIndexedTree(n + 1)
	tree2 := NewBinaryIndexedTree(n + 1)
	tree1.update(sort.SearchInts(st, nums[0])+1, 1)
	tree2.update(sort.SearchInts(st, nums[1])+1, 1)
	arr1 := []int{nums[0]}
	arr2 := []int{nums[1]}
	for _, x := range nums[2:] {
		i := sort.SearchInts(st, x) + 1
		a := len(arr1) - tree1.query(i)
		b := len(arr2) - tree2.query(i)
		if a > b {
			arr1 = append(arr1, x)
			tree1.update(i, 1)
		} else if a < b {
			arr2 = append(arr2, x)
			tree2.update(i, 1)
		} else if len(arr1) <= len(arr2) {
			arr1 = append(arr1, x)
			tree1.update(i, 1)
		} else {
			arr2 = append(arr2, x)
			tree2.update(i, 1)
		}
	}
	arr1 = append(arr1, arr2...)
	return arr1
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

    update(x: number, delta: number): void {
        for (; x <= this.n; x += x & -x) {
            this.c[x] += delta;
        }
    }

    query(x: number): number {
        let s = 0;
        for (; x > 0; x -= x & -x) {
            s += this.c[x];
        }
        return s;
    }
}

function resultArray(nums: number[]): number[] {
    const st: number[] = nums.slice().sort((a, b) => a - b);
    const n: number = st.length;
    const search = (x: number): number => {
        let [l, r] = [0, n];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (st[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    const tree1: BinaryIndexedTree = new BinaryIndexedTree(n + 1);
    const tree2: BinaryIndexedTree = new BinaryIndexedTree(n + 1);
    tree1.update(search(nums[0]) + 1, 1);
    tree2.update(search(nums[1]) + 1, 1);
    const arr1: number[] = [nums[0]];
    const arr2: number[] = [nums[1]];
    for (const x of nums.slice(2)) {
        const i: number = search(x) + 1;
        const a: number = arr1.length - tree1.query(i);
        const b: number = arr2.length - tree2.query(i);
        if (a > b) {
            arr1.push(x);
            tree1.update(i, 1);
        } else if (a < b) {
            arr2.push(x);
            tree2.update(i, 1);
        } else if (arr1.length <= arr2.length) {
            arr1.push(x);
            tree1.update(i, 1);
        } else {
            arr2.push(x);
            tree2.update(i, 1);
        }
    }
    return arr1.concat(arr2);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
