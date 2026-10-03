---
comments: true
difficulty: Medium
rating: 1635
source: Weekly Contest 245 Q3
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [1899. Merge Triplets to Form Target Triplet](https://leetcode.com/problems/merge-triplets-to-form-target-triplet)

[中文文档](/solution/1800-1899/1899.Merge%20Triplets%20to%20Form%20Target%20Triplet/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>bộ ba</strong> là một mảng gồm ba số nguyên. Cho một mảng số nguyên 2 chiều <code>triplets</code>, trong đó <code>triplets[i] = [a<sub>i</sub>, b<sub>i</sub>, c<sub>i</sub>]</code> mô tả <strong>bộ ba</strong> thứ <code>i<sup>th</sup></code>. Ngoài ra, cho một mảng số nguyên <code>target = [x, y, z]</code> mô tả <strong>bộ ba</strong> mà bạn muốn thu được.</p>

<p>Để thu được <code>target</code>, bạn có thể thực hiện thao tác sau trên <code>triplets</code> <strong>bất kỳ số lần nào</strong> (có thể là <strong>0</strong>):</p>

<ul>
	<li>Chọn hai chỉ số (đánh chỉ số từ <strong>0</strong>) <code>i</code> và <code>j</code> (<code>i != j</code>) rồi <strong>cập nhật</strong> <code>triplets[j]</code> thành <code>[max(a<sub>i</sub>, a<sub>j</sub>), max(b<sub>i</sub>, b<sub>j</sub>), max(c<sub>i</sub>, c<sub>j</sub>)]</code>.

    <ul>
    <li>Ví dụ, nếu <code>triplets[i] = [2, 5, 3]</code> và <code>triplets[j] = [1, 7, 5]</code>, <code>triplets[j]</code> sẽ được cập nhật thành <code>[max(2, 1), max(5, 7), max(3, 5)] = [2, 7, 5]</code>.</li>
    </ul>
    </li>

</ul>

<p>Trả về <code>true</code> <em>nếu có thể thu được </em><code>target</code><em> <strong>bộ ba</strong> </em><code>[x, y, z]</code><em> như một<strong> phần tử</strong> của </em><code>triplets</code><em>, hoặc </em><code>false</code><em> ngược lại</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> triplets = [[2,5,3],[1,8,4],[1,7,5]], target = [2,7,5]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Thực hiện các thao tác sau:
- Chọn bộ ba đầu tiên và cuối cùng <u>[2,5,3]</u>,[1,8,4],<u>[1,7,5]</u>. Cập nhật bộ ba cuối cùng thành [max(2,1), max(5,7), max(3,5)] = [2,7,5]. triplets = [[2,5,3],[1,8,4],<u>[2,7,5]</u>]
Bộ ba target [2,7,5] lúc này là một phần tử của triplets.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> triplets = [[3,4,5],[4,5,6]], target = [3,2,5]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể có [3,2,5] là một phần tử vì không có số 2 trong bất kỳ bộ ba nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> triplets = [[2,5,3],[2,3,4],[1,2,5],[5,2,3]], target = [5,5,5]
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>Thực hiện các thao tác sau:
- Chọn bộ ba thứ nhất và thứ ba <u>[2,5,3]</u>,[2,3,4],<u>[1,2,5]</u>,[5,2,3]. Cập nhật bộ ba thứ ba thành [max(2,1), max(5,2), max(3,5)] = [2,5,5]. triplets = [[2,5,3],[2,3,4],<u>[2,5,5]</u>,[5,2,3]].
- Chọn bộ ba thứ ba và thứ tư [[2,5,3],[2,3,4],<u>[2,5,5]</u>,<u>[5,2,3]</u>]. Cập nhật bộ ba thứ tư thành [max(2,5), max(5,2), max(5,3)] = [5,5,5]. triplets = [[2,5,3],[2,3,4],[2,5,5],<u>[5,5,5]</u>].
Bộ ba target [5,5,5] lúc này là một phần tử của triplets.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= triplets.length &lt;= 10<sup>5</sup></code></li>
	<li><code>triplets[i].length == target.length == 3</code></li>
	<li><code>1 &lt;= a<sub>i</sub>, b<sub>i</sub>, c<sub>i</sub>, x, y, z &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Phép gộp thay hai bộ ba bằng giá trị lớn nhất theo từng tọa độ. Bất kỳ bộ ba nào đã vượt target ở một tọa độ đều không thể được sử dụng, vì phép lấy max không thể làm giá trị giảm xuống.
>
> Chỉ giữ các bộ ba bị $target$ chi phối và lấy max theo từng tọa độ của chúng. Nếu kết quả bằng $target$, mỗi tọa độ đều có nguồn phù hợp và các phép gộp có thể tạo ra target.

<!-- thinking:end -->

Gọi $\textit{target} = [x, y, z]$. Ta cần xác định xem có tồn tại một bộ ba $[a, b, c]$ sao cho $a \leq x$, $b \leq y$ và $c \leq z$ hay không.

Ta có thể chia tất cả các bộ ba thành hai nhóm:

1. Các bộ ba thỏa mãn $a \leq x$, $b \leq y$ và $c \leq z$.
2. Các bộ ba không thỏa mãn đồng thời $a \leq x$, $b \leq y$ và $c \leq z$.

Với nhóm thứ nhất, ta lấy các giá trị lớn nhất của $a$, $b$ và $c$ trong các bộ ba này để tạo thành một bộ ba mới $[d, e, f]$.

Ta có thể bỏ qua nhóm thứ hai vì các bộ ba trong đó không thể giúp ta đạt được bộ ba target.

Cuối cùng, ta chỉ cần kiểm tra xem $[d, e, f]$ có bằng $\textit{target}$ hay không. Nếu có, trả về $\textit{true}$; ngược lại, trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{triplets}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mergeTriplets(self, triplets: List[List[int]], target: List[int]) -> bool:
        x, y, z = target
        d = e = f = 0
        for a, b, c in triplets:
            if a <= x and b <= y and c <= z:
                d = max(d, a)
                e = max(e, b)
                f = max(f, c)
        return [d, e, f] == target
```

#### Java

```java
class Solution {
    public boolean mergeTriplets(int[][] triplets, int[] target) {
        int x = target[0], y = target[1], z = target[2];
        int d = 0, e = 0, f = 0;
        for (var t : triplets) {
            int a = t[0], b = t[1], c = t[2];
            if (a <= x && b <= y && c <= z) {
                d = Math.max(d, a);
                e = Math.max(e, b);
                f = Math.max(f, c);
            }
        }
        return d == x && e == y && f == z;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool mergeTriplets(vector<vector<int>>& triplets, vector<int>& target) {
        int x = target[0], y = target[1], z = target[2];
        int d = 0, e = 0, f = 0;
        for (auto& t : triplets) {
            int a = t[0], b = t[1], c = t[2];
            if (a <= x && b <= y && c <= z) {
                d = max(d, a);
                e = max(e, b);
                f = max(f, c);
            }
        }
        return d == x && e == y && f == z;
    }
};
```

#### Go

```go
func mergeTriplets(triplets [][]int, target []int) bool {
	x, y, z := target[0], target[1], target[2]
	d, e, f := 0, 0, 0
	for _, t := range triplets {
		a, b, c := t[0], t[1], t[2]
		if a <= x && b <= y && c <= z {
			d = max(d, a)
			e = max(e, b)
			f = max(f, c)
		}
	}
	return d == x && e == y && f == z
}
```

#### TypeScript

```ts
function mergeTriplets(triplets: number[][], target: number[]): boolean {
    const [x, y, z] = target;
    let [d, e, f] = [0, 0, 0];
    for (const [a, b, c] of triplets) {
        if (a <= x && b <= y && c <= z) {
            d = Math.max(d, a);
            e = Math.max(e, b);
            f = Math.max(f, c);
        }
    }
    return d === x && e === y && f === z;
}
```

#### Rust

```rust
impl Solution {
    pub fn merge_triplets(triplets: Vec<Vec<i32>>, target: Vec<i32>) -> bool {
        let [x, y, z]: [i32; 3] = target.try_into().unwrap();
        let (mut d, mut e, mut f) = (0, 0, 0);

        for triplet in triplets {
            if let [a, b, c] = triplet[..] {
                if a <= x && b <= y && c <= z {
                    d = d.max(a);
                    e = e.max(b);
                    f = f.max(c);
                }
            }
        }

        [d, e, f] == [x, y, z]
    }
}
```

#### Scala

```scala
object Solution {
    def mergeTriplets(triplets: Array[Array[Int]], target: Array[Int]): Boolean = {
        val Array(x, y, z) = target
        var (d, e, f) = (0, 0, 0)

        for (Array(a, b, c) <- triplets) {
            if (a <= x && b <= y && c <= z) {
                d = d.max(a)
                e = e.max(b)
                f = f.max(c)
            }
        }

        d == x && e == y && f == z
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
