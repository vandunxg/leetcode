---
comments: true
difficulty: Medium
rating: 1674
source: Biweekly Contest 10 Q3
tags:
    - Breadth-First Search
    - Math
    - Backtracking
---

<!-- problem:start -->

# [1215. Stepping Numbers 🔒](https://leetcode.com/problems/stepping-numbers)

[中文文档](/solution/1200-1299/1215.Stepping%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Số stepping</strong> là số nguyên mà mọi cặp chữ số liền kề có độ chênh lệch tuyệt đối đúng bằng <code>1</code>.</p>

<ul>
	<li>Ví dụ, <code>321</code> là <strong>số stepping</strong>, còn <code>421</code> thì không.</li>
</ul>

<p>Cho hai số nguyên <code>low</code> và <code>high</code>, hãy trả về <em>danh sách đã sắp xếp gồm tất cả <strong>số stepping</strong> trong đoạn bao gồm cả hai đầu mút</em> <code>[low, high]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> low = 0, high = 21
<strong>Đầu ra:</strong> [0,1,2,3,4,5,6,7,8,9,10,12,21]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> low = 10, high = 15
<strong>Đầu ra:</strong> [10,12]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= low &lt;= high &lt;= 2 * 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: BFS

<!-- thinking:start -->

> **Tư duy**
>
> $high$ có thể bằng $2\times 10^9$, nên không thể kiểm tra từng số nguyên. Chữ số tiếp theo của một số stepping chỉ có thể là chữ số cuối $\pm 1$, vì vậy ta bắt đầu từ $1\sim 9$ rồi mở rộng bằng BFS; số lượng ứng viên nhỏ hơn nhiều so với toàn miền giá trị.
>
> Các giá trị trong queue được lấy theo thứ tự tăng dần; ta dừng khi vượt quá $high$ và chỉ giữ các giá trị thuộc $[low,high]$. Xử lý riêng số 0. BFS vừa tạo kết quả theo thứ tự vừa tránh trùng lặp.

<!-- thinking:end -->

Trước tiên, nếu $low$ bằng $0$, ta thêm $0$ vào kết quả.

Tiếp theo, ta tạo queue $q$ và thêm các số từ $1 \sim 9$ vào queue. Sau đó, liên tục lấy phần tử khỏi queue. Gọi phần tử hiện tại là $v$. Nếu $v$ lớn hơn $high$, ta dừng tìm kiếm. Nếu $v$ thuộc đoạn $[low, high]$, ta thêm $v$ vào kết quả. Tiếp đó, gọi chữ số cuối của $v$ là $x$. Nếu $x \gt 0$, ta thêm $v \times 10 + x - 1$ vào queue. Nếu $x \lt 9$, ta thêm $v \times 10 + x + 1$ vào queue. Lặp lại các bước trên cho đến khi queue rỗng.

Độ phức tạp thời gian là $O(10 \times 2^{\log M})$, độ phức tạp không gian là $O(2^{\log M})$, trong đó $M$ là số chữ số của $high$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSteppingNumbers(self, low: int, high: int) -> List[int]:
        ans = []
        if low == 0:
            ans.append(0)
        q = deque(range(1, 10))
        while q:
            v = q.popleft()
            if v > high:
                break
            if v >= low:
                ans.append(v)
            x = v % 10
            if x:
                q.append(v * 10 + x - 1)
            if x < 9:
                q.append(v * 10 + x + 1)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> countSteppingNumbers(int low, int high) {
        List<Integer> ans = new ArrayList<>();
        if (low == 0) {
            ans.add(0);
        }
        Deque<Long> q = new ArrayDeque<>();
        for (long i = 1; i < 10; ++i) {
            q.offer(i);
        }
        while (!q.isEmpty()) {
            long v = q.pollFirst();
            if (v > high) {
                break;
            }
            if (v >= low) {
                ans.add((int) v);
            }
            int x = (int) v % 10;
            if (x > 0) {
                q.offer(v * 10 + x - 1);
            }
            if (x < 9) {
                q.offer(v * 10 + x + 1);
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
    vector<int> countSteppingNumbers(int low, int high) {
        vector<int> ans;
        if (low == 0) {
            ans.push_back(0);
        }
        queue<long long> q;
        for (int i = 1; i < 10; ++i) {
            q.push(i);
        }
        while (!q.empty()) {
            long long v = q.front();
            q.pop();
            if (v > high) {
                break;
            }
            if (v >= low) {
                ans.push_back(v);
            }
            int x = v % 10;
            if (x > 0) {
                q.push(v * 10 + x - 1);
            }
            if (x < 9) {
                q.push(v * 10 + x + 1);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countSteppingNumbers(low int, high int) []int {
	ans := []int{}
	if low == 0 {
		ans = append(ans, 0)
	}
	q := []int{1, 2, 3, 4, 5, 6, 7, 8, 9}
	for len(q) > 0 {
		v := q[0]
		q = q[1:]
		if v > high {
			break
		}
		if v >= low {
			ans = append(ans, v)
		}
		x := v % 10
		if x > 0 {
			q = append(q, v*10+x-1)
		}
		if x < 9 {
			q = append(q, v*10+x+1)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countSteppingNumbers(low: number, high: number): number[] {
    const ans: number[] = [];
    if (low === 0) {
        ans.push(0);
    }
    const q: number[] = [];
    for (let i = 1; i < 10; ++i) {
        q.push(i);
    }
    while (q.length) {
        const v = q.shift()!;
        if (v > high) {
            break;
        }
        if (v >= low) {
            ans.push(v);
        }
        const x = v % 10;
        if (x > 0) {
            q.push(v * 10 + x - 1);
        }
        if (x < 9) {
            q.push(v * 10 + x + 1);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
