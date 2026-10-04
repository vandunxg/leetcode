---
comments: true
difficulty: Easy
rating: 1278
source: Weekly Contest 335 Q1
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [2582. Pass the Pillow](https://leetcode.com/problems/pass-the-pillow)

[中文文档](/solution/2500-2599/2582.Pass%20the%20Pillow/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> người đứng thành một hàng, được đánh số từ <code>1</code> đến <code>n</code>. Ban đầu, người đầu tiên trong hàng đang cầm một chiếc gối. Mỗi giây, người đang cầm gối chuyền nó cho người tiếp theo trong hàng. Khi gối đến cuối hàng, hướng chuyền thay đổi và mọi người tiếp tục chuyền gối theo hướng ngược lại.</p>

<ul>
	<li>Ví dụ, khi gối đến người thứ <code>n<sup>th</sup></code>, người đó chuyền gối cho người thứ <code>n - 1<sup>th</sup></code>, sau đó đến người thứ <code>n - 2<sup>th</sup></code> và tiếp tục như vậy.</li>
</ul>

<p>Cho hai số nguyên dương <code>n</code> và <code>time</code>, hãy trả về <em>chỉ số của người đang cầm gối sau </em><code>time</code><em> giây</em>.</p>
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, time = 5
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Mọi người chuyền gối theo thứ tự: 1 -&gt; 2 -&gt; 3 -&gt; 4 -&gt; 3 -&gt; 2.
Sau năm giây, người thứ 2<sup>nd</sup> đang cầm gối.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, time = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Mọi người chuyền gối theo thứ tự: 1 -&gt; 2 -&gt; 3.
Sau hai giây, người thứ 3<sup>r</sup><sup>d</sup> đang cầm gối.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= time &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài toán này giống với <a href="https://leetcode.com/problems/find-the-child-who-has-the-ball-after-k-seconds/description/" target="_blank"> 3178: Find the Child Who Has the Ball After K Seconds.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chiếc gối di chuyển qua lại trên $1..n$; ta cần tìm người cầm gối sau $\textit{time}$ giây. Vì $\textit{time}\le 1000$, chỉ cần đổi hướng sau mỗi giây là đủ.

<!-- thinking:end -->

Ta có thể mô phỏng quá trình chuyền gối. Sau mỗi lần chuyền, nếu gối đến đầu hoặc cuối hàng, ta đổi hướng, rồi tiếp tục chuyền gối theo hướng ngược lại.

Độ phức tạp thời gian là $O(time)$ và độ phức tạp không gian là $O(1)$, trong đó $time$ là thời gian đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def passThePillow(self, n: int, time: int) -> int:
        ans = k = 1
        for _ in range(time):
            ans += k
            if ans == 1 or ans == n:
                k *= -1
        return ans
```

#### Java

```java
class Solution {
    public int passThePillow(int n, int time) {
        int ans = 1, k = 1;
        while (time-- > 0) {
            ans += k;
            if (ans == 1 || ans == n) {
                k *= -1;
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
    int passThePillow(int n, int time) {
        int ans = 1, k = 1;
        while (time--) {
            ans += k;
            if (ans == 1 || ans == n) {
                k *= -1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func passThePillow(n int, time int) int {
	ans, k := 1, 1
	for ; time > 0; time-- {
		ans += k
		if ans == 1 || ans == n {
			k *= -1
		}
	}
	return ans
}
```

#### TypeScript

```ts
function passThePillow(n: number, time: number): number {
    let ans = 1,
        k = 1;
    while (time-- > 0) {
        ans += k;
        if (ans === 1 || ans === n) {
            k *= -1;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn pass_the_pillow(n: i32, time: i32) -> i32 {
        let mut ans = 1;
        let mut k = 1;

        for i in 1..=time {
            ans += k;

            if ans == 1 || ans == n {
                k *= -1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 có độ phức tạp tuyến tính theo $\textit{time}$. Mỗi lượt có $n-1$ bước; phép chia cho ta lượt và phần dư. Với lượt chẵn, gối đi sang phải từ $1$; với lượt lẻ, gối đi sang trái từ $n$.

<!-- thinking:end -->

Ta nhận thấy mỗi lượt có $n - 1$ lần chuyền. Vì vậy, ta chia $time$ cho $n - 1$ để nhận được số lượt $k$ mà gối đã được chuyền, sau đó lấy phần dư của $time$ modulo $n - 1$ để nhận được số lần chuyền còn lại $mod$ trong lượt hiện tại.

Tiếp theo, ta xét lượt hiện tại $k$:

- Nếu $k$ là số lẻ, hướng chuyền gối hiện tại là từ cuối hàng về đầu hàng, nên gối sẽ được chuyền cho người có số $n - mod$.
- Nếu $k$ là số chẵn, hướng chuyền gối hiện tại là từ đầu hàng đến cuối hàng, nên gối sẽ được chuyền cho người có số $mod + 1$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def passThePillow(self, n: int, time: int) -> int:
        k, mod = divmod(time, n - 1)
        return n - mod if k & 1 else mod + 1
```

#### Java

```java
class Solution {
    public int passThePillow(int n, int time) {
        int k = time / (n - 1);
        int mod = time % (n - 1);
        return (k & 1) == 1 ? n - mod : mod + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int passThePillow(int n, int time) {
        int k = time / (n - 1);
        int mod = time % (n - 1);
        return k & 1 ? n - mod : mod + 1;
    }
};
```

#### Go

```go
func passThePillow(n int, time int) int {
	k, mod := time/(n-1), time%(n-1)
	if k&1 == 1 {
		return n - mod
	}
	return mod + 1
}
```

#### TypeScript

```ts
function passThePillow(n: number, time: number): number {
    const k = time / (n - 1);
    const mod = time % (n - 1);
    return (k & 1) == 1 ? n - mod : mod + 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn pass_the_pillow(n: i32, time: i32) -> i32 {
        let mut k = time / (n - 1);
        let mut _mod = time % (n - 1);

        if (k & 1) == 1 {
            return n - _mod;
        }

        _mod + 1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
