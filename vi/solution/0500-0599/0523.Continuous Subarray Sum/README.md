---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Math
    - Prefix Sum
    - Pigeonhole Principle
---

<!-- problem:start -->

# [523. Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum)

[中文文档](/solution/0500-0599/0523.Continuous%20Subarray%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên nums và số nguyên k, hãy trả về <code>true</code> <em>nếu </em><code>nums</code><em> có <strong>mảng con hợp lệ</strong>, ngược lại trả về </em><code>false</code>.</p>

<p><strong>Mảng con hợp lệ</strong> là mảng con thỏa mãn:</p>

<ul>
	<li>có độ dài <strong>ít nhất 2</strong>, và</li>
	<li>tổng các phần tử trong mảng con là bội số của <code>k</code>.</li>
</ul>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li><strong>Mảng con</strong> là một đoạn liên tiếp trong mảng.</li>
	<li>Số nguyên <code>x</code> là bội số của <code>k</code> nếu tồn tại số nguyên <code>n</code> sao cho <code>x = n * k</code>. <code>0</code> <strong>luôn</strong> là bội số của <code>k</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [23,<u>2,4</u>,6,7], k = 6
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> [2, 4] là mảng con liên tiếp có độ dài 2 và tổng các phần tử bằng 6.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [<u>23,2,6,4,7</u>], k = 6
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> [23, 2, 6, 4, 7] là mảng con liên tiếp có độ dài 5 và tổng các phần tử bằng 42.
42 là bội số của 6 vì 42 = 7 * 6 và 7 là số nguyên.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [23,2,6,4,7], k = 13
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= sum(nums[i]) &lt;= 2<sup>31</sup> - 1</code></li>
	<li><code>1 &lt;= k &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm mảng con có độ dài ít nhất $2$ và tổng là bội số của $k$. Kiểm tra mọi cặp sẽ mất $O(n^2)$, quá chậm khi $n \le 10^5$.
>
> Các prefix sum có cùng phần dư khi chia cho $k$ nghĩa là tổng ở giữa chia hết cho $k$. Lưu chỉ số đầu tiên của mỗi phần dư; nếu phần dư đó xuất hiện lại ở vị trí cách xa hơn $1$ thì tìm được mảng con cần tìm. Khởi tạo phần dư $0$ tại chỉ số $-1$ để tính cả các prefix bắt đầu từ đầu mảng.

<!-- thinking:end -->

Theo đề bài, nếu tồn tại hai vị trí $i$ và $j$ ($j < i$) có phần dư của prefix sum khi chia cho $k$ bằng nhau, thì tổng của mảng con $\textit{nums}[j+1..i]$ là bội số của $k$.

Vì vậy, ta có thể dùng hash table để lưu lần xuất hiện đầu tiên của mỗi phần dư prefix sum modulo $k$. Ban đầu, ta lưu cặp key-value $(0, -1)$ trong hash table, biểu thị rằng prefix sum $0$ có phần dư $0$ tại vị trí $-1$.

Khi duyệt mảng, ta tính phần dư modulo $k$ của prefix sum hiện tại. Nếu phần dư này chưa có trong hash table, ta lưu phần dư cùng vị trí tương ứng vào hash table. Ngược lại, nếu phần dư hiện tại đã xuất hiện trong hash table tại vị trí $j$, ta tìm được mảng con $\textit{nums}[j+1..i]$ thỏa mãn điều kiện và trả về $\textit{True}$.

Sau khi duyệt xong, nếu không tìm thấy mảng con nào thỏa mãn điều kiện, ta trả về $\textit{False}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkSubarraySum(self, nums: List[int], k: int) -> bool:
        d = {0: -1}
        s = 0
        for i, x in enumerate(nums):
            s = (s + x) % k
            if s not in d:
                d[s] = i
            elif i - d[s] > 1:
                return True
        return False
```

#### Java

```java
class Solution {
    public boolean checkSubarraySum(int[] nums, int k) {
        Map<Integer, Integer> d = new HashMap<>();
        d.put(0, -1);
        int s = 0;
        for (int i = 0; i < nums.length; ++i) {
            s = (s + nums[i]) % k;
            if (!d.containsKey(s)) {
                d.put(s, i);
            } else if (i - d.get(s) > 1) {
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
    bool checkSubarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> d{{0, -1}};
        int s = 0;
        for (int i = 0; i < nums.size(); ++i) {
            s = (s + nums[i]) % k;
            if (!d.contains(s)) {
                d[s] = i;
            } else if (i - d[s] > 1) {
                return true;
            }
        }
        return false;
    }
};
```

#### Go

```go
func checkSubarraySum(nums []int, k int) bool {
	d := map[int]int{0: -1}
	s := 0
	for i, x := range nums {
		s = (s + x) % k
		if _, ok := d[s]; !ok {
			d[s] = i
		} else if i-d[s] > 1 {
			return true
		}
	}
	return false
}
```

#### TypeScript

```ts
function checkSubarraySum(nums: number[], k: number): boolean {
    const d: Record<number, number> = { 0: -1 };
    let s = 0;
    for (let i = 0; i < nums.length; ++i) {
        s = (s + nums[i]) % k;
        if (!d.hasOwnProperty(s)) {
            d[s] = i;
        } else if (i - d[s] > 1) {
            return true;
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
