---
comments: true
difficulty: Medium
tags:
    - Array
    - Binary Search
    - Interactive
---

<!-- problem:start -->

# [702. Search in a Sorted Array of Unknown Size 🔒](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size)

[中文文档](/solution/0700-0799/0702.Search%20in%20a%20Sorted%20Array%20of%20Unknown%20Size/README.md)

## Mô tả

<!-- description:start -->

<p>Đây là một <strong><em>bài toán tương tác</em></strong>.</p>

<p>Bạn có một mảng đã sắp xếp gồm các phần tử <strong>duy nhất</strong> nhưng <strong>không biết kích thước</strong>. Bạn không thể truy cập trực tiếp vào mảng, nhưng có thể dùng interface <code>ArrayReader</code> để đọc mảng. Bạn có thể gọi <code>ArrayReader.get(i)</code>, hàm này:</p>

<ul>
	<li>trả về giá trị tại chỉ số thứ <code>i<sup>th</sup></code> (<strong>đánh chỉ số từ 0</strong>) của mảng bí mật (tức <code>secret[i]</code>), hoặc</li>
	<li>trả về <code>2<sup>31</sup> - 1</code> nếu <code>i</code> nằm ngoài phạm vi của mảng.</li>
</ul>

<p>Bạn cũng được cho số nguyên <code>target</code>.</p>

<p>Hãy trả về chỉ số <code>k</code> trong mảng ẩn sao cho <code>secret[k] == target</code>, hoặc trả về <code>-1</code> nếu không tìm thấy.</p>

<p>Bạn phải viết thuật toán có độ phức tạp thời gian <code>O(log n)</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> secret = [-1,0,3,5,9,12], target = 9
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> 9 có trong secret và nằm ở chỉ số 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> secret = [-1,0,3,5,9,12], target = 2
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> 2 không có trong secret nên trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= secret.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= secret[i], target &lt;= 10<sup>4</sup></code></li>
	<li><code>secret</code> được sắp xếp theo thứ tự tăng nghiêm ngặt.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Search

<!-- thinking:start -->

> **Tư duy**
>
> Mảng đã được sắp xếp nhưng độ dài bị ẩn; ta chỉ có thể truy vấn các chỉ số, và lần đọc ngoài phạm vi sẽ trả về một giá trị sentinel. Duyệt tuyến tính từ $0$ cần $\Theta(M)$ lần gọi và không tận dụng thứ tự đã sắp xếp.
>
> Vì không biết độ dài, ta không thể binary search trên toàn bộ mảng. Tăng cận phải theo cấp số nhân cho đến khi giá trị tại đó không nhỏ hơn target sẽ khoanh vùng đáp án trong một đoạn có kích thước $O(M)$.
>
> Bắt đầu với $r=1$, nhân đôi cho đến khi chỉ số truy vấn đủ lớn, rồi binary search trong đoạn $[r/2, r]$. Số lần gọi API là $O(\log M)$.

<!-- thinking:end -->

Trước tiên, ta đặt pointer $r = 1$. Mỗi lần, ta kiểm tra giá trị tại vị trí $r$ có nhỏ hơn target hay không. Nếu có, ta nhân đôi $r$, tức dịch trái một bit, cho đến khi giá trị tại $r$ lớn hơn hoặc bằng target. Khi đó, ta biết target nằm trong đoạn $[r / 2, r]$.

Tiếp theo, ta đặt pointer $l = r / 2$ rồi dùng binary search để tìm vị trí của target trong đoạn $[l, r]$.

Độ phức tạp thời gian là $O(\log M)$, trong đó $M$ là vị trí của target. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# """
# This is ArrayReader's API interface.
# You should not implement it, or speculate about its implementation
# """
# class ArrayReader:
#    def get(self, index: int) -> int:


class Solution:
    def search(self, reader: "ArrayReader", target: int) -> int:
        r = 1
        while reader.get(r) < target:
            r <<= 1
        l = r >> 1
        while l < r:
            mid = (l + r) >> 1
            if reader.get(mid) >= target:
                r = mid
            else:
                l = mid + 1
        return l if reader.get(l) == target else -1
```

#### Java

```java
/**
 * // This is ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface ArrayReader {
 *     public int get(int index) {}
 * }
 */

class Solution {
    public int search(ArrayReader reader, int target) {
        int r = 1;
        while (reader.get(r) < target) {
            r <<= 1;
        }
        int l = r >> 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (reader.get(mid) >= target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return reader.get(l) == target ? l : -1;
    }
}
```

#### C++

```cpp
/**
 * // This is the ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * class ArrayReader {
 *   public:
 *     int get(int index);
 * };
 */

class Solution {
public:
    int search(const ArrayReader& reader, int target) {
        int r = 1;
        while (reader.get(r) < target) {
            r <<= 1;
        }
        int l = r >> 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (reader.get(mid) >= target) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return reader.get(l) == target ? l : -1;
    }
};
```

#### Go

```go
/**
 * // This is the ArrayReader's API interface.
 * // You should not implement it, or speculate about its implementation
 * type ArrayReader struct {
 * }
 *
 * func (this *ArrayReader) get(index int) int {}
 */

func search(reader ArrayReader, target int) int {
	r := 1
	for reader.get(r) < target {
		r <<= 1
	}
	l := r >> 1
	for l < r {
		mid := (l + r) >> 1
		if reader.get(mid) >= target {
			r = mid
		} else {
			l = mid + 1
		}
	}
	if reader.get(l) == target {
		return l
	}
	return -1
}
```

#### TypeScript

```ts
/**
 * class ArrayReader {
 *		// This is the ArrayReader's API interface.
 *		// You should not implement it, or speculate about its implementation
 *		get(index: number): number {};
 *  };
 */

function search(reader: ArrayReader, target: number): number {
    let r = 1;
    while (reader.get(r) < target) {
        r <<= 1;
    }
    let l = r >> 1;
    while (l < r) {
        const mid = (l + r) >> 1;
        if (reader.get(mid) >= target) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return reader.get(l) === target ? l : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
