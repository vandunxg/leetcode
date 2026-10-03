---
comments: true
difficulty: Easy
rating: 1279
source: Biweekly Contest 78 Q1
tags:
    - Math
    - String
    - Sliding Window
---

<!-- problem:start -->

# [2269. Find the K-Beauty of a Number](https://leetcode.com/problems/find-the-k-beauty-of-a-number)

[Tài liệu tiếng Trung](/solution/2200-2299/2269.Find%20the%20K-Beauty%20of%20a%20Number/README.md)

## Mô tả

<!-- description:start -->

<p><strong>k-beauty</strong> của một số nguyên <code>num</code> được định nghĩa là số lượng <strong>chuỗi con</strong> của <code>num</code> khi đọc dưới dạng một chuỗi, thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Có độ dài bằng <code>k</code>.</li>
	<li>Là một ước của <code>num</code>.</li>
</ul>

<p>Cho hai số nguyên <code>num</code> và <code>k</code>, hãy trả về <em>k-beauty của </em><code>num</code>.</p>

<p>Lưu ý:</p>

<ul>
	<li><strong>Các số 0 ở đầu</strong> được phép.</li>
	<li><code>0</code> không phải là ước của bất kỳ giá trị nào.</li>
</ul>

<p><strong>Chuỗi con</strong> là một dãy ký tự liên tiếp trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 240, k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các chuỗi con độ dài k của num là:
- &quot;24&quot; từ &quot;<strong><u>24</u></strong>0&quot;: 24 là một ước của 240.
- &quot;40&quot; từ &quot;2<u><strong>40</strong></u>&quot;: 40 là một ước của 240.
Do đó, k-beauty là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 430043, k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Các chuỗi con độ dài k của num là:
- &quot;43&quot; từ &quot;<u><strong>43</strong></u>0043&quot;: 43 là một ước của 430043.
- &quot;30&quot; từ &quot;4<u><strong>30</strong></u>043&quot;: 30 không phải là một ước của 430043.
- &quot;00&quot; từ &quot;43<u><strong>00</strong></u>43&quot;: 0 không phải là một ước của 430043.
- &quot;04&quot; từ &quot;430<u><strong>04</strong></u>3&quot;: 4 không phải là một ước của 430043.
- &quot;43&quot; từ &quot;4300<u><strong>43</strong></u>&quot;: 43 là một ước của 430043.
Do đó, k-beauty là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= num.length</code> (coi <code>num</code> là một chuỗi)</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> $k$-beauty đếm các chuỗi con độ dài $k$ là ước của số đó. Vì $num \le 10^9$ có ít chữ số, ta chỉ cần chuyển đổi từng cửa sổ độ dài $k$; bỏ qua ước bằng 0.
>
> Chuyển $num$ thành chuỗi và lấy $s[i:i+k]$ tại mỗi vị trí bắt đầu.

<!-- thinking:end -->

