---
comments: true
difficulty: Medium
rating: 1646
source: Weekly Contest 287 Q3
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [2226. Maximum Candies Allocated to K Children](https://leetcode.com/problems/maximum-candies-allocated-to-k-children)

[Tài liệu tiếng Trung](/solution/2200-2299/2226.Maximum%20Candies%20Allocated%20to%20K%20Children/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>candies</code>. Mỗi phần tử trong mảng biểu thị một đống kẹo có kích thước <code>candies[i]</code>. Bạn có thể chia mỗi đống thành bao nhiêu <strong>đống con</strong> tùy ý, nhưng <strong>không thể gộp</strong> hai đống lại với nhau.</p>

<p>Bạn cũng được cho một số nguyên <code>k</code>. Hãy phân chia các đống kẹo cho <code>k</code> đứa trẻ sao cho mỗi đứa trẻ nhận được <strong>cùng số lượng kẹo</strong>. Mỗi đứa trẻ chỉ có thể nhận kẹo từ <strong>một đống</strong> duy nhất và một số đống kẹo có thể không được sử dụng.</p>

<p>Trả về <em><strong>số lượng kẹo lớn nhất</strong> mà mỗi đứa trẻ có thể nhận.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> candies = [5,8,6], k = 3
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Ta có thể chia candies[1] thành 2 đống có kích thước 5 và 3, đồng thời chia candies[2] thành 2 đống có kích thước 5 và 1. Khi đó, ta có năm đống kẹo với kích thước lần lượt là 5, 5, 3, 5 và 1. Ta có thể phân chia 3 đống có kích thước 5 cho 3 đứa trẻ. Có thể chứng minh rằng mỗi đứa trẻ không thể nhận nhiều hơn 5 viên kẹo.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> candies = [2,5], k = 11
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Có 11 đứa trẻ nhưng tổng cộng chỉ có 7 viên kẹo, nên không thể đảm bảo mỗi đứa trẻ nhận ít nhất một viên kẹo. Vì vậy, mỗi đứa trẻ không nhận được viên kẹo nào và đáp án là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= candies.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= candies[i] &lt;= 10<sup>7</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>12</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đứa trẻ nhận cùng một số lượng kẹo dương, và mỗi phần được cắt từ một đống duy nhất. Vì $k$ có thể lên tới $10^{12}$, ta không thể mô phỏng từng đứa trẻ. Tính khả thi đơn điệu theo $v$: nếu $v$ khả thi thì mọi $v$ dương nhỏ hơn đều khả thi.
>
> Ta tìm kiếm nhị phân $v$ trong khoảng $[0, \max(\textit{candies})]$. Một đống có kích thước $x$ tạo ra được $\lfloor x/v \rfloor$ phần; phép phân bổ hợp lệ khi tổng số phần ít nhất là $k$. Cận dưới $0$ bao quát trường hợp không phân bổ.

<!-- thinking:end -->

Ta nhận thấy rằng nếu mỗi đứa trẻ có thể nhận $v$ viên kẹo, thì với mọi $v' \lt v$, mỗi đứa trẻ cũng có thể nhận $v'$ viên kẹo. Vì vậy, ta có thể sử dụng tìm kiếm nhị phân để tìm giá trị $v$ lớn nhất sao cho mỗi đứa trẻ có thể nhận $v$ viên kẹo.

Ta đặt biên trái của tìm kiếm nhị phân là $l = 0$ và biên phải là $r = \max(\text{candies})$, trong đó $\max(\text{candies})$ là giá trị lớn nhất trong mảng $\text{candies}$. Trong mỗi bước tìm kiếm nhị phân, ta lấy giá trị giữa $v = \left\lfloor \frac{l + r + 1}{2} \right\rfloor$, sau đó tính tổng số phần kẹo có thể tạo ra. Nếu tổng số phần lớn hơn hoặc bằng $k$, điều đó có nghĩa là mỗi đứa trẻ có thể nhận $v$ viên kẹo, nên ta cập nhật biên trái thành $l = v$. Ngược lại, ta cập nhật biên phải thành $r = v - 1$. Cuối cùng, khi $l = r$, ta tìm được $v$ lớn nhất.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ là độ dài của mảng $\text{candies}$ và $M$ là giá trị lớn nhất trong mảng $\text{candies}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumCandies(self, candies: List[int], k: int) -> int:
        l, r = 0, max(candies)
        while l < r:
            mid = (l + r + 1) >> 1
            if sum(x // mid for x in candies) >= k:
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    public int maximumCandies(int[] candies, long k) {
        int l = 0, r = Arrays.stream(candies).max().getAsInt();
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            long cnt = 0;
            for (int x : candies) {
                cnt += x / mid;
            }
            if (cnt >= k) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumCandies(vector<int>& candies, long long k) {
        int l = 0, r = ranges::max(candies);
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            long long cnt = 0;
            for (int x : candies) {
                cnt += x / mid;
            }
            if (cnt >= k) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func maximumCandies(candies []int, k int64) int {
	return sort.Search(1e7, func(v int) bool {
		v++
		var cnt int64
		for _, x := range candies {
			cnt += int64(x / v)
		}
		return cnt < k
	})
}
```

#### TypeScript

```ts
function maximumCandies(candies: number[], k: number): number {
    let [l, r] = [0, Math.max(...candies)];
    while (l < r) {
        const mid = (l + r + 1) >> 1;
        const cnt = candies.reduce((acc, cur) => acc + Math.floor(cur / mid), 0);
        if (cnt >= k) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
