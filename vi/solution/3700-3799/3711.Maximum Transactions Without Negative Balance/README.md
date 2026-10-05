---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3711. Maximum Transactions Without Negative Balance 🔒](https://leetcode.com/problems/maximum-transactions-without-negative-balance)

[中文文档](/solution/3700-3799/3711.Maximum%20Transactions%20Without%20Negative%20Balance/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>transactions</code>, trong đó <code>transactions[i]</code> biểu thị số tiền của giao dịch thứ <code>i<sup>th</sup></code>:</p>

<ul>
	<li>Giá trị dương nghĩa là tiền được <strong>nhận</strong>.</li>
	<li>Giá trị âm nghĩa là tiền được <strong>gửi đi</strong>.</li>
</ul>

<p>Tài khoản bắt đầu với số dư bằng 0 và số dư <strong>không bao giờ được âm</strong>. Các giao dịch phải được xét theo thứ tự đã cho, nhưng bạn được phép bỏ qua một số giao dịch.</p>

<p>Trả về một số nguyên biểu thị <strong>số lượng giao dịch tối đa</strong> có thể thực hiện mà số dư không bao giờ âm.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">transactions = [2,-5,3,-1,-2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một dãy tối ưu là <code>[2, 3, -1, -2]</code>, số dư: <code>0 &rarr; 2 &rarr; 5 &rarr; 4 &rarr; 2</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">transactions = [-1,-2,-3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tất cả giao dịch đều âm. Chọn bất kỳ giao dịch nào cũng khiến số dư bị âm.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">transactions = [3,-2,3,-2,1,-1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể chọn tất cả giao dịch theo đúng thứ tự, số dư: <code>0 &rarr; 3 &rarr; 1 &rarr; 4 &rarr; 2 &rarr; 3 &rarr; 2</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= transactions.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= transactions[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Các giao dịch phải được giữ nguyên thứ tự nhưng có thể bỏ qua, và $n\le 10^5$ khiến việc quay lui không khả thi. Ta tham lam chọn mọi giao dịch và mỗi khi số dư âm, loại bỏ giá trị nhỏ nhất đã chọn, tức giao dịch làm số dư giảm nhiều nhất. Ordered set cho phép xóa phần tử nhỏ nhất trong $O(\log n)$.

<!-- thinking:end -->

Ta sử dụng một ordered set (chẳng hạn như multiset của C++, TreeMap của Java, SortedList của Python) để lưu các số tiền giao dịch đã chọn, đồng thời duy trì biến $s$ để ghi nhận số dư hiện tại. Ban đầu $s=0$, và đáp án $\textit{ans}$ được khởi tạo bằng số lượng giao dịch.

Sau đó, ta duyệt qua từng số tiền giao dịch $x$:

1. Cộng $x$ vào số dư $s$ và thêm $x$ vào ordered set.
2. Nếu số dư $s$ trở thành số âm tại thời điểm này, điều đó có nghĩa là một số giá trị âm trong các giao dịch đang được chọn đã khiến số dư không đủ. Để giữ lại nhiều giao dịch nhất có thể, ta nên loại bỏ giá trị nhỏ nhất trong các giao dịch đang được chọn (vì loại bỏ giá trị nhỏ nhất có thể làm số dư tăng nhiều nhất). Ta loại bỏ giá trị nhỏ nhất $y$ khỏi ordered set, trừ $y$ khỏi số dư $s$ và giảm đáp án $\textit{ans}$ đi $1$.
3. Lặp lại bước 2 cho đến khi số dư $s$ không còn âm.

Sau khi duyệt xong, đáp án $\textit{ans}$ là số lượng giao dịch tối đa có thể thực hiện.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là số lượng giao dịch. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTransactions(self, transactions: List[int]) -> int:
        st = SortedList()
        s = 0
        ans = len(transactions)
        for x in transactions:
            s += x
            st.add(x)
            while s < 0:
                y = st.pop(0)
                s -= y
                ans -= 1
        return ans
```

#### Java

```java
class Solution {
    public int maxTransactions(int[] transactions) {
        TreeMap<Integer, Integer> tm = new TreeMap<>();
        int ans = transactions.length;
        long s = 0;
        for (int x : transactions) {
            s += x;
            tm.merge(x, 1, Integer::sum);
            while (s < 0) {
                int y = tm.firstKey();
                s -= y;
                --ans;
                if (tm.merge(y, -1, Integer::sum) == 0) {
                    tm.remove(y);
                }
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
    int maxTransactions(vector<int>& transactions) {
        multiset<int> st;
        int ans = transactions.size();
        long long s = 0;
        for (int x : transactions) {
            s += x;
            st.insert(x);
            while (s < 0) {
                auto it = st.begin();
                int y = *it;
                st.erase(it);
                s -= y;
                --ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxTransactions(transactions []int) int {
	tm := redblacktree.New[int, int]()
	ans := len(transactions)
	var s int64

	for _, x := range transactions {
		s += int64(x)
		if cnt, ok := tm.Get(x); ok {
			tm.Put(x, cnt+1)
		} else {
			tm.Put(x, 1)
		}

		for s < 0 {
			it := tm.Iterator()
			it.Begin()
			it.Next()
			y := it.Key()
			s -= int64(y)
			ans--

			cnt, _ := tm.Get(y)
			if cnt == 1 {
				tm.Remove(y)
			} else {
				tm.Put(y, cnt-1)
			}
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
