---
comments: true
difficulty: Medium
tags:
    - Greedy
    - String
    - Sorting
---

<!-- problem:start -->

# [3119. Maximum Number of Potholes That Can Be Fixed 🔒](https://leetcode.com/problems/maximum-number-of-potholes-that-can-be-fixed)

[中文文档](/solution/3100-3199/3119.Maximum%20Number%20of%20Potholes%20That%20Can%20Be%20Fixed/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>road</code> chỉ gồm các ký tự <code>&quot;x&quot;</code> và <code>&quot;.&quot;</code>, trong đó mỗi <code>&quot;x&quot;</code> biểu thị một <em>ổ gà</em>, mỗi <code>&quot;.&quot;</code> biểu thị một đoạn đường bằng phẳng, cùng một số nguyên <code>budget</code>.</p>

<p>Trong một thao tác sửa chữa, bạn có thể sửa <code>n</code> ổ gà <strong>liên tiếp</strong> với chi phí <code>n + 1</code>.</p>

<p>Hãy trả về số ổ gà <strong>tối đa</strong> có thể sửa sao cho tổng chi phí của tất cả các lần sửa <strong>không vượt quá</strong> ngân sách đã cho.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">road = &quot;..&quot;, budget = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có ổ gà nào cần sửa.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">road = &quot;..xxxxx&quot;, budget = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sửa ba ổ gà đầu tiên (chúng nằm liên tiếp). Chi phí cần thiết là <code>3 + 1 = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">road = &quot;x.x.xxx...x&quot;, budget = 14</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có thể sửa tất cả các ổ gà. Tổng chi phí là <code>(1 + 1) + (1 + 1) + (3 + 1) + (1 + 1) = 10</code>, nằm trong ngân sách 14.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= road.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= budget &lt;= 10<sup>5</sup> + 1</code></li>
	<li><code>road</code> chỉ gồm các ký tự <code>&#39;.&#39;</code> và <code>&#39;x&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Một đoạn gồm $k$ ổ gà có chi phí sửa là $k+1$, trong khi ngân sách có hạn. Nếu chọn độ dài sửa riêng cho từng đoạn, số trường hợp sẽ tăng rất nhanh theo số đoạn.
>
> Các đoạn dài hơn sửa được nhiều ổ gà hơn trên mỗi đơn vị chi phí còn lại. Sau khi sửa được nhiều đoạn độ dài $k$ nhất có thể với ngân sách, các đoạn chưa dùng sẽ trở thành các đoạn độ dài $k-1$.
>
> Đếm số đoạn theo độ dài, sau đó duyệt từ $k$ lớn xuống nhỏ và chọn $t=\min(\textit{budget}/(k+1),cnt[k])$, cộng $t\cdot k$ vào đáp án, rồi gộp phần còn lại vào $cnt[k-1]$ cho đến khi hết ngân sách.

<!-- thinking:end -->

Đầu tiên, ta đếm số đoạn ổ gà liên tiếp theo từng độ dài và lưu trong mảng $cnt$, trong đó $cnt[k]$ biểu thị có $cnt[k]$ đoạn ổ gà liên tiếp độ dài $k$.

Vì muốn sửa càng nhiều ổ gà càng tốt, mà một đoạn ổ gà liên tiếp độ dài $k$ cần chi phí $k + 1$, nên ta nên ưu tiên sửa các đoạn dài hơn để giảm chi phí.

Do đó, ta bắt đầu sửa từ đoạn dài nhất. Với một đoạn ổ gà độ dài $k$, số đoạn tối đa có thể sửa là $t = \min(\textit{budget} / (k + 1), \textit{cnt}[k])$. Ta cộng số lần sửa nhân với độ dài $k$ vào đáp án, sau đó cập nhật ngân sách còn lại. Với $cnt[k] - t$ đoạn còn lại có độ dài $k$, ta gộp chúng vào các đoạn ổ gà độ dài $k - 1$. Tiếp tục quá trình này cho đến khi duyệt hết các đoạn ổ gà.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $road$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPotholes(self, road: str, budget: int) -> int:
        road += "."
        n = len(road)
        cnt = [0] * n
        k = 0
        for c in road:
            if c == "x":
                k += 1
            elif k:
                cnt[k] += 1
                k = 0
        ans = 0
        for k in range(n - 1, 0, -1):
            if cnt[k] == 0:
                continue
            t = min(budget // (k + 1), cnt[k])
            ans += t * k
            budget -= t * (k + 1)
            if budget == 0:
                break
            cnt[k - 1] += cnt[k] - t
        return ans
```

#### Java

```java
class Solution {
    public int maxPotholes(String road, int budget) {
        road += ".";
        int n = road.length();
        int[] cnt = new int[n];
        int k = 0;
        for (char c : road.toCharArray()) {
            if (c == 'x') {
                ++k;
            } else if (k > 0) {
                ++cnt[k];
                k = 0;
            }
        }
        int ans = 0;
        for (k = n - 1; k > 0 && budget > 0; --k) {
            int t = Math.min(budget / (k + 1), cnt[k]);
            ans += t * k;
            budget -= t * (k + 1);
            cnt[k - 1] += cnt[k] - t;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxPotholes(string road, int budget) {
        road.push_back('.');
        int n = road.size();
        vector<int> cnt(n);
        int k = 0;
        for (char& c : road) {
            if (c == 'x') {
                ++k;
            } else if (k) {
                ++cnt[k];
                k = 0;
            }
        }
        int ans = 0;
        for (k = n - 1; k && budget; --k) {
            int t = min(budget / (k + 1), cnt[k]);
            ans += t * k;
            budget -= t * (k + 1);
            cnt[k - 1] += cnt[k] - t;
        }
        return ans;
    }
};
```

#### Go

```go
func maxPotholes(road string, budget int) (ans int) {
	road += "."
	n := len(road)
	cnt := make([]int, n)
	k := 0
	for _, c := range road {
		if c == 'x' {
			k++
		} else if k > 0 {
			cnt[k]++
			k = 0
		}
	}
	for k = n - 1; k > 0 && budget > 0; k-- {
		t := min(budget/(k+1), cnt[k])
		ans += t * k
		budget -= t * (k + 1)
		cnt[k-1] += cnt[k] - t
	}
	return
}
```

#### TypeScript

```ts
function maxPotholes(road: string, budget: number): number {
    road += '.';
    const n = road.length;
    const cnt: number[] = Array(n).fill(0);
    let k = 0;
    for (const c of road) {
        if (c === 'x') {
            ++k;
        } else if (k) {
            ++cnt[k];
            k = 0;
        }
    }
    let ans = 0;
    for (k = n - 1; k && budget; --k) {
        const t = Math.min(Math.floor(budget / (k + 1)), cnt[k]);
        ans += t * k;
        budget -= t * (k + 1);
        cnt[k - 1] += cnt[k] - t;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_potholes(road: String, budget: i32) -> i32 {
        let mut cs: Vec<char> = road.chars().collect();
        cs.push('.');
        let n = cs.len();
        let mut cnt: Vec<i32> = vec![0; n];
        let mut k = 0;

        for c in cs.iter() {
            if *c == 'x' {
                k += 1;
            } else if k > 0 {
                cnt[k] += 1;
                k = 0;
            }
        }

        let mut ans = 0;
        let mut budget = budget;

        for k in (1..n).rev() {
            if budget == 0 {
                break;
            }
            let t = std::cmp::min(budget / ((k as i32) + 1), cnt[k]);
            ans += t * (k as i32);
            budget -= t * ((k as i32) + 1);
            cnt[k - 1] += cnt[k] - t;
        }

        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int MaxPotholes(string road, int budget) {
        road += '.';
        int n = road.Length;
        int[] cnt = new int[n];
        int k = 0;
        foreach (char c in road) {
            if (c == 'x') {
                ++k;
            } else if (k > 0) {
                ++cnt[k];
                k = 0;
            }
        }
        int ans = 0;
        for (k = n - 1; k > 0 && budget > 0; --k) {
            int t = Math.Min(budget / (k + 1), cnt[k]);
            ans += t * k;
            budget -= t * (k + 1);
            cnt[k - 1] += cnt[k] - t;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
