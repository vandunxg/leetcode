---
comments: true
difficulty: Medium
tags:
    - Breadth-First Search
    - Backtracking
---

<!-- problem:start -->

# [967. Numbers With Same Consecutive Differences](https://leetcode.com/problems/numbers-with-same-consecutive-differences)

[中文文档](/solution/0900-0999/0967.Numbers%20With%20Same%20Consecutive%20Differences/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên n và k, hãy trả về <em>mảng chứa tất cả các số có độ dài </em><code>n</code><em> sao cho độ chênh lệch giữa mọi cặp chữ số liên tiếp đều bằng </em><code>k</code>. Bạn có thể trả về kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Lưu ý rằng các số nguyên không được có chữ số 0 ở đầu. Các số như <code>02</code> và <code>043</code> không hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, k = 7
<strong>Đầu ra:</strong> [181,292,707,818,929]
<strong>Giải thích:</strong> Lưu ý rằng 070 không phải là số hợp lệ vì có chữ số 0 ở đầu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, k = 1
<strong>Đầu ra:</strong> [10,12,21,23,32,34,43,45,54,56,65,67,76,78,87,89,98]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 9</code></li>
	<li><code>0 &lt;= k &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta xây dựng các số có $n$ chữ số sao cho độ chênh lệch giữa hai chữ số liền kề là $k$. Vì $n\le 9$, chỉ cần chạy DFS từ chữ số đầu tiên trong khoảng $1..9$. Chữ số tiếp theo là $last\pm k$ và phải nằm trong $0..9$; khi $k=0$, chỉ xét một nhánh để tránh kết quả trùng lặp. Thêm số vào kết quả khi đã có đủ $n$ chữ số.

<!-- thinking:end -->

Ta có thể duyệt chữ số đầu tiên của mọi số có độ dài $n$, sau đó dùng depth-first search để đệ quy xây dựng tất cả các số thỏa mãn điều kiện.

Cụ thể, trước tiên ta định nghĩa giá trị biên $\textit{boundary} = 10^{n-1}$, biểu thị giá trị nhỏ nhất của số cần xây dựng. Sau đó, ta duyệt chữ số đầu tiên từ $1$ đến $9$. Với mỗi chữ số $i$, ta đệ quy xây dựng số có độ dài $n$ bắt đầu bằng $i$.

Độ phức tạp thời gian là $(n \times 2^n \times |\Sigma|)$, trong đó $|\Sigma|$ là tập chữ số; ở bài này, $|\Sigma| = 9$. Độ phức tạp không gian là $O(2^n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numsSameConsecDiff(self, n: int, k: int) -> List[int]:
        def dfs(x: int):
            if x >= boundary:
                ans.append(x)
                return
            last = x % 10
            if last + k <= 9:
                dfs(x * 10 + last + k)
            if last - k >= 0 and k != 0:
                dfs(x * 10 + last - k)

        ans = []
        boundary = 10 ** (n - 1)
        for i in range(1, 10):
            dfs(i)
        return ans
```

#### Java

```java
class Solution {
    private List<Integer> ans = new ArrayList<>();
    private int boundary;
    private int k;

    public int[] numsSameConsecDiff(int n, int k) {
        this.k = k;
        boundary = (int) Math.pow(10, n - 1);
        for (int i = 1; i < 10; ++i) {
            dfs(i);
        }
        return ans.stream().mapToInt(i -> i).toArray();
    }

    private void dfs(int x) {
        if (x >= boundary) {
            ans.add(x);
            return;
        }
        int last = x % 10;
        if (last + k < 10) {
            dfs(x * 10 + last + k);
        }
        if (k != 0 && last - k >= 0) {
            dfs(x * 10 + last - k);
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> numsSameConsecDiff(int n, int k) {
        vector<int> ans;
        int boundary = pow(10, n - 1);
        auto dfs = [&](this auto&& dfs, int x) {
            if (x >= boundary) {
                ans.push_back(x);
                return;
            }
            int last = x % 10;
            if (last + k < 10) {
                dfs(x * 10 + last + k);
            }
            if (k != 0 && last - k >= 0) {
                dfs(x * 10 + last - k);
            }
        };
        for (int i = 1; i < 10; ++i) {
            dfs(i);
        }
        return ans;
    }
};
```

#### Go

```go
func numsSameConsecDiff(n int, k int) (ans []int) {
	bounary := int(math.Pow10(n - 1))
	var dfs func(int)
	dfs = func(x int) {
		if x >= bounary {
			ans = append(ans, x)
			return
		}
		last := x % 10
		if last+k < 10 {
			dfs(x*10 + last + k)
		}
		if k > 0 && last-k >= 0 {
			dfs(x*10 + last - k)
		}
	}
	for i := 1; i < 10; i++ {
		dfs(i)
	}
	return
}
```

#### TypeScript

```ts
function numsSameConsecDiff(n: number, k: number): number[] {
    const ans: number[] = [];
    const boundary = 10 ** (n - 1);
    const dfs = (x: number) => {
        if (x >= boundary) {
            ans.push(x);
            return;
        }
        const last = x % 10;
        if (last + k < 10) {
            dfs(x * 10 + last + k);
        }
        if (k > 0 && last - k >= 0) {
            dfs(x * 10 + last - k);
        }
    };
    for (let i = 1; i < 10; i++) {
        dfs(i);
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number} k
 * @return {number[]}
 */
var numsSameConsecDiff = function (n, k) {
    const ans = [];
    const boundary = 10 ** (n - 1);
    const dfs = x => {
        if (x >= boundary) {
            ans.push(x);
            return;
        }
        const last = x % 10;
        if (last + k < 10) {
            dfs(x * 10 + last + k);
        }
        if (k > 0 && last - k >= 0) {
            dfs(x * 10 + last - k);
        }
    };
    for (let i = 1; i < 10; i++) {
        dfs(i);
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
