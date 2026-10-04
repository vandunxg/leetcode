---
comments: true
difficulty: Easy
rating: 1350
source: Biweekly Contest 99 Q1
tags:
    - Greedy
    - Math
    - Sorting
---

<!-- problem:start -->

# [2578. Split With Minimum Sum](https://leetcode.com/problems/split-with-minimum-sum)

[中文文档](/solution/2500-2599/2578.Split%20With%20Minimum%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>num</code>, hãy tách nó thành hai số nguyên không âm <code>num1</code> và <code>num2</code> sao cho:</p>

<ul>
	<li>Phép nối của <code>num1</code> và <code>num2</code> là một hoán vị của <code>num</code>.

    <ul>
    <li>Nói cách khác, tổng số lần xuất hiện của mỗi chữ số trong <code>num1</code> và <code>num2</code> bằng số lần xuất hiện của chữ số đó trong <code>num</code>.</li>
    </ul>
    </li>
    <li><code>num1</code> và <code>num2</code> có thể chứa các số 0 ở đầu.</li>

</ul>

<p>Trả về <em>tổng <strong>nhỏ nhất</strong> có thể có của</em> <code>num1</code> <em>và</em> <code>num2</code>.</p>

<p><strong>Ghi chú:</strong></p>

<ul>
	<li>Đảm bảo rằng <code>num</code> không chứa số 0 ở đầu.</li>
	<li>Thứ tự xuất hiện của các chữ số trong <code>num1</code> và <code>num2</code> có thể khác với thứ tự xuất hiện của chúng trong <code>num</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 4325
<strong>Đầu ra:</strong> 59
<strong>Giải thích:</strong> Ta có thể tách 4325 sao cho <code>num1</code> là 24 và <code>num2</code> là 35, khi đó tổng bằng 59. Ta có thể chứng minh rằng 59 thực sự là tổng nhỏ nhất có thể có.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 687
<strong>Đầu ra:</strong> 75
<strong>Giải thích:</strong> Ta có thể tách 687 sao cho <code>num1</code> là 68 và <code>num2</code> là 7, khi đó tổng tối ưu là 75.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>10 &lt;= num &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Tách các chữ số của $num$ thành hai số nguyên sao cho tổng của chúng nhỏ nhất. Các chữ số ở hàng cao hơn cần nhỏ, vì vậy lần lượt dùng các chữ số nhỏ nhất cho những hàng đó của hai số.
>
> Sau khi đếm các chữ số từ $0$ đến $9$, lần lượt nối chúng theo thứ tự tăng dần vào hai số, giữ cho độ dài của chúng gần nhau và các chữ số đầu nhỏ nhất.

<!-- thinking:end -->

Đầu tiên, ta dùng một hash table hoặc mảng $cnt$ để đếm số lần xuất hiện của mỗi chữ số trong $num$, đồng thời dùng biến $n$ để ghi nhận số chữ số của $num$.

Tiếp theo, ta duyệt qua từng chữ số $i$ trong $nums$ và lần lượt phân bổ các chữ số trong $cnt$ cho $num1$ và $num2$ theo thứ tự tăng dần, rồi ghi chúng vào mảng $ans$ có độ dài $2$. Cuối cùng, ta trả về tổng của hai số trong $ans$.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(C)$. Trong đó, $n$ là số chữ số của $num$; còn $C$ là số chữ số khác nhau xuất hiện trong $num$, với bài này $C \leq 10$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def splitNum(self, num: int) -> int:
        cnt = Counter()
        n = 0
        while num:
            cnt[num % 10] += 1
            num //= 10
            n += 1
        ans = [0] * 2
        j = 0
        for i in range(n):
            while cnt[j] == 0:
                j += 1
            cnt[j] -= 1
            ans[i & 1] = ans[i & 1] * 10 + j
        return sum(ans)
```

#### Java

```java
class Solution {
    public int splitNum(int num) {
        int[] cnt = new int[10];
        int n = 0;
        for (; num > 0; num /= 10) {
            ++cnt[num % 10];
            ++n;
        }
        int[] ans = new int[2];
        for (int i = 0, j = 0; i < n; ++i) {
            while (cnt[j] == 0) {
                ++j;
            }
            --cnt[j];
            ans[i & 1] = ans[i & 1] * 10 + j;
        }
        return ans[0] + ans[1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int splitNum(int num) {
        int cnt[10]{};
        int n = 0;
        for (; num; num /= 10) {
            ++cnt[num % 10];
            ++n;
        }
        int ans[2]{};
        for (int i = 0, j = 0; i < n; ++i) {
            while (cnt[j] == 0) {
                ++j;
            }
            --cnt[j];
            ans[i & 1] = ans[i & 1] * 10 + j;
        }
        return ans[0] + ans[1];
    }
};
```

#### Go

```go
func splitNum(num int) int {
	cnt := [10]int{}
	n := 0
	for ; num > 0; num /= 10 {
		cnt[num%10]++
		n++
	}
	ans := [2]int{}
	for i, j := 0, 0; i < n; i++ {
		for cnt[j] == 0 {
			j++
		}
		cnt[j]--
		ans[i&1] = ans[i&1]*10 + j
	}
	return ans[0] + ans[1]
}
```

#### TypeScript

```ts
function splitNum(num: number): number {
    const cnt: number[] = Array(10).fill(0);
    let n = 0;
    for (; num > 0; num = Math.floor(num / 10)) {
        ++cnt[num % 10];
        ++n;
    }
    const ans: number[] = Array(2).fill(0);
    for (let i = 0, j = 0; i < n; ++i) {
        while (cnt[j] === 0) {
            ++j;
        }
        --cnt[j];
        ans[i & 1] = ans[i & 1] * 10 + j;
    }
    return ans[0] + ans[1];
}
```

#### Rust

```rust
impl Solution {
    pub fn split_num(mut num: i32) -> i32 {
        let mut cnt = vec![0; 10];
        let mut n = 0;

        while num != 0 {
            cnt[(num as usize) % 10] += 1;
            num /= 10;
            n += 1;
        }

        let mut ans = vec![0; 2];
        let mut j = 0;
        for i in 0..n {
            while cnt[j] == 0 {
                j += 1;
            }
            cnt[j] -= 1;

            ans[i & 1] = ans[i & 1] * 10 + (j as i32);
        }

        ans[0] + ans[1]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tự đếm các chữ số trong một lượt duyệt. Vì có nhiều nhất mười chữ số, ta có thể sắp xếp chuỗi thập phân rồi nối các vị trí chẵn và lẻ để tạo ra cùng một cách tách với ít code hơn.

<!-- thinking:end -->

Ta có thể chuyển $num$ thành một chuỗi hoặc mảng ký tự, sau đó sắp xếp nó, rồi lần lượt phân bổ các chữ số trong mảng đã sắp xếp cho $num1$ và $num2$ theo thứ tự tăng dần. Cuối cùng, ta trả về tổng của $num1$ và $num2$.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số chữ số của $num$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def splitNum(self, num: int) -> int:
        s = sorted(str(num))
        return int(''.join(s[::2])) + int(''.join(s[1::2]))
```

#### Java

```java
class Solution {
    public int splitNum(int num) {
        char[] s = (num + "").toCharArray();
        Arrays.sort(s);
        int[] ans = new int[2];
        for (int i = 0; i < s.length; ++i) {
            ans[i & 1] = ans[i & 1] * 10 + s[i] - '0';
        }
        return ans[0] + ans[1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int splitNum(int num) {
        string s = to_string(num);
        sort(s.begin(), s.end());
        int ans[2]{};
        for (int i = 0; i < s.size(); ++i) {
            ans[i & 1] = ans[i & 1] * 10 + s[i] - '0';
        }
        return ans[0] + ans[1];
    }
};
```

#### Go

```go
func splitNum(num int) int {
	s := []byte(strconv.Itoa(num))
	sort.Slice(s, func(i, j int) bool { return s[i] < s[j] })
	ans := [2]int{}
	for i, c := range s {
		ans[i&1] = ans[i&1]*10 + int(c-'0')
	}
	return ans[0] + ans[1]
}
```

#### TypeScript

```ts
function splitNum(num: number): number {
    const s: string[] = String(num).split('');
    s.sort();
    const ans: number[] = Array(2).fill(0);
    for (let i = 0; i < s.length; ++i) {
        ans[i & 1] = ans[i & 1] * 10 + Number(s[i]);
    }
    return ans[0] + ans[1];
}
```

#### Rust

```rust
impl Solution {
    pub fn split_num(num: i32) -> i32 {
        let mut s = num.to_string().into_bytes();
        s.sort_unstable();

        let mut ans = vec![0; 2];
        for (i, c) in s.iter().enumerate() {
            ans[i & 1] = ans[i & 1] * 10 + ((c - b'0') as i32);
        }

        ans[0] + ans[1]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