Ta có thể chuyển $num$ thành chuỗi $s$, sau đó liệt kê tất cả chuỗi con độ dài $k$ của $s$, chuyển chúng thành số nguyên $t$ và kiểm tra xem $t$ có chia hết cho $num$ hay không. Nếu có, ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(\log num \times k)$, và độ phức tạp không gian là $O(\log num + k)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def divisorSubstrings(self, num: int, k: int) -> int:
        ans = 0
        s = str(num)
        for i in range(len(s) - k + 1):
            t = int(s[i : i + k])
            if t and num % t == 0:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int divisorSubstrings(int num, int k) {
        int ans = 0;
        String s = "" + num;
        for (int i = 0; i < s.length() - k + 1; ++i) {
            int t = Integer.parseInt(s.substring(i, i + k));
            if (t != 0 && num % t == 0) {
                ++ans;
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
    int divisorSubstrings(int num, int k) {
        int ans = 0;
        string s = to_string(num);
        for (int i = 0; i < s.size() - k + 1; ++i) {
            int t = stoi(s.substr(i, k));
            ans += t && num % t == 0;
        }
        return ans;
    }
};
```

#### Go

```go
func divisorSubstrings(num int, k int) int {
	ans := 0
	s := strconv.Itoa(num)
	for i := 0; i < len(s)-k+1; i++ {
		t, _ := strconv.Atoi(s[i : i+k])
		if t > 0 && num%t == 0 {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function divisorSubstrings(num: number, k: number): number {
    let ans = 0;
    const s = num.toString();
    for (let i = 0; i < s.length - k + 1; ++i) {
        const t = parseInt(s.substring(i, i + k));
        if (t !== 0 && num % t === 0) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tạo lại một số nguyên từ một lát cắt ở mỗi lần. Một cửa sổ chữ số thập phân có thể loại bỏ chữ số thấp nhất và thêm một chữ số mới vào đầu trong $O(1)$.
>
> Lấy $k$ chữ số thấp nhất làm $x$, sau đó liên tục chia $x$ cho $10$ và thêm chữ số tiếp theo vào đầu, đồng thời kiểm tra tính chia hết.

<!-- thinking:end -->

Ta có thể duy trì một sliding window có độ dài $k$. Ban đầu, cửa sổ chứa $k$ chữ số thấp nhất của $num$. Sau đó, ở mỗi bước lặp, ta dịch cửa sổ sang phải một chữ số, cập nhật số trong cửa sổ và kiểm tra xem số đó có chia hết cho $num$ hay không. Nếu có, ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(\log num)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def divisorSubstrings(self, num: int, k: int) -> int:
        x, p = 0, 1
        t = num
        for _ in range(k):
            t, v = divmod(t, 10)
            x = p * v + x
            p *= 10
        ans = int(x != 0 and num % x == 0)
        p //= 10
        while t:
            x //= 10
            t, v = divmod(t, 10)
            x = p * v + x
            ans += int(x != 0 and num % x == 0)
        return ans
```

#### Java

```java
class Solution {
    public int divisorSubstrings(int num, int k) {
        int x = 0, p = 1;
        int t = num;
        for (; k > 0; --k) {
            int v = t % 10;
            t /= 10;
            x = p * v + x;
            p *= 10;
        }
        int ans = x != 0 && num % x == 0 ? 1 : 0;
        for (p /= 10; t > 0; t /= 10) {
            x /= 10;
            int v = t % 10;
            x = p * v + x;
            ans += (x != 0 && num % x == 0 ? 1 : 0);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int divisorSubstrings(int num, int k) {
        int x = 0;
        long long p = 1;
        int t = num;
        for (; k > 0; --k) {
            int v = t % 10;
            t /= 10;
            x = p * v + x;
            p *= 10;
        }
        int ans = x != 0 && num % x == 0 ? 1 : 0;
        for (p /= 10; t > 0; t /= 10) {
            x /= 10;
            int v = t % 10;
            x = p * v + x;
            ans += (x != 0 && num % x == 0 ? 1 : 0);
        }
        return ans;
    }
};
```

#### Go

```go
func divisorSubstrings(num int, k int) (ans int) {
	x, p, t := 0, 1, num
	for ; k > 0; k-- {
		v := t % 10
		t /= 10
		x = p*v + x
		p *= 10
	}
	if x != 0 && num%x == 0 {
		ans++
	}
	for p /= 10; t > 0; t /= 10 {
		x /= 10
		v := t % 10
		x = p*v + x
		if x != 0 && num%x == 0 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function divisorSubstrings(num: number, k: number): number {
    let [x, p, t] = [0, 1, num];
    for (; k > 0; k--) {
        const v = t % 10;
        t = Math.floor(t / 10);
        x = p * v + x;
        p *= 10;
    }
    let ans = x !== 0 && num % x === 0 ? 1 : 0;
    for (p = Math.floor(p / 10); t > 0; t = Math.floor(t / 10)) {
        x = Math.floor(x / 10);
        x = p * (t % 10) + x;
        ans += x !== 0 && num % x === 0 ? 1 : 0;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
