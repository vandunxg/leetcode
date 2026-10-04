---
comments: true
difficulty: Medium
rating: 1784
source: Weekly Contest 374 Q2
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2952. Minimum Number of Coins to be Added](https://leetcode.com/problems/minimum-number-of-coins-to-be-added)

[中文文档](/solution/2900-2999/2952.Minimum%20Number%20of%20Coins%20to%20be%20Added/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>coins</code> được đánh chỉ số từ <strong>0</strong>, biểu diễn các giá trị của những đồng xu hiện có, và một số nguyên <code>target</code>.</p>

<p>Một số nguyên <code>x</code> là <strong>có thể tạo được</strong> nếu tồn tại một dãy con của <code>coins</code> có tổng bằng <code>x</code>.</p>

<p>Trả về <em>số lượng <strong>tối thiểu</strong> đồng xu <strong>với giá trị bất kỳ</strong> cần thêm vào mảng để mọi số nguyên trong đoạn</em> <code>[1, target]</code><em> đều <strong>có thể tạo được</strong></em>.</p>

<p>Một <strong>dãy con</strong> của một mảng là một mảng mới <strong>không rỗng</strong>, được tạo từ mảng ban đầu bằng cách xóa một số phần tử (<strong>có thể không xóa phần tử nào</strong>) mà không làm thay đổi thứ tự tương đối của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> coins = [1,4,10], target = 19
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta cần thêm các đồng xu 2 và 8. Mảng kết quả sẽ là [1,2,4,8,10].
Có thể chứng minh rằng mọi số nguyên từ 1 đến 19 đều có thể tạo được từ mảng kết quả, và 2 là số lượng đồng xu tối thiểu cần thêm vào mảng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> coins = [1,4,10,5,7,19], target = 19
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Ta chỉ cần thêm đồng xu 2. Mảng kết quả sẽ là [1,2,4,5,7,10,19].
Có thể chứng minh rằng mọi số nguyên từ 1 đến 19 đều có thể tạo được từ mảng kết quả, và 1 là số lượng đồng xu tối thiểu cần thêm vào mảng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> coins = [1,1,1], target = 20
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta cần thêm các đồng xu 4, 8 và 16. Mảng kết quả sẽ là [1,1,1,4,8,16].
Có thể chứng minh rằng mọi số nguyên từ 1 đến 20 đều có thể tạo được từ mảng kết quả, và 3 là số lượng đồng xu tối thiểu cần thêm vào mảng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= target &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= coins.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= coins[i] &lt;= target</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Construction

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần tạo ra mọi giá trị trong $[1,target]$ bằng các đồng xu đã cho và các đồng xu bổ sung. Nếu $[0,s-1]$ đã được phủ, một đồng xu mới $x \le s$ mở rộng đoạn thành $s+x-1$; nếu đồng xu tiếp theo lớn hơn, ta phải thêm $s$ và tăng gấp đôi đoạn. Đây là chiến lược greedy tiêu chuẩn để phủ đoạn.
>
> Sắp xếp các đồng xu và di chuyển một con trỏ. Khi $s \le target$, hoặc ta đưa đồng xu tiếp theo vào phạm vi, hoặc đặt $s \leftarrow 2s$ và tăng đáp án.

<!-- thinking:end -->

Giả sử số tiền hiện tại cần tạo là $s$, và ta đã tạo được mọi số trong $[0,...,s-1]$. Nếu có một đồng xu mới $x$, thêm nó vào mảng cho phép tạo mọi số trong $[x, s+x-1]$.

Tiếp theo, xét hai trường hợp:

- Nếu $x \le s$, ta có thể hợp nhất hai đoạn trên để nhận được mọi số trong $[0, s+x-1]$.
- Nếu $x \gt s$, ta cần thêm một đồng xu có mệnh giá $s$ để có thể tạo mọi số trong $[0, 2s-1]$. Sau đó, ta tiếp tục xét mối quan hệ giữa $x$ và $s$.

Do đó, ta sắp xếp mảng $coins$ theo thứ tự tăng dần, rồi duyệt các đồng xu trong mảng từ nhỏ đến lớn. Với mỗi đồng xu $x$, ta xét hai trường hợp trên cho đến khi $s > target$.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumAddedCoins(self, coins: List[int], target: int) -> int:
        coins.sort()
        s = 1
        ans = i = 0
        while s <= target:
            if i < len(coins) and coins[i] <= s:
                s += coins[i]
                i += 1
            else:
                s <<= 1
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int minimumAddedCoins(int[] coins, int target) {
        Arrays.sort(coins);
        int ans = 0;
        for (int i = 0, s = 1; s <= target;) {
            if (i < coins.length && coins[i] <= s) {
                s += coins[i++];
            } else {
                s <<= 1;
                ++ans;
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
    int minimumAddedCoins(vector<int>& coins, int target) {
        sort(coins.begin(), coins.end());
        int ans = 0;
        for (int i = 0, s = 1; s <= target;) {
            if (i < coins.size() && coins[i] <= s) {
                s += coins[i++];
            } else {
                s <<= 1;
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumAddedCoins(coins []int, target int) (ans int) {
	slices.Sort(coins)
	for i, s := 0, 1; s <= target; {
		if i < len(coins) && coins[i] <= s {
			s += coins[i]
			i++
		} else {
			s <<= 1
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function minimumAddedCoins(coins: number[], target: number): number {
    coins.sort((a, b) => a - b);
    let ans = 0;
    for (let i = 0, s = 1; s <= target;) {
        if (i < coins.length && coins[i] <= s) {
            s += coins[i++];
        } else {
            s <<= 1;
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
