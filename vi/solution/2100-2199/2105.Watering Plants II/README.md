---
comments: true
difficulty: Medium
rating: 1507
source: Weekly Contest 271 Q3
tags:
    - Array
    - Two Pointers
    - Simulation
---

<!-- problem:start -->

# [2105. Watering Plants II](https://leetcode.com/problems/watering-plants-ii)

[中文文档](/solution/2100-2199/2105.Watering%20Plants%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob muốn tưới <code>n</code> cây trong vườn. Các cây được xếp thành một hàng và được đánh số từ <code>0</code> đến <code>n - 1</code> từ trái sang phải, trong đó cây thứ <code>i<sup>th</sup></code> nằm tại <code>x = i</code>.</p>

<p>Mỗi cây cần một lượng nước cụ thể. Alice và Bob mỗi người có một bình tưới nước, <strong>ban đầu đều đầy</strong>. Họ tưới cây theo cách sau:</p>

<ul>
	<li>Alice tưới cây theo thứ tự từ <strong>trái sang phải</strong>, bắt đầu từ cây thứ <code>0<sup>th</sup></code>. Bob tưới cây theo thứ tự từ <strong>phải sang trái</strong>, bắt đầu từ cây thứ <code>(n - 1)<sup>th</sup></code>. Họ bắt đầu tưới <strong>đồng thời</strong>.</li>
	<li>Thời gian tưới mỗi cây là như nhau, không phụ thuộc vào lượng nước cây cần.</li>
	<li>Alice/Bob <strong>phải</strong> tưới cây nếu bình của họ đủ nước để <strong>tưới đầy</strong> cây đó. Nếu không đủ, trước tiên họ <strong>đổ đầy</strong> bình (ngay lập tức), rồi tưới cây.</li>
	<li>Trong trường hợp Alice và Bob cùng đi đến một cây, người có <strong>nhiều</strong> nước hơn trong bình hiện tại sẽ tưới cây đó. Nếu lượng nước bằng nhau, Alice sẽ tưới cây.</li>
</ul>

<p>Cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>plants</code> gồm <code>n</code> phần tử, trong đó <code>plants[i]</code> là lượng nước cây <code>i<sup>th</sup></code> cần, cùng hai số nguyên <code>capacityA</code> và <code>capacityB</code> lần lượt biểu thị sức chứa bình tưới của Alice và Bob, hãy trả về <em><strong>số lần</strong> họ phải đổ đầy bình để tưới hết tất cả các cây</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> plants = [2,2,3,3], capacityA = 5, capacityB = 5
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
- Ban đầu, Alice và Bob mỗi người có 5 đơn vị nước trong bình tưới.
- Alice tưới cây 0, Bob tưới cây 3.
- Lúc này Alice và Bob lần lượt còn 3 và 2 đơn vị nước.
- Alice có đủ nước cho cây 1 nên cô tưới cây đó. Bob không đủ nước cho cây 2 nên đổ đầy bình rồi tưới cây đó.
Vậy tổng số lần họ phải đổ đầy bình để tưới hết tất cả các cây là 0 + 0 + 1 + 0 = 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> plants = [2,2,3,3], capacityA = 3, capacityB = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
- Ban đầu, Alice và Bob lần lượt có 3 và 4 đơn vị nước trong bình tưới.
- Alice tưới cây 0, Bob tưới cây 3.
- Lúc này mỗi người còn 1 đơn vị nước, và lần lượt cần tưới cây 1 và cây 2.
- Vì không ai đủ nước cho cây hiện tại của mình, họ đổ đầy bình rồi tưới cây.
Vậy tổng số lần họ phải đổ đầy bình để tưới hết tất cả các cây là 0 + 1 + 1 + 0 = 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> plants = [5], capacityA = 10, capacityB = 8
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
- Chỉ có một cây.
- Bình tưới của Alice có 10 đơn vị nước, trong khi bình của Bob có 8 đơn vị. Vì Alice có nhiều nước hơn trong bình nên cô tưới cây này.
Vậy tổng số lần phải đổ đầy bình là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == plants.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= plants[i] &lt;= 10<sup>6</sup></code></li>
	<li><code>max(plants[i]) &lt;= capacityA, capacityB &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Alice và Bob đi từ hai đầu đối diện. Trước khi tưới một cây, mỗi người phải có đủ nước; nếu không, họ phải đổ đầy bình trước. Quá trình di chuyển đã được xác định duy nhất, nên chỉ cần mô phỏng.
>
> Với $n\le 10^5$, mỗi người sẽ thăm $O(n)$ cây. Hai con trỏ và lượng nước hiện tại $a$, $b$ giúp đếm số lần đổ đầy bình trong thời gian tuyến tính.
>
> Khi hai con trỏ gặp nhau, cây cuối cùng sẽ được tưới bởi người đang có nhiều nước hơn; nếu cả hai đều không đủ nước thì cần đổ đầy bình thêm một lần. Trước khi gặp nhau, mỗi bên tự đổ đầy bình và trừ lượng nước tương ứng.

<!-- thinking:end -->

Ta dùng hai biến $a$ và $b$ để biểu diễn lượng nước Alice và Bob có, ban đầu $a = \textit{capacityA}$, $b = \textit{capacityB}$. Sau đó, ta dùng hai con trỏ $i$ và $j$ trỏ đến đầu và cuối mảng cây, rồi mô phỏng quá trình Alice và Bob tưới cây từ hai đầu vào giữa.

Khi $i < j$, ta kiểm tra Alice và Bob có đủ nước để tưới cây hay không. Nếu không đủ, ta đổ đầy các bình tưới. Sau đó, ta cập nhật lượng nước $a$ và $b$, rồi di chuyển hai con trỏ $i$ và $j$. Cuối cùng, ta cần kiểm tra xem $i$ và $j$ có bằng nhau hay không. Nếu bằng nhau, ta kiểm tra xem $\max(a, b)$ có nhỏ hơn lượng nước cây cần hay không. Nếu nhỏ hơn, ta lại cần đổ đầy bình.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng cây. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumRefill(self, plants: List[int], capacityA: int, capacityB: int) -> int:
        a, b = capacityA, capacityB
        ans = 0
        i, j = 0, len(plants) - 1
        while i < j:
            if a < plants[i]:
                ans += 1
                a = capacityA
            a -= plants[i]
            if b < plants[j]:
                ans += 1
                b = capacityB
            b -= plants[j]
            i, j = i + 1, j - 1
        ans += i == j and max(a, b) < plants[i]
        return ans
```

#### Java

```java
class Solution {
    public int minimumRefill(int[] plants, int capacityA, int capacityB) {
        int a = capacityA, b = capacityB;
        int ans = 0;
        int i = 0, j = plants.length - 1;
        for (; i < j; ++i, --j) {
            if (a < plants[i]) {
                ++ans;
                a = capacityA;
            }
            a -= plants[i];
            if (b < plants[j]) {
                ++ans;
                b = capacityB;
            }
            b -= plants[j];
        }
        ans += i == j && Math.max(a, b) < plants[i] ? 1 : 0;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumRefill(vector<int>& plants, int capacityA, int capacityB) {
        int a = capacityA, b = capacityB;
        int ans = 0;
        int i = 0, j = plants.size() - 1;
        for (; i < j; ++i, --j) {
            if (a < plants[i]) {
                ++ans;
                a = capacityA;
            }
            a -= plants[i];
            if (b < plants[j]) {
                ++ans;
                b = capacityB;
            }
            b -= plants[j];
        }
        ans += i == j && max(a, b) < plants[i];
        return ans;
    }
};
```

#### Go

```go
func minimumRefill(plants []int, capacityA int, capacityB int) (ans int) {
	a, b := capacityA, capacityB
	i, j := 0, len(plants)-1
	for ; i < j; i, j = i+1, j-1 {
		if a < plants[i] {
			ans++
			a = capacityA
		}
		a -= plants[i]
		if b < plants[j] {
			ans++
			b = capacityB
		}
		b -= plants[j]
	}
	if i == j && max(a, b) < plants[i] {
		ans++
	}
	return
}
```

#### TypeScript

```ts
function minimumRefill(plants: number[], capacityA: number, capacityB: number): number {
    let [a, b] = [capacityA, capacityB];
    let ans = 0;
    let [i, j] = [0, plants.length - 1];
    for (; i < j; ++i, --j) {
        if (a < plants[i]) {
            ++ans;
            a = capacityA;
        }
        a -= plants[i];
        if (b < plants[j]) {
            ++ans;
            b = capacityB;
        }
        b -= plants[j];
    }
    ans += i === j && Math.max(a, b) < plants[i] ? 1 : 0;
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_refill(plants: Vec<i32>, capacity_a: i32, capacity_b: i32) -> i32 {
        let mut a = capacity_a;
        let mut b = capacity_b;
        let mut ans = 0;
        let mut i = 0;
        let mut j = plants.len() - 1;

        while i < j {
            if a < plants[i] {
                ans += 1;
                a = capacity_a;
            }
            a -= plants[i];

            if b < plants[j] {
                ans += 1;
                b = capacity_b;
            }
            b -= plants[j];

            i += 1;
            j -= 1;
        }

        if i == j && a.max(b) < plants[i] {
            ans += 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
