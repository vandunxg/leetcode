---
comments: true
difficulty: Medium
rating: 2262
source: Weekly Contest 386 Q3
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [3048. Earliest Second to Mark Indices I](https://leetcode.com/problems/earliest-second-to-mark-indices-i)

[Tài liệu tiếng Trung](/solution/3000-3099/3048.Earliest%20Second%20to%20Mark%20Indices%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <strong>đánh chỉ số từ 1</strong>, <code>nums</code> và <code>changeIndices</code>, có độ dài lần lượt là <code>n</code> và <code>m</code>.</p>

<p>Ban đầu, tất cả các chỉ số trong <code>nums</code> đều chưa được đánh dấu. Nhiệm vụ của bạn là đánh dấu <strong>tất cả</strong> các chỉ số trong <code>nums</code>.</p>

<p>Ở mỗi giây <code>s</code>, theo thứ tự từ <code>1</code> đến <code>m</code> (<strong>bao gồm cả hai đầu</strong>), bạn có thể thực hiện <strong>một</strong> trong các thao tác sau:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> trong khoảng <code>[1, n]</code> và <strong>giảm</strong> <code>nums[i]</code> đi <code>1</code>.</li>
	<li>Nếu <code>nums[changeIndices[s]]</code> <strong>bằng</strong> <code>0</code>, <strong>đánh dấu</strong> chỉ số <code>changeIndices[s]</code>.</li>
	<li>Không làm gì.</li>
</ul>

<p>Trả về <em>một số nguyên biểu thị <strong>giây sớm nhất</strong> trong khoảng </em><code>[1, m]</code><em> mà tại đó có thể đánh dấu <strong>tất cả</strong> các chỉ số trong </em><code>nums</code><em> bằng cách chọn các thao tác tối ưu, hoặc </em><code>-1</code><em> nếu không thể.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,0], changeIndices = [2,2,2,2,3,2,2,1]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Trong ví dụ này, ta có 8 giây. Có thể thực hiện các thao tác sau để đánh dấu tất cả các chỉ số:
Giây 1: Chọn chỉ số 1 và giảm nums[1] đi một đơn vị. nums trở thành [1,2,0].
Giây 2: Chọn chỉ số 1 và giảm nums[1] đi một đơn vị. nums trở thành [0,2,0].
Giây 3: Chọn chỉ số 2 và giảm nums[2] đi một đơn vị. nums trở thành [0,1,0].
Giây 4: Chọn chỉ số 2 và giảm nums[2] đi một đơn vị. nums trở thành [0,0,0].
Giây 5: Đánh dấu chỉ số changeIndices[5], tức là đánh dấu chỉ số 3, vì nums[3] bằng 0.
Giây 6: Đánh dấu chỉ số changeIndices[6], tức là đánh dấu chỉ số 2, vì nums[2] bằng 0.
Giây 7: Không làm gì.
Giây 8: Đánh dấu chỉ số changeIndices[8], tức là đánh dấu chỉ số 1, vì nums[1] bằng 0.
Giờ tất cả các chỉ số đã được đánh dấu.
Có thể chứng minh rằng không thể đánh dấu tất cả các chỉ số trước giây thứ 8.
Vì vậy, đáp án là 8.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3], changeIndices = [1,1,1,2,1,1,1]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Trong ví dụ này, ta có 7 giây. Có thể thực hiện các thao tác sau để đánh dấu tất cả các chỉ số:
Giây 1: Chọn chỉ số 2 và giảm nums[2] đi một đơn vị. nums trở thành [1,2].
Giây 2: Chọn chỉ số 2 và giảm nums[2] đi một đơn vị. nums trở thành [1,1].
Giây 3: Chọn chỉ số 2 và giảm nums[2] đi một đơn vị. nums trở thành [1,0].
Giây 4: Đánh dấu chỉ số changeIndices[4], tức là đánh dấu chỉ số 2, vì nums[2] bằng 0.
Giây 5: Chọn chỉ số 1 và giảm nums[1] đi một đơn vị. nums trở thành [0,0].
Giây 6: Đánh dấu chỉ số changeIndices[6], tức là đánh dấu chỉ số 1, vì nums[1] bằng 0.
Giờ tất cả các chỉ số đã được đánh dấu.
Có thể chứng minh rằng không thể đánh dấu tất cả các chỉ số trước giây thứ 6.
Vì vậy, đáp án là 6.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1], changeIndices = [2,2,2]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Trong ví dụ này, không thể đánh dấu tất cả các chỉ số vì chỉ số 1 không xuất hiện trong changeIndices.
Vì vậy, đáp án là -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 2000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= m == changeIndices.length &lt;= 2000</code></li>
	<li><code>1 &lt;= changeIndices[i] &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Ở giây $s$, ta có thể giảm $\textit{changeIndices}[s]$ hoặc đánh dấu nó khi giá trị đã bằng $0$. $n,m \le 2000$. Tính khả thi đơn điệu theo $t$.
>
> Mỗi chỉ số nên được đánh dấu tại lần xuất hiện cuối cùng của nó trong $t$ giây đầu, để các giây trước đó có thể giảm các giá trị khác.
>
> Ta tìm kiếm nhị phân $t$ và mô phỏng bằng các thời điểm xuất hiện cuối đó: những giây khác trở thành token giảm, còn lần xuất hiện cuối phải có đủ token cho $nums[i]$.

<!-- thinking:end -->

Ta nhận thấy nếu có thể đánh dấu tất cả các chỉ số trong $t$ giây, thì cũng có thể đánh dấu tất cả các chỉ số trong $t' \geq t$ giây. Vì vậy, ta có thể sử dụng tìm kiếm nhị phân để tìm giây sớm nhất.

