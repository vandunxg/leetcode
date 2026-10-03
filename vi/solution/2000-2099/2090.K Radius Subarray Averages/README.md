---
comments: true
difficulty: Medium
rating: 1358
source: Weekly Contest 269 Q2
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [2090. K Radius Subarray Averages](https://leetcode.com/problems/k-radius-subarray-averages)

[中文文档](/solution/2000-2099/2090.K%20Radius%20Subarray%20Averages/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> gồm <code>n</code> phần tử, được <strong>đánh chỉ số từ 0</strong>, và một số nguyên <code>k</code>.</p>

<p><strong>Giá trị trung bình bán kính k</strong> của một mảng con trong <code>nums</code> <strong>có tâm</strong> tại một chỉ số <code>i</code> với <strong>bán kính</strong> <code>k</code> là giá trị trung bình của <strong>tất cả</strong> phần tử trong <code>nums</code> từ chỉ số <code>i - k</code> đến <code>i + k</code> (<strong>bao gồm cả hai đầu</strong>). Nếu có ít hơn <code>k</code> phần tử ở phía trước <strong>hoặc</strong> phía sau chỉ số <code>i</code>, thì <strong>giá trị trung bình bán kính k</strong> là <code>-1</code>.</p>

<p>Hãy xây dựng và trả về <em>một mảng </em><code>avgs</code><em> có độ dài </em><code>n</code><em>, trong đó </em><code>avgs[i]</code><em> là <strong>giá trị trung bình bán kính k</strong> của mảng con có tâm tại chỉ số </em><code>i</code>.</p>

<p><strong>Giá trị trung bình</strong> của <code>x</code> phần tử là tổng của <code>x</code> phần tử đó chia cho <code>x</code>, sử dụng <strong>phép chia nguyên</strong>. Phép chia nguyên làm tròn về phía 0, nghĩa là bỏ đi phần thập phân.</p>

<ul>
	<li>Ví dụ, giá trị trung bình của bốn phần tử <code>2</code>, <code>3</code>, <code>1</code> và <code>5</code> là <code>(2 + 3 + 1 + 5) / 4 = 11 / 4 = 2.75</code>, sau khi chia nguyên sẽ là <code>2</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2090.K%20Radius%20Subarray%20Averages/images/eg1.png" style="width: 343px; height: 119px;" />
<pre>
<strong>Đầu vào:</strong> nums = [7,4,3,9,1,8,5,2,6], k = 3
<strong>Đầu ra:</strong> [-1,-1,-1,5,4,4,-1,-1,-1]
<strong>Giải thích:</strong>
- avg[0], avg[1] và avg[2] là -1 vì có ít hơn k phần tử <strong>ở phía trước</strong> mỗi chỉ số.
- Tổng của mảng con có tâm tại chỉ số 3 và bán kính 3 là: 7 + 4 + 3 + 9 + 1 + 8 + 5 = 37.
  Sử dụng <strong>phép chia nguyên</strong>, avg[3] = 37 / 7 = 5.
- Với mảng con có tâm tại chỉ số 4, avg[4] = (4 + 3 + 9 + 1 + 8 + 5 + 2) / 7 = 4.
- Với mảng con có tâm tại chỉ số 5, avg[5] = (3 + 9 + 1 + 8 + 5 + 2 + 6) / 7 = 4.
- avg[6], avg[7] và avg[8] là -1 vì có ít hơn k phần tử <strong>ở phía sau</strong> mỗi chỉ số.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [100000], k = 0
<strong>Đầu ra:</strong> [100000]
<strong>Giải thích:</strong>
- Tổng của mảng con có tâm tại chỉ số 0 và bán kính 0 là: 100000.
  avg[0] = 100000 / 1 = 100000.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [8], k = 100000
<strong>Đầu ra:</strong> [-1]
<strong>Giải thích:</strong>
- avg[0] là -1 vì có ít hơn k phần tử ở cả phía trước và phía sau chỉ số 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i], k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Các cửa sổ có độ dài cố định $2k+1$; những vị trí ở hai đầu không đủ phần tử sẽ giữ giá trị $-1$. Với $n \le 10^5$, ta trượt một tổng chạy.
>
> Thêm phần tử bên phải, bỏ phần tử bên trái, rồi ghi giá trị trung bình tại tâm $i-k$.

<!-- thinking:end -->

Độ dài của một mảng con có bán kính $k$ là $k \times 2 + 1$, vì vậy ta có thể duy trì một cửa sổ có kích thước $k \times 2 + 1$ và gọi tổng của tất cả phần tử trong cửa sổ là $s$.

Ta tạo một mảng kết quả $\textit{ans}$ có độ dài $n$, ban đầu đặt mọi phần tử là $-1$.

Tiếp theo, ta duyệt mảng $\textit{nums}$, thêm giá trị của $\textit{nums}[i]$ vào tổng cửa sổ $s$. Nếu $i \geq k \times 2$, nghĩa là kích thước cửa sổ đã là $k \times 2 + 1$, ta gán $\textit{ans}[i-k] = \frac{s}{k \times 2 + 1}$. Sau đó, ta loại giá trị $\textit{nums}[i - k \times 2]$ khỏi tổng cửa sổ $s$. Tiếp tục duyệt phần tử kế tiếp.

Cuối cùng, trả về mảng kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Không tính phần bộ nhớ của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getAverages(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        ans = [-1] * n
        s = 0
        for i, x in enumerate(nums):
            s += x
            if i >= k * 2:
                ans[i - k] = s // (k * 2 + 1)
                s -= nums[i - k * 2]
        return ans
```

#### Java

```java
class Solution {
    public int[] getAverages(int[] nums, int k) {
        int n = nums.length;
        int[] ans = new int[n];
        Arrays.fill(ans, -1);
        long s = 0;
        for (int i = 0; i < n; ++i) {
            s += nums[i];
            if (i >= k * 2) {
                ans[i - k] = (int) (s / (k * 2 + 1));
                s -= nums[i - k * 2];
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
    vector<int> getAverages(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> ans(n, -1);
        long long s = 0;
        for (int i = 0; i < n; ++i) {
            s += nums[i];
            if (i >= k * 2) {
                ans[i - k] = s / (k * 2 + 1);
                s -= nums[i - k * 2];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getAverages(nums []int, k int) []int {
	ans := make([]int, len(nums))
	for i := range ans {
		ans[i] = -1
	}
	s := 0
	for i, x := range nums {
		s += x
		if i >= k*2 {
			ans[i-k] = s / (k*2 + 1)
			s -= nums[i-k*2]
		}
	}
	return ans
}
```

#### TypeScript

```ts
function getAverages(nums: number[], k: number): number[] {
    const n = nums.length;
    const ans: number[] = Array(n).fill(-1);
    let s = 0;
    for (let i = 0; i < n; ++i) {
        s += nums[i];
        if (i >= k * 2) {
            ans[i - k] = Math.floor(s / (k * 2 + 1));
            s -= nums[i - k * 2];
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
