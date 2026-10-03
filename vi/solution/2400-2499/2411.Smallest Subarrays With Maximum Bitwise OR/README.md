---
comments: true
difficulty: Medium
rating: 1938
source: Biweekly Contest 87 Q3
tags:
    - Bit Manipulation
    - Array
    - Binary Search
    - Sliding Window
---

<!-- problem:start -->

# [2411. Smallest Subarrays With Maximum Bitwise OR](https://leetcode.com/problems/smallest-subarrays-with-maximum-bitwise-or)

[中文文档](/solution/2400-2499/2411.Smallest%20Subarrays%20With%20Maximum%20Bitwise%20OR/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng <code>nums</code> có độ dài <code>n</code>, được đánh chỉ số từ <strong>0</strong>, gồm các số nguyên không âm. Với mỗi chỉ số <code>i</code> từ <code>0</code> đến <code>n - 1</code>, bạn cần xác định độ dài của mảng con không rỗng <strong>ngắn nhất</strong> của <code>nums</code> bắt đầu tại <code>i</code> (<strong>bao gồm</strong>) có <strong>phép OR bit</strong> đạt giá trị <strong>lớn nhất</strong> có thể.</p>

<ul>
    <li>Nói cách khác, gọi <code>B<sub>ij</sub></code> là phép OR bit của mảng con <code>nums[i...j]</code>. Bạn cần tìm mảng con ngắn nhất bắt đầu tại <code>i</code>, sao cho phép OR bit của mảng con này bằng <code>max(B<sub>ik</sub>)</code> với <code>i &lt;= k &lt;= n - 1</code>.</li>
</ul>

<p>Phép OR bit của một mảng là phép OR bit của tất cả các số trong mảng đó.</p>

<p>Trả về <em>một mảng số nguyên </em><code>answer</code><em> có kích thước </em><code>n</code><em>, trong đó </em><code>answer[i]</code><em> là độ dài của mảng con có kích thước <strong>nhỏ nhất</strong> bắt đầu tại </em><code>i</code><em> và có phép OR bit <strong>lớn nhất</strong>.</em></p>

<p><strong>Mảng con</strong> là một dãy liên tiếp không rỗng gồm các phần tử nằm trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,0,2,1,3]
<strong>Đầu ra:</strong> [3,3,2,2,1]
<strong>Giải thích:</strong>
Giá trị OR bit lớn nhất có thể bắt đầu tại bất kỳ chỉ số nào là 3.
- Bắt đầu tại chỉ số 0, mảng con ngắn nhất tạo ra giá trị đó là [1,0,2].
- Bắt đầu tại chỉ số 1, mảng con ngắn nhất tạo ra giá trị OR bit lớn nhất là [0,2,1].
- Bắt đầu tại chỉ số 2, mảng con ngắn nhất tạo ra giá trị OR bit lớn nhất là [2,1].
- Bắt đầu tại chỉ số 3, mảng con ngắn nhất tạo ra giá trị OR bit lớn nhất là [1,3].
- Bắt đầu tại chỉ số 4, mảng con ngắn nhất tạo ra giá trị đó là [3].
Do đó, ta trả về [3,3,2,2,1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2]
<strong>Đầu ra:</strong> [2,1]
<strong>Giải thích:
</strong>Bắt đầu tại chỉ số 0, mảng con ngắn nhất tạo ra giá trị OR bit lớn nhất có độ dài 2.
Bắt đầu tại chỉ số 1, mảng con ngắn nhất tạo ra giá trị OR bit lớn nhất có độ dài 1.
Do đó, ta trả về [2,1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>n == nums.length</code></li>
    <li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi điểm bắt đầu $i$, việc duyệt sang phải để tìm mảng con ngắn nhất có OR bit lớn nhất sẽ tốn $O(n^2)$ và không đáp ứng được $n\le 10^5$. OR lớn nhất từ $i$ được quyết định bởi vị trí xuất hiện đầu tiên của bit $1$ đối với từng bit, và tất cả các vị trí này đều nằm tại hoặc bên phải $i$.
>
> Duyệt từ phải sang trái và lưu, với mỗi trong $32$ bit, chỉ số gần nhất mà bit đó bằng $1$. Nếu giá trị hiện tại đã có bit đó, ta cập nhật chỉ số; nếu không, cửa sổ phải kéo dài đến chỉ số đã lưu. Độ dài cần tìm là vị trí xa nhất trong các vị trí như vậy.

<!-- thinking:end -->

Để tìm mảng con ngắn nhất bắt đầu tại vị trí $i$ và tối đa hóa phép OR bit, ta cần tối đa hóa số lượng bit $1$ trong kết quả.

Ta sử dụng một mảng $f$ có kích thước $32$ để lưu vị trí sớm nhất của mỗi bit $1$.

Ta duyệt mảng $nums[i]$ theo thứ tự ngược. Với bit thứ $j$ của $nums[i]$, nếu bit đó bằng $1$ thì $f[j]$ là $i$. Ngược lại, nếu $f[j]$ khác $-1$, điều đó có nghĩa là bên phải đã tìm thấy một số có bit thứ $j$ bằng $1$, nên ta cập nhật độ dài.

Độ phức tạp thời gian là $O(n \times \log m)$, trong đó $n$ là độ dài của mảng $nums$ và $m$ là giá trị lớn nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestSubarrays(self, nums: List[int]) -> List[int]:
        n = len(nums)
        ans = [1] * n
        f = [-1] * 32
        for i in range(n - 1, -1, -1):
            t = 1
            for j in range(32):
                if (nums[i] >> j) & 1:
                    f[j] = i
                elif f[j] != -1:
                    t = max(t, f[j] - i + 1)
            ans[i] = t
        return ans
```

#### Java

```java
class Solution {
    public int[] smallestSubarrays(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        int[] f = new int[32];
        Arrays.fill(f, -1);
        for (int i = n - 1; i >= 0; --i) {
            int t = 1;
            for (int j = 0; j < 32; ++j) {
                if (((nums[i] >> j) & 1) == 1) {
                    f[j] = i;
                } else if (f[j] != -1) {
                    t = Math.max(t, f[j] - i + 1);
                }
            }
            ans[i] = t;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> smallestSubarrays(vector<int>& nums) {
        int n = nums.size();
        vector<int> f(32, -1);
        vector<int> ans(n);
        for (int i = n - 1; ~i; --i) {
            int t = 1;
            for (int j = 0; j < 32; ++j) {
                if ((nums[i] >> j) & 1) {
                    f[j] = i;
                } else if (f[j] != -1) {
                    t = max(t, f[j] - i + 1);
                }
            }
            ans[i] = t;
        }
        return ans;
    }
};
```

#### Go

```go
func smallestSubarrays(nums []int) []int {
	n := len(nums)
	f := make([]int, 32)
	for i := range f {
		f[i] = -1
	}
	ans := make([]int, n)
	for i := n - 1; i >= 0; i-- {
		t := 1
		for j := 0; j < 32; j++ {
			if ((nums[i] >> j) & 1) == 1 {
				f[j] = i
			} else if f[j] != -1 {
				t = max(t, f[j]-i+1)
			}
		}
		ans[i] = t
	}
	return ans
}
```

#### TypeScript

```ts
function smallestSubarrays(nums: number[]): number[] {
    const n = nums.length;
    const ans: number[] = Array(n).fill(1);
    const f: number[] = Array(32).fill(-1);

    for (let i = n - 1; i >= 0; i--) {
        let t = 1;
        for (let j = 0; j < 32; j++) {
            if ((nums[i] >> j) & 1) {
                f[j] = i;
            } else if (f[j] !== -1) {
                t = Math.max(t, f[j] - i + 1);
            }
        }
        ans[i] = t;
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_subarrays(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len();
        let mut ans = vec![1; n];
        let mut f = vec![-1; 32];

        for i in (0..n).rev() {
            let mut t = 1;
            for j in 0..32 {
                if (nums[i] >> j) & 1 != 0 {
                    f[j] = i as i32;
                } else if f[j] != -1 {
                    t = t.max(f[j] - i as i32 + 1);
                }
            }
            ans[i] = t;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
