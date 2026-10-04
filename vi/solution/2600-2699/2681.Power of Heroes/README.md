---
comments: true
difficulty: Hard
rating: 2060
source: Biweekly Contest 104 Q4
tags:
    - Array
    - Math
    - Dynamic Programming
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [2681. Power of Heroes](https://leetcode.com/problems/power-of-heroes)

[中文文档](/solution/2600-2699/2681.Power%20of%20Heroes/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, biểu diễn sức mạnh của một số hero. <b>Sức mạnh</b> của một nhóm hero được định nghĩa như sau:</p>

<ul>
	<li>Gọi <code>i<sub>0</sub></code>, <code>i<sub>1</sub></code>, ... ,<code>i<sub>k</sub></code> là các chỉ số của những hero trong một nhóm. Khi đó, sức mạnh của nhóm này là <code>max(nums[i<sub>0</sub>], nums[i<sub>1</sub>], ... ,nums[i<sub>k</sub>])<sup>2</sup> * min(nums[i<sub>0</sub>], nums[i<sub>1</sub>], ... ,nums[i<sub>k</sub>])</code>.</li>
</ul>

<p>Trả về <em>tổng <strong>sức mạnh</strong> của tất cả các nhóm <strong>không rỗng</strong> có thể tạo thành.</em> Vì tổng có thể rất lớn, hãy trả về <strong>phần dư</strong> khi chia cho <code>10<sup>9 </sup>+ 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,4]
<strong>Đầu ra:</strong> 141
<strong>Giải thích:</strong>
Nhóm thứ <sup>1</sup>&nbsp;: [2] có sức mạnh = 2<sup>2</sup>&nbsp;* 2 = 8.
Nhóm thứ <sup>2</sup>&nbsp;: [1] có sức mạnh = 1<sup>2</sup> * 1 = 1.
Nhóm thứ <sup>3</sup>&nbsp;: [4] có sức mạnh = 4<sup>2</sup> * 4 = 64.
Nhóm thứ <sup>4</sup>&nbsp;: [2,1] có sức mạnh = 2<sup>2</sup> * 1 = 4.
Nhóm thứ <sup>5</sup>&nbsp;: [2,4] có sức mạnh = 4<sup>2</sup> * 2 = 32.
Nhóm thứ <sup>6</sup>&nbsp;: [1,4] có sức mạnh = 4<sup>2</sup> * 1 = 16.
​​​​​​​Nhóm thứ <sup>7</sup>&nbsp;: [2,1,4] có sức mạnh = 4<sup>2</sup>​​​​​​​ * 1 = 16.
Tổng sức mạnh của tất cả các nhóm là 8 + 1 + 64 + 4 + 32 + 16 + 16 = 141.

</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Có tổng cộng 7 nhóm có thể tạo thành và sức mạnh của mỗi nhóm đều là 1. Do đó, tổng sức mạnh của tất cả các nhóm là 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi dãy con đóng góp $\max^2 \cdot \min$. Thứ tự không quan trọng; sau khi sắp xếp, mỗi giá trị nhỏ nhất $a_i$ tương ứng với các giá trị lớn nhất ở bên phải cùng các hệ số là lũy thừa của hai. Liệt kê $2^n$ dãy con là không thể với $n \le 10^5$.
>
> Một tổng bình phương có trọng số $p$ được duy trì từ phải sang trái cho phép $x$ cộng $x^3$ và $x \cdot p$, sau đó cập nhật $p \leftarrow 2p+x^2$, nhờ đó tính được đáp án trong thời gian tuyến tính.

<!-- thinking:end -->

Ta nhận thấy bài toán liên quan đến giá trị lớn nhất và nhỏ nhất của một dãy con, còn thứ tự của các phần tử trong mảng không ảnh hưởng đến kết quả cuối cùng. Vì vậy, trước tiên ta có thể sắp xếp mảng.

Tiếp theo, ta xét từng phần tử là giá trị nhỏ nhất của dãy con. Gọi các phần tử của mảng là $a_1, a_2, \cdots, a_n$. Đóng góp của các dãy con có $a_i$ là giá trị nhỏ nhất là:

$$
a_i \times (a_{i}^{2} + a_{i+1}^2 + 2 \times a_{i+2}^2 + 4 \times a_{i+3}^2 + \cdots + 2^{n-i-1} \times a_n^2)
$$

Ta nhận thấy mỗi $a_i$ sẽ được nhân với $a_i^2$, nên có thể cộng trực tiếp vào đáp án. Với phần còn lại, ta duy trì bằng một biến $p$, ban đầu bằng $0$.

Sau đó, ta duyệt $a_i$ từ phải sang trái. Mỗi lần, ta cộng $a_i \times p$ vào đáp án, rồi đặt $p = p \times 2 + a_i^2$.

Sau khi duyệt qua tất cả $a_i$, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfPower(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        nums.sort()
        ans = 0
        p = 0
        for x in nums[::-1]:
            ans = (ans + (x * x % mod) * x) % mod
            ans = (ans + x * p) % mod
            p = (p * 2 + x * x) % mod
        return ans
```

#### Java

```java
class Solution {
    public int sumOfPower(int[] nums) {
        final int mod = (int) 1e9 + 7;
        Arrays.sort(nums);
        long ans = 0, p = 0;
        for (int i = nums.length - 1; i >= 0; --i) {
            long x = nums[i];
            ans = (ans + (x * x % mod) * x) % mod;
            ans = (ans + x * p % mod) % mod;
            p = (p * 2 + x * x % mod) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumOfPower(vector<int>& nums) {
        const int mod = 1e9 + 7;
        sort(nums.rbegin(), nums.rend());
        long long ans = 0, p = 0;
        for (long long x : nums) {
            ans = (ans + (x * x % mod) * x) % mod;
            ans = (ans + x * p % mod) % mod;
            p = (p * 2 + x * x % mod) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func sumOfPower(nums []int) (ans int) {
	const mod = 1e9 + 7
	sort.Ints(nums)
	p := 0
	for i := len(nums) - 1; i >= 0; i-- {
		x := nums[i]
		ans = (ans + (x*x%mod)*x) % mod
		ans = (ans + x*p%mod) % mod
		p = (p*2 + x*x%mod) % mod
	}
	return
}
```

#### TypeScript

```ts
function sumOfPower(nums: number[]): number {
    const mod = 10 ** 9 + 7;
    nums.sort((a, b) => a - b);
    let ans = 0;
    let p = 0;
    for (let i = nums.length - 1; i >= 0; --i) {
        const x = BigInt(nums[i]);
        ans = (ans + Number((x * x * x) % BigInt(mod))) % mod;
        ans = (ans + Number((x * BigInt(p)) % BigInt(mod))) % mod;
        p = Number((BigInt(p) * 2n + x * x) % BigInt(mod));
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
