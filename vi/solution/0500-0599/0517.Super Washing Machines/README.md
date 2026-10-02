---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [517. Super Washing Machines](https://leetcode.com/problems/super-washing-machines)

[中文文档](/solution/0500-0599/0517.Super%20Washing%20Machines/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> máy giặt xếp thành một hàng. Ban đầu, mỗi máy có một số quần áo hoặc đang rỗng.</p>

<p>Mỗi lượt, bạn có thể chọn bất kỳ <code>m</code> máy giặt nào (<code>1 &lt;= m &lt;= n</code>) và đồng thời chuyển một món đồ từ mỗi máy đã chọn sang một máy giặt liền kề.</p>

<p>Cho mảng số nguyên <code>machines</code> biểu diễn số quần áo trong từng máy giặt theo thứ tự từ trái sang phải. Hãy trả về <em>số lượt ít nhất để mọi máy giặt có cùng số quần áo</em>. Nếu không thể thực hiện, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> machines = [1,0,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Lượt 1:    1     0 &lt;-- 5    =&gt;    1     1     4
Lượt 2:    1 &lt;-- 1 &lt;-- 4    =&gt;    2     1     3
Lượt 3:    2     1 &lt;-- 3    =&gt;    2     2     2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> machines = [0,3,0]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Lượt 1:    0 &lt;-- 3     0    =&gt;    1     2     0
Lượt 2:    1     2 --&gt; 0    =&gt;    1     1     1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> machines = [0,2,0]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>
Không thể làm cho cả ba máy giặt có cùng số quần áo.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == machines.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= machines[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Nếu tổng số quần áo không chia hết cho số máy thì không thể cân bằng. Nếu chia hết, mỗi máy cần có trung bình $k$ món. Mỗi lượt chuyển một món sang máy liền kề.
>
> Lấy số quần áo hiện tại trừ $k$ để được phần thừa/thiếu. Tổng tiền tố $s$ là số quần áo ròng phải đi qua ranh giới đó. Máy có phần thừa $x>0$ cũng phải chuyển $x$ món ra ngoài. Đáp án là giá trị lớn nhất giữa $|s|$ và phần thừa của từng máy.

<!-- thinking:end -->

Nếu tổng số quần áo trong các máy giặt không chia hết cho số máy, không thể làm cho mỗi máy có số quần áo bằng nhau, nên trả về $-1$ ngay.

Ngược lại, gọi tổng số quần áo trong các máy là $s$; khi cân bằng, mỗi máy sẽ có $k = s / n$ món.

Định nghĩa $a_i$ là độ lệch giữa số quần áo trong máy thứ $i$ và $k$, tức $a_i = \textit{machines}[i] - k$. Nếu $a_i > 0$, máy thứ $i$ có quần áo thừa và cần chuyển sang máy liền kề; nếu $a_i < 0$, máy bị thiếu quần áo và cần nhận từ máy liền kề.

Gọi tổng độ lệch của $i$ máy đầu tiên là $s_i = \sum_{j=0}^{i-1} a_j$. Xem $i$ máy đầu tiên là nhóm thứ nhất, các máy còn lại là nhóm thứ hai. Nếu $s_i$ dương, nhóm thứ nhất thừa quần áo và cần chuyển sang nhóm thứ hai; nếu $s_i$ âm, nhóm thứ nhất thiếu và cần nhận quần áo từ nhóm thứ hai.

Khi đó, số lượt cần thiết bị giới hạn bởi hai trường hợp sau:

1. Số lượt tối đa cần để chuyển quần áo giữa hai nhóm là $\max_{i=0}^{n-1} \lvert s_i \rvert$;
1. Một máy giặt có quá nhiều quần áo và cần chuyển sang cả hai phía; số lượt tối đa cần thiết là $\max_{i=0}^{n-1} a_i$. 

Đáp án là giá trị lớn hơn trong hai giới hạn này.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số máy giặt. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMinMoves(self, machines: List[int]) -> int:
        n = len(machines)
        k, mod = divmod(sum(machines), n)
        if mod:
            return -1
        ans = s = 0
        for x in machines:
            x -= k
            s += x
            ans = max(ans, abs(s), x)
        return ans
```

#### Java

```java
class Solution {
    public int findMinMoves(int[] machines) {
        int n = machines.length;
        int s = 0;
        for (int x : machines) {
            s += x;
        }
        if (s % n != 0) {
            return -1;
        }
        int k = s / n;
        s = 0;
        int ans = 0;
        for (int x : machines) {
            x -= k;
            s += x;
            ans = Math.max(ans, Math.max(Math.abs(s), x));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMinMoves(vector<int>& machines) {
        int n = machines.size();
        int s = accumulate(machines.begin(), machines.end(), 0);
        if (s % n) {
            return -1;
        }
        int k = s / n;
        s = 0;
        int ans = 0;
        for (int x : machines) {
            x -= k;
            s += x;
            ans = max({ans, abs(s), x});
        }
        return ans;
    }
};
```

#### Go

```go
func findMinMoves(machines []int) (ans int) {
	n := len(machines)
	s := 0
	for _, x := range machines {
		s += x
	}
	if s%n != 0 {
		return -1
	}
	k := s / n
	s = 0
	for _, x := range machines {
		x -= k
		s += x
		ans = max(ans, max(abs(s), x))
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
function findMinMoves(machines: number[]): number {
    const n = machines.length;
    let s = machines.reduce((a, b) => a + b);
    if (s % n !== 0) {
        return -1;
    }
    const k = Math.floor(s / n);
    s = 0;
    let ans = 0;
    for (let x of machines) {
        x -= k;
        s += x;
        ans = Math.max(ans, Math.abs(s), x);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
