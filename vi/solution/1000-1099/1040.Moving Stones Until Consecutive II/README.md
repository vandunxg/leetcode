---
comments: true
difficulty: Medium
rating: 2455
source: Weekly Contest 135 Q4
tags:
    - Array
    - Math
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [1040. Moving Stones Until Consecutive II](https://leetcode.com/problems/moving-stones-until-consecutive-ii)

[中文文档](/solution/1000-1099/1040.Moving%20Stones%20Until%20Consecutive%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Có một số viên đá nằm ở các vị trí khác nhau trên trục X. Bạn được cho mảng số nguyên <code>stones</code> chứa vị trí của các viên đá.</p>

<p>Viên đá nằm ở vị trí nhỏ nhất hoặc lớn nhất được gọi là <strong>viên đá ở đầu mút</strong>. Trong mỗi lượt, bạn nhấc một <strong>viên đá ở đầu mút</strong> và chuyển nó đến một vị trí chưa có đá, sao cho nó không còn là <strong>viên đá ở đầu mút</strong>.</p>

<ul>
	<li>Cụ thể, nếu các viên đá nằm ở các vị trí <code>stones = [1,2,5]</code>, bạn không thể di chuyển viên đá ở đầu mút tại vị trí <code>5</code>, vì chuyển nó đến bất kỳ vị trí nào (chẳng hạn <code>0</code> hoặc <code>3</code>) thì nó vẫn là viên đá ở đầu mút.</li>
</ul>

<p>Trò chơi kết thúc khi bạn không thể thực hiện thêm lượt nào (tức là các viên đá nằm ở ba vị trí liên tiếp).</p>

<p>Trả về <em>mảng số nguyên </em><code>answer</code><em> có độ dài </em><code>2</code><em>, trong đó</em>:</p>

<ul>
	<li><code>answer[0]</code> <em>là số lượt ít nhất bạn có thể thực hiện, còn</em></li>
	<li><code>answer[1]</code> <em>là số lượt nhiều nhất bạn có thể thực hiện</em>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> stones = [7,4,9]
<strong>Output:</strong> [1,2]
<strong>Giải thích:</strong> Ta có thể chuyển 4 -&gt; 8 và kết thúc trò chơi sau một lượt.
Hoặc ta có thể chuyển 9 -&gt; 5, 4 -&gt; 6 và kết thúc trò chơi sau hai lượt.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> stones = [6,5,4,3,10]
<strong>Output:</strong> [2,3]
<strong>Giải thích:</strong> Ta có thể chuyển 3 -&gt; 8 rồi 10 -&gt; 7 để kết thúc trò chơi.
Hoặc ta có thể chuyển 3 -&gt; 7, 4 -&gt; 8, 5 -&gt; 9 để kết thúc trò chơi.
Lưu ý, ta không thể chuyển 10 -&gt; 2 để kết thúc trò chơi vì đó là một nước đi không hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= stones.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= stones[i] &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả giá trị trong <code>stones</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có thể di chuyển viên đá ở đầu mút đến một vị trí trống không phải đầu mút. Vì $n\le 10^4$ và tọa độ có thể lên đến $10^9$, không thể mô phỏng từng lượt. Số lượt tối đa đạt được khi di chuyển các đầu mút chậm nhất có thể; số lượt tối thiểu đạt được khi xếp đá vào một cửa sổ gồm $n$ vị trí.
>
> Sau khi sắp xếp, di chuyển đầu trái trước sẽ bỏ qua khoảng trống sau $stones[0]$ và cho tối đa $stones[n-1]-stones[1]+1-(n-1)$ lượt; trường hợp đầu phải được xử lý đối xứng. Để tìm số lượt tối thiểu, dùng hai con trỏ cho các cửa sổ gồm $n$ vị trí; nếu cửa sổ gần đầy nhưng vẫn thiếu một viên đá thì cần hai lượt.
>
> Một lần sắp xếp và một lượt dùng sliding window sẽ tìm được $[\textit{mi},\textit{mx}]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numMovesStonesII(self, stones: List[int]) -> List[int]:
        stones.sort()
        mi = n = len(stones)
        mx = max(stones[-1] - stones[1] + 1, stones[-2] - stones[0] + 1) - (n - 1)
        i = 0
        for j, x in enumerate(stones):
            while x - stones[i] + 1 > n:
                i += 1
            if j - i + 1 == n - 1 and x - stones[i] == n - 2:
                mi = min(mi, 2)
            else:
                mi = min(mi, n - (j - i + 1))
        return [mi, mx]
```

#### Java

```java
class Solution {
    public int[] numMovesStonesII(int[] stones) {
        Arrays.sort(stones);
        int n = stones.length;
        int mi = n;
        int mx = Math.max(stones[n - 1] - stones[1] + 1, stones[n - 2] - stones[0] + 1) - (n - 1);
        for (int i = 0, j = 0; j < n; ++j) {
            while (stones[j] - stones[i] + 1 > n) {
                ++i;
            }
            if (j - i + 1 == n - 1 && stones[j] - stones[i] == n - 2) {
                mi = Math.min(mi, 2);
            } else {
                mi = Math.min(mi, n - (j - i + 1));
            }
        }
        return new int[] {mi, mx};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> numMovesStonesII(vector<int>& stones) {
        sort(stones.begin(), stones.end());
        int n = stones.size();
        int mi = n;
        int mx = max(stones[n - 1] - stones[1] + 1, stones[n - 2] - stones[0] + 1) - (n - 1);
        for (int i = 0, j = 0; j < n; ++j) {
            while (stones[j] - stones[i] + 1 > n) {
                ++i;
            }
            if (j - i + 1 == n - 1 && stones[j] - stones[i] == n - 2) {
                mi = min(mi, 2);
            } else {
                mi = min(mi, n - (j - i + 1));
            }
        }
        return {mi, mx};
    }
};
```

#### Go

```go
func numMovesStonesII(stones []int) []int {
	sort.Ints(stones)
	n := len(stones)
	mi := n
	mx := max(stones[n-1]-stones[1]+1, stones[n-2]-stones[0]+1) - (n - 1)
	i := 0
	for j, x := range stones {
		for x-stones[i]+1 > n {
			i++
		}
		if j-i+1 == n-1 && stones[j]-stones[i] == n-2 {
			mi = min(mi, 2)
		} else {
			mi = min(mi, n-(j-i+1))
		}
	}
	return []int{mi, mx}
}
```

#### TypeScript

```ts
function numMovesStonesII(stones: number[]): number[] {
    stones.sort((a, b) => a - b);
    const n = stones.length;
    let mi = n;
    const mx = Math.max(stones[n - 1] - stones[1] + 1, stones[n - 2] - stones[0] + 1) - (n - 1);
    for (let i = 0, j = 0; j < n; ++j) {
        while (stones[j] - stones[i] + 1 > n) {
            ++i;
        }
        if (j - i + 1 === n - 1 && stones[j] - stones[i] === n - 2) {
            mi = Math.min(mi, 2);
        } else {
            mi = Math.min(mi, n - (j - i + 1));
        }
    }
    return [mi, mx];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
