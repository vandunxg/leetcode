---
comments: true
difficulty: Easy
tags:
    - Binary Search
    - Interactive
---

<!-- problem:start -->

# [278. First Bad Version](https://leetcode.com/problems/first-bad-version)

[中文文档](/solution/0200-0299/0278.First%20Bad%20Version/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn là product manager, hiện đang dẫn dắt một team phát triển sản phẩm mới. Thật không may, phiên bản mới nhất không vượt qua khâu kiểm tra chất lượng. Vì mỗi phiên bản được phát triển dựa trên phiên bản trước đó, nên tất cả phiên bản sau một phiên bản lỗi cũng đều lỗi.</p>

<p>Giả sử có <code>n</code> phiên bản <code>[1, 2, ..., n]</code> và bạn muốn tìm phiên bản lỗi đầu tiên, khiến tất cả phiên bản tiếp theo đều lỗi.</p>

<p>Bạn được cung cấp API <code>bool isBadVersion(version)</code>, trả về phiên bản <code>version</code> có lỗi hay không. Hãy triển khai hàm tìm phiên bản lỗi đầu tiên và giảm thiểu số lần gọi API.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, bad = 4
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
gọi isBadVersion(3) -&gt; false
gọi isBadVersion(5)&nbsp;-&gt; true
gọi isBadVersion(4)&nbsp;-&gt; true
Vậy 4 là phiên bản lỗi đầu tiên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, bad = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= bad &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Khi một phiên bản bị lỗi thì mọi phiên bản sau đó đều lỗi. Nếu $isBadVersion(\textit{mid})$ là đúng, phiên bản lỗi đầu tiên nằm ở nửa trái, bao gồm cả $\textit{mid}$; nếu không, nó nằm ở phía bên phải.
>
> Quá trình tìm kiếm kết thúc khi $l=r$; đây chính là phiên bản lỗi đầu tiên.

<!-- thinking:end -->

Ta đặt biên trái của tìm kiếm nhị phân là $l = 1$ và biên phải là $r = n$.

Trong khi $l < r$, ta tính vị trí giữa $\textit{mid} = \left\lfloor \frac{l + r}{2} \right\rfloor$, rồi gọi API `isBadVersion(mid)`. Nếu API trả về $\textit{true}$, phiên bản lỗi đầu tiên nằm trong đoạn $[l, \textit{mid}]$, nên ta đặt $r = \textit{mid}$; ngược lại, nó nằm trong đoạn $[\textit{mid} + 1, r]$, nên ta đặt $l = \textit{mid} + 1$.

Cuối cùng, ta trả về $l$.

Độ phức tạp thời gian là $O(\log n)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# The isBadVersion API is already defined for you.
# def isBadVersion(version: int) -> bool:


class Solution:
    def firstBadVersion(self, n: int) -> int:
        l, r = 1, n
        while l < r:
            mid = (l + r) >> 1
            if isBadVersion(mid):
                r = mid
            else:
                l = mid + 1
        return l
```

#### Java

```java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */

public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int l = 1, r = n;
        while (l < r) {
            int mid = (l + r) >>> 1;
            if (isBadVersion(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
// The API isBadVersion is defined for you.
// bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int l = 1, r = n;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (isBadVersion(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
/**
 * Forward declaration of isBadVersion API.
 * @param   version   your guess about first bad version
 * @return 	 	      true if current version is bad
 *			          false if current version is good
 * func isBadVersion(version int) bool;
 */

func firstBadVersion(n int) int {
	l, r := 1, n
	for l < r {
		mid := (l + r) >> 1
		if isBadVersion(mid) {
			r = mid
		} else {
			l = mid + 1
		}
	}
	return l
}
```

#### TypeScript

```ts
/**
 * The knows API is defined in the parent class Relation.
 * isBadVersion(version: number): boolean {
 *     ...
 * };
 */

var solution = function (isBadVersion: any) {
    return function (n: number): number {
        let [l, r] = [1, n];
        while (l < r) {
            const mid = (l + r) >>> 1;
            if (isBadVersion(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
};
```

#### Rust

```rust
// The API isBadVersion is defined for you.
// isBadVersion(version:i32)-> bool;
// to call it use self.isBadVersion(version)

impl Solution {
    pub fn first_bad_version(&self, n: i32) -> i32 {
		let (mut l, mut r) = (1, n);
        while l < r {
            let mid = l + (r - l) / 2;
            if self.isBadVersion(mid) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        l
    }
}
```

#### JavaScript

```js
/**
 * Definition for isBadVersion()
 *
 * @param {integer} version number
 * @return {boolean} whether the version is bad
 * isBadVersion = function(version) {
 *     ...
 * };
 */

/**
 * @param {function} isBadVersion()
 * @return {function}
 */
var solution = function (isBadVersion) {
    /**
     * @param {integer} n Total versions
     * @return {integer} The first bad version
     */
    return function (n) {
        let [l, r] = [1, n];
        while (l < r) {
            const mid = (l + r) >>> 1;
            if (isBadVersion(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
