---
comments: true
difficulty: Hard
rating: 2735
source: Weekly Contest 393 Q4
tags:
    - Bit Manipulation
    - Segment Tree
    - Queue
    - Array
    - Binary Search
    - Dynamic Programming
---

<!-- problem:start -->

# [3117. Minimum Sum of Values by Dividing Array](https://leetcode.com/problems/minimum-sum-of-values-by-dividing-array)

[中文文档](/solution/3100-3199/3117.Minimum%20Sum%20of%20Values%20by%20Dividing%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng <code>nums</code> và <code>andValues</code>, lần lượt có độ dài <code>n</code> và <code>m</code>.</p>

<p><strong>Giá trị</strong> của một mảng bằng phần tử <strong>cuối cùng</strong> của mảng đó.</p>

<p>Bạn cần chia <code>nums</code> thành <code>m</code> <strong>mảng con liên tiếp, không giao nhau</strong> <span data-keyword="subarray-nonempty">subarrays</span> sao cho với mảng con thứ <code>i<sup>th</sup></code> <code>[l<sub>i</sub>, r<sub>i</sub>]</code>, phép <code>AND</code> bitwise của các phần tử trong mảng con bằng <code>andValues[i]</code>; nói cách khác, <code>nums[l<sub>i</sub>] &amp; nums[l<sub>i</sub> + 1] &amp; ... &amp; nums[r<sub>i</sub>] == andValues[i]</code> với mọi <code>1 &lt;= i &lt;= m</code>, trong đó <code>&amp;</code> là toán tử <code>AND</code> bitwise.</p>

<p>Hãy trả về <em>tổng <strong>nhỏ nhất</strong> có thể của các <strong>giá trị</strong> của </em><code>m</code><em> mảng con mà </em><code>nums</code><em> được chia thành</em>. <em>Nếu không thể chia </em><code>nums</code><em> thành </em><code>m</code><em> mảng con thỏa mãn các điều kiện trên, trả về </em><code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,3,3,2], andValues = [0,3,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách duy nhất để chia <code>nums</code> là:</p>

<ol>
	<li><code>[1,4]</code> vì <code>1 &amp; 4 == 0</code>.</li>
	<li><code>[3]</code> vì phép <code>AND</code> bitwise của một mảng con chỉ có một phần tử chính là phần tử đó.</li>
	<li><code>[3]</code> vì phép <code>AND</code> bitwise của một mảng con chỉ có một phần tử chính là phần tử đó.</li>
	<li><code>[2]</code> vì phép <code>AND</code> bitwise của một mảng con chỉ có một phần tử chính là phần tử đó.</li>
</ol>

<p>Tổng các giá trị của những mảng con này là <code>4 + 3 + 3 + 2 = 12</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,5,7,7,7,5], andValues = [0,7,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">17</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có ba cách để chia <code>nums</code>:</p>

<ol>
	<li><code>[[2,3,5],[7,7,7],[5]]</code> với tổng các giá trị là <code>5 + 7 + 5 == 17</code>.</li>
	<li><code>[[2,3,5,7],[7,7],[5]]</code> với tổng các giá trị là <code>7 + 7 + 5 == 19</code>.</li>
	<li><code>[[2,3,5,7,7],[7],[5]]</code> với tổng các giá trị là <code>7 + 7 + 5 == 19</code>.</li>
</ol>

<p>Tổng nhỏ nhất có thể của các giá trị là <code>17</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4], andValues = [2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Phép <code>AND</code> bitwise của toàn bộ mảng <code>nums</code> là <code>0</code>. Vì không có cách nào chia <code>nums</code> thành một mảng con duy nhất có phép <code>AND</code> bitwise của các phần tử bằng <code>2</code>, ta trả về <code>-1</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= m == andValues.length &lt;= min(n, 10)</code></li>
	<li><code>1 &lt;= nums[i] &lt; 10<sup>5</sup></code></li>
	<li><code>0 &lt;= andValues[j] &lt; 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mảng phải được chia thành $m$ đoạn sao cho phép AND của mỗi đoạn bằng $\textit{andValues}[j]$, đồng thời tổng các phần tử cuối đoạn là nhỏ nhất. Việc liệt kê mọi vị trí cắt sẽ có số trường hợp theo cấp số mũ.
>
> Phép AND chỉ làm mất các bit, nên có thể dừng xét một đoạn ngay khi kết quả nhỏ hơn target. Trạng thái (chỉ số, số đoạn đã hoàn thành, kết quả AND hiện tại) là duy nhất, và số giá trị AND khác nhau là $O(\log M)$.
>
> Ta ghi nhớ kết quả của $dfs(i,j,a)$: trước hết thực hiện AND trên $nums[i]$, sau đó hoặc mở rộng đoạn hiện tại, hoặc khi kết quả AND bằng target thì cắt đoạn và cộng $nums[i]$. Trả về vô cùng khi số phần tử còn lại không đủ hoặc kết quả AND nhỏ hơn target.

<!-- thinking:end -->

Ta xây dựng hàm $dfs(i, j, a)$, biểu diễn tổng nhỏ nhất có thể của các giá trị mảng con khi bắt đầu từ phần tử thứ $i$, đã chia được $j$ mảng con, và kết quả phép AND bitwise của mảng con hiện tại cần chia là $a$. Đáp án là $dfs(0, 0, -1)$.

Quá trình thực thi hàm $dfs(i, j, a)$ như sau:

- Nếu $n - i < m - j$, nghĩa là số phần tử còn lại không đủ để chia thành $m - j$ mảng con, trả về $+\infty$.
- Nếu $j = m$, nghĩa là đã chia thành $m$ mảng con. Khi đó, kiểm tra điều kiện $i = n$. Nếu đúng, trả về $0$, ngược lại trả về $+\infty$.
- Nếu không, ta thực hiện phép AND bitwise giữa $a$ và $nums[i]$ để nhận được $a$ mới. Nếu $a < andValues[j]$, nghĩa là kết quả phép AND bitwise của mảng con hiện tại cần chia không thỏa mãn yêu cầu, trả về $+\infty$. Nếu không, ta có hai lựa chọn:
    - Không chia sau phần tử hiện tại, tức là $dfs(i + 1, j, a)$.
    - Chia sau phần tử hiện tại, tức là $dfs(i + 1, j + 1, -1) + nums[i]$.
- Trả về giá trị nhỏ hơn trong hai lựa chọn trên.

Để tránh tính toán lặp lại, ta sử dụng phương pháp tìm kiếm có ghi nhớ và lưu kết quả của $dfs(i, j, a)$ trong một hash table.

Độ phức tạp thời gian là $O(n \times m \times \log M)$, và độ phức tạp không gian là $O(n \times m \times \log M)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các mảng $nums$ và $andValues$; còn $M$ là giá trị lớn nhất trong mảng $nums$, trong bài này $M \leq 10^5$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumValueSum(self, nums: List[int], andValues: List[int]) -> int:
        @cache
        def dfs(i: int, j: int, a: int) -> int:
            if n - i < m - j:
                return inf
            if j == m:
                return 0 if i == n else inf
            a &= nums[i]
            if a < andValues[j]:
                return inf
            ans = dfs(i + 1, j, a)
            if a == andValues[j]:
                ans = min(ans, dfs(i + 1, j + 1, -1) + nums[i])
            return ans

        n, m = len(nums), len(andValues)
        ans = dfs(0, 0, -1)
        return ans if ans < inf else -1
```

#### Java

```java
class Solution {
    private int[] nums;
    private int[] andValues;
    private final int inf = 1 << 29;
    private Map<Long, Integer> f = new HashMap<>();

    public int minimumValueSum(int[] nums, int[] andValues) {
        this.nums = nums;
        this.andValues = andValues;
        int ans = dfs(0, 0, -1);
        return ans >= inf ? -1 : ans;
    }

    private int dfs(int i, int j, int a) {
        if (nums.length - i < andValues.length - j) {
            return inf;
        }
        if (j == andValues.length) {
            return i == nums.length ? 0 : inf;
        }
        a &= nums[i];
        if (a < andValues[j]) {
            return inf;
        }
        long key = (long) i << 36 | (long) j << 32 | a;
        if (f.containsKey(key)) {
            return f.get(key);
        }

        int ans = dfs(i + 1, j, a);
        if (a == andValues[j]) {
            ans = Math.min(ans, dfs(i + 1, j + 1, -1) + nums[i]);
        }
        f.put(key, ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumValueSum(vector<int>& nums, vector<int>& andValues) {
        this->nums = nums;
        this->andValues = andValues;
        n = nums.size();
        m = andValues.size();
        int ans = dfs(0, 0, -1);
        return ans >= inf ? -1 : ans;
    }

private:
    vector<int> nums;
    vector<int> andValues;
    int n;
    int m;
    const int inf = 1 << 29;
    unordered_map<long long, int> f;

    int dfs(int i, int j, int a) {
        if (n - i < m - j) {
            return inf;
        }
        if (j == m) {
            return i == n ? 0 : inf;
        }
        a &= nums[i];
        if (a < andValues[j]) {
            return inf;
        }
        long long key = (long long) i << 36 | (long long) j << 32 | a;
        if (f.contains(key)) {
            return f[key];
        }
        int ans = dfs(i + 1, j, a);
        if (a == andValues[j]) {
            ans = min(ans, dfs(i + 1, j + 1, -1) + nums[i]);
        }
        return f[key] = ans;
    }
};
```

#### Go

```go
func minimumValueSum(nums []int, andValues []int) int {
	n, m := len(nums), len(andValues)
	f := map[int]int{}
	const inf int = 1 << 29
	var dfs func(i, j, a int) int
	dfs = func(i, j, a int) int {
		if n-i < m-j {
			return inf
		}
		if j == m {
			if i == n {
				return 0
			}
			return inf
		}
		a &= nums[i]
		if a < andValues[j] {
			return inf
		}
		key := i<<36 | j<<32 | a
		if v, ok := f[key]; ok {
			return v
		}
		ans := dfs(i+1, j, a)
		if a == andValues[j] {
			ans = min(ans, dfs(i+1, j+1, -1)+nums[i])
		}
		f[key] = ans
		return ans
	}
	if ans := dfs(0, 0, -1); ans < inf {
		return ans
	}
	return -1
}
```

#### TypeScript

```ts
function minimumValueSum(nums: number[], andValues: number[]): number {
    const [n, m] = [nums.length, andValues.length];
    const f: Map<bigint, number> = new Map();
    const dfs = (i: number, j: number, a: number): number => {
        if (n - i < m - j) {
            return Infinity;
        }
        if (j === m) {
            return i === n ? 0 : Infinity;
        }
        a &= nums[i];
        if (a < andValues[j]) {
            return Infinity;
        }
        const key = (BigInt(i) << 36n) | (BigInt(j) << 32n) | BigInt(a);
        if (f.has(key)) {
            return f.get(key)!;
        }
        let ans = dfs(i + 1, j, a);
        if (a === andValues[j]) {
            ans = Math.min(ans, dfs(i + 1, j + 1, -1) + nums[i]);
        }
        f.set(key, ans);
        return ans;
    };
    const ans = dfs(0, 0, -1);
    return ans >= Infinity ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