Ta đặt biên trái và phải của phép tìm kiếm nhị phân lần lượt là $l = 1$ và $r = m + 1$, trong đó $m$ là độ dài của mảng `changeIndices`. Với mỗi $t = \frac{l + r}{2}$, ta kiểm tra xem có thể đánh dấu tất cả các chỉ số trong $t$ giây hay không. Nếu có, ta di chuyển biên phải đến $t$; nếu không, ta di chuyển biên trái đến $t + 1$. Cuối cùng, ta kiểm tra xem biên trái có lớn hơn $m$ hay không; nếu có, trả về $-1$, nếu không, trả về biên trái.

Điểm mấu chốt của bài toán là cách kiểm tra xem có thể đánh dấu tất cả các chỉ số trong $t$ giây hay không. Ta có thể dùng một mảng $last$ để ghi lại thời điểm mới nhất mà mỗi chỉ số cần được đánh dấu, dùng biến $decrement$ để ghi lại số lần giảm hiện có thể thực hiện, và dùng biến $marked$ để ghi lại số chỉ số đã được đánh dấu.

Ta duyệt qua $t$ phần tử đầu tiên của mảng `changeIndices`. Với mỗi phần tử $i$, nếu $last[i] = s$, ta cần kiểm tra xem $decrement$ có lớn hơn hoặc bằng $nums[i - 1]$ hay không. Nếu có, ta trừ $nums[i - 1]$ khỏi $decrement$ và tăng $marked$ lên một; nếu không, trả về `False`. Nếu $last[i] \neq s$, ta tạm thời chưa đánh dấu chỉ số đó, nên tăng $decrement$ lên một. Cuối cùng, ta kiểm tra xem $marked$ có bằng $n$ hay không; nếu có, trả về `True`, nếu không, trả về `False`.

Độ phức tạp thời gian là $O(m \times \log m)$, và độ phức tạp không gian là $O(n)$. Trong đó $n$ và $m$ lần lượt là độ dài của `nums` và `changeIndices`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def earliestSecondToMarkIndices(
        self, nums: List[int], changeIndices: List[int]
    ) -> int:
        def check(t: int) -> bool:
            decrement = 0
            marked = 0
            last = {i: s for s, i in enumerate(changeIndices[:t])}
            for s, i in enumerate(changeIndices[:t]):
                if last[i] == s:
                    if decrement < nums[i - 1]:
                        return False
                    decrement -= nums[i - 1]
                    marked += 1
                else:
                    decrement += 1
            return marked == len(nums)

        m = len(changeIndices)
        l = bisect_left(range(1, m + 2), True, key=check) + 1
        return -1 if l > m else l
```

#### Java

```java
class Solution {
    private int[] nums;
    private int[] changeIndices;

    public int earliestSecondToMarkIndices(int[] nums, int[] changeIndices) {
        this.nums = nums;
        this.changeIndices = changeIndices;
        int m = changeIndices.length;
        int l = 1, r = m + 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l > m ? -1 : l;
    }

    private boolean check(int t) {
        int[] last = new int[nums.length + 1];
        for (int s = 0; s < t; ++s) {
            last[changeIndices[s]] = s;
        }
        int decrement = 0;
        int marked = 0;
        for (int s = 0; s < t; ++s) {
            int i = changeIndices[s];
            if (last[i] == s) {
                if (decrement < nums[i - 1]) {
                    return false;
                }
                decrement -= nums[i - 1];
                ++marked;
            } else {
                ++decrement;
            }
        }
        return marked == nums.length;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int earliestSecondToMarkIndices(vector<int>& nums, vector<int>& changeIndices) {
        int n = nums.size();
        int last[n + 1];
        auto check = [&](int t) {
            memset(last, 0, sizeof(last));
            for (int s = 0; s < t; ++s) {
                last[changeIndices[s]] = s;
            }
            int decrement = 0, marked = 0;
            for (int s = 0; s < t; ++s) {
                int i = changeIndices[s];
                if (last[i] == s) {
                    if (decrement < nums[i - 1]) {
                        return false;
                    }
                    decrement -= nums[i - 1];
                    ++marked;
                } else {
                    ++decrement;
                }
            }
            return marked == n;
        };

        int m = changeIndices.size();
        int l = 1, r = m + 1;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l > m ? -1 : l;
    }
};
```

#### Go

```go
func earliestSecondToMarkIndices(nums []int, changeIndices []int) int {
	n, m := len(nums), len(changeIndices)
	l := sort.Search(m+1, func(t int) bool {
		last := make([]int, n+1)
		for s, i := range changeIndices[:t] {
			last[i] = s
		}
		decrement, marked := 0, 0
		for s, i := range changeIndices[:t] {
			if last[i] == s {
				if decrement < nums[i-1] {
					return false
				}
				decrement -= nums[i-1]
				marked++
			} else {
				decrement++
			}
		}
		return marked == n
	})
	if l > m {
		return -1
	}
	return l
}
```

#### TypeScript

```ts
function earliestSecondToMarkIndices(nums: number[], changeIndices: number[]): number {
    const [n, m] = [nums.length, changeIndices.length];
    let [l, r] = [1, m + 1];
    const check = (t: number): boolean => {
        const last: number[] = Array(n + 1).fill(0);
        for (let s = 0; s < t; ++s) {
            last[changeIndices[s]] = s;
        }
        let [decrement, marked] = [0, 0];
        for (let s = 0; s < t; ++s) {
            const i = changeIndices[s];
            if (last[i] === s) {
                if (decrement < nums[i - 1]) {
                    return false;
                }
                decrement -= nums[i - 1];
                ++marked;
            } else {
                ++decrement;
            }
        }
        return marked === n;
    };
    while (l < r) {
        const mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l > m ? -1 : l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
