---
comments: true
difficulty: Medium
rating: 2095
source: Biweekly Contest 177 Q3
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [3854. Minimum Operations to Make Array Parity Alternating](https://leetcode.com/problems/minimum-operations-to-make-array-parity-alternating)

[中文文档](/solution/3800-3899/3854.Minimum%20Operations%20to%20Make%20Array%20Parity%20Alternating/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một mảng được gọi là <strong>luân phiên chẵn lẻ</strong> nếu với mọi chỉ số <code>i</code> thỏa mãn <code>0 &lt;= i &lt; n - 1</code>, <code>nums[i]</code> và <code>nums[i + 1]</code> có parity khác nhau (một số chẵn và một số lẻ).</p>

<p>Trong một thao tác, bạn có thể chọn một chỉ số <code>i</code> bất kỳ rồi tăng <code>nums[i]</code> thêm 1 hoặc giảm <code>nums[i]</code> đi 1.</p>

<p>Trả về một mảng số nguyên <code>answer</code> có độ dài 2, trong đó:</p>

<ul>
	<li><code>answer[0]</code> là số thao tác <strong>nhỏ nhất</strong> cần thực hiện để mảng trở thành luân phiên chẵn lẻ.</li>
	<li><code>answer[1]</code> là giá trị <strong>nhỏ nhất</strong> có thể của <code>max(nums) - min(nums)</code>, xét trên tất cả các mảng luân phiên chẵn lẻ có thể thu được bằng cách thực hiện <strong>đúng</strong> <code>answer[0]</code> thao tác.</li>
</ul>

<p>Một mảng có độ dài 1 được xem là luân phiên chẵn lẻ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-2,-3,1,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,6]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thực hiện các thao tác sau:</p>

<ul>
	<li>Tăng <code>nums[2]</code> thêm 1, khi đó <code>nums = [-2, -3, 2, 4]</code>.</li>
	<li>Giảm <code>nums[3]</code> đi 1, khi đó <code>nums = [-2, -3, 2, 3]</code>.</li>
</ul>

<p>Mảng kết quả là luân phiên chẵn lẻ, và giá trị <code>max(nums) - min(nums) = 3 - (-3) = 6</code> là nhỏ nhất trong tất cả các mảng luân phiên chẵn lẻ có thể thu được bằng đúng 2 thao tác.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,2,-2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thực hiện thao tác sau:</p>

<ul>
	<li>Giảm <code>nums[1]</code> đi 1, khi đó <code>nums = [0, 1, -2]</code>.</li>
</ul>

<p>Mảng kết quả là luân phiên chẵn lẻ, và giá trị <code>max(nums) - min(nums) = 1 - (-2) = 3</code> là nhỏ nhất trong tất cả các mảng luân phiên chẵn lẻ có thể thu được bằng đúng 1 thao tác.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,0]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không cần thực hiện thao tác nào. Mảng đã là luân phiên chẵn lẻ, và giá trị <code>max(nums) - min(nums) = 7 - 7 = 0</code> là nhỏ nhất có thể.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phép $\pm 1$ thay đổi một phần tử để các parity kề nhau luân phiên, và trong số các mảng dùng số bước tối thiểu, ta cần tối thiểu hóa range. $n \le 10^5$.
>
> Chỉ có hai mẫu: các chỉ số chẵn chứa số chẵn, hoặc các chỉ số chẵn chứa số lẻ. Trong mỗi mẫu, một phần tử không khớp cần đúng một bước, không phụ thuộc vào dấu tăng hay giảm.
>
> Để giữ range nhỏ, phần tử không khớp đang là min toàn cục chỉ có thể tăng, còn phần tử đang là max toàn cục chỉ có thể giảm; các lựa chọn dấu khác không tạo ra cực trị mới vượt quá các cực trị đó.
>
> Tính $(\textit{moves},\textit{range})$ cho cả hai mẫu rồi chọn cặp nhỏ hơn theo thứ tự từ điển; mảng một phần tử không cần thao tác.

<!-- thinking:end -->

Ta có thể biến đổi mảng thành một trong hai dạng luân phiên chẵn lẻ: dạng thứ nhất có số chẵn ở các chỉ số chẵn và số lẻ ở các chỉ số lẻ, dạng thứ hai có số lẻ ở các chỉ số chẵn và số chẵn ở các chỉ số lẻ.

Với mỗi dạng, ta tính số thao tác cần thiết cũng như giá trị lớn nhất và nhỏ nhất của mảng kết quả. Cuối cùng, ta chọn phương án có ít thao tác hơn; nếu số thao tác bằng nhau, ta chọn phương án có hiệu giữa giá trị lớn nhất và nhỏ nhất nhỏ hơn.

Ta định nghĩa hàm $f(k)$, trong đó $k$ biểu diễn parity mong muốn của các số nằm ở chỉ số chẵn ($k=0$ nghĩa là chẵn và $k=1$ nghĩa là lẻ). Hàm $f(k)$ tính số thao tác cần thiết để biến mảng thành dạng luân phiên chẵn lẻ tương ứng, đồng thời tính giá trị lớn nhất và nhỏ nhất của mảng kết quả.

