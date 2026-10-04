---
comments: true
difficulty: Medium
rating: 1705
source: Weekly Contest 344 Q3
tags:
    - Array
---

<!-- problem:start -->

# [2672. Number of Adjacent Elements With the Same Color](https://leetcode.com/problems/number-of-adjacent-elements-with-the-same-color)

[Tài liệu tiếng Trung](/solution/2600-2699/2672.Number%20of%20Adjacent%20Elements%20With%20the%20Same%20Color/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu diễn một mảng <code>colors</code> có độ dài <code>n</code>, trong đó tất cả phần tử đều được đặt thành 0, nghĩa là <strong>chưa được tô màu</strong>. Bạn cũng được cho một mảng số nguyên 2D <code>queries</code>, trong đó <code>queries[i] = [index<sub>i</sub>, color<sub>i</sub>]</code>. Với <strong>truy vấn</strong> thứ <code>i<sup>th</sup></code>:</p>

<ul>
	<li>Đặt <code>colors[index<sub>i</sub>]</code> thành <code>color<sub>i</sub></code>.</li>
	<li>Đếm số cặp phần tử kề nhau trong <code>colors</code> có cùng màu (không phụ thuộc vào <code>color<sub>i</sub></code>).</li>
</ul>

<p>Trả về một mảng <code>answer</code> có cùng độ dài với <code>queries</code>, trong đó <code>answer[i]</code> là đáp án của truy vấn thứ <code>i<sup>th</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, queries = [[0,2],[1,2],[3,1],[1,1],[2,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1,1,0,2]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ban đầu, mảng colors = [0,0,0,0], trong đó 0 biểu thị các phần tử chưa được tô màu.</li>
	<li>Sau truy vấn thứ <sup>1</sup>, colors = [2,0,0,0]. Số cặp phần tử kề nhau có cùng màu là 0.</li>
	<li>Sau truy vấn thứ <sup>2</sup>, colors = [2,2,0,0]. Số cặp phần tử kề nhau có cùng màu là 1.</li>
	<li>Sau truy vấn thứ <sup>3</sup>, colors = [2,2,0,1]. Số cặp phần tử kề nhau có cùng màu là 1.</li>
	<li>Sau truy vấn thứ <sup>4</sup>, colors = [2,1,0,1]. Số cặp phần tử kề nhau có cùng màu là 0.</li>
	<li>Sau truy vấn thứ <sup>5</sup>, colors = [2,1,1,1]. Số cặp phần tử kề nhau có cùng màu là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 1, queries = [[0,100000]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau truy vấn thứ <sup>1</sup>, colors = [100000]. Số cặp phần tử kề nhau có cùng màu là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length&nbsp;== 2</code></li>
	<li><code>0 &lt;= index<sub>i</sub>&nbsp;&lt;= n - 1</code></li>
	<li><code>1 &lt;=&nbsp; color<sub>i</sub>&nbsp;&lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi truy vấn tô lại màu một chỉ số và cần trả về số cặp phần tử kề nhau có cùng màu. Nếu quét lại mảng, độ phức tạp sẽ là $O(nq)$ với $n,q \le 10^5$. Chỉ các phần tử kề với chỉ số được thay đổi mới có thể làm thay đổi kết quả.
>
> Trừ các cặp đang có cùng màu cũ, gán màu mới, rồi cộng các cặp được tạo bởi màu mới. Các số 0 biểu thị phần tử chưa được tô màu nên không được tính. Biến toàn cục $x$ lưu tổng số cặp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def colorTheArray(self, n: int, queries: List[List[int]]) -> List[int]:
        nums = [0] * n
        ans = [0] * len(queries)
        x = 0
        for k, (i, c) in enumerate(queries):
            if i > 0 and nums[i] and nums[i - 1] == nums[i]:
                x -= 1
            if i < n - 1 and nums[i] and nums[i + 1] == nums[i]:
                x -= 1
            if i > 0 and nums[i - 1] == c:
                x += 1
            if i < n - 1 and nums[i + 1] == c:
                x += 1
            ans[k] = x
            nums[i] = c
        return ans
```

#### Java

```java
class Solution {
    public int[] colorTheArray(int n, int[][] queries) {
        int m = queries.length;
        int[] nums = new int[n];
        int[] ans = new int[m];
        for (int k = 0, x = 0; k < m; ++k) {
            int i = queries[k][0], c = queries[k][1];
            if (i > 0 && nums[i] > 0 && nums[i - 1] == nums[i]) {
                --x;
            }
            if (i < n - 1 && nums[i] > 0 && nums[i + 1] == nums[i]) {
                --x;
            }
            if (i > 0 && nums[i - 1] == c) {
                ++x;
            }
            if (i < n - 1 && nums[i + 1] == c) {
                ++x;
            }
            ans[k] = x;
            nums[i] = c;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> colorTheArray(int n, vector<vector<int>>& queries) {
        vector<int> nums(n);
        vector<int> ans;
        int x = 0;
        for (auto& q : queries) {
            int i = q[0], c = q[1];
            if (i > 0 && nums[i] > 0 && nums[i - 1] == nums[i]) {
                --x;
            }
            if (i < n - 1 && nums[i] > 0 && nums[i + 1] == nums[i]) {
                --x;
            }
            if (i > 0 && nums[i - 1] == c) {
                ++x;
            }
            if (i < n - 1 && nums[i + 1] == c) {
                ++x;
            }
            ans.push_back(x);
            nums[i] = c;
        }
        return ans;
    }
};
```

#### Go

```go
func colorTheArray(n int, queries [][]int) (ans []int) {
	nums := make([]int, n)
	x := 0
	for _, q := range queries {
		i, c := q[0], q[1]
		if i > 0 && nums[i] > 0 && nums[i-1] == nums[i] {
			x--
		}
		if i < n-1 && nums[i] > 0 && nums[i+1] == nums[i] {
			x--
		}
		if i > 0 && nums[i-1] == c {
			x++
		}
		if i < n-1 && nums[i+1] == c {
			x++
		}
		ans = append(ans, x)
		nums[i] = c
	}
	return
}
```

#### TypeScript

```ts
function colorTheArray(n: number, queries: number[][]): number[] {
    const nums: number[] = new Array(n).fill(0);
    const ans: number[] = [];
    let x = 0;
    for (const [i, c] of queries) {
        if (i > 0 && nums[i] > 0 && nums[i - 1] == nums[i]) {
            --x;
        }
        if (i < n - 1 && nums[i] > 0 && nums[i + 1] == nums[i]) {
            --x;
        }
        if (i > 0 && nums[i - 1] == c) {
            ++x;
        }
        if (i < n - 1 && nums[i + 1] == c) {
            ++x;
        }
        ans.push(x);
        nums[i] = c;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
