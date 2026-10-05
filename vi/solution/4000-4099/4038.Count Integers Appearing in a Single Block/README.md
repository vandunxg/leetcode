---
comments: true
difficulty: Easy
rating: 1165
source: Weekly Contest 517 Q1
---

<!-- problem:start -->

# [4038. Count Integers Appearing in a Single Block](https://leetcode.com/problems/count-integers-appearing-in-a-single-block)

[中文文档](/solution/4000-4099/4038.Count%20Integers%20Appearing%20in%20a%20Single%20Block/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một số nguyên <code>x</code> là <strong>đặc biệt</strong> nếu mọi lần xuất hiện của <code>x</code> trong <code>nums</code> đều nằm trong cùng một <strong>block liên tiếp</strong>.</p>

<p>Hãy trả về số lượng số nguyên <strong>đặc biệt phân biệt</strong> trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>1 xuất hiện tại các chỉ số 0 và 3, tạo thành hai block riêng biệt, nên không đặc biệt.</li>
	<li>2 xuất hiện trong một block liên tiếp duy nhất tại các chỉ số <code>[1, 2]</code>, nên đặc biệt.</li>
</ul>

<p>Do đó, có một số nguyên đặc biệt.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,3,1,2,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>3 xuất hiện trong một block liên tiếp duy nhất tại các chỉ số <code>[0, 1]</code>, nên đặc biệt.</li>
	<li>1 xuất hiện tại các chỉ số 2 và 5, tạo thành hai block riêng biệt, nên không đặc biệt.</li>
	<li>2 xuất hiện trong một block liên tiếp duy nhất tại các chỉ số <code>[3, 4]</code>, nên đặc biệt.</li>
</ul>

<p>Do đó, có hai số nguyên đặc biệt.</p>
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

### Lời giải 1: Đếm các block của từng số nguyên

<!-- thinking:start -->

> **Tư duy**
>
> Một số nguyên đặc biệt là số tạo thành đúng một block liên tiếp gồm các phần tử bằng nhau. Việc thu thập mọi chỉ số của từng giá trị chỉ để kiểm tra tính liên tiếp sẽ lưu các danh sách dư thừa.
>
> Duyệt mảng và tăng một bộ đếm tại đầu mỗi block; sau đó đếm có bao nhiêu giá trị có bộ đếm bằng $1$.
>
> Miền giá trị có kích thước $100$, nên chỉ cần một bảng tần suất.

<!-- thinking:end -->

Gọi mỗi dãy cực đại gồm các phần tử bằng nhau liên tiếp là một **block**. Một số nguyên $x$ đặc biệt khi và chỉ khi nó tạo thành đúng một block.

Vì vậy, ta duyệt qua mảng, và mỗi khi $i = 0$ hoặc $\textit{nums}[i] \neq \textit{nums}[i - 1]$, vị trí $i$ bắt đầu một block mới, nên ta tăng $\textit{cnt}[\textit{nums}[i]]$. Sau khi duyệt xong, đáp án là số lượng các số nguyên có số lần xuất hiện trong $\textit{cnt}$ đúng bằng $1$.

Độ phức tạp thời gian là $O(n + M)$, còn độ phức tạp không gian là $O(M)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$ và $M = 100$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSpecialIntegers(self, nums: List[int]) -> int:
        cnt = Counter(x for i, x in enumerate(nums) if i == 0 or x != nums[i - 1])
        return sum(v == 1 for v in cnt.values())
```

#### Java

```java
class Solution {
    public int countSpecialIntegers(int[] nums) {
        int[] cnt = new int[101];
        for (int i = 0; i < nums.length; ++i) {
            if (i == 0 || nums[i] != nums[i - 1]) {
                ++cnt[nums[i]];
            }
        }
        int ans = 0;
        for (int c : cnt) {
            if (c == 1) {
                ++ans;
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
    int countSpecialIntegers(vector<int>& nums) {
        int cnt[101]{};
        for (int i = 0; i < nums.size(); ++i) {
            if (i == 0 || nums[i] != nums[i - 1]) {
                ++cnt[nums[i]];
            }
        }
        return count(begin(cnt), end(cnt), 1);
    }
};
```

#### Go

```go
func countSpecialIntegers(nums []int) int {
	cnt := [101]int{}
	for i, x := range nums {
		if i == 0 || x != nums[i-1] {
			cnt[x]++
		}
	}
	ans := 0
	for _, c := range cnt {
		if c == 1 {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countSpecialIntegers(nums: number[]): number {
    const cnt: number[] = Array(101).fill(0);
    for (let i = 0; i < nums.length; ++i) {
        if (i === 0 || nums[i] !== nums[i - 1]) {
            ++cnt[nums[i]];
        }
    }
    return cnt.filter(c => c === 1).length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
