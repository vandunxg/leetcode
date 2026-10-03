---
comments: true
difficulty: Medium
rating: 1334
source: Weekly Contest 274 Q3
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2126. Destroying Asteroids](https://leetcode.com/problems/destroying-asteroids)

[中文文档](/solution/2100-2199/2126.Destroying%20Asteroids/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>mass</code>, biểu thị khối lượng ban đầu của một hành tinh. Bạn cũng được cho một mảng số nguyên <code>asteroids</code>, trong đó <code>asteroids[i]</code> là khối lượng của tiểu hành tinh thứ <code>i<sup>th</sup></code>.</p>

<p>Bạn có thể sắp xếp cho hành tinh va chạm với các tiểu hành tinh theo <strong>bất kỳ thứ tự nào</strong>. Nếu khối lượng của hành tinh <b>lớn hơn hoặc bằng</b> khối lượng của tiểu hành tinh, tiểu hành tinh sẽ bị <strong>phá hủy</strong> và hành tinh <strong>nhận thêm</strong> khối lượng của tiểu hành tinh. Ngược lại, hành tinh sẽ bị phá hủy.</p>

<p>Trả về <code>true</code><em> nếu có thể phá hủy <strong>tất cả</strong> các tiểu hành tinh. Nếu không, trả về </em><code>false</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> mass = 10, asteroids = [3,9,19,5,21]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Một thứ tự sắp xếp các tiểu hành tinh có thể là [9,19,5,3,21]:
- Hành tinh va chạm với tiểu hành tinh có khối lượng 9. Khối lượng mới của hành tinh: 10 + 9 = 19
- Hành tinh va chạm với tiểu hành tinh có khối lượng 19. Khối lượng mới của hành tinh: 19 + 19 = 38
- Hành tinh va chạm với tiểu hành tinh có khối lượng 5. Khối lượng mới của hành tinh: 38 + 5 = 43
- Hành tinh va chạm với tiểu hành tinh có khối lượng 3. Khối lượng mới của hành tinh: 43 + 3 = 46
- Hành tinh va chạm với tiểu hành tinh có khối lượng 21. Khối lượng mới của hành tinh: 46 + 21 = 67
Tất cả các tiểu hành tinh đều bị phá hủy.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> mass = 5, asteroids = [4,9,23,4]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
Hành tinh không thể nào tăng khối lượng đủ để phá hủy tiểu hành tinh có khối lượng 23.
Sau khi phá hủy các tiểu hành tinh còn lại, khối lượng của nó sẽ là 5 + 4 + 9 + 4 = 22.
Giá trị này nhỏ hơn 23, nên một vụ va chạm sẽ không phá hủy được tiểu hành tinh cuối cùng.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= mass &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= asteroids.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= asteroids[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Hành tinh có thể hấp thụ một tiểu hành tinh có khối lượng không lớn hơn khối lượng của nó, sau đó lớn dần lên. Nếu va chạm với một tiểu hành tinh lớn trước, ta có thể thất bại dù những tiểu hành tinh nhỏ hơn có thể đã giúp tăng khối lượng đủ nhiều. Cả tổng khối lượng và thứ tự đều quan trọng.
>
> Hấp thụ các tiểu hành tinh nhỏ hơn trước chỉ làm tăng $mass$, nên không bao giờ cản trở một phép so sánh về sau; sắp xếp theo khối lượng là an toàn. Với $n\le 10^5$, chỉ cần duyệt một lần qua mảng đã sắp xếp.
>
> Sắp xếp $\textit{asteroids}$ và trả về thất bại nếu gặp $mass<x$; nếu không thì cộng $x$ vào $mass$.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể sắp xếp các tiểu hành tinh theo khối lượng tăng dần, sau đó duyệt qua các tiểu hành tinh. Nếu khối lượng của hành tinh nhỏ hơn khối lượng của tiểu hành tinh, hành tinh sẽ bị phá hủy và ta trả về `false`. Ngược lại, khối lượng của hành tinh sẽ tăng thêm khối lượng của tiểu hành tinh.

Nếu có thể phá hủy tất cả các tiểu hành tinh, trả về `true`.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó $n$ là số lượng tiểu hành tinh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def asteroidsDestroyed(self, mass: int, asteroids: List[int]) -> bool:
        asteroids.sort()
        for x in asteroids:
            if mass < x:
                return False
            mass += x
        return True
```

#### Java

```java
class Solution {
    public boolean asteroidsDestroyed(int mass, int[] asteroids) {
        Arrays.sort(asteroids);
        long m = mass;
        for (int x : asteroids) {
            if (m < x) {
                return false;
            }
            m += x;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool asteroidsDestroyed(int mass, vector<int>& asteroids) {
        ranges::sort(asteroids);
        long long m = mass;
        for (int x : asteroids) {
            if (m < x) {
                return false;
            }
            m += x;
        }
        return true;
    }
};
```

#### Go

```go
func asteroidsDestroyed(mass int, asteroids []int) bool {
	sort.Ints(asteroids)
	for _, x := range asteroids {
		if mass < x {
			return false
		}
		mass += x
	}
	return true
}
```

#### TypeScript

```ts
function asteroidsDestroyed(mass: number, asteroids: number[]): boolean {
    asteroids.sort((a, b) => a - b);
    for (const x of asteroids) {
        if (mass < x) {
            return false;
        }
        mass += x;
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn asteroids_destroyed(mass: i32, mut asteroids: Vec<i32>) -> bool {
        let mut mass = mass as i64;
        asteroids.sort_unstable();
        for &x in &asteroids {
            if mass < x as i64 {
                return false;
            }
            mass += x as i64;
        }
        true
    }
}
```

#### JavaScript

```js
/**
 * @param {number} mass
 * @param {number[]} asteroids
 * @return {boolean}
 */
var asteroidsDestroyed = function (mass, asteroids) {
    asteroids.sort((a, b) => a - b);
    for (const x of asteroids) {
        if (mass < x) {
            return false;
        }
        mass += x;
    }
    return true;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
