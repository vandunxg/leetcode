---
comments: true
difficulty: Medium
rating: 2023
source: Weekly Contest 337 Q3
tags:
    - Array
    - Hash Table
    - Math
    - Dynamic Programming
    - Backtracking
    - Combinatorics
    - Sorting
---

<!-- problem:start -->

# [2597. The Number of Beautiful Subsets](https://leetcode.com/problems/the-number-of-beautiful-subsets)

[中文文档](/solution/2500-2599/2597.The%20Number%20of%20Beautiful%20Subsets/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng các số nguyên dương <code>nums</code> và một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p>Một tập con của <code>nums</code> được gọi là <strong>đẹp</strong> nếu không chứa hai số nguyên có hiệu tuyệt đối bằng <code>k</code>.</p>

<p>Trả về <em>số lượng tập con <strong>đẹp không rỗng</strong> của mảng</em> <code>nums</code>.</p>

<p>Một <strong>tập con</strong> của <code>nums</code> là một mảng có thể thu được bằng cách xóa một số phần tử khỏi <code>nums</code> (có thể không xóa phần tử nào). Hai tập con khác nhau khi và chỉ khi các chỉ số được chọn để xóa khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,4,6], k = 2
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các tập con đẹp của mảng nums là: [2], [4], [6], [2, 6].
Có thể chứng minh rằng chỉ có 4 tập con đẹp trong mảng [2,4,6].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1], k = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Tập con đẹp của mảng nums là [1].
Có thể chứng minh rằng chỉ có 1 tập con đẹp trong mảng [1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 18</code></li>
	<li><code>1 &lt;= nums[i], k &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Quay lui

<!-- thinking:start -->

> **Tư duy**
>
> Một tập con đẹp khi không có hai giá trị nào chênh lệch nhau $k$; tập rỗng không được tính. Vì $n\le 20$, ta có thể liệt kê $2^n$ tập con.
>
> Sử dụng quay lui với hai lựa chọn lấy hoặc bỏ qua. Chỉ được lấy $x$ khi cả $x-k$ và $x+k$ đều chưa được chọn. Một bộ đếm lưu tần suất; đáp án bắt đầu từ $-1$ để loại tập rỗng.

<!-- thinking:end -->

Ta sử dụng một hash table hoặc mảng $\textit{cnt}$ để ghi nhận các số đang được chọn và số lần xuất hiện của chúng, đồng thời sử dụng $\textit{ans}$ để lưu số lượng tập con đẹp. Ban đầu, $\textit{ans} = -1$ để loại tập rỗng.

Với mỗi số $x$ trong mảng $\textit{nums}$, ta có hai lựa chọn:

- Không chọn $x$ và gọi đệ quy trực tiếp đến số tiếp theo;
- Chọn $x$ và kiểm tra xem $x + k$ và $x - k$ đã xuất hiện trong $\textit{cnt}$ hay chưa. Nếu cả hai đều chưa xuất hiện, ta có thể chọn $x$. Khi đó, tăng số lần xuất hiện của $x$ lên một, gọi đệ quy đến số tiếp theo, sau đó giảm số lần xuất hiện của $x$ đi một.

Cuối cùng, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(2^n)$, và độ phức tạp không gian là $O(n)$. Trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def beautifulSubsets(self, nums: List[int], k: int) -> int:
        def dfs(i: int) -> None:
            nonlocal ans
            if i >= len(nums):
                ans += 1
                return
            dfs(i + 1)
            if cnt[nums[i] + k] == 0 and cnt[nums[i] - k] == 0:
                cnt[nums[i]] += 1
                dfs(i + 1)
                cnt[nums[i]] -= 1

        ans = -1
        cnt = Counter()
        dfs(0)
        return ans
```

#### Java

```java
class Solution {
    private int[] nums;
    private int[] cnt = new int[1010];
    private int ans = -1;
    private int k;

    public int beautifulSubsets(int[] nums, int k) {
        this.k = k;
        this.nums = nums;
        dfs(0);
        return ans;
    }

    private void dfs(int i) {
        if (i >= nums.length) {
            ++ans;
            return;
        }
        dfs(i + 1);
        boolean ok1 = nums[i] + k >= cnt.length || cnt[nums[i] + k] == 0;
        boolean ok2 = nums[i] - k < 0 || cnt[nums[i] - k] == 0;
        if (ok1 && ok2) {
            ++cnt[nums[i]];
            dfs(i + 1);
            --cnt[nums[i]];
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int beautifulSubsets(vector<int>& nums, int k) {
        int ans = -1;
        int cnt[1010]{};
        int n = nums.size();

        auto dfs = [&](this auto&& dfs, int i) {
            if (i >= n) {
                ++ans;
                return;
            }
            dfs(i + 1);
            bool ok1 = nums[i] + k >= 1010 || cnt[nums[i] + k] == 0;
            bool ok2 = nums[i] - k < 0 || cnt[nums[i] - k] == 0;
            if (ok1 && ok2) {
                ++cnt[nums[i]];
                dfs(i + 1);
                --cnt[nums[i]];
            }
        };
        dfs(0);
        return ans;
    }
};
```

#### Go

```go
func beautifulSubsets(nums []int, k int) int {
	ans := -1
	n := len(nums)
	cnt := [1010]int{}
	var dfs func(int)
	dfs = func(i int) {
		if i >= n {
			ans++
			return
		}
		dfs(i + 1)
		ok1 := nums[i]+k >= len(cnt) || cnt[nums[i]+k] == 0
		ok2 := nums[i]-k < 0 || cnt[nums[i]-k] == 0
		if ok1 && ok2 {
			cnt[nums[i]]++
			dfs(i + 1)
			cnt[nums[i]]--
		}
	}
	dfs(0)
	return ans
}
```

#### TypeScript

```ts
function beautifulSubsets(nums: number[], k: number): number {
    let ans: number = -1;
    const cnt: number[] = new Array(1010).fill(0);
    const n: number = nums.length;
    const dfs = (i: number) => {
        if (i >= n) {
            ++ans;
            return;
        }
        dfs(i + 1);
        const ok1: boolean = nums[i] + k >= 1010 || cnt[nums[i] + k] === 0;
        const ok2: boolean = nums[i] - k < 0 || cnt[nums[i] - k] === 0;
        if (ok1 && ok2) {
            ++cnt[nums[i]];
            dfs(i + 1);
            --cnt[nums[i]];
        }
    };
    dfs(0);
    return ans;
}
```

#### C#

```cs
public class Solution {
    public int BeautifulSubsets(int[] nums, int k) {
        int ans = -1;
        int[] cnt = new int[1010];
        int n = nums.Length;

        void Dfs(int i) {
            if (i >= n) {
                ans++;
                return;
            }
            Dfs(i + 1);
            bool ok1 = nums[i] + k >= 1010 || cnt[nums[i] + k] == 0;
            bool ok2 = nums[i] - k < 0 || cnt[nums[i] - k] == 0;
            if (ok1 && ok2) {
                cnt[nums[i]]++;
                Dfs(i + 1);
                cnt[nums[i]]--;
            }
        }

        Dfs(0);
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
