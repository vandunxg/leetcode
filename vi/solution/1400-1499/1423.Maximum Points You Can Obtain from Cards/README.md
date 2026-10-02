---
comments: true
difficulty: Medium
rating: 1573
source: Weekly Contest 186 Q2
tags:
    - Array
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [1423. Maximum Points You Can Obtain from Cards](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards)

[中文文档](/solution/1400-1499/1423.Maximum%20Points%20You%20Can%20Obtain%20from%20Cards/README.md)

## Mô tả

<!-- description:start -->

<p>Có một số thẻ <strong>được xếp thành một hàng</strong>, mỗi thẻ có một số điểm tương ứng. Các điểm được cho trong mảng số nguyên <code>cardPoints</code>.</p>

<p>Trong một bước, bạn có thể lấy một thẻ ở đầu hoặc ở cuối hàng. Bạn phải lấy chính xác <code>k</code> thẻ.</p>

<p>Điểm số của bạn là tổng điểm của các thẻ đã lấy.</p>

<p>Cho mảng số nguyên <code>cardPoints</code> và số nguyên <code>k</code>, hãy trả về <em>điểm số lớn nhất</em> mà bạn có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> cardPoints = [1,2,3,4,5,6,1], k = 3
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Sau bước đầu tiên, điểm số của bạn luôn là 1. Tuy nhiên, chọn thẻ ngoài cùng bên phải trước sẽ tối đa hóa tổng điểm. Chiến lược tối ưu là lấy ba thẻ bên phải, cho điểm cuối cùng là 1 + 6 + 5 = 12.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> cardPoints = [2,2,2], k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Dù lấy hai thẻ nào, điểm số của bạn luôn là 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> cardPoints = [9,7,7,9,7,7,9], k = 7
<strong>Đầu ra:</strong> 55
<strong>Giải thích:</strong> Bạn phải lấy tất cả các thẻ. Điểm số của bạn là tổng điểm của tất cả các thẻ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= cardPoints.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= cardPoints[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= k &lt;= cardPoints.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Lấy $k$ thẻ từ hai đầu tương đương với lấy $i$ thẻ từ bên trái và $k-i$ thẻ từ bên phải. Vì $n\le 10^5$, ta không thể tính lại tổng cho từng $i$.
>
> Bắt đầu với $k$ thẻ ngoài cùng bên phải, sau đó thay thẻ ngoài cùng bên trái trong số đó bằng thẻ tiếp theo ở đầu bên trái, cập nhật tổng trong $O(1)$ và giữ lại giá trị lớn nhất.

<!-- thinking:end -->

Ta có thể dùng sliding window có độ dài $k$ để mô phỏng quá trình này.

Ban đầu, đặt window ở cuối mảng, tức là $k$ vị trí từ chỉ số $n-k$ đến chỉ số $n-1$. Gọi tổng điểm của các thẻ trong window là $s$, giá trị ban đầu của đáp án $ans$ cũng là $s$.

Tiếp theo, lần lượt xét trường hợp lấy $1, 2, ..., k$ thẻ từ đầu mảng. Giả sử thẻ được lấy là $cardPoints[i]$. Khi đó, ta cộng nó vào $s$. Vì độ dài của window bị giới hạn ở $k$, ta cần trừ $cardPoints[n-k+i]$ khỏi $s$. Nhờ vậy, ta có thể tính tổng điểm của $k$ thẻ đã lấy và cập nhật đáp án $ans$.

Độ phức tạp thời gian là $O(k)$, trong đó $k$ là số nguyên được cho trong đề bài. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxScore(self, cardPoints: List[int], k: int) -> int:
        ans = s = sum(cardPoints[-k:])
        for i, x in enumerate(cardPoints[:k]):
            s += x - cardPoints[-k + i]
            ans = max(ans, s)
        return ans
```

#### Java

```java
class Solution {
    public int maxScore(int[] cardPoints, int k) {
        int s = 0, n = cardPoints.length;
        for (int i = n - k; i < n; ++i) {
            s += cardPoints[i];
        }
        int ans = s;
        for (int i = 0; i < k; ++i) {
            s += cardPoints[i] - cardPoints[n - k + i];
            ans = Math.max(ans, s);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxScore(vector<int>& cardPoints, int k) {
        int n = cardPoints.size();
        int s = accumulate(cardPoints.end() - k, cardPoints.end(), 0);
        int ans = s;
        for (int i = 0; i < k; ++i) {
            s += cardPoints[i] - cardPoints[n - k + i];
            ans = max(ans, s);
        }
        return ans;
    }
};
```

#### Go

```go
func maxScore(cardPoints []int, k int) int {
	n := len(cardPoints)
	s := 0
	for _, x := range cardPoints[n-k:] {
		s += x
	}
	ans := s
	for i := 0; i < k; i++ {
		s += cardPoints[i] - cardPoints[n-k+i]
		ans = max(ans, s)
	}
	return ans
}
```

#### TypeScript

```ts
function maxScore(cardPoints: number[], k: number): number {
    const n = cardPoints.length;
    let s = cardPoints.slice(-k).reduce((a, b) => a + b);
    let ans = s;
    for (let i = 0; i < k; ++i) {
        s += cardPoints[i] - cardPoints[n - k + i];
        ans = Math.max(ans, s);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_score(card_points: Vec<i32>, k: i32) -> i32 {
        let n = card_points.len();
        let k = k as usize;
        let mut s: i32 = card_points[n - k..].iter().sum();
        let mut ans: i32 = s;
        for i in 0..k {
            s += card_points[i] - card_points[n - k + i];
            ans = ans.max(s);
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} cardPoints
 * @param {number} k
 * @return {number}
 */
var maxScore = function (cardPoints, k) {
    const n = cardPoints.length;
    let s = cardPoints.slice(-k).reduce((a, b) => a + b);
    let ans = s;
    for (let i = 0; i < k; ++i) {
        s += cardPoints[i] - cardPoints[n - k + i];
        ans = Math.max(ans, s);
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int MaxScore(int[] cardPoints, int k) {
        int n = cardPoints.Length;
        int s = cardPoints[^k..].Sum();
        int ans = s;
        for (int i = 0; i < k; ++i) {
            s += cardPoints[i] - cardPoints[n - k + i];
            ans = Math.Max(ans, s);
        }
        return ans;
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $cardPoints
     * @param Integer $k
     * @return Integer
     */
    function maxScore($cardPoints, $k) {
        $n = count($cardPoints);
        $s = array_sum(array_slice($cardPoints, -$k));
        $ans = $s;
        for ($i = 0; $i < $k; ++$i) {
            $s += $cardPoints[$i] - $cardPoints[$n - $k + $i];
            $ans = max($ans, $s);
        }
        return $ans;
    }
}
```

#### Scala

```scala
object Solution {
    def maxScore(cardPoints: Array[Int], k: Int): Int = {
        val n = cardPoints.length
        var s = cardPoints.takeRight(k).sum
        var ans = s
        for (i <- 0 until k) {
            s += cardPoints(i) - cardPoints(n - k + i)
            ans = ans.max(s)
        }
        ans
    }
}
```

#### Swift

```swift
class Solution {
    func maxScore(_ cardPoints: [Int], _ k: Int) -> Int {
        let n = cardPoints.count
        var s = cardPoints.suffix(k).reduce(0, +)
        var ans = s
        for i in 0..<k {
            s += cardPoints[i] - cardPoints[n - k + i]
            ans = max(ans, s)
        }
        return ans
    }
}
```

#### Ruby

```rb
# @param {Integer[]} card_points
# @param {Integer} k
# @return {Integer}
def max_score(card_points, k)
  n = card_points.length
  s = card_points[-k..].sum
  ans = s
  k.times do |i|
    s += card_points[i] - card_points[n - k + i]
    ans = [ans, s].max
  end
  ans
end
```

#### Kotlin

```kotlin
class Solution {
    fun maxScore(cardPoints: IntArray, k: Int): Int {
        val n = cardPoints.size
        var s = cardPoints.sliceArray(n - k until n).sum()
        var ans = s
        for (i in 0 until k) {
            s += cardPoints[i] - cardPoints[n - k + i]
            ans = maxOf(ans, s)
        }
        return ans
    }
}
```

#### Dart

```dart
class Solution {
  int maxScore(List<int> cardPoints, int k) {
    int n = cardPoints.length;
    int s = cardPoints.sublist(n - k).reduce((a, b) => a + b);
    int ans = s;
    for (int i = 0; i < k; ++i) {
      s += cardPoints[i] - cardPoints[n - k + i];
      ans = s > ans ? s : ans;
    }
    return ans;
  }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
