---
comments: true
difficulty: Medium
rating: 1235
source: Weekly Contest 277 Q2
tags:
    - Array
    - Two Pointers
    - Simulation
---

<!-- problem:start -->

# [2149. Rearrange Array Elements by Sign](https://leetcode.com/problems/rearrange-array-elements-by-sign)

[中文文档](/solution/2100-2199/2149.Rearrange%20Array%20Elements%20by%20Sign/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được <strong>đánh chỉ số từ 0</strong>, có độ dài <strong>chẵn</strong> và chứa số lượng số nguyên dương và âm <strong>bằng nhau</strong>.</p>

<p>Hãy trả về mảng nums sao cho thỏa mãn các điều kiện sau:</p>

<ol>
	<li>Mọi <strong>cặp số nguyên liên tiếp</strong> đều có <strong>dấu trái ngược nhau</strong>.</li>
	<li>Với các số nguyên cùng dấu, <strong>thứ tự</strong> xuất hiện trong <code>nums</code> được <strong>giữ nguyên</strong>.</li>
	<li>Mảng sau khi sắp xếp bắt đầu bằng một số nguyên dương.</li>
</ol>

<p>Hãy trả về <em>mảng sau khi sắp xếp lại các phần tử để thỏa mãn các điều kiện nêu trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,-2,-5,2,-4]
<strong>Đầu ra:</strong> [3,-2,1,-5,2,-4]
<strong>Giải thích:</strong>
Các số nguyên dương trong nums là [3,1,2]. Các số nguyên âm là [-2,-5,-4].
Cách duy nhất để sắp xếp lại chúng sao cho thỏa mãn tất cả điều kiện là [3,-2,1,-5,2,-4].
Các cách khác như [1,-2,2,-5,3,-4], [3,1,2,-2,-5,-4], [-2,3,-5,1,-4,2] đều không đúng vì không thỏa mãn một hoặc nhiều điều kiện.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,1]
<strong>Đầu ra:</strong> [1,-1]
<strong>Giải thích:</strong>
1 là số nguyên dương duy nhất và -1 là số nguyên âm duy nhất trong nums.
Vì vậy, nums được sắp xếp lại thành [1,-1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>nums.length</code> là <strong>số chẵn</strong></li>
	<li><code>1 &lt;= |nums[i]| &lt;= 10<sup>5</sup></code></li>
	<li><code>nums</code> chứa số lượng số nguyên dương và âm <strong>bằng nhau</strong>.</li>
</ul>

<p>&nbsp;</p>
Không bắt buộc phải thực hiện các thay đổi tại chỗ.

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Số lượng số dương và số âm bằng nhau; chúng phải xen kẽ và giữ nguyên thứ tự tương đối. Ta có thể tách chúng thành hai mảng rồi trộn lại, hoặc ghi trực tiếp vào các chỉ số đích.
>
> Các chỉ số chẵn nhận số dương, còn các chỉ số lẻ nhận số âm; lần lượt dùng các con trỏ $i$ và $j$ để duyệt mảng ban đầu.
>
> Một lượt duyệt là đủ để điền mảng mới.

<!-- thinking:end -->

Trước tiên, ta tạo một mảng $\textit{ans}$ có độ dài $n$. Sau đó, ta dùng hai con trỏ $i$ và $j$ lần lượt trỏ tới các chỉ số chẵn và lẻ của $\textit{ans}$, với giá trị ban đầu là $i = 0$, $j = 1$.

Ta duyệt qua mảng $\textit{nums}$. Nếu phần tử hiện tại $x$ là một số nguyên dương, ta đặt $x$ vào $\textit{ans}[i]$ và tăng $i$ thêm $2$; ngược lại, ta đặt $x$ vào $\textit{ans}[j]$ và tăng $j$ thêm $2$.

Cuối cùng, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def rearrangeArray(self, nums: List[int]) -> List[int]:
        ans = [0] * len(nums)
        i, j = 0, 1
        for x in nums:
            if x > 0:
                ans[i] = x
                i += 2
            else:
                ans[j] = x
                j += 2
        return ans
```

#### Java

```java
class Solution {
    public int[] rearrangeArray(int[] nums) {
        int[] ans = new int[nums.length];
        int i = 0, j = 1;
        for (int x : nums) {
            if (x > 0) {
                ans[i] = x;
                i += 2;
            } else {
                ans[j] = x;
                j += 2;
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
        vector<int> ans(nums.size());
        int i = 0, j = 1;
        for (int x : nums) {
            if (x > 0) {
                ans[i] = x;
                i += 2;
            } else {
                ans[j] = x;
                j += 2;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func rearrangeArray(nums []int) []int {
	ans := make([]int, len(nums))
	i, j := 0, 1
	for _, x := range nums {
		if x > 0 {
			ans[i] = x
			i += 2
		} else {
			ans[j] = x
			j += 2
		}
	}
	return ans
}
```

#### TypeScript

```ts
function rearrangeArray(nums: number[]): number[] {
    const ans: number[] = Array(nums.length);
    let [i, j] = [0, 1];
    for (const x of nums) {
        if (x > 0) {
            ans[i] = x;
            i += 2;
        } else {
            ans[j] = x;
            j += 2;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
