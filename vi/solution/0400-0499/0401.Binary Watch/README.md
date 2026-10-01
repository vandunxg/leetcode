---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
    - Backtracking
---

<!-- problem:start -->

# [401. Binary Watch](https://leetcode.com/problems/binary-watch)

[中文文档](/solution/0400-0499/0401.Binary%20Watch/README.md)

## Mô tả

<!-- description:start -->

<p>Đồng hồ nhị phân có 4 đèn LED phía trên biểu diễn giờ (0-11) và 6 đèn LED phía dưới biểu diễn phút (0-59). Mỗi đèn LED biểu diễn bit 0 hoặc 1; bit có trọng số thấp nhất nằm bên phải.</p>

<ul>
	<li>Ví dụ, đồng hồ nhị phân dưới đây hiển thị <code>&quot;4:51&quot;</code>.</li>
</ul>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0400-0499/0401.Binary%20Watch/images/binarywatch.jpg" style="width: 500px; height: 500px;" /></p>

<p>Cho số nguyên <code>turnedOn</code> biểu thị số đèn LED đang bật (không xét AM/PM), hãy trả về <em>tất cả thời điểm mà đồng hồ có thể hiển thị</em>. Có thể trả lời theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>Phần giờ không được có số 0 ở đầu.</p>

<ul>
	<li>Ví dụ, <code>&quot;01:00&quot;</code> không hợp lệ; phải viết là <code>&quot;1:00&quot;</code>.</li>
</ul>

<p>Phần phút phải gồm hai chữ số và có thể có số 0 ở đầu.</p>

<ul>
	<li>Ví dụ, <code>&quot;10:2&quot;</code> không hợp lệ; phải viết là <code>&quot;10:02&quot;</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> turnedOn = 1
<strong>Đầu ra:</strong> ["0:01","0:02","0:04","0:08","0:16","0:32","1:00","2:00","4:00","8:00"]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> turnedOn = 9
<strong>Đầu ra:</strong> []
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= turnedOn &lt;= 10</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê các tổ hợp

<!-- thinking:start -->

> **Tư duy**
>
> Giờ nằm trong $[0,12)$, phút nằm trong $[0,60)$ nên chỉ có $720$ cách hiển thị. Số đèn sáng bằng tổng số bit $1$, vì vậy ta duyệt từng cặp $(i,j)$ và đếm số bit.
>
> Không gian tìm kiếm có kích thước cố định. Duyệt các thời điểm hợp lệ rồi so sánh với $\textit{turnedOn}$ đơn giản hơn chọn các tập con đèn rồi kiểm tra thời gian có hợp lệ hay không.

<!-- thinking:end -->

Có thể chuyển bài toán thành tìm tất cả tổ hợp của $i \in [0, 12)$ và $j \in [0, 60)$.

Một tổ hợp hợp lệ khi tổng số bit $1$ trong biểu diễn nhị phân của $i$ và $j$ bằng $\textit{turnedOn}$.

Độ phức tạp thời gian và không gian đều là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def readBinaryWatch(self, turnedOn: int) -> List[str]:
        return [
            '{:d}:{:02d}'.format(i, j)
            for i in range(12)
            for j in range(60)
            if (bin(i) + bin(j)).count('1') == turnedOn
        ]
