---
comments: true
difficulty: Medium
rating: 1387
source: Biweekly Contest 52 Q2
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [1860. Incremental Memory Leak](https://leetcode.com/problems/incremental-memory-leak)

[中文文档](/solution/1800-1899/1860.Incremental%20Memory%20Leak/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>memory1</code> và <code>memory2</code> biểu diễn dung lượng bộ nhớ khả dụng tính bằng bit trên hai thanh nhớ. Hiện có một chương trình bị lỗi đang chạy và tiêu thụ lượng bộ nhớ tăng dần theo từng giây.</p>

<p>Ở giây thứ <code>i<sup>th</sup></code> (bắt đầu từ 1), <code>i</code> bit bộ nhớ được cấp phát cho thanh nhớ có <strong>nhiều bộ nhớ khả dụng hơn</strong> (hoặc thanh nhớ thứ nhất nếu hai thanh có cùng lượng bộ nhớ khả dụng). Nếu không thanh nào còn ít nhất <code>i</code> bit bộ nhớ khả dụng, chương trình sẽ <strong>bị crash</strong>.</p>

<p>Trả về <em>một mảng chứa </em><code>[crashTime, memory1<sub>crash</sub>, memory2<sub>crash</sub>]</code><em>, trong đó </em><code>crashTime</code><em> là thời điểm (tính bằng giây) chương trình bị crash, còn </em><code>memory1<sub>crash</sub></code><em> và </em><code>memory2<sub>crash</sub></code><em> lần lượt là số bit bộ nhớ khả dụng còn lại trên thanh nhớ thứ nhất và thứ hai</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> memory1 = 2, memory2 = 2
<strong>Đầu ra:</strong> [3,1,0]
<strong>Giải thích:</strong> Bộ nhớ được cấp phát như sau:
- Ở giây thứ 1<sup>st</sup>, 1 bit bộ nhớ được cấp phát cho thanh nhớ 1. Thanh nhớ thứ nhất còn 1 bit bộ nhớ khả dụng.
- Ở giây thứ 2<sup>nd</sup>, 2 bit bộ nhớ được cấp phát cho thanh nhớ 2. Thanh nhớ thứ hai còn 0 bit bộ nhớ khả dụng.
- Ở giây thứ 3<sup>rd</sup>, chương trình bị crash. Hai thanh nhớ lần lượt còn 1 và 0 bit khả dụng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> memory1 = 8, memory2 = 11
<strong>Đầu ra:</strong> [6,0,4]
<strong>Giải thích:</strong> Bộ nhớ được cấp phát như sau:
- Ở giây thứ 1<sup>st</sup>, 1 bit bộ nhớ được cấp phát cho thanh nhớ 2. Thanh nhớ thứ hai còn 10 bit bộ nhớ khả dụng.
- Ở giây thứ 2<sup>nd</sup>, 2 bit bộ nhớ được cấp phát cho thanh nhớ 2. Thanh nhớ thứ hai còn 8 bit bộ nhớ khả dụng.
- Ở giây thứ 3<sup>rd</sup>, 3 bit bộ nhớ được cấp phát cho thanh nhớ 1. Thanh nhớ thứ nhất còn 5 bit bộ nhớ khả dụng.
- Ở giây thứ 4<sup>th</sup>, 4 bit bộ nhớ được cấp phát cho thanh nhớ 2. Thanh nhớ thứ hai còn 4 bit bộ nhớ khả dụng.
- Ở giây thứ 5<sup>th</sup>, 5 bit bộ nhớ được cấp phát cho thanh nhớ 1. Thanh nhớ thứ nhất còn 0 bit bộ nhớ khả dụng.
- Ở giây thứ 6<sup>th</sup>, chương trình bị crash. Hai thanh nhớ lần lượt còn 0 và 4 bit khả dụng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= memory1, memory2 &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ở giây thứ $i$, ta tiêu thụ $i$ đơn vị từ thanh nhớ hiện có nhiều dung lượng hơn cho đến khi cả hai thanh đều không đủ. Dung lượng có thể đạt $2^{31}-1$, nhưng tổng lượng đã tiêu thụ tăng theo bậc hai, nên thời điểm crash xấp xỉ $\sqrt{m_1+m_2}$ và mô phỏng trực tiếp là đủ.
>
> Bắt đầu từ $i=1$, trừ $i$ khỏi thanh nhớ còn nhiều dung lượng hơn nếu có thể, rồi trả về giây bị crash cùng lượng bộ nhớ còn lại.

<!-- thinking:end -->

Ta mô phỏng trực tiếp quá trình cấp phát bộ nhớ.

Giả sử $t$ là thời điểm chương trình thoát bất ngờ, khi đó hai thanh nhớ chắc chắn chứa đủ lượng bộ nhớ đã tiêu thụ ở thời điểm $t-1$ và trước đó, nên ta có:

$$
\sum_{i=1}^{t-1} i = \frac{t\times (t-1)}{2}  \leq (m_1+m_2)
$$

Độ phức tạp thời gian là $O(\sqrt{m_1+m_2})$, trong đó $m_1$ và $m_2$ lần lượt là dung lượng của hai thanh nhớ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def memLeak(self, memory1: int, memory2: int) -> List[int]:
        i = 1
        while i <= max(memory1, memory2):
            if memory1 >= memory2:
                memory1 -= i
            else:
                memory2 -= i
            i += 1
        return [i, memory1, memory2]
```

#### Java

```java
class Solution {
    public int[] memLeak(int memory1, int memory2) {
        int i = 1;
        for (; i <= Math.max(memory1, memory2); ++i) {
            if (memory1 >= memory2) {
                memory1 -= i;
            } else {
                memory2 -= i;
            }
        }
        return new int[] {i, memory1, memory2};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> memLeak(int memory1, int memory2) {
        int i = 1;
        for (; i <= max(memory1, memory2); ++i) {
            if (memory1 >= memory2) {
                memory1 -= i;
            } else {
                memory2 -= i;
            }
        }
        return {i, memory1, memory2};
    }
};
```

#### Go

```go
func memLeak(memory1 int, memory2 int) []int {
	i := 1
	for ; i <= memory1 || i <= memory2; i++ {
		if memory1 >= memory2 {
			memory1 -= i
		} else {
			memory2 -= i
		}
	}
	return []int{i, memory1, memory2}
}
```

#### TypeScript

```ts
function memLeak(memory1: number, memory2: number): number[] {
    let i = 1;
    for (; i <= Math.max(memory1, memory2); ++i) {
        if (memory1 >= memory2) {
            memory1 -= i;
        } else {
            memory2 -= i;
        }
    }
    return [i, memory1, memory2];
}
```

#### JavaScript

```js
/**
 * @param {number} memory1
 * @param {number} memory2
 * @return {number[]}
 */
var memLeak = function (memory1, memory2) {
    let i = 1;
    for (; i <= Math.max(memory1, memory2); ++i) {
        if (memory1 >= memory2) {
            memory1 -= i;
        } else {
            memory2 -= i;
        }
    }
    return [i, memory1, memory2];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
