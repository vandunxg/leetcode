---
comments: true
difficulty: Hard
rating: 2619
source: Weekly Contest 355 Q3
tags:
    - Greedy
    - Array
    - Math
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2790. Maximum Number of Groups With Increasing Length](https://leetcode.com/problems/maximum-number-of-groups-with-increasing-length)

[中文文档](/solution/2700-2799/2790.Maximum%20Number%20of%20Groups%20With%20Increasing%20Length/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>0-indexed</strong> <code>usageLimits</code> có độ dài <code>n</code>.</p>

<p>Nhiệm vụ của bạn là tạo các <strong>nhóm</strong> bằng cách sử dụng các số từ <code>0</code> đến <code>n - 1</code>, đảm bảo rằng mỗi số <code>i</code> được sử dụng không quá <code>usageLimits[i]</code> lần trong tổng số <strong>ở tất cả các nhóm</strong>. Bạn cũng phải thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Mỗi nhóm phải gồm các số <strong>khác nhau</strong>, nghĩa là không được có số trùng lặp trong cùng một nhóm.</li>
	<li>Mỗi nhóm (trừ nhóm đầu tiên) phải có độ dài <strong>lớn hơn nghiêm ngặt</strong> nhóm ngay trước đó.</li>
</ul>

<p>Trả về <em>một số nguyên biểu thị <strong>số nhóm lớn nhất</strong> mà bạn có thể tạo ra trong khi thỏa mãn các điều kiện trên.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> <code>usageLimits</code> = [1,2,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trong ví dụ này, ta có thể sử dụng 0 nhiều nhất một lần, 1 nhiều nhất hai lần và 2 nhiều nhất năm lần.
Một cách tạo số nhóm lớn nhất mà vẫn thỏa mãn các điều kiện là:
Nhóm 1 chứa số [2].
Nhóm 2 chứa các số [1,2].
Nhóm 3 chứa các số [0,1,2].
Có thể chứng minh rằng số nhóm lớn nhất là 3.
Vì vậy, đầu ra là 3. </pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> <code>usageLimits</code> = [2,1,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong ví dụ này, ta có thể sử dụng 0 nhiều nhất hai lần, 1 nhiều nhất một lần và 2 nhiều nhất hai lần.
Một cách tạo số nhóm lớn nhất mà vẫn thỏa mãn các điều kiện là:
Nhóm 1 chứa số [0].
Nhóm 2 chứa các số [1,2].
Có thể chứng minh rằng số nhóm lớn nhất là 2.
Vì vậy, đầu ra là 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> <code>usageLimits</code> = [1,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Trong ví dụ này, ta có thể sử dụng cả 0 và 1 nhiều nhất một lần.
Một cách tạo số nhóm lớn nhất mà vẫn thỏa mãn các điều kiện là:
Nhóm 1 chứa số [0].
Có thể chứng minh rằng số nhóm lớn nhất là 1.
Vì vậy, đầu ra là 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= usageLimits.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= usageLimits[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Nhóm $k$ cần $k$ số khác nhau, và số $i$ có thể được sử dụng nhiều nhất $usageLimits[i]$ lần; ta muốn tạo ra nhiều nhóm nhất. Việc thử từng số lượng nhóm rồi ghép các quota sẽ khá tốn kém.
>
> Sắp xếp các giới hạn rồi sử dụng từ trái sang phải: mỗi khi phần quota còn lại đủ để mở nhóm $k+1$, ta mở nhóm đó và trừ đi $k+1$, sau đó dồn phần còn lại vào giới hạn tiếp theo. Sử dụng các quota nhỏ trước giúp tối đa hóa số nhóm.

<!-- thinking:end -->

Sắp xếp các giới hạn theo thứ tự tăng dần và cộng dồn chúng. Mỗi đơn vị quota còn lại được dùng để thử mở thêm một nhóm; nếu thành công, ta trừ chi phí của nhóm đó khỏi tổng đang xét. Số nhóm cuối cùng chính là đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxIncreasingGroups(self, usageLimits: List[int]) -> int:
        usageLimits.sort()
        k, n = 0, len(usageLimits)
        for i in range(n):
            if usageLimits[i] > k:
                k += 1
                usageLimits[i] -= k
            if i + 1 < n:
                usageLimits[i + 1] += usageLimits[i]
        return k
```

#### Java

```java
class Solution {
    public int maxIncreasingGroups(List<Integer> usageLimits) {
        Collections.sort(usageLimits);
        int k = 0;
        long s = 0;
        for (int x : usageLimits) {
            s += x;
            if (s > k) {
                ++k;
                s -= k;
            }
        }
        return k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxIncreasingGroups(vector<int>& usageLimits) {
        sort(usageLimits.begin(), usageLimits.end());
        int k = 0;
        long long s = 0;
        for (int x : usageLimits) {
            s += x;
            if (s > k) {
                ++k;
                s -= k;
            }
        }
        return k;
    }
};
```

#### Go

```go
func maxIncreasingGroups(usageLimits []int) int {
	sort.Ints(usageLimits)
	s, k := 0, 0
	for _, x := range usageLimits {
		s += x
		if s > k {
			k++
			s -= k
		}
	}
	return k
}
```

#### TypeScript

```ts
function maxIncreasingGroups(usageLimits: number[]): number {
    usageLimits.sort((a, b) => a - b);
    let k = 0;
    let s = 0;
    for (const x of usageLimits) {
        s += x;
        if (s > k) {
            ++k;
            s -= k;
        }
    }
    return k;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
