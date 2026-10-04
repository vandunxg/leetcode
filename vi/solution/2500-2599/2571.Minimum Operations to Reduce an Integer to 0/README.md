---
comments: true
difficulty: Medium
rating: 1649
source: Weekly Contest 333 Q2
tags:
    - Greedy
    - Bit Manipulation
    - Dynamic Programming
---

<!-- problem:start -->

# [2571. Minimum Operations to Reduce an Integer to 0](https://leetcode.com/problems/minimum-operations-to-reduce-an-integer-to-0)

[中文文档](/solution/2500-2599/2571.Minimum%20Operations%20to%20Reduce%20an%20Integer%20to%200/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code>, bạn có thể thực hiện thao tác sau <strong>bất kỳ</strong> số lần nào:</p>

<ul>
	<li>Cộng hoặc trừ một <strong>lũy thừa</strong> của <code>2</code> vào <code>n</code>.</li>
</ul>

<p>Trả về <em>số thao tác <strong>ít nhất</strong> để đưa </em><code>n</code><em> về </em><code>0</code>.</p>

<p>Một số <code>x</code> là lũy thừa của <code>2</code> nếu <code>x == 2<sup>i</sup></code>&nbsp;trong đó <code>i &gt;= 0</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 39
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau:
- Cộng 2<sup>0</sup> = 1 vào n, khi đó n = 40.
- Trừ 2<sup>3</sup> = 8 khỏi n, khi đó n = 32.
- Trừ 2<sup>5</sup> = 32 khỏi n, khi đó n = 0.
Có thể chứng minh rằng 3 là số thao tác ít nhất cần thực hiện để đưa n về 0.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 54
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau:
- Cộng 2<sup>1</sup> = 2 vào n, khi đó n = 56.
- Cộng 2<sup>3</sup> = 8 vào n, khi đó n = 64.
- Trừ 2<sup>6</sup> = 64 khỏi n, khi đó n = 0.
Vì vậy, số thao tác ít nhất là 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi bước cộng hoặc trừ một lũy thừa của hai. Xóa từng bit riêng lẻ tốn một bước cho mỗi bit 1, nhưng một chuỗi các bit 1 có thể được loại bỏ bằng cách cộng lũy thừa tiếp theo rồi trừ đi, nhờ đó tiết kiệm hơn.
>
> Duyệt các chuỗi từ bit thấp nhất. Một chuỗi đơn lẻ được trừ trong một bước; chuỗi dài hơn được gộp bằng cách cộng bit kế tiếp, để lại một bit nhớ bằng một. Một chuỗi còn lại ở cuối tốn một bước nếu độ dài là $1$, ngược lại tốn hai bước.

<!-- thinking:end -->

Ta chuyển số nguyên $n$ sang dạng nhị phân, bắt đầu từ bit thấp nhất:

- Nếu bit hiện tại là 1, ta tăng số lượng các bit 1 liên tiếp hiện tại;
- Nếu bit hiện tại là 0, ta kiểm tra xem số lượng các bit 1 liên tiếp hiện tại có lớn hơn 0 hay không. Nếu có, ta kiểm tra xem số lượng đó có bằng 1 hay không. Nếu bằng 1, điều đó có nghĩa là ta có thể loại bỏ bit 1 bằng một thao tác; nếu lớn hơn 1, ta có thể giảm số lượng các bit 1 liên tiếp xuống còn 1 bằng một thao tác.

Cuối cùng, ta cũng cần kiểm tra xem số lượng các bit 1 liên tiếp hiện tại có bằng 1 hay không. Nếu bằng 1, ta có thể loại bỏ bit 1 bằng một thao tác; nếu lớn hơn 1, ta có thể loại bỏ các bit 1 liên tiếp bằng hai thao tác.

Độ phức tạp thời gian là $O(\log n)$, còn độ phức tạp không gian là $O(1)$. Trong đó, $n$ là số nguyên được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, n: int) -> int:
        ans = cnt = 0
        while n:
            if n & 1:
                cnt += 1
            elif cnt:
                ans += 1
                cnt = 0 if cnt == 1 else 1
            n >>= 1
        if cnt == 1:
            ans += 1
        elif cnt > 1:
            ans += 2
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(int n) {
        int ans = 0, cnt = 0;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ++cnt;
            } else if (cnt > 0) {
                ++ans;
                cnt = cnt == 1 ? 0 : 1;
            }
        }
        ans += cnt == 1 ? 1 : 0;
        ans += cnt > 1 ? 2 : 0;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(int n) {
        int ans = 0, cnt = 0;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ++cnt;
            } else if (cnt > 0) {
                ++ans;
                cnt = cnt == 1 ? 0 : 1;
            }
        }
        ans += cnt == 1 ? 1 : 0;
        ans += cnt > 1 ? 2 : 0;
        return ans;
    }
};
```

#### Go

```go
func minOperations(n int) (ans int) {
	cnt := 0
	for ; n > 0; n >>= 1 {
		if n&1 == 1 {
			cnt++
		} else if cnt > 0 {
			ans++
			if cnt == 1 {
				cnt = 0
			} else {
				cnt = 1
			}
		}
	}
	if cnt == 1 {
		ans++
	} else if cnt > 1 {
		ans += 2
	}
	return
}
```

#### TypeScript

```ts
function minOperations(n: number): number {
    let [ans, cnt] = [0, 0];
    for (; n; n >>= 1) {
        if (n & 1) {
            ++cnt;
        } else if (cnt) {
            ++ans;
            cnt = cnt === 1 ? 0 : 1;
        }
    }
    if (cnt === 1) {
        ++ans;
    } else if (cnt > 1) {
        ans += 2;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
