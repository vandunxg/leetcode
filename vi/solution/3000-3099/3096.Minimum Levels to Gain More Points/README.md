---
comments: true
difficulty: Medium
rating: 1500
source: Biweekly Contest 127 Q2
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [3096. Minimum Levels to Gain More Points](https://leetcode.com/problems/minimum-levels-to-gain-more-points)

[中文文档](/solution/3000-3099/3096.Minimum%20Levels%20to%20Gain%20More%20Points/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng nhị phân <code>possible</code> có độ dài <code>n</code>.</p>

<p>Alice và Bob chơi một trò chơi gồm <code>n</code> cấp độ. Một số cấp độ <strong>không thể</strong> vượt qua, còn các cấp độ khác thì <strong>luôn</strong> có thể vượt qua. Cụ thể, nếu <code>possible[i] == 0</code>, thì cấp độ thứ <code>i<sup>th</sup></code> là <strong>không thể</strong> vượt qua đối với <strong>cả hai</strong> người chơi. Người chơi được <code>1</code> điểm khi vượt qua một cấp độ và mất <code>1</code> điểm nếu không vượt qua được cấp độ đó.</p>

<p>Ban đầu, Alice sẽ chơi một số cấp độ theo <strong>thứ tự đã cho</strong>, bắt đầu từ cấp độ <code>0<sup>th</sup></code>, sau đó Bob sẽ chơi phần cấp độ còn lại.</p>

<p>Alice muốn biết <strong>số cấp độ nhỏ nhất</strong> mà cô ấy nên chơi để có nhiều điểm hơn Bob, nếu cả hai người chơi đều chơi tối ưu để <strong>tối đa hóa</strong> số điểm của mình.</p>

<p>Trả về <em><strong>số cấp độ nhỏ nhất Alice nên chơi để có nhiều điểm hơn Bob</strong></em>. <em>Nếu <strong>không thể</strong>, trả về</em> <code>-1</code>.</p>

<p><strong>Lưu ý</strong> rằng mỗi người chơi phải chơi ít nhất <code>1</code> cấp độ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">possible = [1,0,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hãy xét tất cả các cấp độ mà Alice có thể chơi đến:</p>

<ul>
	<li>Nếu Alice chỉ chơi cấp độ 0 và Bob chơi các cấp độ còn lại, Alice có 1 điểm, còn Bob có -1 + 1 - 1 = -1 điểm.</li>
	<li>Nếu Alice chơi đến cấp độ 1 và Bob chơi các cấp độ còn lại, Alice có 1 - 1 = 0 điểm, còn Bob có 1 - 1 = 0 điểm.</li>
	<li>Nếu Alice chơi đến cấp độ 2 và Bob chơi các cấp độ còn lại, Alice có 1 - 1 + 1 = 1 điểm, còn Bob có -1 điểm.</li>
</ul>

<p>Alice phải chơi tối thiểu 1 cấp độ để có nhiều điểm hơn.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">possible = [1,1,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hãy xét tất cả các cấp độ mà Alice có thể chơi đến:</p>

<ul>
	<li>Nếu Alice chỉ chơi cấp độ 0 và Bob chơi các cấp độ còn lại, Alice có 1 điểm, còn Bob có 4 điểm.</li>
	<li>Nếu Alice chơi đến cấp độ 1 và Bob chơi các cấp độ còn lại, Alice có 2 điểm, còn Bob có 3 điểm.</li>
	<li>Nếu Alice chơi đến cấp độ 2 và Bob chơi các cấp độ còn lại, Alice có 3 điểm, còn Bob có 2 điểm.</li>
	<li>Nếu Alice chơi đến cấp độ 3 và Bob chơi các cấp độ còn lại, Alice có 4 điểm, còn Bob có 1 điểm.</li>
</ul>

<p>Alice phải chơi tối thiểu 3 cấp độ để có nhiều điểm hơn.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">possible = [0,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách duy nhất là mỗi người chơi chơi 1 cấp độ. Alice chơi cấp độ 0 và mất 1 điểm. Bob chơi cấp độ 1 và mất 1 điểm. Vì hai người có số điểm bằng nhau, Alice không thể có nhiều điểm hơn Bob.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == possible.length &lt;= 10<sup>5</sup></code></li>
	<li><code>possible[i]</code> là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Phần tiền tố thuộc về người chơi thứ nhất và phần hậu tố thuộc về người chơi thứ hai; vượt qua được cấp độ tương ứng với $+1$, còn thất bại tương ứng với $-1$. Người chơi thứ nhất phải có nhiều điểm hơn một cách nghiêm ngặt và phải chừa lại ít nhất một cấp độ. $n \le 10^5$.
>
> Tổng $s$ là cố định. Sau $i$ cấp độ, người chơi thứ nhất có $t$ điểm và đối thủ có $s-t$ điểm, nên ta cần $t>s-t$.
>
> Tính $s$, sau đó cộng dồn $t$ trên $n-1$ vị trí đầu tiên và kiểm tra bất đẳng thức.

<!-- thinking:end -->

Trước hết, ta tính tổng điểm mà cả hai người chơi có thể nhận được, ký hiệu là $s$.

Sau đó, ta liệt kê số cấp độ mà người chơi 1 có thể hoàn thành, ký hiệu là $i$, theo thứ tự tăng dần. Ta tính tổng điểm mà người chơi 1 nhận được, ký hiệu là $t$. Nếu $t > s - t$, thì số cấp độ người chơi 1 cần hoàn thành là $i$.

Nếu đã liệt kê $n - 1$ cấp độ đầu tiên mà vẫn chưa tìm được $i$ thỏa mãn, ta trả về $-1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumLevels(self, possible: List[int]) -> int:
        s = sum(-1 if x == 0 else 1 for x in possible)
        t = 0
        for i, x in enumerate(possible[:-1], 1):
            t += -1 if x == 0 else 1
            if t > s - t:
                return i
        return -1
```

#### Java

```java
class Solution {
    public int minimumLevels(int[] possible) {
        int s = 0;
        for (int x : possible) {
            s += x == 0 ? -1 : 1;
        }
        int t = 0;
        for (int i = 1; i < possible.length; ++i) {
            t += possible[i - 1] == 0 ? -1 : 1;
            if (t > s - t) {
                return i;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumLevels(vector<int>& possible) {
        int s = 0;
        for (int x : possible) {
            s += x == 0 ? -1 : 1;
        }
        int t = 0;
        for (int i = 1; i < possible.size(); ++i) {
            t += possible[i - 1] == 0 ? -1 : 1;
            if (t > s - t) {
                return i;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minimumLevels(possible []int) int {
	s := 0
	for _, x := range possible {
		if x == 0 {
			x = -1
		}
		s += x
	}
	t := 0
	for i, x := range possible[:len(possible)-1] {
		if x == 0 {
			x = -1
		}
		t += x
		if t > s-t {
			return i + 1
		}
	}
	return -1
}
```

#### TypeScript

```ts
function minimumLevels(possible: number[]): number {
    const s = possible.reduce((acc, x) => acc + (x === 0 ? -1 : 1), 0);
    let t = 0;
    for (let i = 1; i < possible.length; ++i) {
        t += possible[i - 1] === 0 ? -1 : 1;
        if (t > s - t) {
            return i;
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
