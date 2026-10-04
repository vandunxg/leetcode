---
comments: true
difficulty: Hard
rating: 2019
source: Weekly Contest 329 Q4
tags:
    - Array
    - Hash Table
    - Dynamic Programming
    - Counting
---

<!-- problem:start -->

# [2547. Minimum Cost to Split an Array](https://leetcode.com/problems/minimum-cost-to-split-an-array)

[中文文档](/solution/2500-2599/2547.Minimum%20Cost%20to%20Split%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Hãy chia mảng thành một số lượng bất kỳ các mảng con không rỗng. <strong>Chi phí</strong> của phép chia là tổng <strong>giá trị quan trọng</strong> của mỗi mảng con trong phép chia.</p>

<p>Gọi <code>trimmed(subarray)</code> là phiên bản của mảng con sau khi loại bỏ tất cả các số chỉ xuất hiện đúng một lần.</p>

<ul>
	<li>Ví dụ, <code>trimmed([3,1,2,4,3,4]) = [3,4,3,4].</code></li>
</ul>

<p><strong>Giá trị quan trọng</strong> của một mảng con là <code>k + trimmed(subarray).length</code>.</p>

<ul>
	<li>Ví dụ, nếu một mảng con là <code>[1,2,3,3,3,4,4]</code>, thì <font face="monospace">trimmed(</font><code>[1,2,3,3,3,4,4]) = [3,3,3,4,4].</code> Giá trị quan trọng của mảng con này là <code>k + 5</code>.</li>
</ul>

<p>Hãy trả về <em>chi phí nhỏ nhất có thể của một phép chia</em> <code>nums</code>.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp <strong>không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,2,1,3,3], k = 2
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Ta chia nums thành hai mảng con: [1,2], [1,2,1,3,3].
Giá trị quan trọng của [1,2] là 2 + (0) = 2.
Giá trị quan trọng của [1,2,1,3,3] là 2 + (2 + 2) = 6.
Chi phí của phép chia là 2 + 6 = 8. Có thể chứng minh đây là chi phí nhỏ nhất trong tất cả các phép chia có thể.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,2,1], k = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Ta chia nums thành hai mảng con: [1,2], [1,2,1].
Giá trị quan trọng của [1,2] là 2 + (0) = 2.
Giá trị quan trọng của [1,2,1] là 2 + (2) = 4.
Chi phí của phép chia là 2 + 4 = 6. Có thể chứng minh đây là chi phí nhỏ nhất trong tất cả các phép chia có thể.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,2,1], k = 5
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Ta chia nums thành một mảng con duy nhất: [1,2,1,2,1].
Giá trị quan trọng của [1,2,1,2,1] là 5 + (3 + 2) = 10.
Chi phí của phép chia là 10. Có thể chứng minh đây là chi phí nhỏ nhất trong tất cả các phép chia có thể.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt; nums.length</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>
<style type="text/css">.spoilerbutton {display:block; border:dashed; padding: 0px 0px; margin:10px 0px; font-size:150%; font-weight: bold; color:#000000; background-color:cyan; outline:0; 
}
.spoiler {overflow:hidden;}
.spoiler > div {-webkit-transition: all 0s ease;-moz-transition: margin 0s ease;-o-transition: margin 0s ease;transition: margin 0s ease;}
.spoilerbutton[value="Show Message"] + .spoiler > div {margin-top:-500%;}
.spoilerbutton[value="Hide Message"] + .spoiler {padding:5px;}
</style>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi mảng con có chi phí bằng $k$ cộng với số lượng giá trị xuất hiện nhiều hơn một lần (độ dài trừ đi số lượng giá trị xuất hiện đúng một lần). Với $n\le 10^3$, việc liệt kê các phép chia là quá nặng, nhưng chỉ có $n$ vị trí có thể cắt.
>
> Gọi $\textit{dfs}(i)$ là chi phí nhỏ nhất để xử lý từ $i$ đến hết mảng. Ta lần lượt chọn điểm cuối $j$ của mảng con, theo dõi số lượng giá trị xuất hiện đúng một lần, rồi cộng $k+(j-i+1)-\textit{one}$ vào $\textit{dfs}(j+1)$. Phép ghi nhớ có $O(n)$ trạng thái và $O(n)$ chuyển trạng thái.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i)$, biểu diễn chi phí nhỏ nhất khi chia mảng bắt đầu từ chỉ số $i$. Do đó, đáp án là $dfs(0)$.

