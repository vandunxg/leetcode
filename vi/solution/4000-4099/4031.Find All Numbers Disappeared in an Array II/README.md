---
comments: true
difficulty: Medium
rating: 1416
source: Weekly Contest 516 Q2
---

<!-- problem:start -->

# [4031. Find All Numbers Disappeared in an Array II](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array-ii)

[中文文档](/solution/4000-4099/4031.Find%20All%20Numbers%20Disappeared%20in%20an%20Array%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và hai số nguyên <code>lower</code> và <code>upper</code>.</p>

<p>Một <strong>số nguyên bị thiếu</strong> là một số nguyên thuộc đoạn đóng <code>[lower, upper]</code> nhưng không xuất hiện trong <code>nums</code>.</p>

<p>Hãy trả về một mảng số nguyên 2 chiều, trong đó mỗi phần tử có dạng <code>[start, end]</code>, biểu diễn một đoạn <strong>liên tiếp</strong> gồm các số nguyên bị thiếu. Trả về các đoạn theo thứ tự <strong>tăng dần</strong>. Nếu không có số nguyên nào bị thiếu, hãy trả về một mảng rỗng.</p>

<p><strong>Lưu ý:</strong> Các số nguyên bị thiếu liên tiếp phải được gộp thành một đoạn duy nhất.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,9,7], lower = 1, upper = 12</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[1,2],[4,6],[8,8],[10,12]]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các số nguyên bị thiếu là <code>[1, 2, 4, 5, 6, 8, 10, 11, 12]</code>.</li>
	<li>Gộp các số nguyên bị thiếu thành số lượng đoạn liên tiếp ít nhất, ta được <code>[1, 2]</code>, <code>[4, 6]</code>, <code>[8, 8]</code> và <code>[10, 12]</code>.</li>
	<li>Do đó, đáp án là <code>[[1, 2], [4, 6], [8, 8], [10, 12]]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1], lower = 5, upper = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[5,7]]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các số nguyên bị thiếu là <code>[5, 6, 7]</code>.</li>
	<li>Gộp các số nguyên bị thiếu thành số lượng đoạn liên tiếp ít nhất, ta được <code>[5, 7]</code>.</li>
	<li>Do đó, đáp án là <code>[[5, 7]]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,5], lower = 2, upper = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Không có số nguyên nào bị thiếu.</li>
	<li>Do đó, đáp án là <code>[]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= lower &lt;= upper &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Các phần tử còn thiếu chính là những khoảng liên tiếp trong $[\textit{lower},\textit{upper}]$ không xuất hiện. Kiểm tra từng số nguyên trong đoạn này sẽ khiến độ phức tạp phụ thuộc vào miền giá trị thay vì $n$.
>
> Sau khi sắp xếp và loại bỏ các giá trị trùng lặp, mỗi khoảng giữa hai số liên tiếp nằm trong đoạn là một khoảng bị thiếu; hai khoảng ở hai đầu tương ứng với $\textit{lower}$ và $\textit{upper}$ cũng được xử lý theo cách tương tự.
>
> Duyệt với $\textit{prev}=\textit{lower}-1$, ta thêm $[\textit{prev}+1,x-1]$ bất cứ khi nào $x-\textit{prev}>1$.

<!-- thinking:end -->

Ta sắp xếp $\textit{nums}$ rồi duyệt qua mảng. Gọi $\textit{prev}$ là số trước đó xuất hiện trong $[\textit{lower}, \textit{upper}]$, ban đầu bằng $\textit{lower} - 1$.

Duyệt qua mảng đã sắp xếp và bỏ qua các giá trị nằm ngoài $[\textit{lower}, \textit{upper}]$. Nếu có khoảng trống giữa số hiện tại $x$ và $\textit{prev}$, tức là $x - \textit{prev} > 1$, thêm đoạn bị thiếu $[\textit{prev} + 1, x - 1]$ vào đáp án, sau đó đặt $\textit{prev}$ bằng $x$.

Sau khi duyệt xong, nếu $\textit{prev} < \textit{upper}$, thêm đoạn còn lại $[\textit{prev} + 1, \textit{upper}]$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findDisappearedNumbers(
        self, nums: List[int], lower: int, upper: int
    ) -> List[List[int]]:
        ans = []
        prev = lower - 1
        for x in sorted(set(nums)):
            if x < lower:
                continue
            if x > upper:
                break
            if x - prev > 1:
                ans.append([prev + 1, x - 1])
            prev = x
        if prev < upper:
            ans.append([prev + 1, upper])
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> findDisappearedNumbers(int[] nums, int lower, int upper) {
        Arrays.sort(nums);
        List<List<Integer>> ans = new ArrayList<>();
        int prev = lower - 1;
        for (int x : nums) {
            if (x < lower || x > upper) {
                continue;
            }
            if (x - prev > 1) {
                ans.add(List.of(prev + 1, x - 1));
            }
            prev = x;
        }
        if (prev < upper) {
            ans.add(List.of(prev + 1, upper));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> findDisappearedNumbers(vector<int>& nums, int lower, int upper) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> ans;
        int prev = lower - 1;
        for (int x : nums) {
            if (x < lower || x > upper) {
                continue;
            }
            if (x - prev > 1) {
                ans.push_back({prev + 1, x - 1});
            }
            prev = x;
        }
        if (prev < upper) {
            ans.push_back({prev + 1, upper});
        }
        return ans;
    }
};
```

#### Go

```go
func findDisappearedNumbers(nums []int, lower int, upper int) (ans [][]int) {
	sort.Ints(nums)
	prev := lower - 1
	for _, x := range nums {
		if x < lower || x > upper {
			continue
		}
		if x-prev > 1 {
			ans = append(ans, []int{prev + 1, x - 1})
		}
		prev = x
	}
	if prev < upper {
		ans = append(ans, []int{prev + 1, upper})
	}
	return
}
```

#### TypeScript

```ts
function findDisappearedNumbers(nums: number[], lower: number, upper: number): number[][] {
    nums.sort((a, b) => a - b);
    const ans: number[][] = [];
    let prev = lower - 1;
    for (const x of nums) {
        if (x < lower || x > upper) {
            continue;
        }
        if (x - prev > 1) {
            ans.push([prev + 1, x - 1]);
        }
        prev = x;
    }
    if (prev < upper) {
        ans.push([prev + 1, upper]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
