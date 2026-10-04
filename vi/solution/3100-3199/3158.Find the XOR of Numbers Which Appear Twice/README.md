---
comments: true
difficulty: Easy
rating: 1172
source: Biweekly Contest 131 Q1
tags:
    - Bit Manipulation
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3158. Find the XOR of Numbers Which Appear Twice](https://leetcode.com/problems/find-the-xor-of-numbers-which-appear-twice)

[中文文档](/solution/3100-3199/3158.Find%20the%20XOR%20of%20Numbers%20Which%20Appear%20Twice/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code>, trong đó mỗi số trong mảng xuất hiện <strong>hoặc</strong><em> </em>một lần<em> </em>hoặc<em> </em>hai lần.</p>

<p>Trả về phép<em> </em><code>XOR</code> theo bit của tất cả các số xuất hiện hai lần trong mảng, hoặc 0 nếu không có số nào xuất hiện hai lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số duy nhất xuất hiện hai lần trong&nbsp;<code>nums</code>&nbsp;là 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số nào xuất hiện hai lần trong&nbsp;<code>nums</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các số 1 và 2 xuất hiện hai lần. <code>1 XOR 2 == 3</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 50</code></li>
	<li>Mỗi số trong <code>nums</code> xuất hiện một hoặc hai lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Thực hiện phép XOR với các giá trị xuất hiện đúng hai lần. Miền giá trị nhỏ, nên chỉ cần một bảng tần suất.
>
> Đếm trong một lượt duyệt, sau đó thực hiện XOR với các khóa có tần suất bằng $2$. Danh sách rỗng cho kết quả $0$.
>
> Kết hợp `Counter` với `reduce(xor, ...)` sẽ giải quyết bài toán trong thời gian tuyến tính.

<!-- thinking:end -->

Ta định nghĩa một mảng hoặc hash table `cnt` để ghi lại số lần xuất hiện của mỗi số.

Tiếp theo, ta duyệt mảng `nums`. Khi một số xuất hiện lần thứ hai, ta thực hiện phép XOR với đáp án.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(M)$. Trong đó, $n$ là độ dài của mảng `nums`, còn $M$ là giá trị lớn nhất trong mảng `nums` hoặc số lượng các số phân biệt trong mảng `nums`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def duplicateNumbersXOR(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        return reduce(xor, [x for x, v in cnt.items() if v == 2], 0)
```

#### Java

```java
class Solution {
    public int duplicateNumbersXOR(int[] nums) {
        int[] cnt = new int[51];
        int ans = 0;
        for (int x : nums) {
            if (++cnt[x] == 2) {
                ans ^= x;
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
    int duplicateNumbersXOR(vector<int>& nums) {
        int cnt[51]{};
        int ans = 0;
        for (int x : nums) {
            if (++cnt[x] == 2) {
                ans ^= x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func duplicateNumbersXOR(nums []int) (ans int) {
	cnt := [51]int{}
	for _, x := range nums {
		cnt[x]++
		if cnt[x] == 2 {
			ans ^= x
		}
	}
	return
}
```

#### TypeScript

```ts
function duplicateNumbersXOR(nums: number[]): number {
    const cnt: number[] = Array(51).fill(0);
    let ans: number = 0;
    for (const x of nums) {
        if (++cnt[x] === 2) {
            ans ^= x;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Khi mọi giá trị không vượt quá $50$, ta không cần hash map.
>
> Một mặt nạ 64-bit ghi lại việc một giá trị đã xuất hiện hay chưa; lần xuất hiện thứ hai sẽ được XOR vào đáp án.
>
> Nếu bit $x$ của $mask$ đã được bật, ta thực hiện XOR $x$ vào $ans$; nếu không, ta bật bit đó. Nhờ vậy, các mảng phụ được loại bỏ.

<!-- thinking:end -->

Vì miền giá trị đã cho trong đề bài là $1 \leq \textit{nums}[i] \leq 50$, ta có thể sử dụng một số nguyên $64$-bit để lưu số lần xuất hiện của mỗi số.

Ta định nghĩa một số nguyên $\textit{mask}$ để ghi lại việc mỗi số đã xuất hiện hay chưa.

Tiếp theo, ta duyệt mảng $\textit{nums}$. Khi một số xuất hiện lần thứ hai, tức là bit thứ $x$ trong biểu diễn nhị phân của $\textit{mask}$ bằng $1$, ta thực hiện phép XOR với đáp án. Nếu không, ta đặt bit thứ $x$ của $\textit{mask}$ bằng $1$.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def duplicateNumbersXOR(self, nums: List[int]) -> int:
        ans = mask = 0
        for x in nums:
            if mask >> x & 1:
                ans ^= x
            else:
                mask |= 1 << x
        return ans
```

#### Java

```java
class Solution {
    public int duplicateNumbersXOR(int[] nums) {
        int ans = 0;
        long mask = 0;
        for (int x : nums) {
            if ((mask >> x & 1) == 1) {
                ans ^= x;
            } else {
                mask |= 1L << x;
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
    int duplicateNumbersXOR(vector<int>& nums) {
        int ans = 0;
        long long mask = 0;
        for (int x : nums) {
            if (mask >> x & 1) {
                ans ^= x;
            } else {
                mask |= 1LL << x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func duplicateNumbersXOR(nums []int) (ans int) {
	mask := 0
	for _, x := range nums {
		if mask>>x&1 == 1 {
			ans ^= x
		} else {
			mask |= 1 << x
		}
	}
	return
}
```

#### TypeScript

```ts
function duplicateNumbersXOR(nums: number[]): number {
    let ans = 0;
    let mask = 0n;
    for (const x of nums) {
        if ((mask >> BigInt(x)) & 1n) {
            ans ^= x;
        } else {
            mask |= 1n << BigInt(x);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
