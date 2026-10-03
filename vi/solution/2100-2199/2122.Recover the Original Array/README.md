---
comments: true
difficulty: Hard
rating: 2158
source: Weekly Contest 273 Q4
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [2122. Recover the Original Array](https://leetcode.com/problems/recover-the-original-array)

[中文文档](/solution/2100-2199/2122.Recover%20the%20Original%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Alice có một mảng <strong>0-indexed</strong> <code>arr</code> gồm <code>n</code> số nguyên <strong>dương</strong>. Cô chọn một số nguyên <strong>dương</strong> <code>k</code> tùy ý và tạo hai mảng số nguyên <strong>0-indexed</strong> mới là <code>lower</code> và <code>higher</code> theo cách sau:</p>

<ol>
	<li><code>lower[i] = arr[i] - k</code>, với mọi chỉ số <code>i</code> thỏa mãn <code>0 &lt;= i &lt; n</code></li>
	<li><code>higher[i] = arr[i] + k</code>, với mọi chỉ số <code>i</code> thỏa mãn <code>0 &lt;= i &lt; n</code></li>
</ol>

<p>Không may, Alice đã làm mất cả ba mảng. Tuy nhiên, cô vẫn nhớ các số nguyên xuất hiện trong hai mảng <code>lower</code> và <code>higher</code>, nhưng không nhớ mỗi số nguyên thuộc về mảng nào. Hãy giúp Alice khôi phục mảng ban đầu.</p>

<p>Cho một mảng <code>nums</code> gồm <code>2n</code> số nguyên, trong đó <strong>chính xác</strong> <code>n</code> số nguyên từng xuất hiện trong <code>lower</code> và các số còn lại từng xuất hiện trong <code>higher</code>. Hãy trả về <em>mảng <strong>ban đầu</strong></em> <code>arr</code>. Nếu đáp án không duy nhất, hãy trả về <em><strong>bất kỳ</strong> mảng hợp lệ nào</em>.</p>

<p><strong>Lưu ý:</strong> Các test được tạo sao cho tồn tại <strong>ít nhất một</strong> mảng hợp lệ <code>arr</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,10,6,4,8,12]
<strong>Đầu ra:</strong> [3,7,11]
<strong>Giải thích:</strong>
Nếu arr = [3,7,11] và k = 1, ta có lower = [2,6,10] và higher = [4,8,12].
Gộp lower và higher, ta được [2,6,10,4,8,12], là một hoán vị của nums.
Một khả năng hợp lệ khác là arr = [5,7,9] và k = 3. Khi đó, lower = [2,4,6] và higher = [8,10,12].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,3,3]
<strong>Đầu ra:</strong> [2,2]
<strong>Giải thích:</strong>
Nếu arr = [2,2] và k = 1, ta có lower = [1,1] và higher = [3,3].
Gộp lower và higher, ta được [1,1,3,3], đúng bằng nums.
Lưu ý rằng arr không thể là [1,3] vì trong trường hợp đó, cách duy nhất để tạo ra [1,1,3,3] là dùng k = 0.
Điều này không hợp lệ vì k phải là số dương.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,435]
<strong>Đầu ra:</strong> [220]
<strong>Giải thích:</strong>
Tổ hợp duy nhất có thể là arr = [220] và k = 215. Với hai giá trị này, ta có lower = [5] và higher = [435].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 * n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li>Các test được tạo sao cho tồn tại <strong>ít nhất một</strong> mảng hợp lệ <code>arr</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{nums}$ là multiset gồm mọi giá trị ban đầu cộng hoặc trừ $k$. $k$ chưa biết, nên thử tất cả cách ghép cặp sẽ tốn quá nhiều. Hai nửa lower và higher phải khớp nhau với cùng một $k$.
>
> Sau khi sắp xếp, giá trị nhỏ nhất là một $a_i-k$, nên mọi giá trị $k$ ứng viên đều có dạng $(\textit{nums}[i]-\textit{nums}[0])/2$ với hiệu dương và chẵn. Với mỗi $k$ như vậy, dùng hai con trỏ để tham lam ghép $x$ với $x+2k$ trong mảng đã sắp xếp.
>
> Cách ghép đầu tiên tạo đủ $n/2$ cặp sẽ cho ta mảng ban đầu bằng các giá trị trung bình của từng cặp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def recoverArray(self, nums: List[int]) -> List[int]:
        nums.sort()
        n = len(nums)
        for i in range(1, n):
            d = nums[i] - nums[0]
            if d == 0 or d % 2 == 1:
                continue
            vis = [False] * n
            vis[i] = True
            ans = [(nums[0] + nums[i]) >> 1]
            l, r = 1, i + 1
            while r < n:
                while l < n and vis[l]:
                    l += 1
                while r < n and nums[r] - nums[l] < d:
                    r += 1
                if r == n or nums[r] - nums[l] > d:
                    break
                vis[r] = True
                ans.append((nums[l] + nums[r]) >> 1)
                l, r = l + 1, r + 1
            if len(ans) == (n >> 1):
                return ans
        return []
```

#### Java

```java
class Solution {
    public int[] recoverArray(int[] nums) {
        Arrays.sort(nums);
        for (int i = 1, n = nums.length; i < n; ++i) {
            int d = nums[i] - nums[0];
            if (d == 0 || d % 2 == 1) {
                continue;
            }
            boolean[] vis = new boolean[n];
            vis[i] = true;
            List<Integer> t = new ArrayList<>();
            t.add((nums[0] + nums[i]) >> 1);
            for (int l = 1, r = i + 1; r < n; ++l, ++r) {
                while (l < n && vis[l]) {
                    ++l;
                }
                while (r < n && nums[r] - nums[l] < d) {
                    ++r;
                }
                if (r == n || nums[r] - nums[l] > d) {
                    break;
                }
                vis[r] = true;
                t.add((nums[l] + nums[r]) >> 1);
            }
            if (t.size() == (n >> 1)) {
                int[] ans = new int[t.size()];
                int idx = 0;
                for (int e : t) {
                    ans[idx++] = e;
                }
                return ans;
            }
        }
        return null;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> recoverArray(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        for (int i = 1, n = nums.size(); i < n; ++i) {
            int d = nums[i] - nums[0];
            if (d == 0 || d % 2 == 1) continue;
            vector<bool> vis(n);
            vis[i] = true;
            vector<int> ans;
            ans.push_back((nums[0] + nums[i]) >> 1);
            for (int l = 1, r = i + 1; r < n; ++l, ++r) {
                while (l < n && vis[l]) ++l;
                while (r < n && nums[r] - nums[l] < d) ++r;
                if (r == n || nums[r] - nums[l] > d) break;
                vis[r] = true;
                ans.push_back((nums[l] + nums[r]) >> 1);
            }
            if (ans.size() == (n >> 1)) return ans;
        }
        return {};
    }
};
```

#### Go

```go
func recoverArray(nums []int) []int {
	sort.Ints(nums)
	for i, n := 1, len(nums); i < n; i++ {
		d := nums[i] - nums[0]
		if d == 0 || d%2 == 1 {
			continue
		}
		vis := make([]bool, n)
		vis[i] = true
		ans := []int{(nums[0] + nums[i]) >> 1}
		for l, r := 1, i+1; r < n; l, r = l+1, r+1 {
			for l < n && vis[l] {
				l++
			}
			for r < n && nums[r]-nums[l] < d {
				r++
			}
			if r == n || nums[r]-nums[l] > d {
				break
			}
			vis[r] = true
			ans = append(ans, (nums[l]+nums[r])>>1)
		}
		if len(ans) == (n >> 1) {
			return ans
		}
	}
	return []int{}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
