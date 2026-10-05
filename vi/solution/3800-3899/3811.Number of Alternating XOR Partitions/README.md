---
comments: true
difficulty: Medium
rating: 2005
source: Biweekly Contest 174 Q3
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [3811. Number of Alternating XOR Partitions](https://leetcode.com/problems/number-of-alternating-xor-partitions)

[中文文档](/solution/3800-3899/3811.Number%20of%20Alternating%20XOR%20Partitions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và hai số nguyên <strong>phân biệt</strong> <code>target1</code> và <code>target2</code>.</p>

<p>Một <strong>phân hoạch</strong> của <code>nums</code> chia nó thành một hoặc nhiều <strong>đoạn liên tiếp, không rỗng</strong>, bao phủ toàn bộ mảng và không chồng lấn.</p>

<p>Một phân hoạch được gọi là <strong>hợp lệ</strong> nếu <strong>XOR bitwise</strong> của các phần tử trong các đoạn <strong>luân phiên</strong> giữa <code>target1</code> và <code>target2</code>, bắt đầu bằng <code>target1</code>.</p>

<p>Cụ thể, với các đoạn <code>b1</code>, <code>b2</code>, &hellip;:</p>

<ul>
	<li><code>XOR(b1) = target1</code></li>
	<li><code>XOR(b2) = target2</code> (nếu tồn tại)</li>
	<li><code>XOR(b3) = target1</code>, và tiếp tục như vậy.</li>
</ul>

<p>Hãy trả về số lượng phân hoạch hợp lệ của <code>nums</code>, lấy modulo <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Lưu ý:</strong> Một đoạn duy nhất là hợp lệ nếu <strong>XOR</strong> của nó bằng <code>target1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,1,4], target1 = 1, target2 = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>XOR của <code>[2, 3]</code> là 1, khớp với <code>target1</code>.</li>
	<li>XOR của đoạn còn lại <code>[1, 4]</code> là 5, khớp với <code>target2</code>.</li>
	<li>Đây là phân hoạch luân phiên hợp lệ duy nhất, nên đáp án là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,0], target1 = 1, target2 = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><strong>​​​​​​​</strong>XOR của <code>[1, 0, 0]</code> là 1, khớp với <code>target1</code>.</li>
	<li>XOR của <code>[1]</code> và <code>[0, 0]</code> lần lượt là 1 và 0, khớp với <code>target1</code> và <code>target2</code>.</li>
	<li>XOR của <code>[1, 0]</code> và <code>[0]</code> lần lượt là 1 và 0, khớp với <code>target1</code> và <code>target2</code>.</li>
	<li>Vì vậy, đáp án là 3.​​​​​​​</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7], target1 = 1, target2 = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>XOR của <code>[7]</code> là 7, không khớp với <code>target1</code>, nên không tồn tại phân hoạch hợp lệ.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i], target1, target2 &lt;= 10<sup>5</sup></code></li>
	<li><code>target1 != target2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Truy hồi

<!-- thinking:start -->

> **Tư duy**
>
> XOR của các đoạn trong một phân hoạch hợp lệ phải luân phiên giữa $\textit{target1}$ và $\textit{target2}$, bắt đầu bằng $\textit{target1}$. Vì $n \le 10^5$, ta không thể liệt kê các vị trí cắt.
>
> Với XOR tiền tố $pre$, XOR của một đoạn $[l,r]$ là $pre_r \oplus pre_{l-1}$. Điều kiện luân phiên trở thành việc đếm trên các tiền tố trước đó.
>
> Gọi $\textit{cnt1}[x]$ và $\textit{cnt2}[x]$ lần lượt là số cách kết thúc bằng $\textit{target1}$ hoặc $\textit{target2}$ tại tiền tố có XOR bằng $x$. Tiền tố rỗng có $\textit{cnt2}[0]=1$.
>
> Sau mỗi giá trị, ta cập nhật $pre$, đọc bảng đối diện để lấy các cách có thể nối thêm, rồi ghi kết quả trở lại. Một lần duyệt tuyến tính sẽ cho số cách kết thúc tại chỉ số hiện tại.

<!-- thinking:end -->

Ta định nghĩa hai bảng băm $\textit{cnt1}$ và $\textit{cnt2}$, trong đó $\textit{cnt1}[x]$ biểu diễn số cách phân hoạch có kết quả XOR bitwise là $x$ và phân hoạch kết thúc bằng $\textit{target1}$, còn $\textit{cnt2}[x]$ biểu diễn số cách phân hoạch có kết quả XOR bitwise là $x$ và phân hoạch kết thúc bằng $\textit{target2}$. Ban đầu, $\textit{cnt2}[0] = 1$, biểu diễn một phân hoạch rỗng.

