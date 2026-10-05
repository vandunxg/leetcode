---
comments: true
difficulty: Medium
rating: 1656
source: Biweekly Contest 171 Q2
tags:
    - Bit Manipulation
    - Array
    - Two Pointers
    - Binary Search
---

<!-- problem:start -->

# [3766. Minimum Operations to Make Binary Palindrome](https://leetcode.com/problems/minimum-operations-to-make-binary-palindrome)

[中文文档](/solution/3700-3799/3766.Minimum%20Operations%20to%20Make%20Binary%20Palindrome/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Với mỗi phần tử <code>nums[i]</code>, bạn có thể thực hiện các thao tác sau số lần <strong>tùy ý</strong> (kể cả không lần nào):</p>

<ul>
	<li>Tăng <code>nums[i]</code> thêm 1, hoặc</li>
	<li>Giảm <code>nums[i]</code> đi 1.</li>
</ul>

<p>Một số được gọi là <strong>palindrome nhị phân</strong> nếu biểu diễn nhị phân không có các số 0 ở đầu đọc xuôi và ngược giống nhau.</p>

<p>Nhiệm vụ của bạn là trả về một mảng số nguyên <code>ans</code>, trong đó <code>ans[i]</code> biểu thị số thao tác <strong>ít nhất</strong> cần thực hiện để chuyển <code>nums[i]</code> thành một <strong>palindrome nhị phân</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một tập hợp thao tác tối ưu:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>nums[i]</code></th>
			<th style="border: 1px solid black;">Nhị phân(<code>nums[i]</code>)</th>
			<th style="border: 1px solid black;">Palindrome<br />
			gần nhất</th>
			<th style="border: 1px solid black;">Nhị phân<br />
			(Palindrome)</th>
			<th style="border: 1px solid black;">Số thao tác cần thiết</th>
			<th style="border: 1px solid black;"><code>ans[i]</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Đã là palindrome</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">10</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">11</td>
			<td style="border: 1px solid black;">Tăng 1</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">100</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">11</td>
			<td style="border: 1px solid black;">Giảm 1</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [0, 1, 1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,7,12]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,0,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một tập hợp thao tác tối ưu:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;"><code>nums[i]</code></th>
			<th style="border: 1px solid black;">Nhị phân(<code>nums[i]</code>)</th>
			<th style="border: 1px solid black;">Palindrome<br />
			gần nhất</th>
			<th style="border: 1px solid black;">Nhị phân<br />
			(Palindrome)</th>
			<th style="border: 1px solid black;">Số thao tác cần thiết</th>
			<th style="border: 1px solid black;"><code>ans[i]</code></th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">110</td>
			<td style="border: 1px solid black;">5</td>
			<td style="border: 1px solid black;">101</td>
			<td style="border: 1px solid black;">Giảm 1</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">111</td>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">111</td>
			<td style="border: 1px solid black;">Đã là palindrome</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">12</td>
			<td style="border: 1px solid black;">1100</td>
			<td style="border: 1px solid black;">15</td>
			<td style="border: 1px solid black;">1111</td>
			<td style="border: 1px solid black;">Tăng 3</td>
			<td style="border: 1px solid black;">3</td>
		</tr>
	</tbody>
</table>

<p>Do đó, <code>ans = [1, 0, 3]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 5000</code></li>
	<li><code><sup>​​​​​​​</sup>1 &lt;= nums[i] &lt;=<sup> </sup>5000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Vì $nums[i]\le 5000$, palindrome nhị phân gần nhất vẫn nằm trong một khoảng vừa phải. Chúng ta tiền xử lý mọi palindrome nhị phân nhỏ hơn $2^{14}$, rồi với mỗi $x$, dùng tìm kiếm nhị phân để tìm hai số lân cận và lấy chênh lệch tuyệt đối nhỏ hơn.

<!-- thinking:end -->

Ta nhận thấy rằng phạm vi các số được cho trong đề bài chỉ là $[1, 5000]$. Vì vậy, ta tiền xử lý trực tiếp tất cả các số palindrome nhị phân trong phạm vi $[0, 2^{14})$ và lưu chúng vào một mảng, ký hiệu là $\textit{p}$.

Tiếp theo, với mỗi số $x$, ta dùng tìm kiếm nhị phân để tìm số palindrome đầu tiên lớn hơn hoặc bằng $x$ trong mảng $\textit{p}$, ký hiệu là $\textit{p}[i]$, cũng như số palindrome đầu tiên nhỏ hơn $x$, ký hiệu là $\textit{p}[i - 1]$. Sau đó, ta tính số thao tác cần thiết để chuyển $x$ thành hai số palindrome này và lấy giá trị nhỏ hơn làm đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$, và độ phức tạp không gian là $O(M)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$, còn $M$ là số lượng palindrome nhị phân được tiền xử lý.

<!-- tabs:start -->

#### Python3

```python
p = []
for i in range(1 << 14):
    s = bin(i)[2:]
    if s == s[::-1]:
        p.append(i)


class Solution:
    def minOperations(self, nums: List[int]) -> List[int]:
        ans = []
        for x in nums:
            i = bisect_left(p, x)
            times = inf
            if i < len(p):
                times = min(times, p[i] - x)
            if i >= 1:
                times = min(times, x - p[i - 1])
            ans.append(times)
        return ans
```

#### Java

```java
class Solution {
    private static final List<Integer> p = new ArrayList<>();

    static {
        int N = 1 << 14;
        for (int i = 0; i < N; i++) {
            String s = Integer.toBinaryString(i);
            String rs = new StringBuilder(s).reverse().toString();
            if (s.equals(rs)) {
                p.add(i);
            }
        }
    }

    public int[] minOperations(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        Arrays.fill(ans, Integer.MAX_VALUE);
        for (int k = 0; k < n; ++k) {
            int x = nums[k];
            int i = binarySearch(p, x);
            if (i < p.size()) {
                ans[k] = Math.min(ans[k], p.get(i) - x);
            }
            if (i >= 1) {
                ans[k] = Math.min(ans[k], x - p.get(i - 1));
            }
        }

        return ans;
    }

    private int binarySearch(List<Integer> p, int x) {
        int l = 0, r = p.size();
        while (l < r) {
            int mid = (l + r) >>> 1;
            if (p.get(mid) >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
vector<int> p;

auto init = [] {
    int N = 1 << 14;
    for (int i = 0; i < N; ++i) {
        string s = bitset<14>(i).to_string();
        s = s.substr(s.find_first_not_of('0') == string::npos ? 13 : s.find_first_not_of('0'));
        string rs = s;
        reverse(rs.begin(), rs.end());
        if (s == rs) {
            p.push_back(i);
        }
    }
    return 0;
}();

class Solution {
public:
    vector<int> minOperations(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n, INT_MAX);
        for (int k = 0; k < n; ++k) {
            int x = nums[k];
            int i = lower_bound(p.begin(), p.end(), x) - p.begin();
            if (i < (int) p.size()) {
                ans[k] = min(ans[k], p[i] - x);
            }
            if (i >= 1) {
                ans[k] = min(ans[k], x - p[i - 1]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
var p []int

func init() {
	N := 1 << 14
	for i := 0; i < N; i++ {
		s := strconv.FormatInt(int64(i), 2)
		if isPalindrome(s) {
			p = append(p, i)
		}
	}
}

func isPalindrome(s string) bool {
	runes := []rune(s)
	for i := 0; i < len(runes)/2; i++ {
		if runes[i] != runes[len(runes)-1-i] {
			return false
		}
	}
	return true
}

func minOperations(nums []int) []int {
	ans := make([]int, len(nums))
	for k, x := range nums {
		i := sort.SearchInts(p, x)
		t := math.MaxInt32
		if i < len(p) {
			t = p[i] - x
		}
		if i >= 1 {
			t = min(t, x-p[i-1])
		}
		ans[k] = t
	}
	return ans
}
```

#### TypeScript

```ts
const p: number[] = (() => {
    const res: number[] = [];
    const N = 1 << 14;
    for (let i = 0; i < N; i++) {
        const s = i.toString(2);
        if (s === s.split('').reverse().join('')) {
            res.push(i);
        }
    }
    return res;
})();

function minOperations(nums: number[]): number[] {
    const ans: number[] = Array(nums.length).fill(Number.MAX_SAFE_INTEGER);

    for (let k = 0; k < nums.length; k++) {
        const x = nums[k];
        const i = _.sortedIndex(p, x);
        if (i < p.length) {
            ans[k] = p[i] - x;
        }
        if (i >= 1) {
            ans[k] = Math.min(ans[k], x - p[i - 1]);
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
