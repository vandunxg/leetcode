---
comments: true
difficulty: Medium
rating: 1548
source: Biweekly Contest 159 Q1
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [3587. Minimum Adjacent Swaps to Alternate Parity](https://leetcode.com/problems/minimum-adjacent-swaps-to-alternate-parity)

[中文文档](/solution/3500-3599/3587.Minimum%20Adjacent%20Swaps%20to%20Alternate%20Parity/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> gồm các số nguyên <strong>phân biệt</strong>.</p>

<p>Trong một thao tác, bạn có thể hoán đổi hai phần tử <strong>liền kề</strong> bất kỳ trong mảng.</p>

<p>Một cách sắp xếp mảng được xem là <strong>hợp lệ</strong> nếu tính chẵn lẻ của các phần tử liền kề <strong>xen kẽ</strong>, nghĩa là mỗi cặp phần tử kề nhau gồm một số chẵn và một số lẻ.</p>

<p>Trả về số lần hoán đổi phần tử liền kề <strong>nhỏ nhất</strong> cần thực hiện để biến đổi <code>nums</code> thành một cách sắp xếp hợp lệ bất kỳ.</p>

<p>Nếu không thể sắp xếp lại <code>nums</code> để không có hai phần tử liền kề nào có cùng tính chẵn lẻ, hãy trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4,6,5,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hoán đổi 5 và 6, mảng trở thành <code>[2,4,5,6,7]</code></p>

<p>Hoán đổi 5 và 4, mảng trở thành <code>[2,5,4,6,7]</code></p>

<p>Hoán đổi 6 và 7, mảng trở thành <code>[2,5,4,7,6]</code>. Mảng hiện đã được sắp xếp hợp lệ. Do đó, đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4,5,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hoán đổi 4 và 5, mảng trở thành <code>[2,5,4,7]</code>, đây là một cách sắp xếp hợp lệ. Do đó, đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng đã được sắp xếp hợp lệ. Do đó, không cần thực hiện thao tác nào.</p>
</div>

<p><strong class="example">Ví dụ 4:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,5,6,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể tạo ra cách sắp xếp hợp lệ. Do đó, đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li>Tất cả phần tử trong <code>nums</code> đều <strong>phân biệt</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Một hoán vị có tính chẵn lẻ xen kẽ chỉ có nhiều nhất hai dạng: các chỉ số chẵn chứa số chẵn, hoặc các chỉ số chẵn chứa số lẻ. Nếu số lượng hai loại chênh lệch quá $1$, thì không thể thực hiện.
>
> Lưu các chỉ số hiện tại của mỗi loại và ghép chúng với các vị trí trong mẫu $0,2,4,\ldots$. Số lần hoán đổi phần tử liền kề bằng tổng các khoảng cách chỉ số. Khi số lượng hai loại bằng nhau, thử cả hai mẫu và chọn tổng nhỏ hơn.

<!-- thinking:end -->

Trong một cách sắp xếp hợp lệ, số lượng số lẻ và số chẵn chỉ có thể chênh lệch 1 hoặc bằng nhau. Vì vậy, nếu độ chênh lệch giữa số lượng số lẻ và số chẵn lớn hơn 1, thì không thể tạo ra cách sắp xếp hợp lệ và ta trả về -1 ngay lập tức.

Ta dùng một mảng $\text{pos}$ để lưu chỉ số của các số lẻ và số chẵn, trong đó $\text{pos}[0]$ lưu chỉ số của các số chẵn và $\text{pos}[1]$ lưu chỉ số của các số lẻ.

Nếu số lượng số lẻ và số chẵn bằng nhau, có hai cách sắp xếp hợp lệ: số lẻ đứng trước số chẵn, hoặc số chẵn đứng trước số lẻ. Ta có thể tính số lần hoán đổi cần thiết cho cả hai cách và lấy giá trị nhỏ hơn.

Nếu số lượng số lẻ lớn hơn số lượng số chẵn, chỉ có một cách sắp xếp hợp lệ, đó là số lẻ đứng trước số chẵn. Trong trường hợp này, ta chỉ cần tính số lần hoán đổi cho cách sắp xếp đó.

Do đó, ta định nghĩa hàm $\text{calc}(k)$, trong đó $k$ biểu thị tính chẵn lẻ của phần tử đầu tiên (0 là chẵn, 1 là lẻ). Hàm này tính số lần hoán đổi cần thiết để biến cách sắp xếp hiện tại thành một cách sắp xếp hợp lệ bắt đầu bằng $k$. Ta chỉ cần duyệt qua các chỉ số trong $\text{pos}[k]$ và cộng chênh lệch giữa mỗi chỉ số với vị trí tương ứng trong cách sắp xếp hợp lệ.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\text{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSwaps(self, nums: List[int]) -> int:
        def calc(k: int) -> int:
            return sum(abs(i - j) for i, j in zip(range(0, len(nums), 2), pos[k]))

        pos = [[], []]
        for i, x in enumerate(nums):
            pos[x & 1].append(i)
        if abs(len(pos[0]) - len(pos[1])) > 1:
            return -1
        if len(pos[0]) > len(pos[1]):
            return calc(0)
        if len(pos[0]) < len(pos[1]):
            return calc(1)
        return min(calc(0), calc(1))
```

#### Java

```java
class Solution {
    private List<Integer>[] pos = new List[2];
    private int[] nums;

    public int minSwaps(int[] nums) {
        this.nums = nums;
        Arrays.setAll(pos, k -> new ArrayList<>());
        for (int i = 0; i < nums.length; ++i) {
            pos[nums[i] & 1].add(i);
        }
        if (Math.abs(pos[0].size() - pos[1].size()) > 1) {
            return -1;
        }
        if (pos[0].size() > pos[1].size()) {
            return calc(0);
        }
        if (pos[0].size() < pos[1].size()) {
            return calc(1);
        }
        return Math.min(calc(0), calc(1));
    }

    private int calc(int k) {
        int res = 0;
        for (int i = 0; i < nums.length; i += 2) {
            res += Math.abs(pos[k].get(i / 2) - i);
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSwaps(vector<int>& nums) {
        vector<int> pos[2];
        for (int i = 0; i < nums.size(); ++i) {
            pos[nums[i] & 1].push_back(i);
        }
        if (abs(int(pos[0].size() - pos[1].size())) > 1) {
            return -1;
        }
        auto calc = [&](int k) {
            int res = 0;
            for (int i = 0; i < nums.size(); i += 2) {
                res += abs(pos[k][i / 2] - i);
            }
            return res;
        };
        if (pos[0].size() > pos[1].size()) {
            return calc(0);
        }
        if (pos[0].size() < pos[1].size()) {
            return calc(1);
        }
        return min(calc(0), calc(1));
    }
};
```

#### Go

```go
func minSwaps(nums []int) int {
	pos := [2][]int{}
	for i, x := range nums {
		pos[x&1] = append(pos[x&1], i)
	}
	if abs(len(pos[0])-len(pos[1])) > 1 {
		return -1
	}
	calc := func(k int) int {
		res := 0
		for i := 0; i < len(nums); i += 2 {
			res += abs(pos[k][i/2] - i)
		}
		return res
	}
	if len(pos[0]) > len(pos[1]) {
		return calc(0)
	}
	if len(pos[0]) < len(pos[1]) {
		return calc(1)
	}
	return min(calc(0), calc(1))
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minSwaps(nums: number[]): number {
    const pos: number[][] = [[], []];
    for (let i = 0; i < nums.length; ++i) {
        pos[nums[i] & 1].push(i);
    }
    if (Math.abs(pos[0].length - pos[1].length) > 1) {
        return -1;
    }
    const calc = (k: number): number => {
        let res = 0;
        for (let i = 0; i < nums.length; i += 2) {
            res += Math.abs(pos[k][i >> 1] - i);
        }
        return res;
    };
    if (pos[0].length > pos[1].length) {
        return calc(0);
    }
    if (pos[0].length < pos[1].length) {
        return calc(1);
    }
    return Math.min(calc(0), calc(1));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