```

#### Java

```java
class Solution {
    public List<String> readBinaryWatch(int turnedOn) {
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < 12; ++i) {
            for (int j = 0; j < 60; ++j) {
                if (Integer.bitCount(i) + Integer.bitCount(j) == turnedOn) {
                    ans.add(String.format("%d:%02d", i, j));
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
    vector<string> readBinaryWatch(int turnedOn) {
        vector<string> ans;
        for (int i = 0; i < 12; ++i) {
            for (int j = 0; j < 60; ++j) {
                if (__builtin_popcount(i) + __builtin_popcount(j) == turnedOn) {
                    ans.push_back(to_string(i) + ":" + (j < 10 ? "0" : "") + to_string(j));
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func readBinaryWatch(turnedOn int) []string {
	var ans []string
	for i := 0; i < 12; i++ {
		for j := 0; j < 60; j++ {
			if bits.OnesCount(uint(i))+bits.OnesCount(uint(j)) == turnedOn {
				ans = append(ans, fmt.Sprintf("%d:%02d", i, j))
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function readBinaryWatch(turnedOn: number): string[] {
    const ans: string[] = [];

    for (let i = 0; i < 12; ++i) {
        for (let j = 0; j < 60; ++j) {
            if (bitCount(i) + bitCount(j) === turnedOn) {
                ans.push(`${i}:${j.toString().padStart(2, '0')}`);
            }
        }
    }

    return ans;
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
    pub fn read_binary_watch(turned_on: i32) -> Vec<String> {
        let mut ans: Vec<String> = Vec::new();

        for i in 0u32..12 {
            for j in 0u32..60 {
                if (i.count_ones() + j.count_ones()) as i32 == turned_on {
                    ans.push(format!("{}:{:02}", i, j));
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

### Lời giải 2: Liệt kê nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt các giá trị giờ và phút hợp lệ. Mười đèn LED cũng có thể biểu diễn bằng một mask trong $[0,2^{10})$: 4 bit cao biểu diễn giờ, 6 bit thấp biểu diễn phút. Chỉ giữ các mask có $h<12$, $m<60$ và số bit $1$ bằng $\textit{turnedOn}$.
>
> Một vòng lặp duy nhất tương ứng với cách bố trí phần cứng; độ phức tạp tiệm cận vẫn là $O(1)$.

<!-- thinking:end -->

Ta có thể dùng $10$ bit nhị phân để biểu diễn đồng hồ: 4 bit đầu biểu diễn giờ, 6 bit cuối biểu diễn phút. Duyệt từng số trong $[0, 2^{10})$, kiểm tra số bit $1$ trong biểu diễn nhị phân có bằng $\textit{turnedOn}$ hay không; nếu bằng thì chuyển sang định dạng thời gian và thêm vào kết quả.

Độ phức tạp thời gian và không gian đều là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def readBinaryWatch(self, turnedOn: int) -> List[str]:
        ans = []
        for i in range(1 << 10):
            h, m = i >> 6, i & 0b111111
            if h < 12 and m < 60 and i.bit_count() == turnedOn:
                ans.append('{:d}:{:02d}'.format(h, m))
        return ans
```

#### Java

```java
class Solution {
    public List<String> readBinaryWatch(int turnedOn) {
        List<String> ans = new ArrayList<>();
        for (int i = 0; i < 1 << 10; ++i) {
            int h = i >> 6, m = i & 0b111111;
            if (h < 12 && m < 60 && Integer.bitCount(i) == turnedOn) {
                ans.add(String.format("%d:%02d", h, m));
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
    vector<string> readBinaryWatch(int turnedOn) {
        vector<string> ans;
        for (int i = 0; i < 1 << 10; ++i) {
            int h = i >> 6, m = i & 0b111111;
            if (h < 12 && m < 60 && __builtin_popcount(i) == turnedOn) {
                ans.push_back(to_string(h) + ":" + (m < 10 ? "0" : "") + to_string(m));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func readBinaryWatch(turnedOn int) []string {
	var ans []string
	for i := 0; i < 1<<10; i++ {
		h, m := i>>6, i&0b111111
		if h < 12 && m < 60 && bits.OnesCount(uint(i)) == turnedOn {
			ans = append(ans, fmt.Sprintf("%d:%02d", h, m))
		}
	}
	return ans
}
```

#### TypeScript

```ts
function readBinaryWatch(turnedOn: number): string[] {
    const ans: string[] = [];

    for (let i = 0; i < 1 << 10; ++i) {
        const h = i >> 6;
        const m = i & 0b111111;

        if (h < 12 && m < 60 && bitCount(i) === turnedOn) {
            ans.push(`${h}:${m < 10 ? '0' : ''}${m}`);
        }
    }

    return ans;
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
    pub fn read_binary_watch(turned_on: i32) -> Vec<String> {
        let mut ans: Vec<String> = Vec::new();

        for i in 0u32..(1 << 10) {
            let h = i >> 6;
            let m = i & 0b111111;

            if h < 12 && m < 60 && i.count_ones() as i32 == turned_on {
                ans.push(format!("{}:{:02}", h, m));
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