Quy trình tính hàm $dfs(i)$ như sau:

Nếu $i \ge n$, nghĩa là đã chia đến cuối mảng, khi đó trả về $0$.
Ngược lại, ta duyệt qua điểm cuối $j$ của mảng con. Trong quá trình này, ta dùng một mảng hoặc hash table cnt để đếm số lần mỗi số xuất hiện trong mảng con, đồng thời dùng biến one để đếm số lượng số xuất hiện đúng một lần trong mảng con. Vì vậy, giá trị quan trọng của mảng con là $k + j - i + 1 - one$, còn chi phí của phép chia là $k + j - i + 1 - one + dfs(j + 1)$. Ta duyệt qua mọi $j$ và chọn giá trị nhỏ nhất làm giá trị trả về của $dfs(i)$.
Trong quá trình này, ta có thể dùng tìm kiếm có ghi nhớ, tức là dùng một mảng $f$ để lưu giá trị trả về của hàm $dfs(i)$, tránh tính toán lặp lại.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, nums: List[int], k: int) -> int:
        @cache
        def dfs(i):
            if i >= n:
                return 0
            cnt = Counter()
            one = 0
            ans = inf
            for j in range(i, n):
                cnt[nums[j]] += 1
                if cnt[nums[j]] == 1:
                    one += 1
                elif cnt[nums[j]] == 2:
                    one -= 1
                ans = min(ans, k + j - i + 1 - one + dfs(j + 1))
            return ans

        n = len(nums)
        return dfs(0)
```

#### Java

```java
class Solution {
    private Integer[] f;
    private int[] nums;
    private int n, k;

    public int minCost(int[] nums, int k) {
        n = nums.length;
        this.k = k;
        this.nums = nums;
        f = new Integer[n];
        return dfs(0);
    }

    private int dfs(int i) {
        if (i >= n) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        int[] cnt = new int[n];
        int one = 0;
        int ans = 1 << 30;
        for (int j = i; j < n; ++j) {
            int x = ++cnt[nums[j]];
            if (x == 1) {
                ++one;
            } else if (x == 2) {
                --one;
            }
            ans = Math.min(ans, k + j - i + 1 - one + dfs(j + 1));
        }
        return f[i] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(vector<int>& nums, int k) {
        int n = nums.size();
        int f[n];
        memset(f, 0, sizeof f);
        function<int(int)> dfs = [&](int i) {
            if (i >= n) {
                return 0;
            }
            if (f[i]) {
                return f[i];
            }
            int cnt[n];
            memset(cnt, 0, sizeof cnt);
            int one = 0;
            int ans = 1 << 30;
            for (int j = i; j < n; ++j) {
                int x = ++cnt[nums[j]];
                if (x == 1) {
                    ++one;
                } else if (x == 2) {
                    --one;
                }
                ans = min(ans, k + j - i + 1 - one + dfs(j + 1));
            }
            return f[i] = ans;
        };
        return dfs(0);
    }
};
```

#### Go

```go
func minCost(nums []int, k int) int {
	n := len(nums)
	f := make([]int, n)
	var dfs func(int) int
	dfs = func(i int) int {
		if i >= n {
			return 0
		}
		if f[i] > 0 {
			return f[i]
		}
		ans, one := 1<<30, 0
		cnt := make([]int, n)
		for j := i; j < n; j++ {
			cnt[nums[j]]++
			x := cnt[nums[j]]
			if x == 1 {
				one++
			} else if x == 2 {
				one--
			}
			ans = min(ans, k+j-i+1-one+dfs(j+1))
		}
		f[i] = ans
		return ans
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function minCost(nums: number[], k: number): number {
    const n = nums.length;
    const f = new Array(n).fill(0);
    const dfs = (i: number) => {
        if (i >= n) {
            return 0;
        }
        if (f[i]) {
            return f[i];
        }
        const cnt = new Array(n).fill(0);
        let one = 0;
        let ans = 1 << 30;
        for (let j = i; j < n; ++j) {
            const x = ++cnt[nums[j]];
            if (x == 1) {
                ++one;
            } else if (x == 2) {
                --one;
            }
            ans = Math.min(ans, k + j - i + 1 - one + dfs(j + 1));
        }
        f[i] = ans;
        return f[i];
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
