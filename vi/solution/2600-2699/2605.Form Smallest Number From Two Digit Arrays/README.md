---
comments: true
difficulty: Easy
rating: 1241
source: Biweekly Contest 101 Q1
tags:
    - Array
    - Hash Table
    - Enumeration
---

<!-- problem:start -->

# [2605. Form Smallest Number From Two Digit Arrays](https://leetcode.com/problems/form-smallest-number-from-two-digit-arrays)

[中文文档](/solution/2600-2699/2605.Form%20Smallest%20Number%20From%20Two%20Digit%20Arrays/README.md)

## Mô tả

<!-- description:start -->

Cho hai mảng chữ số <strong>không trùng nhau</strong> <code>nums1</code> và <code>nums2</code>, hãy trả về <em>số <strong>nhỏ nhất</strong> chứa <strong>ít nhất</strong> một chữ số từ mỗi mảng</em>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [4,1,3], nums2 = [5,7]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Số 15 chứa chữ số 1 từ nums1 và chữ số 5 từ nums2. Có thể chứng minh rằng 15 là số nhỏ nhất có thể tạo ra.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [3,5,2,6], nums2 = [3,1,7]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Số 3 chứa chữ số 3 xuất hiện trong cả hai mảng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 9</code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 9</code></li>
	<li>Các chữ số trong mỗi mảng đều <strong>không trùng nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Đáp án có nhiều nhất hai chữ số, mỗi chữ số lấy từ một mảng. Vì mỗi mảng dài không quá $9$, ta có thể liệt kê mọi cặp để bao quát trường hợp có chữ số chung và trường hợp ghép hai chữ số khác nhau.
>
> Nếu $a=b$, số có một chữ số $a$ nhỏ hơn mọi số có hai chữ số; ngược lại, ta so sánh $10a+b$ và $10b+a$, rồi giữ lại giá trị nhỏ nhất trên toàn bộ các cặp.

<!-- thinking:end -->

Ta nhận thấy rằng nếu hai mảng $nums1$ và $nums2$ có cùng chữ số, thì chữ số nhỏ nhất trong các chữ số chung chính là số nhỏ nhất. Nếu không, ta lấy chữ số $a$ trong mảng $nums1$ và chữ số $b$ trong mảng $nums2$, ghép $a$ và $b$ theo cả hai thứ tự để tạo thành hai số, rồi chọn số nhỏ hơn.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(1)$, trong đó $m$ và $n$ lần lượt là độ dài của hai mảng $nums1$ và $nums2$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minNumber(self, nums1: List[int], nums2: List[int]) -> int:
        ans = 100
        for a in nums1:
            for b in nums2:
                if a == b:
                    ans = min(ans, a)
                else:
                    ans = min(ans, 10 * a + b, 10 * b + a)
        return ans