Ta dùng biến $\textit{pre}$ để lưu kết quả XOR bitwise của tiền tố hiện tại, và biến $\textit{ans}$ để lưu đáp án cuối cùng. Sau đó, ta duyệt mảng $\textit{nums}$. Với mỗi phần tử $x$, ta cập nhật $\textit{pre}$ và tính:

$$
a = \textit{cnt2}[\textit{pre} \oplus \textit{target1}]
$$

$$
b = \textit{cnt1}[\textit{pre} \oplus \textit{target2}]
$$

Tiếp theo, ta cập nhật đáp án:

$$
\textit{ans} = (a + b) \mod (10^9 + 7)
$$

Sau đó, ta cập nhật các bảng băm:

$$
\textit{cnt1}[\textit{pre}] = (\textit{cnt1}[\textit{pre}] + a) \mod (10^9 + 7)
$$

$$
\textit{cnt2}[\textit{pre}] = (\textit{cnt2}[\textit{pre}] + b) \mod (10^9 + 7)
$$

Cuối cùng, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def alternatingXOR(self, nums: List[int], target1: int, target2: int) -> int:
        cnt1 = defaultdict(int)
        cnt2 = defaultdict(int)
        cnt2[0] = 1
        ans = pre = 0
        mod = 10**9 + 7
        for x in nums:
            pre ^= x
            a = cnt2[pre ^ target1]
            b = cnt1[pre ^ target2]
            ans = (a + b) % mod
            cnt1[pre] = (cnt1[pre] + a) % mod
            cnt2[pre] = (cnt2[pre] + b) % mod
        return ans
```

#### Java

```java
class Solution {
    public int alternatingXOR(int[] nums, int target1, int target2) {
        final int mod = (int) 1e9 + 7;

        Map<Integer, Integer> cnt1 = new HashMap<>();
        Map<Integer, Integer> cnt2 = new HashMap<>();
        cnt2.put(0, 1);

        int ans = 0;
        int pre = 0;
        for (int x : nums) {
            pre ^= x;
            int a = cnt2.getOrDefault(pre ^ target1, 0);
            int b = cnt1.getOrDefault(pre ^ target2, 0);
            ans = (a + b) % mod;
            cnt1.put(pre, (cnt1.getOrDefault(pre, 0) + a) % mod);
            cnt2.put(pre, (cnt2.getOrDefault(pre, 0) + b) % mod);
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int alternatingXOR(vector<int>& nums, int target1, int target2) {
        const int MOD = 1e9 + 7;
        unordered_map<int, int> cnt1, cnt2;
        cnt2[0] = 1;

        int pre = 0, ans = 0;
        for (int x : nums) {
            pre ^= x;
            int a = cnt2[pre ^ target1];
            int b = cnt1[pre ^ target2];
            ans = (a + b) % MOD;
            cnt1[pre] = (cnt1[pre] + a) % MOD;
            cnt2[pre] = (cnt2[pre] + b) % MOD;
        }

        return ans;
    }
};
```

#### Go

```go
func alternatingXOR(nums []int, target1 int, target2 int) int {
	mod := 1_000_000_007
	cnt1 := make(map[int]int)
	cnt2 := make(map[int]int)
	cnt2[0] = 1

	pre := 0
	ans := 0

	for _, x := range nums {
		pre ^= x
		a := cnt2[pre^target1]
		b := cnt1[pre^target2]
		ans = (a + b) % mod
		cnt1[pre] = (cnt1[pre] + a) % mod
		cnt2[pre] = (cnt2[pre] + b) % mod
	}

	return ans
}
```

#### TypeScript

```ts
function alternatingXOR(nums: number[], target1: number, target2: number): number {
    const MOD = 1_000_000_007;
    const cnt1 = new Map<number, number>();
    const cnt2 = new Map<number, number>();
    cnt2.set(0, 1);

    let pre = 0;
    let ans = 0;

    for (const x of nums) {
        pre ^= x;
        const a = cnt2.get(pre ^ target1) ?? 0;
        const b = cnt1.get(pre ^ target2) ?? 0;
        ans = (a + b) % MOD;
        cnt1.set(pre, ((cnt1.get(pre) ?? 0) + a) % MOD);
        cnt2.set(pre, ((cnt2.get(pre) ?? 0) + b) % MOD);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
