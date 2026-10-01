---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [260. Single Number III](https://leetcode.com/problems/single-number-iii)

[中文文档](/solution/0200-0299/0260.Single%20Number%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, trong đó có đúng hai phần tử chỉ xuất hiện một lần, còn tất cả phần tử khác xuất hiện đúng hai lần. Hãy tìm hai phần tử chỉ xuất hiện một lần. Bạn có thể trả về kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Hãy viết thuật toán có độ phức tạp thời gian tuyến tính và chỉ sử dụng bộ nhớ phụ không đổi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1,3,2,5]
<strong>Đầu ra:</strong> [3,5]
<strong>Giải thích: </strong> [5, 3] cũng là một đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,0]
<strong>Đầu ra:</strong> [-1,0]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1]
<strong>Đầu ra:</strong> [1,0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>-2<sup>31</sup> &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
	<li>Mỗi số nguyên trong <code>nums</code> sẽ xuất hiện hai lần, chỉ có hai số nguyên xuất hiện một lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Tất cả giá trị ngoại trừ hai giá trị đều xuất hiện hai lần, nên XOR của cả mảng là XOR $xs$ của hai giá trị đó. Hai giá trị này khác nhau, vì vậy $xs$ có ít nhất một bit bằng $1$.
>
> Chia các giá trị thành nhóm dựa trên $\mathrm{lowbit}$ của $xs$; hai giá trị cần tìm sẽ nằm ở hai nhóm khác nhau. XOR các giá trị trong nhóm có bit đó để tìm $a$, sau đó tính $b=xs\oplus a$.

<!-- thinking:end -->

Phép XOR có các tính chất sau:

- XOR một số bất kỳ với 0 vẫn cho ra chính số đó, tức là $x \oplus 0 = x$;
- XOR một số bất kỳ với chính nó cho kết quả 0, tức là $x \oplus x = 0$;

Vì mọi số trong mảng, ngoại trừ hai số, đều xuất hiện hai lần, ta XOR tất cả các số trong mảng để thu được XOR của hai số chỉ xuất hiện một lần.

Vì hai số này khác nhau nên kết quả XOR có ít nhất một bit bằng 1. Ta có thể dùng phép `lowbit` để tìm bit 1 thấp nhất trong kết quả XOR, rồi chia các số trong mảng thành hai nhóm tùy theo bit đó có bằng 1 hay không. Nhờ vậy, hai số chỉ xuất hiện một lần sẽ nằm ở hai nhóm khác nhau.

Thực hiện XOR riêng trên từng nhóm để tìm hai số $a$ và $b$ chỉ xuất hiện một lần.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> List[int]:
        xs = reduce(xor, nums)
        a = 0
        lb = xs & -xs
        for x in nums:
            if x & lb:
                a ^= x
        b = xs ^ a
        return [a, b]
```

#### Java

```java
class Solution {
    public int[] singleNumber(int[] nums) {
        int xs = 0;
        for (int x : nums) {
            xs ^= x;
        }
        int lb = xs & -xs;
        int a = 0;
        for (int x : nums) {
            if ((x & lb) != 0) {
                a ^= x;
            }
        }
        int b = xs ^ a;
        return new int[] {a, b};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        long long xs = 0;
        for (int& x : nums) {
            xs ^= x;
        }
        int lb = xs & -xs;
        int a = 0;
        for (int& x : nums) {
            if (x & lb) {
                a ^= x;
            }
        }
        int b = xs ^ a;
        return {a, b};
    }
};
```

#### Go

```go
func singleNumber(nums []int) []int {
	xs := 0
	for _, x := range nums {
		xs ^= x
	}
	lb := xs & -xs
	a := 0
	for _, x := range nums {
		if x&lb != 0 {
			a ^= x
		}
	}
	b := xs ^ a
	return []int{a, b}
}
```

#### TypeScript

```ts
function singleNumber(nums: number[]): number[] {
    const xs = nums.reduce((a, b) => a ^ b);
    const lb = xs & -xs;
    let a = 0;
    for (const x of nums) {
        if (x & lb) {
            a ^= x;
        }
    }
    const b = xs ^ a;
    return [a, b];
}
```

#### Rust

```rust
impl Solution {
    pub fn single_number(nums: Vec<i32>) -> Vec<i32> {
        let xs = nums.iter().fold(0, |r, v| r ^ v);
        let lb = xs & -xs;
        let mut a = 0;
        for x in &nums {
            if (x & lb) != 0 {
                a ^= x;
            }
        }
        let b = xs ^ a;
        vec![a, b]
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var singleNumber = function (nums) {
    const xs = nums.reduce((a, b) => a ^ b);
    const lb = xs & -xs;
    let a = 0;
    for (const x of nums) {
        if (x & lb) {
            a ^= x;
        }
    }
    const b = xs ^ a;
    return [a, b];
};
```

#### C#

```cs
public class Solution {
    public int[] SingleNumber(int[] nums) {
        int xs = nums.Aggregate(0, (a, b) => a ^ b);
        int lb = xs & -xs;
        int a = 0;
        foreach(int x in nums) {
            if ((x & lb) != 0) {
                a ^= x;
            }
        }
        int b = xs ^ a;
        return new int[] {a, b};
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hash table

<!-- thinking:start -->

> **Tư duy**
>
> Cách dùng bit chỉ cần bộ nhớ phụ không đổi. Nếu dùng bộ nhớ phụ tuyến tính, có thể dùng set để đảo trạng thái tồn tại: thêm số ở lần gặp đầu tiên và xóa ở lần gặp thứ hai. Các phần tử còn lại chính là hai số chỉ xuất hiện một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function singleNumber(nums: number[]): number[] {
    const set = new Set<number>();

    for (const x of nums) {
        if (set.has(x)) set.delete(x);
        else set.add(x);
    }

    return [...set];
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
function singleNumber(nums) {
    const set = new Set();

    for (const x of nums) {
        if (set.has(x)) set.delete(x);
        else set.add(x);
    }

    return [...set];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
