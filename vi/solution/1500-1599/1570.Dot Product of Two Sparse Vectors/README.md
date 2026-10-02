---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - Hash Table
    - Two Pointers
    - Linear Algebra
---

<!-- problem:start -->

# [1570. Dot Product of Two Sparse Vectors 🔒](https://leetcode.com/problems/dot-product-of-two-sparse-vectors)

[中文文档](/solution/1500-1599/1570.Dot%20Product%20of%20Two%20Sparse%20Vectors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai vector thưa, hãy tính tích vô hướng của chúng.</p>

<p>Triển khai lớp <code>SparseVector</code>:</p>

<ul data-indent="0" data-stringify-type="unordered-list">
	<li><code>SparseVector(nums)</code>&nbsp;Khởi tạo đối tượng bằng vector <code>nums</code></li>
	<li><code>dotProduct(vec)</code>&nbsp;Tính tích vô hướng giữa thể hiện của <em>SparseVector</em> và <code>vec</code></li>
</ul>

<p><strong>Vector thưa</strong> là vector có phần lớn giá trị bằng 0. Bạn cần lưu vector thưa một cách <strong>hiệu quả</strong> và tính tích vô hướng giữa hai <em>SparseVector</em>.</p>

<p><strong>Câu hỏi mở rộng:&nbsp;</strong>Nếu chỉ một trong hai vector là vector thưa thì sao?</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,0,0,2,3], nums2 = [0,3,0,4,0]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> v1 = SparseVector(nums1) , v2 = SparseVector(nums2)
v1.dotProduct(v2) = 1*0 + 0*3 + 0*0 + 2*4 + 3*0 = 8
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [0,1,0,0,0], nums2 = [0,0,0,0,2]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> v1 = SparseVector(nums1) , v2 = SparseVector(nums2)
v1.dotProduct(v2) = 0*0 + 1*0 + 0*0 + 0*0 + 0*2 = 0
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [0,1,0,0,2,0,0], nums2 = [1,0,0,0,3,0,4]
<strong>Đầu ra:</strong> 6
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>1 &lt;= n &lt;= 10^5</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i]&nbsp;&lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Map

<!-- thinking:start -->

> **Tư duy**
>
> Tích vô hướng của vector thưa chỉ cần các chỉ số khác 0 ở cả hai vector. Nhân toàn bộ mảng độ dài $n$ sẽ lãng phí công sức cho các số 0, trong khi $n\le 10^5$.
>
> Hàm khởi tạo chỉ lưu các phần tử khác 0 dưới dạng index $\to$ value. Khi tính tích, ta duyệt map nhỏ hơn và dùng $get$ để tra phía còn lại. Khi số phần tử khác 0 ít, cách này nhanh hơn nhiều so với vòng lặp trên mảng đầy đủ.

<!-- thinking:end -->

Ta dùng hash map $d$ để lưu các phần tử khác 0, trong đó key là chỉ số và value là giá trị tương ứng. Ta duyệt qua $\textit{nums}$; nếu $\textit{nums}[i]$ khác $0$, ta thêm $(i, \textit{nums}[i])$ vào hash map $d$.

Khi tính tích vô hướng, ta duyệt hash map có ít phần tử khác 0 hơn và kiểm tra hash map còn lại có key tương ứng hay không. Nếu có, ta nhân hai giá trị tương ứng và cộng vào kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class SparseVector:
    def __init__(self, nums: List[int]):
        self.d = {i: v for i, v in enumerate(nums) if v}

    # Return the dotProduct of two sparse vectors
    def dotProduct(self, vec: "SparseVector") -> int:
        a, b = self.d, vec.d
        if len(b) < len(a):
            a, b = b, a
        return sum(v * b.get(i, 0) for i, v in a.items())


# Your SparseVector object will be instantiated and called as such:
# v1 = SparseVector(nums1)
# v2 = SparseVector(nums2)
# ans = v1.dotProduct(v2)
```

#### Java

```java
class SparseVector {
    public Map<Integer, Integer> d = new HashMap<>(128);