Trong hàm $f(k)$, ta duyệt qua từng phần tử của mảng. Nếu parity của phần tử hiện tại không khớp với parity cần có, ta thực hiện một thao tác để điều chỉnh nó. Để tối thiểu hóa hiệu giữa giá trị lớn nhất và nhỏ nhất, ta đưa phần tử hiện tại về số gần nhất, tức là tăng thêm $1$ hoặc giảm đi $1$ tùy thuộc vào việc phần tử hiện tại có bằng giá trị nhỏ nhất hay lớn nhất của mảng hay không. Nếu phần tử hiện tại bằng giá trị nhỏ nhất, ta tăng nó thêm $1$; nếu bằng giá trị lớn nhất, ta giảm nó đi $1$; các trường hợp khác có thể tăng hoặc giảm $1$. Sau đó, ta cập nhật giá trị lớn nhất và nhỏ nhất hiện tại. Cuối cùng, hàm $f(k)$ trả về số thao tác và hiệu giữa giá trị lớn nhất và nhỏ nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$ vì ta chỉ sử dụng thêm một lượng không gian hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeParityAlternating(self, nums: List[int]) -> List[int]:
        def f(k: int) -> List[int]:
            cnt = 0
            a, b = inf, -inf
            for i, x in enumerate(nums):
                if ((x - i) & 1) != k:
                    cnt += 1
                    if x == mn:
                        x += 1
                    elif x == mx:
                        x -= 1
                a = min(a, x)
                b = max(b, x)
            return [cnt, max(1, b - a)]

        if len(nums) == 1:
            return [0, 0]

        mn, mx = min(nums), max(nums)
        return min(f(0), f(1))
```

#### Java

```java
class Solution {
    private int[] nums;
    private int mn;
    private int mx;
    private static final int INF = Integer.MAX_VALUE / 2;

    public int[] makeParityAlternating(int[] nums) {
        if (nums.length == 1) {
            return new int[] {0, 0};
        }
        this.nums = nums;

        mn = INF;
        mx = -INF;
        for (int x : nums) {
            mn = Math.min(mn, x);
            mx = Math.max(mx, x);
        }

        int[] r0 = f(0);
        int[] r1 = f(1);

        if (r0[0] != r1[0]) {
            return r0[0] < r1[0] ? r0 : r1;
        }
        return r0[1] <= r1[1] ? r0 : r1;
    }

    private int[] f(int k) {
        int cnt = 0;
        int a = INF;
        int b = -INF;

        for (int i = 0; i < nums.length; i++) {
            int x = nums[i];
            if (((x - i) & 1) != k) {
                cnt++;
                if (x == mn) {
                    x += 1;
                } else if (x == mx) {
                    x -= 1;
                }
            }
            a = Math.min(a, x);
            b = Math.max(b, x);
        }
        return new int[] {cnt, Math.max(1, b - a)};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> makeParityAlternating(vector<int>& nums) {
        if (nums.size() == 1) {
            return {0, 0};
        }

        auto [mnIt, mxIt] = minmax_element(nums.begin(), nums.end());
        int mn = *mnIt;
        int mx = *mxIt;

        auto f = [&](int k) {
            int cnt = 0;
            int a = INT_MAX;
            int b = INT_MIN;

            for (int i = 0; i < nums.size(); i++) {
                int x = nums[i];
                if (((x - i) & 1) != k) {
                    cnt++;
                    if (x == mn) {
                        ++x;
                    } else if (x == mx) {
                        --x;
                    }
                }
                a = min(a, x);
                b = max(b, x);
            }
            return vector<int>{cnt, max(1, b - a)};
        };

        vector<int> r0 = f(0);
        vector<int> r1 = f(1);

        return r0 < r1 ? r0 : r1;
    }
};
```

#### Go

```go
func makeParityAlternating(nums []int) []int {
	if len(nums) == 1 {
		return []int{0, 0}
	}

	mn := slices.Min(nums)
	mx := slices.Max(nums)

	f := func(k int) []int {
		cnt := 0
		a, b := math.MaxInt, math.MinInt

		for i, x := range nums {
			if ((x - i) & 1) != k {
				cnt++
				if x == mn {
					x++
				} else if x == mx {
					x--
				}
			}
			a = min(a, x)
			b = max(b, x)
		}

		return []int{cnt, max(1, b-a)}
	}

	r0 := f(0)
	r1 := f(1)

	if r0[0] != r1[0] {
		if r0[0] < r1[0] {
			return r0
		}
		return r1
	}
	if r0[1] <= r1[1] {
		return r0
	}
	return r1
}
```

#### TypeScript

```ts
function makeParityAlternating(nums: number[]): number[] {
    if (nums.length === 1) {
        return [0, 0];
    }

    const mn = Math.min(...nums);
    const mx = Math.max(...nums);

    const f = (k: number): number[] => {
        let cnt = 0;
        let a = Number.MAX_SAFE_INTEGER;
        let b = Number.MIN_SAFE_INTEGER;

        for (let i = 0; i < nums.length; i++) {
            let x = nums[i];
            if (((x - i) & 1) !== k) {
                cnt++;
                if (x === mn) {
                    ++x;
                } else if (x === mx) {
                    --x;
                }
            }
            a = Math.min(a, x);
            b = Math.max(b, x);
        }
        return [cnt, Math.max(1, b - a)];
    };

    const r0 = f(0);
    const r1 = f(1);

    if (r0[0] !== r1[0]) {
        return r0[0] < r1[0] ? r0 : r1;
    }
    return r0[1] <= r1[1] ? r0 : r1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
