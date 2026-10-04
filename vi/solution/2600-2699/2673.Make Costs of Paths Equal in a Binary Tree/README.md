---
comments: true
difficulty: Medium
rating: 1917
source: Weekly Contest 344 Q4
tags:
    - Greedy
    - Tree
    - Array
    - Dynamic Programming
    - Binary Tree
---

<!-- problem:start -->

# [2673. Make Costs of Paths Equal in a Binary Tree](https://leetcode.com/problems/make-costs-of-paths-equal-in-a-binary-tree)

[中文文档](/solution/2600-2699/2673.Make%20Costs%20of%20Paths%20Equal%20in%20a%20Binary%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị số node trong một <strong>cây nhị phân hoàn hảo</strong> gồm các node được đánh số từ <code>1</code> đến <code>n</code>. Gốc của cây là node <code>1</code> và mỗi node <code>i</code> trong cây có hai node con: node con trái là node <code>2 * i</code> và node con phải là node <code>2 * i + 1</code>.</p>

<p>Mỗi node trong cây cũng có một <strong>chi phí</strong>, được biểu diễn bằng một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>cost</code> có kích thước <code>n</code>, trong đó <code>cost[i]</code> là chi phí của node <code>i + 1</code>. Bạn được phép <strong>tăng</strong> chi phí của <strong>bất kỳ</strong> node nào thêm <code>1</code> <strong>bao nhiêu lần tùy ý</strong>.</p>

<p>Trả về <em><strong>số lần tăng ít nhất</strong> cần thực hiện để tổng chi phí trên các đường đi từ gốc đến mỗi node <strong>lá</strong> bằng nhau</em>.</p>

<p><strong>Lưu ý</strong>:</p>

<ul>
	<li><strong>Cây nhị phân hoàn hảo</strong> là cây mà mỗi node, ngoại trừ các node lá, có đúng 2 node con.</li>
	<li><strong>Chi phí của một đường đi</strong> là tổng chi phí của các node trên đường đi đó.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2673.Make%20Costs%20of%20Paths%20Equal%20in%20a%20Binary%20Tree/images/binaryytreeedrawio-4.png" />
<pre>
<strong>Đầu vào:</strong> n = 7, cost = [1,5,2,2,3,3,1]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Ta có thể thực hiện các lần tăng sau:
- Tăng chi phí của node 4 một lần.
- Tăng chi phí của node 3 ba lần.
- Tăng chi phí của node 7 hai lần.
Chi phí trên mỗi đường đi từ gốc đến node lá sẽ bằng 9.
Tổng số lần tăng đã thực hiện là 1 + 3 + 2 = 6.
Có thể chứng minh đây là đáp án nhỏ nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2673.Make%20Costs%20of%20Paths%20Equal%20in%20a%20Binary%20Tree/images/binaryytreee2drawio.png" style="width: 205px; height: 151px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, cost = [5,3,3]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Hai đường đi đã có tổng chi phí bằng nhau, nên không cần thực hiện lần tăng nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>n + 1</code> là lũy thừa của <code>2</code></li>
	<li><code>cost.length == n</code></li>
	<li><code>1 &lt;= cost[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Vì chỉ được tăng các giá trị, ta cần làm cho tổng trên mọi đường đi từ gốc đến node lá bằng nhau. Cách xử lý từ trên xuống không biết mỗi subtree còn thiếu bao nhiêu; tổng đường đi bằng nhau khi và chỉ khi tại mỗi node bên trong, tổng đường đi đến các node lá ở hai cây con bằng nhau.
>
> Xử lý từ dưới lên, ta cần cộng độ chênh lệch của hai node con vào phía nhỏ hơn; sau đó node cha tiếp nhận tổng đường đi lớn hơn làm tổng đường đi chung của subtree đó. Cây nhị phân hoàn hảo được đánh chỉ số trực tiếp.

<!-- thinking:end -->

Theo mô tả bài toán, chúng ta cần tính số lần tăng ít nhất để làm cho giá trị đường đi từ node gốc đến mỗi node lá bằng nhau.

Việc giá trị đường đi từ node gốc đến mỗi node lá bằng nhau thực ra tương đương với việc giá trị đường đi từ bất kỳ node nào được xem là gốc của một subtree đến mỗi node lá của subtree đó bằng nhau.

Tại sao lại như vậy? Ta có thể chứng minh bằng phản chứng. Giả sử có một node $x$ sao cho giá trị đường đi từ nó, với tư cách là gốc của một subtree, đến một số node lá không bằng nhau. Khi đó tồn tại trường hợp giá trị đường đi từ node gốc đến các node lá không bằng nhau, mâu thuẫn với điều kiện "giá trị đường đi từ node gốc đến mỗi node lá bằng nhau". Do đó, giả thiết trên không đúng, và giá trị đường đi từ bất kỳ node nào được xem là gốc của một subtree đến mỗi node lá của subtree đó đều bằng nhau.

Ta có thể bắt đầu từ dưới cùng của cây và tính số lần tăng theo từng layer. Với mỗi node không phải node lá, ta tính giá trị đường đi qua node con trái và node con phải. Số lần tăng là độ chênh lệch giữa hai giá trị đường đi, sau đó cập nhật giá trị đường đi qua node con trái và node con phải thành giá trị lớn hơn trong hai giá trị đó.

Cuối cùng, trả về tổng số lần tăng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số node. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minIncrements(self, n: int, cost: List[int]) -> int:
        ans = 0
        for i in range(n >> 1, 0, -1):
            l, r = i << 1, i << 1 | 1
            ans += abs(cost[l - 1] - cost[r - 1])
            cost[i - 1] += max(cost[l - 1], cost[r - 1])
        return ans
```

#### Java

```java
class Solution {
    public int minIncrements(int n, int[] cost) {
        int ans = 0;
        for (int i = n >> 1; i > 0; --i) {
            int l = i << 1, r = i << 1 | 1;
            ans += Math.abs(cost[l - 1] - cost[r - 1]);
            cost[i - 1] += Math.max(cost[l - 1], cost[r - 1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minIncrements(int n, vector<int>& cost) {
        int ans = 0;
        for (int i = n >> 1; i > 0; --i) {
            int l = i << 1, r = i << 1 | 1;
            ans += abs(cost[l - 1] - cost[r - 1]);
            cost[i - 1] += max(cost[l - 1], cost[r - 1]);
        }
        return ans;
    }
};
```

#### Go

```go
func minIncrements(n int, cost []int) (ans int) {
	for i := n >> 1; i > 0; i-- {
		l, r := i<<1, i<<1|1
		ans += abs(cost[l-1] - cost[r-1])
		cost[i-1] += max(cost[l-1], cost[r-1])
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minIncrements(n: number, cost: number[]): number {
    let ans = 0;
    for (let i = n >> 1; i; --i) {
        const [l, r] = [i << 1, (i << 1) | 1];
        ans += Math.abs(cost[l - 1] - cost[r - 1]);
        cost[i - 1] += Math.max(cost[l - 1], cost[r - 1]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
