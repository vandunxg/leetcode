---
comments: true
difficulty: Medium
rating: 1724
source: Weekly Contest 291 Q3
tags:
    - Trie
    - Array
    - Hash Table
    - Enumeration
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [2261. K Divisible Elements Subarrays](https://leetcode.com/problems/k-divisible-elements-subarrays)

[中文文档](/solution/2200-2299/2261.K%20Divisible%20Elements%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và hai số nguyên <code>k</code> và <code>p</code>, hãy trả về <em>số lượng <strong>mảng con phân biệt</strong> có <strong>nhiều nhất</strong></em> <code>k</code> <em>phần tử </em><em>chia hết cho</em> <code>p</code>.</p>

<p>Hai mảng <code>nums1</code> và <code>nums2</code> được gọi là <strong>phân biệt</strong> nếu:</p>

<ul>
	<li>Chúng có độ dài <strong>khác nhau</strong>, hoặc</li>
	<li>Tồn tại <strong>ít nhất</strong> một chỉ số <code>i</code> sao cho <code>nums1[i] != nums2[i]</code>.</li>
</ul>

<p><strong>Mảng con</strong> là một dãy phần tử <strong>liền kề không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [<u><strong>2</strong></u>,3,3,<u><strong>2</strong></u>,<u><strong>2</strong></u>], k = 2, p = 2
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong>
Các phần tử tại các chỉ số 0, 3 và 4 chia hết cho p = 2.
11 mảng con phân biệt có nhiều nhất k = 2 phần tử chia hết cho 2 là:
[2], [2,3], [2,3,3], [2,3,3,2], [3], [3,3], [3,3,2], [3,3,2,2], [3,2], [3,2,2] và [2,2].
Lưu ý rằng các mảng con [2] và [3] xuất hiện nhiều hơn một lần trong nums, nhưng mỗi mảng chỉ được tính một lần.
Mảng con [2,3,3,2,2] không được tính vì nó có 3 phần tử chia hết cho 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4], k = 4, p = 1
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong>
Tất cả phần tử của nums đều chia hết cho p = 1.
Ngoài ra, mọi mảng con của nums đều có nhiều nhất 4 phần tử chia hết cho 1.
Vì tất cả các mảng con đều phân biệt, tổng số mảng con thỏa mãn mọi ràng buộc là 10.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 200</code></li>
	<li><code>1 &lt;= nums[i], p &lt;= 200</code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<p>Bạn có thể giải bài toán với độ phức tạp thời gian O(n<sup>2</sup>) không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt + Hash chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đếm các mảng con phân biệt có nhiều nhất $k$ phần tử chia hết cho $p$. Vì $n \le 200$, ta có thể liệt kê tất cả mảng con; vấn đề là loại bỏ các mảng trùng lặp. Lưu cả mảng sẽ tốn nhiều bộ nhớ, còn rolling hash có thể nén một đoạn thành một giá trị hằng số.
>
> Cố định đầu trái $i$, mở rộng $j$ khi số phần tử chia hết vẫn không vượt quá $k$, rồi thêm hash dùng hai modulus vào một set. Kích thước của set chính là đáp án.

<!-- thinking:end -->

Ta có thể duyệt đầu trái $i$ của mảng con, sau đó duyệt đầu phải $j$ trong phạm vi $[i, n)$. Trong quá trình duyệt đầu phải, ta dùng double hashing để lưu giá trị hash của mảng con vào một set. Cuối cùng, ta trả về kích thước của set.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDistinct(self, nums: List[int], k: int, p: int) -> int:
        s = set()
        n = len(nums)
        base1, base2 = 131, 13331
        mod1, mod2 = 10**9 + 7, 10**9 + 9
        for i in range(n):
            h1 = h2 = cnt = 0
            for j in range(i, n):
                cnt += nums[j] % p == 0
                if cnt > k:
                    break
                h1 = (h1 * base1 + nums[j]) % mod1
                h2 = (h2 * base2 + nums[j]) % mod2
                s.add(h1 << 32 | h2)
        return len(s)
```

#### Java

```java
class Solution {
    public int countDistinct(int[] nums, int k, int p) {
        Set<Long> s = new HashSet<>();
        int n = nums.length;
        int base1 = 131, base2 = 13331;
        int mod1 = (int) 1e9 + 7, mod2 = (int) 1e9 + 9;
        for (int i = 0; i < n; ++i) {
            long h1 = 0, h2 = 0;
            int cnt = 0;
            for (int j = i; j < n; ++j) {
                cnt += nums[j] % p == 0 ? 1 : 0;
                if (cnt > k) {
                    break;
                }
                h1 = (h1 * base1 + nums[j]) % mod1;
                h2 = (h2 * base2 + nums[j]) % mod2;
                s.add(h1 << 32 | h2);
            }
        }
        return s.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countDistinct(vector<int>& nums, int k, int p) {
        unordered_set<long long> s;
        int n = nums.size();
        int base1 = 131, base2 = 13331;
        int mod1 = 1e9 + 7, mod2 = 1e9 + 9;
        for (int i = 0; i < n; ++i) {
            long long h1 = 0, h2 = 0;
            int cnt = 0;
            for (int j = i; j < n; ++j) {
                cnt += nums[j] % p == 0;
                if (cnt > k) {
                    break;
                }
                h1 = (h1 * base1 + nums[j]) % mod1;
                h2 = (h2 * base2 + nums[j]) % mod2;
                s.insert(h1 << 32 | h2);
            }
        }
        return s.size();
    }
};
```

#### Go

```go
func countDistinct(nums []int, k int, p int) int {
	s := map[int]bool{}
	base1, base2 := 131, 13331
	mod1, mod2 := 1000000007, 1000000009
	for i := range nums {
		h1, h2, cnt := 0, 0, 0
		for j := i; j < len(nums); j++ {
			if nums[j]%p == 0 {
				cnt++
				if cnt > k {
					break
				}
			}
			h1 = (h1*base1 + nums[j]) % mod1
			h2 = (h2*base2 + nums[j]) % mod2
			s[h1<<32|h2] = true
		}
	}
	return len(s)
}
```

#### TypeScript

```ts
function countDistinct(nums: number[], k: number, p: number): number {
    const s = new Set<bigint>();
    const [base1, base2] = [131, 13331];
    const [mod1, mod2] = [1000000007, 1000000009];
    for (let i = 0; i < nums.length; i++) {
        let [h1, h2, cnt] = [0, 0, 0];
        for (let j = i; j < nums.length; j++) {
            if (nums[j] % p === 0) {
                cnt++;
                if (cnt > k) {
                    break;
                }
            }
            h1 = (h1 * base1 + nums[j]) % mod1;
            h2 = (h2 * base2 + nums[j]) % mod2;
            s.add((BigInt(h1) << 32n) | BigInt(h2));
        }
    }
    return s.size;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Duyệt + Nối chuỗi

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tránh việc lưu toàn bộ đoạn bằng cách dùng hash. Vì $n$ khá nhỏ, ta cũng có thể nối các phần tử thành một chuỗi làm khóa. Cách duyệt và cắt tỉa theo $k$ vẫn giữ nguyên, chỉ có các hằng số thời gian tăng lên.

<!-- thinking:end -->

Liệt kê mọi mảng con và lưu chuỗi được nối từ các phần tử vào một set. Độ phức tạp thời gian và không gian đều là $O(n^2)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDistinct(self, nums: List[int], k: int, p: int) -> int:
        n = len(nums)
        s = set()
        for i in range(n):
            cnt = 0
            t = ""
            for x in nums[i:]:
                cnt += x % p == 0
                if cnt > k:
                    break
                t += str(x) + ","
                s.add(t)
        return len(s)
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
