---
comments: true
difficulty: Hard
rating: 2451
source: Weekly Contest 395 Q4
tags:
    - Array
    - Hash Table
    - Binary Search
    - Sliding Window
---

<!-- problem:start -->

# [3134. Find the Median of the Uniqueness Array](https://leetcode.com/problems/find-the-median-of-the-uniqueness-array)

[中文文档](/solution/3100-3199/3134.Find%20the%20Median%20of%20the%20Uniqueness%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng số nguyên <code>nums</code>. <strong>Mảng uniqueness</strong> của <code>nums</code> là mảng đã được sắp xếp, chứa số lượng phần tử phân biệt của tất cả <span data-keyword="subarray-nonempty">subarray</span> của <code>nums</code>. Nói cách khác, đây là một mảng đã được sắp xếp gồm <code>distinct(nums[i..j])</code> với mọi <code>0 &lt;= i &lt;= j &lt; nums.length</code>.</p>

<p>Ở đây, <code>distinct(nums[i..j])</code> biểu thị số lượng phần tử phân biệt trong subarray bắt đầu tại chỉ số <code>i</code> và kết thúc tại chỉ số <code>j</code>.</p>

<p>Hãy trả về <strong>median</strong> của <strong>mảng uniqueness</strong> của <code>nums</code>.</p>

<p><strong>Lưu ý</strong> rằng <strong>median</strong> của một mảng được định nghĩa là phần tử ở giữa mảng khi mảng được sắp xếp theo thứ tự không giảm. Nếu có hai lựa chọn cho median, chọn giá trị <strong>nhỏ hơn</strong>.<!-- notionvc: 7e0f5178-4273-4a82-95ce-3395297921dc --></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng uniqueness của <code>nums</code> là <code>[distinct(nums[0..0]), distinct(nums[1..1]), distinct(nums[2..2]), distinct(nums[0..1]), distinct(nums[1..2]), distinct(nums[0..2])]</code>, tương đương với <code>[1, 1, 1, 2, 2, 3]</code>. Mảng uniqueness có median là 1. Vì vậy, đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,4,3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng uniqueness của <code>nums</code> là <code>[1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 3, 3, 3]</code>. Mảng uniqueness có median là 2. Vì vậy, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,3,5,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng uniqueness của <code>nums</code> là <code>[1, 1, 1, 1, 2, 2, 2, 3, 3, 3]</code>. Mảng uniqueness có median là 2. Vì vậy, đáp án là 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Mảng uniqueness lưu số lượng phần tử phân biệt của mọi subarray. Không thể tạo ra $O(n^2)$ giá trị rồi tìm median.
>
> Số subarray có nhiều nhất $x$ giá trị phân biệt tăng theo $x$, vì vậy median là giá trị $x$ nhỏ nhất sao cho số lượng này đạt một nửa $m=n(n+1)/2$. Cửa sổ trượt có thể đếm các subarray đó trong thời gian tuyến tính.
>
> Ta tìm kiếm nhị phân trên $x$. Mở rộng $r$, thu hẹp $l$ khi cửa sổ có nhiều hơn $mx$ phần tử phân biệt, rồi cộng $r-l+1$. Phép kiểm tra thành công khi số lượng đạt $\lceil m/2\rceil$.

<!-- thinking:end -->

Gọi độ dài của mảng $\textit{nums}$ là $n$. Độ dài của mảng uniqueness là $m = \frac{(1 + n) \times n}{2}$, và median của mảng uniqueness là số nhỏ thứ $\frac{m + 1}{2}$ trong $m$ số này.

Hãy xét có bao nhiêu số trong mảng uniqueness nhỏ hơn hoặc bằng $x$. Khi $x$ tăng, sẽ có ngày càng nhiều số nhỏ hơn hoặc bằng $x$. Tính chất này có tính đơn điệu, vì vậy ta có thể dùng tìm kiếm nhị phân để liệt kê $x$ và tìm giá trị $x$ đầu tiên sao cho số phần tử trong mảng uniqueness nhỏ hơn hoặc bằng $x$ lớn hơn hoặc bằng $\frac{m + 1}{2}$. Giá trị $x$ này là median của mảng uniqueness.

Ta đặt biên trái của tìm kiếm nhị phân là $l = 0$ và biên phải là $r = n$. Sau đó thực hiện tìm kiếm nhị phân. Với mỗi $\textit{mid}$, ta kiểm tra xem số phần tử trong mảng uniqueness nhỏ hơn hoặc bằng $\textit{mid}$ có lớn hơn hoặc bằng $\frac{m + 1}{2}$ hay không. Ta thực hiện việc này thông qua hàm $\text{check}(mx)$.

Ý tưởng triển khai hàm $\text{check}(mx)$ như sau:

Vì subarray càng dài thì càng chứa nhiều phần tử khác nhau, ta có thể dùng hai con trỏ để duy trì một cửa sổ trượt sao cho số phần tử khác nhau trong cửa sổ không vượt quá $mx$. Cụ thể, ta duy trì một hash table $\textit{cnt}$, trong đó $\textit{cnt}[x]$ biểu thị số lần xuất hiện của phần tử $x$ trong cửa sổ. Ta sử dụng hai con trỏ $l$ và $r$, trong đó $l$ là biên trái của cửa sổ còn $r$ là biên phải. Ban đầu, $l = r = 0$.

Ta duyệt $r$. Với mỗi $r$, ta thêm $\textit{nums}[r]$ vào cửa sổ và cập nhật $\textit{cnt}[\textit{nums}[r]]$. Nếu số phần tử khác nhau trong cửa sổ vượt quá $mx$, ta cần di chuyển $l$ sang phải cho đến khi số phần tử khác nhau trong cửa sổ không vượt quá $mx$. Khi đó, các subarray có điểm cuối phải là $r$ và điểm đầu nằm trong đoạn $[l, .., r]$ đều thỏa mãn điều kiện, có tổng cộng $r - l + 1$ subarray như vậy. Ta cộng dồn số lượng này vào $k$. Nếu $k$ lớn hơn hoặc bằng $\frac{m + 1}{2}$, điều đó có nghĩa là số phần tử trong mảng uniqueness nhỏ hơn hoặc bằng $\textit{mid}$ lớn hơn hoặc bằng $\frac{m + 1}{2}$, và ta trả về $\text{true}$; nếu không, ta trả về $\text{false}$.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def medianOfUniquenessArray(self, nums: List[int]) -> int:
        def check(mx: int) -> bool:
            cnt = defaultdict(int)
            k = l = 0
            for r, x in enumerate(nums):
                cnt[x] += 1
                while len(cnt) > mx:
                    y = nums[l]
                    cnt[y] -= 1
                    if cnt[y] == 0:
                        cnt.pop(y)
                    l += 1
                k += r - l + 1
                if k >= (m + 1) // 2:
                    return True
            return False

        n = len(nums)
        m = (1 + n) * n // 2
        return bisect_left(range(n), True, key=check)
```

#### Java

```java
class Solution {
    private long m;
    private int[] nums;

    public int medianOfUniquenessArray(int[] nums) {
        int n = nums.length;
        this.nums = nums;
        m = (1L + n) * n / 2;
        int l = 0, r = n;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }

    private boolean check(int mx) {
        Map<Integer, Integer> cnt = new HashMap<>();
        long k = 0;
        for (int l = 0, r = 0; r < nums.length; ++r) {
            int x = nums[r];
            cnt.merge(x, 1, Integer::sum);
            while (cnt.size() > mx) {
                int y = nums[l++];
                if (cnt.merge(y, -1, Integer::sum) == 0) {
                    cnt.remove(y);
                }
            }
            k += r - l + 1;
            if (k >= (m + 1) / 2) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int medianOfUniquenessArray(vector<int>& nums) {
        int n = nums.size();
        using ll = long long;
        ll m = (1LL + n) * n / 2;
        int l = 0, r = n;
        auto check = [&](int mx) -> bool {
            unordered_map<int, int> cnt;
            ll k = 0;
            for (int l = 0, r = 0; r < n; ++r) {
                int x = nums[r];
                ++cnt[x];
                while (cnt.size() > mx) {
                    int y = nums[l++];
                    if (--cnt[y] == 0) {
                        cnt.erase(y);
                    }
                }
                k += r - l + 1;
                if (k >= (m + 1) / 2) {
                    return true;
                }
            }
            return false;
        };
        while (l < r) {
            int mid = (l + r) >> 1;
            if (check(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func medianOfUniquenessArray(nums []int) int {
	n := len(nums)
	m := (1 + n) * n / 2
	return sort.Search(n, func(mx int) bool {
		cnt := map[int]int{}
		l, k := 0, 0
		for r, x := range nums {
			cnt[x]++
			for len(cnt) > mx {
				y := nums[l]
				cnt[y]--
				if cnt[y] == 0 {
					delete(cnt, y)
				}
				l++
			}
			k += r - l + 1
			if k >= (m+1)/2 {
				return true
			}
		}
		return false
	})
}
```

#### TypeScript

```ts
function medianOfUniquenessArray(nums: number[]): number {
    const n = nums.length;
    const m = Math.floor(((1 + n) * n) / 2);
    let [l, r] = [0, n];
    const check = (mx: number): boolean => {
        const cnt = new Map<number, number>();
        let [l, k] = [0, 0];
        for (let r = 0; r < n; ++r) {
            const x = nums[r];
            cnt.set(x, (cnt.get(x) || 0) + 1);
            while (cnt.size > mx) {
                const y = nums[l++];
                cnt.set(y, cnt.get(y)! - 1);
                if (cnt.get(y) === 0) {
                    cnt.delete(y);
                }
            }
            k += r - l + 1;
            if (k >= Math.floor((m + 1) / 2)) {
                return true;
            }
        }
        return false;
    };
    while (l < r) {
        const mid = (l + r) >> 1;
        if (check(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