    SparseVector(int[] nums) {
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] != 0) {
                d.put(i, nums[i]);
            }
        }
    }

    // Return the dotProduct of two sparse vectors
    public int dotProduct(SparseVector vec) {
        var a = d;
        var b = vec.d;
        if (b.size() < a.size()) {
            var t = a;
            a = b;
            b = t;
        }
        int ans = 0;
        for (var e : a.entrySet()) {
            int i = e.getKey(), v = e.getValue();
            ans += v * b.getOrDefault(i, 0);
        }
        return ans;
    }
}

// Your SparseVector object will be instantiated and called as such:
// SparseVector v1 = new SparseVector(nums1);
// SparseVector v2 = new SparseVector(nums2);
// int ans = v1.dotProduct(v2);
```

#### C++

```cpp
class SparseVector {
public:
    unordered_map<int, int> d;

    SparseVector(vector<int>& nums) {
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i]) {
                d[i] = nums[i];
            }
        }
    }

    // Return the dotProduct of two sparse vectors
    int dotProduct(SparseVector& vec) {
        auto a = d;
        auto b = vec.d;
        if (a.size() > b.size()) {
            swap(a, b);
        }
        int ans = 0;
        for (auto& [i, v] : a) {
            if (b.count(i)) {
                ans += v * b[i];
            }
        }
        return ans;
    }
};

// Your SparseVector object will be instantiated and called as such:
// SparseVector v1(nums1);
// SparseVector v2(nums2);
// int ans = v1.dotProduct(v2);
```

#### Go

```go
type SparseVector struct {
	d map[int]int
}

func Constructor(nums []int) SparseVector {
	d := map[int]int{}
	for i, x := range nums {
		if x != 0 {
			d[i] = x
		}
	}
	return SparseVector{d}
}

// Return the dotProduct of two sparse vectors
func (this *SparseVector) dotProduct(vec SparseVector) (ans int) {
	a, b := this.d, vec.d
	if len(a) > len(b) {
		a, b = b, a
	}
	for i, x := range a {
		if y, has := b[i]; has {
			ans += x * y
		}
	}
	return
}

/**
 * Your SparseVector object will be instantiated and called as such:
 * v1 := Constructor(nums1);
 * v2 := Constructor(nums2);
 * ans := v1.dotProduct(v2);
 */
```

#### TypeScript

```ts
class SparseVector {
    d: Map<number, number>;

    constructor(nums: number[]) {
        this.d = new Map();
        for (let i = 0; i < nums.length; ++i) {
            if (nums[i] != 0) {
                this.d.set(i, nums[i]);
            }
        }
    }

    // Return the dotProduct of two sparse vectors
    dotProduct(vec: SparseVector): number {
        let a = this.d;
        let b = vec.d;
        if (a.size > b.size) {
            [a, b] = [b, a];
        }
        let ans = 0;
        for (const [i, x] of a) {
            if (b.has(i)) {
                ans += x * b.get(i)!;
            }
        }
        return ans;
    }
}

/**
 * Your SparseVector object will be instantiated and called as such:
 * var v1 = new SparseVector(nums1)
 * var v2 = new SparseVector(nums1)
 * var ans = v1.dotProduct(v2)
 */
```

#### Rust

```rust
use std::collections::HashMap;

#[derive(Clone)]
struct SparseVector {
    d: HashMap<usize, i32>,
}

impl SparseVector {
    fn new(nums: Vec<i32>) -> Self {
        let mut d = HashMap::new();
        for (i, &x) in nums.iter().enumerate() {
            if x != 0 {
                d.insert(i, x);
            }
        }
        SparseVector { d }
    }

    fn dot_product(&self, vec: SparseVector) -> i32 {
        let (a, b) = (&self.d, &vec.d);
        let mut ans = 0;

        if a.len() > b.len() {
            return vec.dot_product(self.clone());
        }

        for (&i, &x) in a.iter() {
            if let Some(&y) = b.get(&i) {
                ans += x * y;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
