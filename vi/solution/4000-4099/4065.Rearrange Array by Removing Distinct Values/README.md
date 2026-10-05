---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [4065. Rearrange Array by Removing Distinct Values](https://leetcode.com/problems/rearrange-array-by-removing-distinct-values)

[Tài liệu tiếng Trung](/solution/4000-4099/4065.Rearrange%20Array%20by%20Removing%20Distinct%20Values/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Ban đầu, bạn có một mảng <strong>rỗng</strong> <code>ans</code>. Lặp lại thao tác sau cho đến khi <code>nums</code> <strong>rỗng</strong>:</p>

<ul>
	<li>Xác định <strong>tất cả</strong> các giá trị <strong>phân biệt</strong> hiện có trong <code>nums</code>.</li>
	<li>Xóa <strong>một</strong> lần xuất hiện của mỗi giá trị <strong>phân biệt</strong> hiện có trong <code>nums</code>, rồi thêm các giá trị đó vào <code>ans</code> theo thứ tự <strong>tăng dần</strong>.</li>
</ul>

<p>Trả về mảng <code>ans</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,3,2,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,2,3,1,3,3]</span></p>

<p><strong>Giải thích:</strong></p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<thead>
		<tr>
			<th scope="col" style="text-align:center;">Thao tác</th>
			<th scope="col" style="text-align:center;">Thêm vào <code>ans</code></th>
			<th scope="col" style="text-align:center;"><code>nums</code> sau đó</th>
			<th scope="col" style="text-align:center;"><code>ans</code> sau đó</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="text-align:center;">1</td>
			<td style="text-align:center;">1, 2, 3</td>
			<td style="text-align:center;"><code>[3, 1, 3]</code></td>
			<td style="text-align:center;"><code>[1, 2, 3]</code></td>
		</tr>
		<tr>
			<td style="text-align:center;">2</td>
			<td style="text-align:center;">1, 3</td>
			<td style="text-align:center;"><code>[3]</code></td>
			<td style="text-align:center;"><code>[1, 2, 3, 1, 3]</code></td>
		</tr>
		<tr>
			<td style="text-align:center;">3</td>
			<td style="text-align:center;">3</td>
			<td style="text-align:center;"><code>[]</code></td>
			<td style="text-align:center;"><code>[1, 2, 3, 1, 3, 3]</code></td>
		</tr>
	</tbody>
</table>

<p><code>nums</code> hiện đã rỗng, nên đáp án là <code>[1, 2, 3, 1, 3, 3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,7,4,4,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[4,7,4,7,4]</span></p>

<p><strong>Giải thích:</strong></p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse;">
	<thead>
		<tr>
			<th scope="col" style="text-align:center;">Thao tác</th>
			<th scope="col" style="text-align:center;">Thêm vào <code>ans</code></th>
			<th scope="col" style="text-align:center;"><code>nums</code> sau đó</th>
			<th scope="col" style="text-align:center;"><code>ans</code> sau đó</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="text-align:center;">1</td>
			<td style="text-align:center;">4, 7</td>
			<td style="text-align:center;"><code>[7, 4, 4]</code></td>
			<td style="text-align:center;"><code>[4, 7]</code></td>
		</tr>
		<tr>
			<td style="text-align:center;">2</td>
			<td style="text-align:center;">4, 7</td>
			<td style="text-align:center;"><code>[4]</code></td>
			<td style="text-align:center;"><code>[4, 7, 4, 7]</code></td>
		</tr>
		<tr>
			<td style="text-align:center;">3</td>
			<td style="text-align:center;">4</td>
			<td style="text-align:center;"><code>[]</code></td>
			<td style="text-align:center;"><code>[4, 7, 4, 7, 4]</code></td>
		</tr>
	</tbody>
</table>

<p><code>nums</code> hiện đã rỗng, nên đáp án là <code>[4, 7, 4, 7, 4]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n$ và mọi giá trị đều không vượt quá $100$, việc lấy một bản sao của mỗi giá trị còn lại trong từng lượt là phù hợp với giới hạn. Mỗi lượt cần thu thập các giá trị vẫn còn và xóa mỗi giá trị đúng một lần theo thứ tự tăng dần. Việc tìm kiếm và xóa trong mảng ban đầu khiến các chỉ số liên tục thay đổi.
>
> Một giá trị được đưa vào kết quả một lần cho mỗi lần nó xuất hiện, còn thứ tự trong mỗi lượt chỉ phụ thuộc vào giá trị chứ không phụ thuộc vào chỉ số ban đầu.
>
> Hãy đếm theo giá trị. Miền giá trị là $[1,m]$. Duyệt từ nhỏ đến lớn, khi số lần xuất hiện vẫn dương thì thêm giá trị đó vào kết quả và giảm số lần xuất hiện. Lặp lại cho đến khi đáp án có độ dài $n$. Mỗi lần duyệt tương ứng với một thao tác.

<!-- thinking:end -->

Gọi $m=\max(\textit{nums})$ và gọi $\textit{cnt}[x]$ là số lần $x$ xuất hiện. Lượt $k$, bắt đầu từ $0$, thêm mọi giá trị vẫn còn theo thứ tự tăng dần. Đó chính xác là các giá trị có tần suất ban đầu lớn hơn $k$.

Lưu tần suất trong một mảng có độ dài $m+1$. Khi đáp án chưa có đủ $n$ phần tử, duyệt $x$ từ $1$ đến $m$. Nếu $\textit{cnt}[x]>0$, thêm $x$ vào đáp án và giảm số lần xuất hiện. Mỗi lần duyệt tương ứng với một thao tác, và các giá trị được thêm đã được sắp xếp.

Độ phức tạp thời gian là $O(nm)$ và độ phức tạp không gian là $O(m)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rearrangeArray(self, nums: list[int]) -> list[int]:
        mx = max(nums)
        cnt = [0] * (mx + 1)
        for x in nums:
            cnt[x] += 1

        ans = []
        while len(ans) < len(nums):
            for x in range(1, mx + 1):
                if cnt[x]:
                    ans.append(x)
                    cnt[x] -= 1
        return ans
```

#### Java

```java
class Solution {
    public int[] rearrangeArray(int[] nums) {
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }
        int[] cnt = new int[mx + 1];
        for (int x : nums) {
            cnt[x]++;
        }

        int[] ans = new int[nums.length];
        int idx = 0;
        while (idx < nums.length) {
            for (int x = 1; x <= mx; x++) {
                if (cnt[x] > 0) {
                    ans[idx++] = x;
                    cnt[x]--;
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
    vector<int> rearrangeArray(vector<int>& nums) {
        int mx = ranges::max(nums);
        vector<int> cnt(mx + 1);
        for (int x : nums) {
            cnt[x]++;
        }

        vector<int> ans;
        while (ans.size() < nums.size()) {
            for (int x = 1; x <= mx; x++) {
                if (cnt[x]) {
                    ans.push_back(x);
                    cnt[x]--;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func rearrangeArray(nums []int) []int {
	mx := slices.Max(nums)

	cnt := make([]int, mx+1)
	for _, x := range nums {
		cnt[x]++
	}

	ans := make([]int, 0, len(nums))
	for len(ans) < len(nums) {
		for x := 1; x <= mx; x++ {
			if cnt[x] > 0 {
				ans = append(ans, x)
				cnt[x]--
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function rearrangeArray(nums: number[]): number[] {
    const mx = Math.max(...nums);
    const cnt = new Array(mx + 1).fill(0);

    for (const x of nums) {
        cnt[x]++;
    }

    const ans: number[] = [];
    while (ans.length < nums.length) {
        for (let x = 1; x <= mx; x++) {
            if (cnt[x]) {
                ans.push(x);
                cnt[x]--;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
