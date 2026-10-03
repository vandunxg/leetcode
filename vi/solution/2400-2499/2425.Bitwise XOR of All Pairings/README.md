---
comments: true
difficulty: Medium
rating: 1622
source: Biweekly Contest 88 Q3
tags:
    - Bit Manipulation
    - Brainteaser
    - Array
---

<!-- problem:start -->

# [2425. Bitwise XOR of All Pairings](https://leetcode.com/problems/bitwise-xor-of-all-pairings)

[中文文档](/solution/2400-2499/2425.Bitwise%20XOR%20of%20All%20Pairings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng được đánh chỉ số từ <strong>0</strong> là <code>nums1</code> và <code>nums2</code>, chỉ chứa các số nguyên không âm. Gọi một mảng khác là <code>nums3</code>, chứa phép XOR bitwise của <strong>tất cả các cặp</strong> số nguyên giữa <code>nums1</code> và <code>nums2</code> (mỗi số nguyên trong <code>nums1</code> được ghép với mỗi số nguyên trong <code>nums2</code> <strong>đúng một lần</strong>).</p>

<p>Hãy trả về <em>phép <strong>XOR bitwise</strong> của tất cả các số nguyên trong </em><code>nums3</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [2,1,3], nums2 = [10,2,5,0]
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong>
Một mảng nums3 có thể là [8,0,7,2,11,3,4,1,9,1,6,3].
Phép XOR bitwise của tất cả các số này là 13, nên ta trả về 13.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2], nums2 = [3,4]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Tất cả các cặp XOR bitwise có thể là nums1[0] ^ nums2[0], nums1[0] ^ nums2[1], nums1[1] ^ nums2[0],
và nums1[1] ^ nums2[1].
Vì vậy, một mảng nums3 có thể là [2,5,1,6].
2 ^ 5 ^ 1 ^ 6 = 0, nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums1[i], nums2[j] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tư duy nhanh + Bit Manipulation

<!-- thinking:start -->

> **Tư duy**
>
> Tích Descartes của phép XOR có $mn$ phần tử; với $m,n\le 10^5$ thì không thể liệt kê. Một giá trị khi XOR với chính nó một số lần chẵn sẽ bị triệt tiêu, nên $nums1[i]$ đóng góp vào kết quả khi và chỉ khi $n$ là số lẻ.
>
> Nếu $n$ lẻ, XOR toàn bộ các phần tử của $nums1$ vào đáp án; nếu $m$ lẻ, XOR toàn bộ các phần tử của $nums2$. XOR hai kết quả này chính là XOR của mọi cặp.

<!-- thinking:end -->

Vì mỗi phần tử của một mảng sẽ được XOR với mỗi phần tử của mảng còn lại, ta biết rằng kết quả không thay đổi khi một số được XOR với chính nó hai lần, tức là $a \oplus a = 0$. Do đó, ta chỉ cần đếm độ dài của các mảng để biết mỗi phần tử được XOR với từng phần tử của mảng kia bao nhiêu lần.

Nếu độ dài của mảng `nums2` là số lẻ, điều đó có nghĩa là mỗi phần tử trong `nums1` được XOR với mỗi phần tử trong `nums2` một số lần lẻ, nên kết quả XOR cuối cùng của phần `nums1` là XOR của tất cả phần tử trong mảng `nums1`. Nếu độ dài là số chẵn, mỗi phần tử trong `nums1` được XOR với mỗi phần tử trong `nums2` một số lần chẵn, nên kết quả XOR cuối cùng của phần `nums1` là 0.

Tương tự, ta có thể xác định kết quả XOR cuối cùng của phần `nums2`.

Cuối cùng, XOR hai kết quả này với nhau một lần nữa để nhận được kết quả cuối cùng.

Độ phức tạp thời gian là $O(m+n)$, trong đó $m$ và $n$ lần lượt là độ dài của các mảng `nums1` và `nums2`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def xorAllNums(self, nums1: List[int], nums2: List[int]) -> int:
        ans = 0
        if len(nums2) & 1:
            for v in nums1:
                ans ^= v
        if len(nums1) & 1:
            for v in nums2:
                ans ^= v
        return ans
```

#### Java

```java
class Solution {
    public int xorAllNums(int[] nums1, int[] nums2) {
        int ans = 0;
        if (nums2.length % 2 == 1) {
            for (int v : nums1) {
                ans ^= v;
            }
        }
        if (nums1.length % 2 == 1) {
            for (int v : nums2) {
                ans ^= v;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int xorAllNums(vector<int>& nums1, vector<int>& nums2) {
        int ans = 0;
        if (nums2.size() % 2 == 1) {
            for (int v : nums1) {
                ans ^= v;
            }
        }
        if (nums1.size() % 2 == 1) {
            for (int v : nums2) {
                ans ^= v;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func xorAllNums(nums1 []int, nums2 []int) int {
	ans := 0
	if len(nums2)%2 == 1 {
		for _, v := range nums1 {
			ans ^= v
		}
	}
	if len(nums1)%2 == 1 {
		for _, v := range nums2 {
			ans ^= v
		}
	}
	return ans
}
```

#### TypeScript

```ts
function xorAllNums(nums1: number[], nums2: number[]): number {
    let ans = 0;
    if (nums2.length % 2 != 0) {
        ans ^= nums1.reduce((a, c) => a ^ c, 0);
    }
    if (nums1.length % 2 != 0) {
        ans ^= nums2.reduce((a, c) => a ^ c, 0);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
