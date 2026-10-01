---
comments: true
difficulty: Easy
tags:
    - Binary Search
    - Interactive
---

<!-- problem:start -->

# [374. Guess Number Higher or Lower](https://leetcode.com/problems/guess-number-higher-or-lower)

[中文文档](/solution/0300-0399/0374.Guess%20Number%20Higher%20or%20Lower/README.md)

## Mô tả

<!-- description:start -->

<p>Ta đang chơi trò đoán số. Luật chơi như sau:</p>

<p>Tôi chọn một số từ <code>1</code> đến <code>n</code>. Bạn phải đoán số tôi đã chọn (số này không thay đổi trong suốt trò chơi).</p>

<p>Mỗi khi bạn đoán sai, tôi sẽ cho biết số đã chọn lớn hơn hay nhỏ hơn số bạn đoán.</p>

<p>Bạn có thể gọi API được định nghĩa sẵn <code>int guess(int num)</code>, API này trả về một trong ba kết quả:</p>

<ul>
	<li><code>-1</code>: Số bạn đoán lớn hơn số tôi đã chọn (tức là <code>num &gt; pick</code>).</li>
	<li><code>1</code>: Số bạn đoán nhỏ hơn số tôi đã chọn (tức là <code>num &lt; pick</code>).</li>
	<li><code>0</code>: Số bạn đoán bằng số tôi đã chọn (tức là <code>num == pick</code>).</li>
</ul>

<p>Hãy trả về <em>số mà tôi đã chọn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 10, pick = 6
<strong>Đầu ra:</strong> 6
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, pick = 1
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, pick = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
	<li><code>1 &lt;= pick &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Đoán một số trong $[1,n]$; API `guess` cho biết số cần tìm lớn hơn hay nhỏ hơn. Dò tuần tự có thể cần $n$ lần thử. Vì các số nằm trên một khoảng có thứ tự, ta dùng tìm kiếm nhị phân.
>
> Tìm $x$ đầu tiên sao cho `guess(x)\le 0`. Code dùng key $-guess(x)$ cùng một lần gọi `bisect`.

<!-- thinking:end -->

Ta tìm kiếm nhị phân trong đoạn $[1,..n]$ để tìm số đầu tiên thỏa mãn `guess(x) <= 0`; đó chính là đáp án.

Độ phức tạp thời gian là $O(\log n)$, với $n$ là giới hạn trên được cho trong đề bài. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if num is higher than the picked number
#          1 if num is lower than the picked number
#          otherwise return 0
# def guess(num: int) -> int:


class Solution:
    def guessNumber(self, n: int) -> int:
        return bisect.bisect(range(1, n + 1), 0, key=lambda x: -guess(x))
```

#### Java

```java
/**
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return 	     -1 if num is lower than the guess number
 *			      1 if num is higher than the guess number
 *               otherwise return 0
 * int guess(int num);
 */

public class Solution extends GuessGame {
    public int guessNumber(int n) {
        int left = 1, right = n;
        while (left < right) {
            int mid = (left + right) >>> 1;
            if (guess(mid) <= 0) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### C++

```cpp
/**
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return 	     -1 if num is lower than the guess number
 *			      1 if num is higher than the guess number
 *               otherwise return 0
 * int guess(int num);
 */

class Solution {
public:
    int guessNumber(int n) {
        int left = 1, right = n;
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (guess(mid) <= 0) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};
```

#### Go

```go
/**
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return 	     -1 if num is higher than the picked number
 *			      1 if num is lower than the picked number
 *               otherwise return 0
 * func guess(num int) int;
 */

func guessNumber(n int) int {
	return sort.Search(n, func(i int) bool {
		i++
		return guess(i) <= 0
	}) + 1
}
```

#### TypeScript

```ts
/**
 * Forward declaration of guess API.
 * @param {number} num   your guess
 * @return 	            -1 if num is lower than the guess number
 *			             1 if num is higher than the guess number
 *                       otherwise return 0
 * var guess = function(num) {}
 */

function guessNumber(n: number): number {
    let l = 1;
    let r = n;
    while (l < r) {
        const mid = (l + r) >>> 1;
        if (guess(mid) <= 0) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

#### Rust

```rust
/**
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return 	     -1 if num is lower than the guess number
 *			      1 if num is higher than the guess number
 *               otherwise return 0
 * unsafe fn guess(num: i32) -> i32 {}
 */

impl Solution {
    unsafe fn guessNumber(n: i32) -> i32 {
        let mut l = 1;
        let mut r = n;
        loop {
            let mid = l + (r - l) / 2;
            match guess(mid) {
                -1 => {
                    r = mid - 1;
                }
                1 => {
                    l = mid + 1;
                }
                _ => {
                    break mid;
                }
            }
        }
    }
}
```

#### C#

```cs
/**
 * Forward declaration of guess API.
 * @param  num   your guess
 * @return 	     -1 if num is higher than the picked number
 *			      1 if num is lower than the picked number
 *               otherwise return 0
 * int guess(int num);
 */

public class Solution : GuessGame {
    public int GuessNumber(int n) {
        int left = 1, right = n;
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (guess(mid) <= 0) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
