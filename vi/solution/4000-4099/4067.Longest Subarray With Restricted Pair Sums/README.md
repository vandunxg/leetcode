---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [4067. Longest Subarray With Restricted Pair Sums](https://leetcode.com/problems/longest-subarray-with-restricted-pair-sums)

[中文文档](/solution/4000-4099/4067.Longest%20Subarray%20With%20Restricted%20Pair%20Sums/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một <strong>mảng con</strong> <code>nums[l..r]</code> được gọi là hợp lệ nếu không tồn tại ba chỉ số <strong>phân biệt</strong> <code>i</code>, <code>j</code> và <code>k</code> sao cho <code>l &lt;= i, j, k &lt;= r</code> và:</p>

<ul>
	<li><code>nums[i] + nums[j] == nums[k]</code></li>
</ul>

<p>Trả về độ dài <strong>lớn nhất</strong> của một mảng con hợp lệ của <code>nums</code>.</p>

<p>Một <strong>mảng con</strong> là một dãy phần tử liên tiếp <strong>không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,5,3,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xét mảng con <code>[3, 5, 3]</code>. Tổng của các cặp phần tử tại các chỉ số khác nhau là:</p>

<ul>
	<li><code>3 + 5 = 8</code></li>
	<li><code>3 + 3 = 6</code>, sử dụng hai lần xuất hiện khác nhau của 3</li>
	<li><code>5 + 3 = 8</code></li>
</ul>

<p>Không có tổng nào trong số này là phần tử tại chỉ số còn lại, nên mảng con là hợp lệ.</p>

<p>Mọi mảng con có độ dài 4 đều chứa 2, 3 và 5 tại các chỉ số khác nhau, trong đó <code>2 + 3 = 5</code>. Vì vậy, không tồn tại mảng con hợp lệ nào dài hơn, và đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,4,5,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các tổng thu được từ mọi cặp phần tử tại các chỉ số khác nhau là 7, 8, 9, 9, 10 và 11. Không có giá trị nào trong số này xuất hiện tại chỉ số còn lại, nên toàn bộ mảng là hợp lệ.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 1000$. Việc kiểm tra ba chỉ số trong từng mảng con sẽ thêm một thừa số bậc hai bên cạnh việc xét hai đầu mút, nên không thể hoàn thành trong thời gian cho phép.
>
> Khi xảy ra $a+b=c$, mọi đoạn dài hơn chứa đoạn đó cũng không hợp lệ, nên đầu trái chỉ di chuyển sang phải khi đầu phải di chuyển sang phải. Một $x$ mới được thêm vào chỉ làm hỏng cửa sổ khi nó bằng tổng của hai giá trị đã có bên trong, hoặc bằng hiệu của hai giá trị đó.
>
> Duy trì số lượng các tổng cặp và hiệu tuyệt đối trong cửa sổ. Nếu $x$ xuất hiện trong một trong hai số lượng này, xóa các phần tử từ bên trái và loại bỏ các cặp chứa chúng. Mỗi cặp được thêm một lần và xóa một lần.

<!-- thinking:end -->

Cửa sổ $\textit{nums}[l..r]$ hợp lệ chính xác khi không có ba chỉ số phân biệt nào có hai phần tử cộng lại bằng phần tử thứ ba. Một đoạn không hợp lệ vẫn không hợp lệ trong mọi đoạn dài hơn chứa nó, nên đầu trái chỉ tăng khi đầu phải tăng.

Đặt $m=\max(\textit{nums})$. $\textit{cntS}[s]$ là số cặp trong cửa sổ có tổng bằng $s$, còn $\textit{cntD}[d]$ là số cặp có hiệu tuyệt đối bằng $d$. Tổng không vượt quá $2m$ và hiệu không vượt quá $m$.

Giá trị ở đầu phải là $x$, còn cửa sổ trước khi thêm nó là $[l,r)$. Việc thêm $x$ làm cửa sổ không hợp lệ chính xác khi có một cặp đã có tổng bằng $x$, hoặc có một cặp đã có hiệu bằng $x$. Trường hợp đầu tiên nghĩa là $x$ là tổng. Trường hợp thứ hai nghĩa là $x$ là một số hạng và hai phần tử còn lại vẫn nằm trong cửa sổ. Vì mọi giá trị đều dương, chỉ cần dùng hiệu tuyệt đối.

Trong khi cửa sổ còn không hợp lệ, xóa phần tử bên trái $y$ và trừ các tổng, hiệu của $y$ với từng phần tử còn lại trong $[l,r)$. Sau khi cửa sổ hợp lệ trở lại, thêm các tổng và hiệu của $x$ với từng phần tử trong $[l,r)$, rồi cập nhật đáp án bằng $r-l+1$.

Mỗi cặp được thêm khi đầu phải muộn hơn đi vào cửa sổ và bị xóa khi đầu trái sớm hơn rời đi. Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(m)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSubarray(self, nums: List[int]) -> int:
        mx = max(nums)
        cnt_s = [0] * (mx << 1 | 1)
        cnt_d = [0] * (mx + 1)
        ans = l = 0

        for r, x in enumerate(nums):
            while cnt_s[x] > 0 or cnt_d[x] > 0:
                y = nums[l]
                l += 1
                for z in nums[l:r]:
                    cnt_s[y + z] -= 1
                    cnt_d[abs(y - z)] -= 1

            for y in nums[l:r]:
                cnt_s[x + y] += 1
                cnt_d[abs(x - y)] += 1

            ans = max(ans, r - l + 1)
        return ans
```

#### Java

```java
class Solution {
    public int maxSubarray(int[] nums) {
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }

        int[] cntS = new int[(mx << 1) | 1];
        int[] cntD = new int[mx + 1];
        int ans = 0;
        int l = 0;

        for (int r = 0; r < nums.length; r++) {
            int x = nums[r];
            while (cntS[x] > 0 || cntD[x] > 0) {
                int y = nums[l++];
                for (int i = l; i < r; i++) {
                    int z = nums[i];
                    cntS[y + z]--;
                    cntD[Math.abs(y - z)]--;
                }
            }

            for (int i = l; i < r; i++) {
                int y = nums[i];
                cntS[x + y]++;
                cntD[Math.abs(x - y)]++;
            }

            ans = Math.max(ans, r - l + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSubarray(vector<int>& nums) {
        int mx = ranges::max(nums);

        vector<int> cntS((mx << 1) | 1);
        vector<int> cntD(mx + 1);

        int ans = 0;
        int l = 0;

        for (int r = 0; r < nums.size(); r++) {
            int x = nums[r];

            while (cntS[x] > 0 || cntD[x] > 0) {
                int y = nums[l++];

                for (int i = l; i < r; i++) {
                    int z = nums[i];
                    cntS[y + z]--;
                    cntD[abs(y - z)]--;
                }
            }

            for (int i = l; i < r; i++) {
                int y = nums[i];
                cntS[x + y]++;
                cntD[abs(x - y)]++;
            }

            ans = max(ans, r - l + 1);
        }

        return ans;
    }
};
```

#### Go

```go
func maxSubarray(nums []int) int {
	mx := slices.Max(nums)

	cntS := make([]int, (mx<<1)|1)
	cntD := make([]int, mx+1)

	ans := 0
	l := 0

	for r, x := range nums {
		for cntS[x] > 0 || cntD[x] > 0 {
			y := nums[l]
			l++

			for i := l; i < r; i++ {
				z := nums[i]
				cntS[y+z]--

				d := y - z
				if d < 0 {
					d = -d
				}
				cntD[d]--
			}
		}

		for i := l; i < r; i++ {
			y := nums[i]
			cntS[x+y]++

			d := x - y
			if d < 0 {
				d = -d
			}
			cntD[d]++
		}

		ans = max(ans, r-l+1)
	}

	return ans
}
```

#### TypeScript

```ts
function maxSubarray(nums: number[]): number {
    let mx = 0;
    for (const x of nums) {
        mx = Math.max(mx, x);
    }

    const cntS = new Array((mx << 1) | 1).fill(0);
    const cntD = new Array(mx + 1).fill(0);

    let ans = 0;
    let l = 0;

    for (let r = 0; r < nums.length; r++) {
        const x = nums[r];

        while (cntS[x] > 0 || cntD[x] > 0) {
            const y = nums[l++];
            for (let i = l; i < r; i++) {
                const z = nums[i];
                cntS[y + z]--;
                cntD[Math.abs(y - z)]--;
            }
        }

        for (let i = l; i < r; i++) {
            const y = nums[i];
            cntS[x + y]++;
            cntD[Math.abs(x - y)]++;
        }

        ans = Math.max(ans, r - l + 1);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
