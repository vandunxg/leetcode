---
comments: true
difficulty: Easy
rating: 1426
source: Biweekly Contest 89 Q1
tags:
    - String
    - Enumeration
---

<!-- problem:start -->

# [2437. Number of Valid Clock Times](https://leetcode.com/problems/number-of-valid-clock-times)

[中文文档](/solution/2400-2499/2437.Number%20of%20Valid%20Clock%20Times/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một chuỗi có độ dài <code>5</code> tên là <code>time</code>, biểu diễn thời gian hiện tại trên đồng hồ điện tử theo định dạng <code>&quot;hh:mm&quot;</code>. Thời gian <strong>sớm nhất</strong> có thể là <code>&quot;00:00&quot;</code> và thời gian <strong>muộn nhất</strong> có thể là <code>&quot;23:59&quot;</code>.</p>

<p>Trong chuỗi <code>time</code>, các chữ số được biểu diễn bằng ký hiệu <code>?</code> là <strong>chưa biết</strong> và phải được <strong>thay thế</strong> bằng một chữ số từ <code>0</code> đến <code>9</code>.</p>

<p>Trả về<em> một số nguyên </em><code>answer</code><em>, là số thời gian hợp lệ có thể tạo ra bằng cách thay thế mọi </em><code>?</code><em>&nbsp;bằng một chữ số từ </em><code>0</code><em> đến </em><code>9</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> time = &quot;?5:00&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể thay ? bằng 0 hoặc 1, tạo ra &quot;05:00&quot; hoặc &quot;15:00&quot;. Lưu ý rằng ta không thể thay bằng 2, vì thời gian &quot;25:00&quot; không hợp lệ. Tổng cộng có hai lựa chọn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> time = &quot;0?:0?&quot;
<strong>Đầu ra:</strong> 100
<strong>Giải thích:</strong> Mỗi ? có thể được thay bằng bất kỳ chữ số nào từ 0 đến 9, nên có tổng cộng 100 lựa chọn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> time = &quot;??:??&quot;
<strong>Đầu ra:</strong> 1440
<strong>Giải thích:</strong> Có 24 lựa chọn cho phần giờ và 60 lựa chọn cho phần phút. Tổng cộng có 24 * 60 = 1440 lựa chọn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>time</code> là một chuỗi hợp lệ có độ dài <code>5</code> theo định dạng <code>&quot;hh:mm&quot;</code>.</li>
	<li><code>&quot;00&quot; &lt;= hh &lt;= &quot;23&quot;</code></li>
	<li><code>&quot;00&quot; &lt;= mm &lt;= &quot;59&quot;</code></li>
	<li>Một số chữ số có thể được thay bằng <code>&#39;?&#39;</code> và cần được thay bằng các chữ số từ <code>0</code> đến <code>9</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có $24\times 60$ thời gian hợp lệ và nhiều nhất là bốn ký tự đại diện. Ta liệt kê mọi thời gian rồi đối chiếu với pattern, trong đó xem `?` là ký tự tự do.

<!-- thinking:end -->

Ta có thể liệt kê trực tiếp mọi thời gian từ $00:00$ đến $23:59$, sau đó kiểm tra xem mỗi thời gian có hợp lệ hay không; nếu hợp lệ thì tăng đáp án lên một.

Sau khi kết thúc việc liệt kê, trả về đáp án.

Độ phức tạp thời gian là $O(24 \times 60)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countTime(self, time: str) -> int:
        def check(s: str, t: str) -> bool:
            return all(a == b or b == '?' for a, b in zip(s, t))

        return sum(
            check(f'{h:02d}:{m:02d}', time) for h in range(24) for m in range(60)
        )
```

#### Java

```java
class Solution {
    public int countTime(String time) {
        int ans = 0;
        for (int h = 0; h < 24; ++h) {
            for (int m = 0; m < 60; ++m) {
                String s = String.format("%02d:%02d", h, m);
                int ok = 1;
                for (int i = 0; i < 5; ++i) {
                    if (s.charAt(i) != time.charAt(i) && time.charAt(i) != '?') {
                        ok = 0;
                        break;
                    }
                }
                ans += ok;
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
    int countTime(string time) {
        int ans = 0;
        for (int h = 0; h < 24; ++h) {
            for (int m = 0; m < 60; ++m) {
                char s[20];
                sprintf(s, "%02d:%02d", h, m);
                int ok = 1;
                for (int i = 0; i < 5; ++i) {
                    if (s[i] != time[i] && time[i] != '?') {
                        ok = 0;
                        break;
                    }
                }
                ans += ok;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countTime(time string) int {
	ans := 0
	for h := 0; h < 24; h++ {
		for m := 0; m < 60; m++ {
			s := fmt.Sprintf("%02d:%02d", h, m)
			ok := 1
			for i := 0; i < 5; i++ {
				if s[i] != time[i] && time[i] != '?' {
					ok = 0
					break
				}
			}
			ans += ok
		}
	}
	return ans
}
```

#### TypeScript

```ts
function countTime(time: string): number {
    let ans = 0;
    for (let h = 0; h < 24; ++h) {
        for (let m = 0; m < 60; ++m) {
            const s = `${h}`.padStart(2, '0') + ':' + `${m}`.padStart(2, '0');
            let ok = 1;
            for (let i = 0; i < 5; ++i) {
                if (s[i] !== time[i] && time[i] !== '?') {
                    ok = 0;
                    break;
                }
            }
            ans += ok;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_time(time: String) -> i32 {
        let mut ans = 0;

        for i in 0..24 {
            for j in 0..60 {
                let mut ok = true;
                let t = format!("{:02}:{:02}", i, j);

                for (k, ch) in time.chars().enumerate() {
                    if ch != '?' && ch != t.chars().nth(k).unwrap() {
                        ok = false;
                    }
                }

                if ok {
                    ans += 1;
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

### Lời giải 2: Liệt kê tối ưu

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 đã liệt kê mọi thời gian. Phần giờ và phần phút độc lập với nhau: đếm số khớp trong $00..23$ và trong $00..59$, sau đó nhân hai kết quả. Số vòng lặp giảm từ $1440$ xuống còn $84$.

<!-- thinking:end -->

Ta có thể liệt kê riêng phần giờ và phần phút, đếm có bao nhiêu giờ và phút thỏa mãn điều kiện, rồi nhân hai kết quả với nhau.

Độ phức tạp thời gian là $O(24 + 60)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countTime(self, time: str) -> int:
        def f(s: str, m: int) -> int:
            cnt = 0
            for i in range(m):
                a = s[0] == '?' or (int(s[0]) == i // 10)
                b = s[1] == '?' or (int(s[1]) == i % 10)
                cnt += a and b
            return cnt

        return f(time[:2], 24) * f(time[3:], 60)
```

#### Java

```java
class Solution {
    public int countTime(String time) {
        return f(time.substring(0, 2), 24) * f(time.substring(3), 60);
    }

    private int f(String s, int m) {
        int cnt = 0;
        for (int i = 0; i < m; ++i) {
            boolean a = s.charAt(0) == '?' || s.charAt(0) - '0' == i / 10;
            boolean b = s.charAt(1) == '?' || s.charAt(1) - '0' == i % 10;
            cnt += a && b ? 1 : 0;
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countTime(string time) {
        auto f = [](string s, int m) {
            int cnt = 0;
            for (int i = 0; i < m; ++i) {
                bool a = s[0] == '?' || s[0] - '0' == i / 10;
                bool b = s[1] == '?' || s[1] - '0' == i % 10;
                cnt += a && b;
            }
            return cnt;
        };
        return f(time.substr(0, 2), 24) * f(time.substr(3, 2), 60);
    }
};
```

#### Go

```go
func countTime(time string) int {
	f := func(s string, m int) (cnt int) {
		for i := 0; i < m; i++ {
			a := s[0] == '?' || int(s[0]-'0') == i/10
			b := s[1] == '?' || int(s[1]-'0') == i%10
			if a && b {
				cnt++
			}
		}
		return
	}
	return f(time[:2], 24) * f(time[3:], 60)
}
```

#### TypeScript

```ts
function countTime(time: string): number {
    const f = (s: string, m: number): number => {
        let cnt = 0;
        for (let i = 0; i < m; ++i) {
            const a = s[0] === '?' || s[0] === Math.floor(i / 10).toString();
            const b = s[1] === '?' || s[1] === (i % 10).toString();
            if (a && b) {
                ++cnt;
            }
        }
        return cnt;
    };
    return f(time.slice(0, 2), 24) * f(time.slice(3), 60);
}
```

#### Rust

```rust
impl Solution {
    pub fn count_time(time: String) -> i32 {
        let f = |s: &str, m: usize| -> i32 {
            let mut cnt = 0;
            let first = s.chars().nth(0).unwrap();
            let second = s.chars().nth(1).unwrap();

            for i in 0..m {
                let a = first == '?' || (first.to_digit(10).unwrap() as usize) == i / 10;

                let b = second == '?' || (second.to_digit(10).unwrap() as usize) == i % 10;

                if a && b {
                    cnt += 1;
                }
            }

            cnt
        };

        f(&time[..2], 24) * f(&time[3..], 60)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