```

#### Java

```java
class Solution {
    public int minNumber(int[] nums1, int[] nums2) {
        int ans = 100;
        for (int a : nums1) {
            for (int b : nums2) {
                if (a == b) {
                    ans = Math.min(ans, a);
                } else {
                    ans = Math.min(ans, Math.min(a * 10 + b, b * 10 + a));
                }
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
    int minNumber(vector<int>& nums1, vector<int>& nums2) {
        int ans = 100;
        for (int a : nums1) {
            for (int b : nums2) {
                if (a == b) {
                    ans = min(ans, a);
                } else {
                    ans = min({ans, a * 10 + b, b * 10 + a});
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minNumber(nums1 []int, nums2 []int) int {
	ans := 100
	for _, a := range nums1 {
		for _, b := range nums2 {
			if a == b {
				ans = min(ans, a)
			} else {
				ans = min(ans, min(a*10+b, b*10+a))
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minNumber(nums1: number[], nums2: number[]): number {
    let ans = 100;
    for (const a of nums1) {
        for (const b of nums2) {
            if (a === b) {
                ans = Math.min(ans, a);
            } else {
                ans = Math.min(ans, a * 10 + b, b * 10 + a);
            }
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_number(nums1: Vec<i32>, nums2: Vec<i32>) -> i32 {
        let mut ans = 100;

        for &a in &nums1 {
            for &b in &nums2 {
                if a == b {
                    ans = std::cmp::min(ans, a);
                } else {
                    ans = std::cmp::min(ans, std::cmp::min(a * 10 + b, b * 10 + a));
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Bảng băm hoặc mảng + liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 so sánh mọi cặp. Khi tồn tại chữ số chung, giá trị nhỏ nhất trong các chữ số chung chính là đáp án, nên không cần xét các phép ghép; nếu không có chữ số chung, chỉ cần ghép hai chữ số nhỏ nhất.
>
> Một tập hợp giao (hoặc phép lấy $\min$ trực tiếp trên mỗi mảng) giúp giảm các vòng lặp lồng nhau xuống còn một lần duyệt tuyến tính.

<!-- thinking:end -->

Ta có thể dùng bảng băm hoặc mảng để đánh dấu các chữ số xuất hiện trong hai mảng $nums1$ và $nums2$, sau đó liệt kê các chữ số $1 \sim 9$. Nếu chữ số $i$ xuất hiện trong cả hai mảng, thì $i$ là số nhỏ nhất. Nếu không, ta lấy chữ số $a$ trong mảng $nums1$ và chữ số $b$ trong mảng $nums2$, ghép $a$ và $b$ theo cả hai thứ tự để tạo thành hai số, rồi chọn số nhỏ hơn.

Độ phức tạp thời gian là $(m + n)$, còn độ phức tạp không gian là $O(C)$. Trong đó $m$ và $n$ lần lượt là độ dài của hai mảng $nums1$ và $nums2$; $C$ là miền giá trị của các chữ số trong hai mảng $nums1$ và $nums2$, và trong bài toán này $C = 10$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minNumber(self, nums1: List[int], nums2: List[int]) -> int:
        s = set(nums1) & set(nums2)
        if s:
            return min(s)
        a, b = min(nums1), min(nums2)
        return min(a * 10 + b, b * 10 + a)
```

#### Java

```java
class Solution {
    public int minNumber(int[] nums1, int[] nums2) {
        boolean[] s1 = new boolean[10];
        boolean[] s2 = new boolean[10];
        for (int x : nums1) {
            s1[x] = true;
        }
        for (int x : nums2) {
            s2[x] = true;
        }
        int a = 0, b = 0;
        for (int i = 1; i < 10; ++i) {
            if (s1[i] && s2[i]) {
                return i;
            }
            if (a == 0 && s1[i]) {
                a = i;
            }
            if (b == 0 && s2[i]) {
                b = i;
            }
        }
        return Math.min(a * 10 + b, b * 10 + a);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minNumber(vector<int>& nums1, vector<int>& nums2) {
        bitset<10> s1;
        bitset<10> s2;
        for (int x : nums1) {
            s1[x] = 1;
        }
        for (int x : nums2) {
            s2[x] = 1;
        }
        int a = 0, b = 0;
        for (int i = 1; i < 10; ++i) {
            if (s1[i] && s2[i]) {
                return i;
            }
            if (!a && s1[i]) {
                a = i;
            }
            if (!b && s2[i]) {
                b = i;
            }
        }
        return min(a * 10 + b, b * 10 + a);
    }
};
```

#### Go

```go
func minNumber(nums1 []int, nums2 []int) int {
	s1 := [10]bool{}
	s2 := [10]bool{}
	for _, x := range nums1 {
		s1[x] = true
	}
	for _, x := range nums2 {
		s2[x] = true
	}
	a, b := 0, 0
	for i := 1; i < 10; i++ {
		if s1[i] && s2[i] {
			return i
		}
		if a == 0 && s1[i] {
			a = i
		}
		if b == 0 && s2[i] {
			b = i
		}
	}
	return min(a*10+b, b*10+a)
}
```

#### TypeScript

```ts
function minNumber(nums1: number[], nums2: number[]): number {
    const s1: boolean[] = new Array(10).fill(false);
    const s2: boolean[] = new Array(10).fill(false);
    for (const x of nums1) {
        s1[x] = true;
    }
    for (const x of nums2) {
        s2[x] = true;
    }
    let a = 0;
    let b = 0;
    for (let i = 1; i < 10; ++i) {
        if (s1[i] && s2[i]) {
            return i;
        }
        if (a === 0 && s1[i]) {
            a = i;
        }
        if (b === 0 && s2[i]) {
            b = i;
        }
    }
    return Math.min(a * 10 + b, b * 10 + a);
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn min_number(nums1: Vec<i32>, nums2: Vec<i32>) -> i32 {
        let mut h1: HashMap<i32, bool> = HashMap::new();

        for &n in &nums1 {
            h1.insert(n, true);
        }

        let mut h2: HashMap<i32, bool> = HashMap::new();
        for &n in &nums2 {
            h2.insert(n, true);
        }

        let mut a = 0;
        let mut b = 0;
        for i in 1..10 {
            if h1.contains_key(&i) && h2.contains_key(&i) {
                return i;
            }

            if a == 0 && h1.contains_key(&i) {
                a = i;
            }

            if b == 0 && h2.contains_key(&i) {
                b = i;
            }
        }

        std::cmp::min(a * 10 + b, b * 10 + a)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Phép toán bit

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ số nằm trong khoảng $1..9$, nên ta có thể biểu diễn các tập hợp bằng bit mask. Nếu phép AND theo bit cho kết quả khác rỗng, chữ số chung nhỏ nhất là bit thấp nhất; nếu không, ta ghép bit thấp nhất của mỗi mask.
>
> Nhận xét này giống Lời giải 2; điểm khác biệt duy nhất là cách biểu diễn tập hợp được thu gọn thành các phép toán bit với không gian hằng số.

<!-- thinking:end -->

Vì miền giá trị của các chữ số là $1 \sim 9$, ta có thể dùng một số nhị phân có độ dài $10$ để biểu diễn các chữ số trong hai mảng $nums1$ và $nums2$. Ta dùng $mask1$ để biểu diễn các chữ số trong mảng $nums1$, và dùng $mask2$ để biểu diễn các chữ số trong mảng $nums2$.

Nếu số $mask$ nhận được bằng cách thực hiện phép AND theo bit giữa $mask1$ và $mask2$ khác $0$, ta lấy vị trí của bit $1$ thấp nhất trong số $mask$, đó chính là chữ số nhỏ nhất.

Ngược lại, ta lần lượt lấy vị trí của bit $1$ thấp nhất trong $mask1$ và $mask2$, ký hiệu tương ứng là $a$ và $b$. Khi đó số nhỏ nhất là $min(a \times 10 + b, b \times 10 + a)$.

Độ phức tạp thời gian là $O(m + n)$, còn độ phức tạp không gian là $O(1)$. Trong đó $m$ và $n$ lần lượt là độ dài của hai mảng $nums1$ và $nums2$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minNumber(self, nums1: List[int], nums2: List[int]) -> int:
        mask1 = mask2 = 0
        for x in nums1:
            mask1 |= 1 << x
        for x in nums2:
            mask2 |= 1 << x
        mask = mask1 & mask2
        if mask:
            return (mask & -mask).bit_length() - 1
        a = (mask1 & -mask1).bit_length() - 1
        b = (mask2 & -mask2).bit_length() - 1
        return min(a * 10 + b, b * 10 + a)
```

#### Java

```java
class Solution {
    public int minNumber(int[] nums1, int[] nums2) {
        int mask1 = 0, mask2 = 0;
        for (int x : nums1) {
            mask1 |= 1 << x;
        }
        for (int x : nums2) {
            mask2 |= 1 << x;
        }
        int mask = mask1 & mask2;
        if (mask != 0) {
            return Integer.numberOfTrailingZeros(mask);
        }
        int a = Integer.numberOfTrailingZeros(mask1);
        int b = Integer.numberOfTrailingZeros(mask2);
        return Math.min(a * 10 + b, b * 10 + a);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minNumber(vector<int>& nums1, vector<int>& nums2) {
        int mask1 = 0, mask2 = 0;
        for (int x : nums1) {
            mask1 |= 1 << x;
        }
        for (int x : nums2) {
            mask2 |= 1 << x;
        }
        int mask = mask1 & mask2;
        if (mask) {
            return __builtin_ctz(mask);
        }
        int a = __builtin_ctz(mask1);
        int b = __builtin_ctz(mask2);
        return min(a * 10 + b, b * 10 + a);
    }
};
```

#### Go

```go
func minNumber(nums1 []int, nums2 []int) int {
	var mask1, mask2 uint
	for _, x := range nums1 {
		mask1 |= 1 << x
	}
	for _, x := range nums2 {
		mask2 |= 1 << x
	}
	if mask := mask1 & mask2; mask != 0 {
		return bits.TrailingZeros(mask)
	}
	a, b := bits.TrailingZeros(mask1), bits.TrailingZeros(mask2)
	return min(a*10+b, b*10+a)
}
```

#### TypeScript

```ts
function minNumber(nums1: number[], nums2: number[]): number {
    let mask1: number = 0;
    let mask2: number = 0;
    for (const x of nums1) {
        mask1 |= 1 << x;
    }
    for (const x of nums2) {
        mask2 |= 1 << x;
    }
    const mask = mask1 & mask2;
    if (mask !== 0) {
        return numberOfTrailingZeros(mask);
    }
    const a = numberOfTrailingZeros(mask1);
    const b = numberOfTrailingZeros(mask2);
    return Math.min(a * 10 + b, b * 10 + a);
}

function numberOfTrailingZeros(i: number): number {
    let y = 0;
    if (i === 0) {
        return 32;
    }
    let n = 31;
    y = i << 16;
    if (y != 0) {
        n = n - 16;
        i = y;
    }
    y = i << 8;
    if (y != 0) {
        n = n - 8;
        i = y;
    }
    y = i << 4;
    if (y != 0) {
        n = n - 4;
        i = y;
    }
    y = i << 2;
    if (y != 0) {
        n = n - 2;
        i = y;
    }
    return n - ((i << 1) >>> 31);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
