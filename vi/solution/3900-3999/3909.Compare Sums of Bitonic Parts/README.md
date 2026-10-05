---
comments: true
difficulty: Medium
rating: 1408
source: Biweekly Contest 181 Q2
---

<!-- problem:start -->

# [3909. Compare Sums of Bitonic Parts](https://leetcode.com/problems/compare-sums-of-bitonic-parts)

[中文文档](/solution/3900-3999/3909.Compare%20Sums%20of%20Bitonic%20Parts/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>bitonic</strong> <code>nums</code> có độ dài <code>n</code>.</p>

<p>Hãy chia mảng thành <strong>hai</strong> phần:</p>

<ul>
	<li><strong>Phần tăng dần</strong>: từ chỉ số 0 đến phần tử đỉnh (bao gồm cả đỉnh).</li>
	<li><strong>Phần giảm dần</strong>: từ phần tử đỉnh đến chỉ số <code>n - 1</code> (bao gồm cả đỉnh).</li>
</ul>

<p>Phần tử đỉnh thuộc cả hai phần.</p>

<p>Trả về:</p>

<ul>
	<li>0 nếu tổng của phần <strong>tăng dần</strong> lớn hơn.</li>
	<li>1 nếu tổng của phần <strong>giảm dần</strong> lớn hơn.</li>
	<li>-1 nếu hai tổng <strong>bằng nhau</strong>.</li>
</ul>

<p><strong>Ghi chú</strong>:</p>

<ul>
	<li>Một mảng <strong>bitonic</strong> là mảng tăng <strong>nghiêm ngặt</strong> đến một phần tử <strong>đỉnh</strong> duy nhất, sau đó giảm <strong>nghiêm ngặt</strong>.</li>
	<li>Một mảng được gọi là <strong>tăng nghiêm ngặt</strong> nếu mỗi phần tử <strong>lớn hơn nghiêm ngặt</strong> phần tử <strong>đứng trước</strong> nó (nếu có).</li>
	<li>Một mảng được gọi là <strong>giảm nghiêm ngặt</strong> nếu mỗi phần tử <strong>nhỏ hơn nghiêm ngặt</strong> phần tử <strong>đứng trước</strong> nó (nếu có).</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Phần tử đỉnh là <code>nums[1] = 3</code></li>
	<li>Phần tăng dần = <code>[1, 3]</code>, tổng là <code>1 + 3 = 4</code></li>
	<li>Phần giảm dần = <code>[3, 2, 1]</code>, tổng là <code>3 + 2 + 1 = 6</code></li>
	<li>Vì phần giảm dần có tổng lớn hơn, trả về 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4,5,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Phần tử đỉnh là <code>nums[2] = 5</code></li>
	<li>Phần tăng dần = <code>[2, 4, 5]</code>, tổng là <code>2 + 4 + 5 = 11</code></li>
	<li>Phần giảm dần = <code>[5, 2]</code>, tổng là <code>5 + 2 = 7</code></li>
	<li>Vì phần tăng dần có tổng lớn hơn, trả về 0.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,4,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Phần tử đỉnh là <code>nums[2] = 4</code></li>
	<li>Phần tăng dần = <code>[1, 2, 4]</code>, tổng là <code>1 + 2 + 4 = 7</code></li>
	<li>Phần giảm dần = <code>[4, 3]</code>, tổng là <code>4 + 3 = 7</code></li>
	<li>Vì tổng của hai phần bằng nhau, trả về -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>nums</code> là một mảng bitonic.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mảng đã là bitonic, nên đỉnh là duy nhất và chúng ta không cần kiểm tra lại dạng tăng/giảm. Việc tìm đỉnh rồi tính riêng tổng hai phía cần hai lần duyệt; ta có thể tách hai tổng cùng lúc trong một lần duyệt.
>
> Gọi $\textit{l}$ là tổng của phần tăng dần và khởi tạo $\textit{r}$ bằng tổng toàn bộ mảng. Khi các cặp phần tử kề nhau vẫn còn tăng, ta cộng giá trị mới vào $\textit{l}$ và loại giá trị trước đó khỏi $\textit{r}$ (đỉnh vẫn nằm ở bên phải). Dừng tại lần giảm đầu tiên.
>
> So sánh $\textit{l}$ và $\textit{r}$ sẽ cho biết phía nào lớn hơn.

<!-- thinking:end -->

Ta sử dụng hai biến $\textit{l}$ và $\textit{r}$ để lần lượt lưu tổng của phần tăng dần và phần giảm dần. Ban đầu, $\textit{l}$ được đặt bằng phần tử đầu tiên của mảng, còn $\textit{r}$ được đặt bằng tổng của tất cả phần tử trong mảng.

Ta duyệt từ phần tử thứ hai của mảng cho đến khi tìm thấy phần tử đỉnh. Trong quá trình duyệt, ta cộng phần tử hiện tại vào $\textit{l}$ và trừ phần tử trước đó khỏi $\textit{r}$.

Cuối cùng, ta so sánh giá trị của $\textit{l}$ và $\textit{r}$ rồi trả về kết quả tương ứng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def compareBitonicSums(self, nums: list[int]) -> int:
        l, r = nums[0], sum(nums)
        for a, b in pairwise(nums):
            if a > b:
                break
            l += b
            r -= a
        if l == r:
            return -1
        return 0 if l > r else 1
```

#### Java

```java
class Solution {
    public int compareBitonicSums(int[] nums) {
        long l = nums[0], r = 0;
        for (int x : nums) {
            r += x;
        }
        for (int i = 1; i < nums.length; ++i) {
            if (nums[i - 1] > nums[i]) {
                break;
            }
            l += nums[i];
            r -= nums[i - 1];
        }
        if (l == r) {
            return -1;
        }
        return l > r ? 0 : 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int compareBitonicSums(vector<int>& nums) {
        long long l = nums[0], r = 0;
        for (int x : nums) {
            r += x;
        }
        for (int i = 1; i < nums.size(); ++i) {
            if (nums[i - 1] > nums[i]) {
                break;
            }
            l += nums[i];
            r -= nums[i - 1];
        }
        if (l == r) {
            return -1;
        }
        return l > r ? 0 : 1;
    }
};
```

#### Go

```go
func compareBitonicSums(nums []int) int {
	var l, r int64
	l = int64(nums[0])
	r = 0
	for _, x := range nums {
		r += int64(x)
	}
	for i := 1; i < len(nums); i++ {
		if nums[i-1] > nums[i] {
			break
		}
		l += int64(nums[i])
		r -= int64(nums[i-1])
	}
	if l == r {
		return -1
	}
	if l > r {
		return 0
	}
	return 1
}
```

#### TypeScript

```ts
function compareBitonicSums(nums: number[]): number {
    let l: number = nums[0];
    let r: number = nums.reduce((acc, curr) => acc + curr, 0);

    for (let i = 1; i < nums.length; i++) {
        if (nums[i - 1] > nums[i]) {
            break;
        }
        l += nums[i];
        r -= nums[i - 1];
    }

    if (l === r) {
        return -1;
    }
    return l > r ? 0 : 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
