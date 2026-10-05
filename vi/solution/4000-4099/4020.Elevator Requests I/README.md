---
comments: true
difficulty: Easy
rating: 1187
source: Biweekly Contest 189 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [4020. Elevator Requests I](https://leetcode.com/problems/elevator-requests-i)

[Tài liệu tiếng Trung](/solution/4000-4099/4020.Elevator%20Requests%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị số tầng trong một tòa nhà, trong đó các tầng được đánh số từ 0 đến <code>n - 1</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>requests</code>, trong đó <code>requests</code> biểu thị chuỗi các yêu cầu về tầng.</p>

<p>Một thang máy bắt đầu ở tầng 0 và tuân theo các quy tắc sau:</p>

<ul>
	<li>Thang máy di chuyển một tầng mỗi giây.</li>
	<li>Thang máy phục vụ các yêu cầu theo thứ tự đã cho.</li>
	<li>Nếu thang máy đã ở tầng được yêu cầu thì không cần di chuyển.</li>
	<li>Sau khi phục vụ một yêu cầu, thang máy lập tức bắt đầu di chuyển về phía yêu cầu tiếp theo.</li>
</ul>

<p>Hãy trả về <strong>tổng thời gian</strong>, tính bằng giây, cần để phục vụ tất cả các yêu cầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, requests = [2,1,4,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>requests[0] = 2</code>: Di chuyển từ tầng 0 đến tầng 2 mất 2 giây.</li>
	<li><code>requests[1] = 1</code>: Di chuyển từ tầng 2 đến tầng 1 mất 1 giây.</li>
	<li><code>requests[2] = 4</code>: Di chuyển từ tầng 1 đến tầng 4 mất 3 giây.</li>
	<li><code>requests[3] = 3</code>: Di chuyển từ tầng 4 đến tầng 3 mất 1 giây.</li>
</ul>

<p>Tổng thời gian cần thiết là <code>2 + 1 + 3 + 1 = 7</code> giây.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, requests = [2,0,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>requests[0] = 2</code>: Di chuyển từ tầng 0 đến tầng 2 mất 2 giây.</li>
	<li><code>requests[1] = 0</code>: Di chuyển từ tầng 2 đến tầng 0 mất 2 giây.</li>
	<li><code>requests[2] = 0</code>: Không cần di chuyển.</li>
</ul>

<p>Tổng thời gian cần thiết là <code>2 + 2 + 0 = 4</code> giây.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= requests.length &lt;= 100</code></li>
	<li><code>0 &lt;= requests[i] &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Thứ tự các yêu cầu đã cố định, nên thang máy không có lựa chọn hoán vị.
>
> Thời gian di chuyển giữa hai yêu cầu liên tiếp là hiệu tuyệt đối giữa số tầng của chúng; chặng đầu tiên từ tầng $0$ chính là $\textit{requests}[0]$.
>
> Cộng các hiệu này sẽ cho tổng thời gian và chỉ cần duyệt tuyến tính.

<!-- thinking:end -->

Thang máy bắt đầu ở tầng $0$ và phục vụ các yêu cầu theo thứ tự đã cho. Thời gian di chuyển giữa hai yêu cầu liên tiếp là hiệu tuyệt đối giữa số tầng của chúng. Yêu cầu đầu tiên là di chuyển từ tầng $0$ đến $\textit{requests}[0]$, mất $\textit{requests}[0]$ giây. Sau đó, ta cộng các hiệu tuyệt đối của những yêu cầu liền kề.

Độ phức tạp thời gian là $O(m)$, còn độ phức tạp không gian là $O(1)$, trong đó $m$ là số lượng yêu cầu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def elevatorRequests(self, n: int, requests: list[int]) -> int:
        return requests[0] + sum(abs(x - y) for x, y in pairwise(requests))
```

#### Java

```java
class Solution {
    public int elevatorRequests(int n, int[] requests) {
        int ans = requests[0];
        for (int i = 1; i < requests.length; ++i) {
            ans += Math.abs(requests[i - 1] - requests[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int elevatorRequests(int n, vector<int>& requests) {
        int ans = requests[0];
        for (int i = 1; i < requests.size(); ++i) {
            ans += abs(requests[i - 1] - requests[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func elevatorRequests(n int, requests []int) int {
	ans := requests[0]
	for i, x := range requests[1:] {
		ans += abs(x - requests[i])
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function elevatorRequests(n: number, requests: number[]): number {
    let ans: number = requests[0];
    for (let i = 1; i < requests.length; ++i) {
        ans += Math.abs(requests[i] - requests[i - 1]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
