---
comments: true
difficulty: Hard
rating: 2076
source: Weekly Contest 316 Q4
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [2449. Minimum Number of Operations to Make Arrays Similar](https://leetcode.com/problems/minimum-number-of-operations-to-make-arrays-similar)

[中文文档](/solution/2400-2499/2449.Minimum%20Number%20of%20Operations%20to%20Make%20Arrays%20Similar/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên dương <code>nums</code> và <code>target</code> có cùng độ dài.</p>

<p>Trong một thao tác, bạn có thể chọn hai chỉ số <strong>khác nhau</strong> <code>i</code> và <code>j</code>, với <code>0 &lt;= i, j &lt; nums.length</code>, rồi:</p>

<ul>
	<li>gán <code>nums[i] = nums[i] + 2</code> và</li>
	<li>gán <code>nums[j] = nums[j] - 2</code>.</li>
</ul>

<p>Hai mảng được xem là <strong>tương tự</strong> nếu tần suất xuất hiện của mỗi phần tử là như nhau.</p>

<p>Hãy trả về <em>số thao tác ít nhất cần thực hiện để </em><code>nums</code><em> tương tự </em><code>target</code>. Các test được tạo sao cho <code>nums</code> luôn có thể trở nên tương tự <code>target</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [8,12,6], target = [2,14,10]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có thể biến nums thành tương tự target sau hai thao tác:
- Chọn i = 0 và j = 2, nums = [10,12,4].
- Chọn i = 1 và j = 2, nums = [10,14,2].
Có thể chứng minh rằng 2 là số thao tác ít nhất cần thực hiện.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,5], target = [4,1,3]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có thể biến nums thành tương tự target sau một thao tác:
- Chọn i = 1 và j = 2, nums = [1,4,3].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,1,1], target = [1,1,1,1,1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mảng nums đã tương tự target.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length == target.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], target[i] &lt;= 10<sup>6</sup></code></li>
	<li>Có thể biến <code>nums</code> thành tương tự <code>target</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân loại chẵn-lẻ + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Việc cộng hoặc trừ $2$ giữ nguyên tính chẵn lẻ, nên số lẻ chỉ ghép với số lẻ. Với $n\le 10^5$, ta sắp xếp cả hai mảng theo (tính chẵn lẻ, giá trị), tính tổng các chênh lệch tuyệt đối rồi chia cho $4$: một thao tác thay đổi hai vị trí, mỗi vị trí $2$ đơn vị.

<!-- thinking:end -->

Lưu ý rằng mỗi thao tác chỉ tăng hoặc giảm giá trị của một phần tử đi $2$, nên tính chẵn lẻ của phần tử sẽ không thay đổi.

Do đó, ta có thể chia hai mảng $nums$ và $target$ thành hai nhóm theo tính chẵn lẻ, lần lượt ký hiệu là $a_1$, $a_2$ và $b_1$, $b_2$.

Sau đó, ta chỉ cần ghép các phần tử trong $a_1$ với các phần tử trong $b_1$, các phần tử trong $a_2$ với các phần tử trong $b_2$, rồi thực hiện các thao tác. Trong quá trình ghép, ta có thể dùng chiến lược greedy: mỗi lần ghép các phần tử nhỏ hơn trong $a_i$ với các phần tử nhỏ hơn trong $b_i$, từ đó đảm bảo số thao tác là nhỏ nhất. Cách này có thể được triển khai trực tiếp bằng việc sắp xếp.

Vì mỗi thao tác có thể giảm chênh lệch của hai phần tử tương ứng đi $4$, ta cộng dồn chênh lệch tại mỗi vị trí tương ứng, rồi chia kết quả cho $4$ để nhận được đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeSimilar(self, nums: List[int], target: List[int]) -> int:
        nums.sort(key=lambda x: (x & 1, x))
        target.sort(key=lambda x: (x & 1, x))
        return sum(abs(a - b) for a, b in zip(nums, target)) // 4
```

#### Java

```java
class Solution {
    public long makeSimilar(int[] nums, int[] target) {
        Arrays.sort(nums);
        Arrays.sort(target);
        List<Integer> a1 = new ArrayList<>();
        List<Integer> a2 = new ArrayList<>();
        List<Integer> b1 = new ArrayList<>();
        List<Integer> b2 = new ArrayList<>();
        for (int v : nums) {
            if (v % 2 == 0) {
                a1.add(v);
            } else {
                a2.add(v);
            }
        }
        for (int v : target) {
            if (v % 2 == 0) {
                b1.add(v);
            } else {
                b2.add(v);
            }
        }
        long ans = 0;
        for (int i = 0; i < a1.size(); ++i) {
            ans += Math.abs(a1.get(i) - b1.get(i));
        }
        for (int i = 0; i < a2.size(); ++i) {
            ans += Math.abs(a2.get(i) - b2.get(i));
        }
        return ans / 4;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long makeSimilar(vector<int>& nums, vector<int>& target) {
        sort(nums.begin(), nums.end());
        sort(target.begin(), target.end());
        vector<int> a1;
        vector<int> a2;
        vector<int> b1;
        vector<int> b2;
        for (int v : nums) {
            if (v & 1)
                a1.emplace_back(v);
            else
                a2.emplace_back(v);
        }
        for (int v : target) {
            if (v & 1)
                b1.emplace_back(v);
            else
                b2.emplace_back(v);
        }
        long long ans = 0;
        for (int i = 0; i < a1.size(); ++i) ans += abs(a1[i] - b1[i]);
        for (int i = 0; i < a2.size(); ++i) ans += abs(a2[i] - b2[i]);
        return ans / 4;
    }
};
```

#### Go

```go
func makeSimilar(nums []int, target []int) int64 {
	sort.Ints(nums)
	sort.Ints(target)
	a1, a2, b1, b2 := []int{}, []int{}, []int{}, []int{}
	for _, v := range nums {
		if v%2 == 0 {
			a1 = append(a1, v)
		} else {
			a2 = append(a2, v)
		}
	}
	for _, v := range target {
		if v%2 == 0 {
			b1 = append(b1, v)
		} else {
			b2 = append(b2, v)
		}
	}
	ans := 0
	for i := 0; i < len(a1); i++ {
		ans += abs(a1[i] - b1[i])
	}
	for i := 0; i < len(a2); i++ {
		ans += abs(a2[i] - b2[i])
	}
	return int64(ans / 4)
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
function makeSimilar(nums: number[], target: number[]): number {
    nums.sort((a, b) => a - b);
    target.sort((a, b) => a - b);

    const a1: number[] = [];
    const a2: number[] = [];
    const b1: number[] = [];
    const b2: number[] = [];

    for (const v of nums) {
        if (v % 2 === 0) {
            a1.push(v);
        } else {
            a2.push(v);
        }
    }

    for (const v of target) {
        if (v % 2 === 0) {
            b1.push(v);
        } else {
            b2.push(v);
        }
    }

    let ans = 0;
    for (let i = 0; i < a1.length; ++i) {
        ans += Math.abs(a1[i] - b1[i]);
    }

    for (let i = 0; i < a2.length; ++i) {
        ans += Math.abs(a2[i] - b2[i]);
    }

    return ans / 4;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
