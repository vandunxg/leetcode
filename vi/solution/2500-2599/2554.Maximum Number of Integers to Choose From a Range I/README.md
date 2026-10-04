---
comments: true
difficulty: Medium
rating: 1333
source: Biweekly Contest 97 Q2
tags:
    - Greedy
    - Array
    - Hash Table
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2554. Maximum Number of Integers to Choose From a Range I](https://leetcode.com/problems/maximum-number-of-integers-to-choose-from-a-range-i)

[中文文档](/solution/2500-2599/2554.Maximum%20Number%20of%20Integers%20to%20Choose%20From%20a%20Range%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>banned</code> và hai số nguyên <code>n</code> và <code>maxSum</code>. Bạn cần chọn một số lượng số nguyên theo các quy tắc sau:</p>

<ul>
	<li>Các số nguyên được chọn phải nằm trong đoạn <code>[1, n]</code>.</li>
	<li>Mỗi số nguyên chỉ được chọn <strong>nhiều nhất một lần</strong>.</li>
	<li>Các số nguyên được chọn không được xuất hiện trong mảng <code>banned</code>.</li>
	<li>Tổng các số nguyên được chọn không được vượt quá <code>maxSum</code>.</li>
</ul>

<p>Trả về <em><strong>số lượng lớn nhất</strong> các số nguyên có thể chọn theo những quy tắc trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> banned = [1,6,5], n = 5, maxSum = 6
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có thể chọn các số nguyên 2 và 4.
2 và 4 đều thuộc đoạn [1, 5], không xuất hiện trong banned, và tổng của chúng là 6, không vượt quá maxSum.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> banned = [1,2,3,4,5,6,7], n = 8, maxSum = 1
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không thể chọn số nguyên nào mà vẫn thỏa mãn các điều kiện đã cho.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> banned = [11], n = 7, maxSum = 50
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Có thể chọn các số nguyên 1, 2, 3, 4, 5, 6 và 7.
Chúng thuộc đoạn [1, 7], không xuất hiện trong banned, và tổng của chúng là 28, không vượt quá maxSum.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= banned.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= banned[i], n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= maxSum &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Chọn nhiều số nguyên phân biệt nhất có thể từ $[1,n]$, bỏ qua các số trong $\textit{banned}$, sao cho tổng không vượt quá $\textit{maxSum}$. Các giá trị nhỏ hơn để lại nhiều khoảng trống hơn, vì vậy hãy chọn chúng trước.
>
> Với $n\le 10^4$, duyệt $i=1,2,\ldots$, dùng một set để bỏ qua các giá trị bị cấm, và dừng khi $i$ tiếp theo vượt quá tổng còn lại.

<!-- thinking:end -->

Ta dùng biến $s$ để biểu diễn tổng của các số nguyên đang được chọn, và biến $ans$ để biểu diễn số lượng số nguyên đang được chọn. Chuyển mảng `banned` thành một hash table để dễ dàng xác định một số nguyên có thể được chọn hay không.

Tiếp theo, bắt đầu liệt kê số nguyên $i$ từ $1$. Nếu $s + i \leq maxSum$ và $i$ không nằm trong `banned`, ta có thể chọn số nguyên $i$, đồng thời lần lượt cộng $i$ và $1$ vào $s$ và $ans$.

Cuối cùng, trả về $ans$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số nguyên được cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxCount(self, banned: List[int], n: int, maxSum: int) -> int:
        ans = s = 0
        ban = set(banned)
        for i in range(1, n + 1):
            if s + i > maxSum:
                break
            if i not in ban:
                ans += 1
                s += i
        return ans
```

#### Java

```java
class Solution {
    public int maxCount(int[] banned, int n, int maxSum) {
        Set<Integer> ban = new HashSet<>(banned.length);
        for (int x : banned) {
            ban.add(x);
        }
        int ans = 0, s = 0;
        for (int i = 1; i <= n && s + i <= maxSum; ++i) {
            if (!ban.contains(i)) {
                ++ans;
                s += i;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxCount(vector<int>& banned, int n, int maxSum) {
        unordered_set<int> ban(banned.begin(), banned.end());
        int ans = 0, s = 0;
        for (int i = 1; i <= n && s + i <= maxSum; ++i) {
            if (!ban.count(i)) {
                ++ans;
                s += i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxCount(banned []int, n int, maxSum int) (ans int) {
	ban := map[int]bool{}
	for _, x := range banned {
		ban[x] = true
	}
	s := 0
	for i := 1; i <= n && s+i <= maxSum; i++ {
		if !ban[i] {
			ans++
			s += i
		}
	}
	return
}
```

#### TypeScript

```ts
function maxCount(banned: number[], n: number, maxSum: number): number {
    const set = new Set(banned);
    let sum = 0;
    let ans = 0;
    for (let i = 1; i <= n; i++) {
        if (i + sum > maxSum) {
            break;
        }
        if (set.has(i)) {
            continue;
        }
        sum += i;
        ans++;
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashSet;
impl Solution {
    pub fn max_count(banned: Vec<i32>, n: i32, max_sum: i32) -> i32 {
        let mut set = banned.into_iter().collect::<HashSet<i32>>();
        let mut sum = 0;
        let mut ans = 0;
        for i in 1..=n {
            if sum + i > max_sum {
                break;
            }
            if set.contains(&i) {
                continue;
            }
            sum += i;
            ans += 1;
        }
        ans
    }
}
```

#### C

```c
int cmp(const void* a, const void* b) {
    return *(int*) a - *(int*) b;
}

int maxCount(int* banned, int bannedSize, int n, int maxSum) {
    qsort(banned, bannedSize, sizeof(int), cmp);
    int sum = 0;
    int ans = 0;
    for (int i = 1, j = 0; i <= n; i++) {
        if (sum + i > maxSum) {
            break;
        }
        if (j < bannedSize && i == banned[j]) {
            while (j < bannedSize && i == banned[j]) {
                j++;
            }
        } else {
            sum += i;
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tham lam + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 có độ phức tạp tuyến tính theo $n$ nên sẽ thất bại khi $n$ rất lớn. Các giá trị bị cấm chia $[1,n]$ thành những khoảng liên tiếp có tổng tiền tố là cấp số cộng; tìm kiếm nhị phân giúp tìm số phần tử mà ta vẫn có thể chọn. Lấp đầy các khoảng từ trái sang phải cho đến khi hết ngân sách.

<!-- thinking:end -->

Nếu $n$ rất lớn, việc liệt kê trong Phương pháp Một sẽ bị quá thời gian.

Ta có thể thêm $0$ và $n + 1$ vào mảng `banned`, loại bỏ các phần tử trùng nhau khỏi mảng `banned`, xóa các phần tử lớn hơn $n+1$, rồi sắp xếp mảng.

Tiếp theo, duyệt qua từng cặp phần tử kề nhau $i$ và $j$ trong mảng `banned`. Đoạn các số nguyên có thể chọn là $[i + 1, j - 1]$. Dùng tìm kiếm nhị phân để xác định số phần tử có thể chọn trong đoạn này, tìm số lượng phần tử tối đa có thể chọn, rồi cộng số lượng đó vào $ans$. Đồng thời, trừ tổng của các phần tử này khỏi `maxSum`. Nếu `maxSum` nhỏ hơn $0$, ta dừng vòng lặp. Trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng `banned`.

Các bài tương tự:

- [2557. Maximum Number of Integers to Choose From a Range II](https://github.com/doocs/leetcode/blob/main/solution/2500-2599/2557.Maximum%20Number%20of%20Integers%20to%20Choose%20From%20a%20Range%20II/README.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxCount(self, banned: List[int], n: int, maxSum: int) -> int:
        banned.extend([0, n + 1])
        ban = sorted(x for x in set(banned) if x < n + 2)
        ans = 0
        for i, j in pairwise(ban):
            left, right = 0, j - i - 1
            while left < right:
                mid = (left + right + 1) >> 1
                if (i + 1 + i + mid) * mid // 2 <= maxSum:
                    left = mid
                else:
                    right = mid - 1
            ans += left
            maxSum -= (i + 1 + i + left) * left // 2
            if maxSum <= 0:
                break
        return ans
```

#### Java

```java
class Solution {
    public int maxCount(int[] banned, int n, int maxSum) {
        Set<Integer> black = new HashSet<>();
        black.add(0);
        black.add(n + 1);
        for (int x : banned) {
            if (x < n + 2) {
                black.add(x);
            }
        }
        List<Integer> ban = new ArrayList<>(black);
        Collections.sort(ban);
        int ans = 0;
        for (int k = 1; k < ban.size(); ++k) {
            int i = ban.get(k - 1), j = ban.get(k);
            int left = 0, right = j - i - 1;
            while (left < right) {
                int mid = (left + right + 1) >>> 1;
                if ((i + 1 + i + mid) * 1L * mid / 2 <= maxSum) {
                    left = mid;
                } else {
                    right = mid - 1;
                }
            }
            ans += left;
            maxSum -= (i + 1 + i + left) * 1L * left / 2;
            if (maxSum <= 0) {
                break;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxCount(vector<int>& banned, int n, int maxSum) {
        banned.push_back(0);
        banned.push_back(n + 1);
        sort(banned.begin(), banned.end());
        banned.erase(unique(banned.begin(), banned.end()), banned.end());
        banned.erase(remove_if(banned.begin(), banned.end(), [&](int x) { return x > n + 1; }), banned.end());
        int ans = 0;
        for (int k = 1; k < banned.size(); ++k) {
            int i = banned[k - 1], j = banned[k];
            int left = 0, right = j - i - 1;
            while (left < right) {
                int mid = left + ((right - left + 1) / 2);
                if ((i + 1 + i + mid) * 1LL * mid / 2 <= maxSum) {
                    left = mid;
                } else {
                    right = mid - 1;
                }
            }
            ans += left;
            maxSum -= (i + 1 + i + left) * 1LL * left / 2;
            if (maxSum <= 0) {
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxCount(banned []int, n int, maxSum int) (ans int) {
	banned = append(banned, []int{0, n + 1}...)
	sort.Ints(banned)
	ban := []int{}
	for i, x := range banned {
		if (i > 0 && x == banned[i-1]) || x > n+1 {
			continue
		}
		ban = append(ban, x)
	}
	for k := 1; k < len(ban); k++ {
		i, j := ban[k-1], ban[k]
		left, right := 0, j-i-1
		for left < right {
			mid := (left + right + 1) >> 1
			if (i+1+i+mid)*mid/2 <= maxSum {
				left = mid
			} else {
				right = mid - 1
			}
		}
		ans += left
		maxSum -= (i + 1 + i + left) * left / 2
		if maxSum <= 0 {
			break
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
