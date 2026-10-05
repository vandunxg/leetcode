---
comments: true
difficulty: Medium
rating: 1381
source: Weekly Contest 486 Q2
---

<!-- problem:start -->

# [3819. Rotate Non Negative Elements](https://leetcode.com/problems/rotate-non-negative-elements)

[中文文档](/solution/3800-3899/3819.Rotate%20Non%20Negative%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Chỉ xoay các phần tử <strong>không âm</strong> của mảng sang <strong>bên trái</strong> <code>k</code> vị trí, theo cách tuần hoàn.</p>

<p>Tất cả các phần tử <strong>âm</strong> phải giữ nguyên vị trí ban đầu và không được di chuyển.</p>

<p>Sau khi xoay, đưa các phần tử <strong>không âm</strong> trở lại mảng theo thứ tự mới, chỉ điền vào các vị trí ban đầu chứa giá trị <strong>không âm</strong> và <strong>bỏ qua tất cả</strong> các vị trí âm.</p>

<p>Trả về mảng kết quả.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,-2,3,-4], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,-2,1,-4]</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Các phần tử không âm theo thứ tự là <code>[1, 3]</code>.</li>
	<li>Xoay sang trái với <code>k = 3</code> cho kết quả:
	<ul>
		<li><code>[1, 3] -&gt; [3, 1] -&gt; [1, 3] -&gt; [3, 1]</code></li>
	</ul>
	</li>
	<li>Đưa chúng trở lại các chỉ số không âm cho kết quả <code>[3, -2, 1, -4]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-3,-2,7], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-3,-2,7]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các phần tử không âm theo thứ tự là <code>[7]</code>.</li>
	<li>Xoay sang trái với <code>k = 1</code> cho kết quả <code>[7]</code>.</li>
	<li>Đưa chúng trở lại các chỉ số không âm cho kết quả <code>[-3, -2, 7]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,4,-9,6], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[6,5,-9,4]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các phần tử không âm theo thứ tự là <code>[5, 4, 6]</code>.</li>
	<li>Xoay sang trái với <code>k = 2</code> cho kết quả <code>[6, 5, 4]</code>.</li>
	<li>Đưa chúng trở lại các chỉ số không âm cho kết quả <code>[6, 5, -9, 4]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị không âm được xoay sang trái $k$ vị trí; các giá trị âm giữ nguyên. Vì $n \le 10^5$, việc đan xen các phần tử ngay trong mảng vừa dễ sai vừa không cần thiết.
>
> Các giá trị không âm tạo thành một dãy riêng; các giá trị âm chỉ đóng vai trò là phần tử giữ chỗ. Ta trích xuất dãy đó, xoay nó, rồi ghi trở lại các vị trí không âm ban đầu.
>
> Chỉ số mới là $((i-k)\bmod m+m)\bmod m$ với $m$ là số phần tử không âm.
>
> Quét lần thứ hai để chỉ điền vào các vị trí đó; các giá trị âm giữ nguyên vị trí ban đầu.

<!-- thinking:end -->

Trước tiên, chúng ta trích xuất tất cả phần tử không âm khỏi mảng và lưu chúng vào một mảng mới $t$.

Sau đó, chúng ta tạo một mảng $d$ có cùng kích thước với $t$ để lưu các phần tử không âm sau khi xoay. Với mỗi phần tử $t[i]$ trong $t$, chúng ta đặt nó vào vị trí $((i - k) \bmod m + m) \bmod m$ trong $d$, trong đó $m$ là số phần tử không âm.

Tiếp theo, chúng ta duyệt qua mảng ban đầu $\textit{nums}$. Với mỗi vị trí chứa một phần tử không âm, chúng ta thay thế nó bằng phần tử ở vị trí tương ứng trong $d$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(m)$, trong đó $m$ là số phần tử không âm.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rotateElements(self, nums: List[int], k: int) -> List[int]:
        t = [x for x in nums if x >= 0]
        m = len(t)
        d = [0] * m
        for i, x in enumerate(t):
            d[((i - k) % m + m) % m] = x
        j = 0
        for i, x in enumerate(nums):
            if x >= 0:
                nums[i] = d[j]
                j += 1
        return nums
```

#### Java

```java
class Solution {
    public int[] rotateElements(int[] nums, int k) {
        int m = 0;
        for (int x : nums) {
            if (x >= 0) {
                m++;
            }
        }
        int[] t = new int[m];
        int p = 0;
        for (int x : nums) {
            if (x >= 0) {
                t[p++] = x;
            }
        }
        int[] d = new int[m];
        for (int i = 0; i < m; i++) {
            d[((i - k) % m + m) % m] = t[i];
        }
        int j = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] >= 0) {
                nums[i] = d[j++];
            }
        }
        return nums;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> rotateElements(vector<int>& nums, int k) {
        vector<int> t;
        for (int x : nums) {
            if (x >= 0) {
                t.push_back(x);
            }
        }
        int m = t.size();
        vector<int> d(m);
        for (int i = 0; i < m; i++) {
            d[((i - k) % m + m) % m] = t[i];
        }
        int j = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] >= 0) {
                nums[i] = d[j++];
            }
        }
        return nums;
    }
};
```

#### Go

```go
func rotateElements(nums []int, k int) []int {
	t := make([]int, 0)
	for _, x := range nums {
		if x >= 0 {
			t = append(t, x)
		}
	}
	m := len(t)
	d := make([]int, m)
	for i, x := range t {
		d[((i-k)%m+m)%m] = x
	}
	j := 0
	for i, x := range nums {
		if x >= 0 {
			nums[i] = d[j]
			j++
		}
	}
	return nums
}
```

#### TypeScript

```ts
function rotateElements(nums: number[], k: number): number[] {
    const t: number[] = nums.filter(x => x >= 0);
    const m = t.length;
    const d = new Array<number>(m);
    for (let i = 0; i < m; i++) {
        d[(((i - k) % m) + m) % m] = t[i];
    }
    let j = 0;
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] >= 0) {
            nums[i] = d[j++];
        }
    }
    return nums;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
