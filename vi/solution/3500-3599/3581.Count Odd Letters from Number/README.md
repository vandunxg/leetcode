---
comments: true
difficulty: Easy
tags:
    - Hash Table
    - String
    - Counting
    - Simulation
---

<!-- problem:start -->

# [3581. Count Odd Letters from Number 🔒](https://leetcode.com/problems/count-odd-letters-from-number)

[中文文档](/solution/3500-3599/3581.Count%20Odd%20Letters%20from%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code>, hãy thực hiện các bước sau:</p>

<ul>
	<li>Chuyển từng chữ số của <code>n</code> thành từ tiếng Anh <em>viết thường</em> tương ứng (ví dụ: 4 &rarr; &quot;four&quot;, 1 &rarr; &quot;one&quot;).</li>
	<li><strong>Nối</strong> các từ đó theo <strong>thứ tự chữ số ban đầu</strong> để tạo thành một chuỗi <code>s</code>.</li>
</ul>

<p>Trả về số lượng ký tự <strong>phân biệt</strong> trong <code>s</code> xuất hiện <strong>số lần lẻ</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 41</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>41 &rarr; <code>&quot;fourone&quot;</code></p>

<p>Các ký tự có tần suất lẻ: <code>&#39;f&#39;</code>, <code>&#39;u&#39;</code>, <code>&#39;r&#39;</code>, <code>&#39;n&#39;</code>, <code>&#39;e&#39;</code>. Vì vậy, đáp án là 5.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 20</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>20 &rarr; <code>&quot;twozero&quot;</code></p>

<p>Các ký tự có tần suất lẻ: <code>&#39;t&#39;</code>, <code>&#39;w&#39;</code>, <code>&#39;z&#39;</code>, <code>&#39;e&#39;</code>, <code>&#39;r&#39;</code>. Vì vậy, đáp án là 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng + thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Viết mỗi chữ số của $n$ bằng tiếng Anh rồi đếm các chữ cái xuất hiện số lần lẻ. Hai mươi sáu chữ cái có thể vừa trong một bit mask lưu tính chẵn lẻ.
>
> XOR bit của từng chữ cái; popcount là số chữ cái xuất hiện lẻ. Không cần nối các từ thành một chuỗi.

<!-- thinking:end -->

Ta có thể chuyển mỗi chữ số thành từ tiếng Anh tương ứng, sau đó đếm số lần xuất hiện của từng chữ cái. Vì số lượng chữ cái bị giới hạn, ta có thể dùng một số nguyên $\textit{mask}$ để biểu diễn sự xuất hiện của từng chữ cái. Cụ thể, ta ánh xạ mỗi chữ cái với một bit nhị phân của số nguyên. Nếu một chữ cái xuất hiện số lần lẻ, bit nhị phân tương ứng là 1; ngược lại là 0. Cuối cùng, ta chỉ cần đếm số bit bằng 1 trong $\textit{mask}$, đó là đáp án.

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là số nguyên đầu vào. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
d = {
    0: "zero",
    1: "one",
    2: "two",
    3: "three",
    4: "four",
    5: "five",
    6: "six",
    7: "seven",
    8: "eight",
    9: "nine",
}


class Solution:
    def countOddLetters(self, n: int) -> int:
        mask = 0
        while n:
            x = n % 10
            n //= 10
            for c in d[x]:
                mask ^= 1 << (ord(c) - ord("a"))
        return mask.bit_count()
```

#### Java

```java
class Solution {
    private static final Map<Integer, String> d = new HashMap<>();
    static {
        d.put(0, "zero");
        d.put(1, "one");
        d.put(2, "two");
        d.put(3, "three");
        d.put(4, "four");
        d.put(5, "five");
        d.put(6, "six");
        d.put(7, "seven");
        d.put(8, "eight");
        d.put(9, "nine");
    }

    public int countOddLetters(int n) {
        int mask = 0;
        while (n > 0) {
            int x = n % 10;
            n /= 10;
            for (char c : d.get(x).toCharArray()) {
                mask ^= 1 << (c - 'a');
            }
        }
        return Integer.bitCount(mask);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countOddLetters(int n) {
        static const unordered_map<int, string> d = {
            {0, "zero"},
            {1, "one"},
            {2, "two"},
            {3, "three"},
            {4, "four"},
            {5, "five"},
            {6, "six"},
            {7, "seven"},
            {8, "eight"},
            {9, "nine"}};

        int mask = 0;
        while (n > 0) {
            int x = n % 10;
            n /= 10;
            for (char c : d.at(x)) {
                mask ^= 1 << (c - 'a');
            }
        }
        return __builtin_popcount(mask);
    }
};
```

#### Go

```go
func countOddLetters(n int) int {
	d := map[int]string{
		0: "zero",
		1: "one",
		2: "two",
		3: "three",
		4: "four",
		5: "five",
		6: "six",
		7: "seven",
		8: "eight",
		9: "nine",
	}

	mask := 0
	for n > 0 {
		x := n % 10
		n /= 10
		for _, c := range d[x] {
			mask ^= 1 << (c - 'a')
		}
	}

	return bits.OnesCount32(uint32(mask))
}
```

#### TypeScript

```ts
function countOddLetters(n: number): number {
    const d: Record<number, string> = {
        0: 'zero',
        1: 'one',
        2: 'two',
        3: 'three',
        4: 'four',
        5: 'five',
        6: 'six',
        7: 'seven',
        8: 'eight',
        9: 'nine',
    };

    let mask = 0;
    while (n > 0) {
        const x = n % 10;
        n = Math.floor(n / 10);
        for (const c of d[x]) {
            mask ^= 1 << (c.charCodeAt(0) - 'a'.charCodeAt(0));
        }
    }

    return bitCount(mask);
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_odd_letters(mut n: i32) -> i32 {
        use std::collections::HashMap;

        let d: HashMap<i32, &str> = [
            (0, "zero"),
            (1, "one"),
            (2, "two"),
            (3, "three"),
            (4, "four"),
            (5, "five"),
            (6, "six"),
            (7, "seven"),
            (8, "eight"),
            (9, "nine"),
        ]
        .iter()
        .cloned()
        .collect();

        let mut mask: u32 = 0;

        while n > 0 {
            let x = n % 10;
            n /= 10;
            if let Some(word) = d.get(&x) {
                for c in word.chars() {
                    let bit = 1 << (c as u8 - b'a');
                    mask ^= bit as u32;
                }
            }
        }

        mask.count_ones() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
