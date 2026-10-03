---
comments: true
difficulty: Hard
rating: 2057
source: Weekly Contest 283 Q4
tags:
    - Stack
    - Array
    - Math
    - Greatest Common Divisor
    - Number Theory
    - Least Common Multiple
---

<!-- problem:start -->

# [2197. Replace Non-Coprime Numbers in Array](https://leetcode.com/problems/replace-non-coprime-numbers-in-array)

[中文文档](/solution/2100-2199/2197.Replace%20Non-Coprime%20Numbers%20in%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Hãy thực hiện các bước sau:</p>

<ol>
	<li>Tìm <strong>bất kỳ</strong> hai số <strong>liền kề</strong> nào trong <code>nums</code> <strong>không nguyên tố cùng nhau</strong>.</li>
	<li>Nếu không tìm thấy hai số như vậy, <strong>dừng</strong> quá trình.</li>
	<li>Nếu không, xóa hai số đó và <strong>thay thế</strong> chúng bằng <strong>LCM (Bội chung nhỏ nhất)</strong> của chúng.</li>
	<li><strong>Lặp lại</strong> quá trình này chừng nào vẫn còn tìm thấy hai số không nguyên tố cùng nhau liền kề.</li>
</ol>

<p>Trả về <em>mảng đã được chỉnh sửa <strong>cuối cùng</strong>.</em> Có thể chứng minh rằng việc thay thế các số không nguyên tố cùng nhau liền kề theo <strong>bất kỳ</strong> thứ tự nào cũng cho cùng một kết quả.</p>

<p>Các test case được tạo sao cho các giá trị trong mảng cuối cùng <strong>nhỏ hơn hoặc bằng</strong> <code>10<sup>8</sup></code>.</p>

<p>Hai giá trị <code>x</code> và <code>y</code> <strong>không nguyên tố cùng nhau</strong> nếu <code>GCD(x, y) &gt; 1</code>, trong đó <code>GCD(x, y)</code> là <strong>ước chung lớn nhất</strong> của <code>x</code> và <code>y</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [6,4,3,2,7,6,2]
<strong>Đầu ra:</strong> [12,7,6]
<strong>Giải thích:</strong>
- (6, 4) không nguyên tố cùng nhau, với LCM(6, 4) = 12. Khi đó, nums = [<strong><u>12</u></strong>,3,2,7,6,2].
- (12, 3) không nguyên tố cùng nhau, với LCM(12, 3) = 12. Khi đó, nums = [<strong><u>12</u></strong>,2,7,6,2].
- (12, 2) không nguyên tố cùng nhau, với LCM(12, 2) = 12. Khi đó, nums = [<strong><u>12</u></strong>,7,6,2].
- (6, 2) không nguyên tố cùng nhau, với LCM(6, 2) = 6. Khi đó, nums = [12,7,<u><strong>6</strong></u>].
Không còn cặp số không nguyên tố cùng nhau liền kề nào trong nums.
Do đó, mảng được chỉnh sửa cuối cùng là [12,7,6].
Lưu ý rằng có những cách khác để thu được cùng một mảng kết quả.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,1,1,3,3,3]
<strong>Đầu ra:</strong> [2,1,1,3]
<strong>Giải thích:</strong>
- (3, 3) không nguyên tố cùng nhau, với LCM(3, 3) = 3. Khi đó, nums = [2,2,1,1,<u><strong>3</strong></u>,3].
- (3, 3) không nguyên tố cùng nhau, với LCM(3, 3) = 3. Khi đó, nums = [2,2,1,1,<u><strong>3</strong></u>].
- (2, 2) không nguyên tố cùng nhau, với LCM(2, 2) = 2. Khi đó, nums = [<u><strong>2</strong></u>,1,1,3].
Không còn cặp số không nguyên tố cùng nhau liền kề nào trong nums.
Do đó, mảng được chỉnh sửa cuối cùng là [2,1,1,3].
Lưu ý rằng có những cách khác để thu được cùng một mảng kết quả.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li>Các test case được tạo sao cho các giá trị trong mảng cuối cùng <strong>nhỏ hơn hoặc bằng</strong> <code>10<sup>8</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị liền kề không nguyên tố cùng nhau được thay bằng LCM của chúng, và kết quả này có thể tiếp tục gộp với phần tử ở một trong hai phía. LCM của ba số không phụ thuộc vào thứ tự kết hợp, nên ta luôn có thể gộp sang bên trái.
>
> Đưa giá trị tiếp theo vào stack; khi hai phần tử trên cùng không nguyên tố cùng nhau, lấy chúng ra và thay phần tử mới ở đỉnh stack bằng LCM của chúng.
>
> Mỗi giá trị được đưa vào và lấy ra nhiều nhất một lần, còn mỗi phép tính $\gcd$ có độ phức tạp logarit.

<!-- thinking:end -->

Nếu có ba số liền kề $x$, $y$, $z$ có thể được gộp, thì kết quả của việc gộp $x$ và $y$ trước, sau đó gộp với $z$, giống với kết quả của việc gộp $y$ và $z$ trước, sau đó gộp với $x$. Cả hai kết quả đều là $\textit{LCM}(x, y, z)$.

Do đó, ta luôn có thể ưu tiên gộp các số liền kề bên trái, rồi gộp kết quả với số liền kề bên phải.

Ta dùng một stack để mô phỏng quá trình này. Duyệt qua mảng, với mỗi số, ta đưa số đó vào stack. Sau đó, ta liên tục kiểm tra xem hai số trên cùng của stack có nguyên tố cùng nhau hay không. Nếu không, ta lấy hai số này ra khỏi stack, rồi đưa bội chung nhỏ nhất của chúng vào stack, cho đến khi hai số trên cùng nguyên tố cùng nhau hoặc stack có ít hơn hai phần tử.

Các phần tử còn lại trong stack chính là kết quả cuối cùng.

Độ phức tạp thời gian là $O(n \times \log M)$, và độ phức tạp không gian là $O(n)$. Trong đó, $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def replaceNonCoprimes(self, nums: List[int]) -> List[int]:
        stk = []
        for x in nums:
            stk.append(x)
            while len(stk) > 1:
                x, y = stk[-2:]
                g = gcd(x, y)
                if g == 1:
                    break
                stk.pop()
                stk[-1] = x * y // g
        return stk
```

#### Java

```java
class Solution {
    public List<Integer> replaceNonCoprimes(int[] nums) {
        List<Integer> stk = new ArrayList<>();
        for (int x : nums) {
            stk.add(x);
            while (stk.size() > 1) {
                x = stk.get(stk.size() - 1);
                int y = stk.get(stk.size() - 2);
                int g = gcd(x, y);
                if (g == 1) {
                    break;
                }
                stk.remove(stk.size() - 1);
                stk.set(stk.size() - 1, (int) ((long) x * y / g));
            }
        }
        return stk;
    }

    private int gcd(int a, int b) {
        if (b == 0) {
            return a;
        }
        return gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> replaceNonCoprimes(vector<int>& nums) {
        vector<int> stk;
        for (int x : nums) {
            stk.push_back(x);
            while (stk.size() > 1) {
                x = stk.back();
                int y = stk[stk.size() - 2];
                int g = __gcd(x, y);
                if (g == 1) {
                    break;
                }
                stk.pop_back();
                stk.back() = 1LL * x * y / g;
            }
        }
        return stk;
    }
};
```

#### Go

```go
func replaceNonCoprimes(nums []int) []int {
	stk := []int{}
	for _, x := range nums {
		stk = append(stk, x)
		for len(stk) > 1 {
			x = stk[len(stk)-1]
			y := stk[len(stk)-2]
			g := gcd(x, y)
			if g == 1 {
				break
			}
			stk = stk[:len(stk)-1]
			stk[len(stk)-1] = x * y / g
		}
	}
	return stk
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### TypeScript

```ts
function replaceNonCoprimes(nums: number[]): number[] {
    const gcd = (a: number, b: number): number => {
        if (b === 0) {
            return a;
        }
        return gcd(b, a % b);
    };
    const stk: number[] = [];
    for (let x of nums) {
        stk.push(x);
        while (stk.length > 1) {
            x = stk.at(-1)!;
            const y = stk.at(-2)!;
            const g = gcd(x, y);
            if (g === 1) {
                break;
            }
            stk.pop();
            stk.pop();
            stk.push(((x * y) / g) | 0);
        }
    }
    return stk;
}
```

#### Rust

```rust
impl Solution {
    pub fn replace_non_coprimes(nums: Vec<i32>) -> Vec<i32> {
        fn gcd(mut a: i64, mut b: i64) -> i64 {
            while b != 0 {
                let t = a % b;
                a = b;
                b = t;
            }
            a
        }

        let mut stk: Vec<i64> = Vec::new();
        for x in nums {
            stk.push(x as i64);
            while stk.len() > 1 {
                let x = *stk.last().unwrap();
                let y = stk[stk.len() - 2];
                let g = gcd(x, y);
                if g == 1 {
                    break;
                }
                stk.pop();
                let last = stk.last_mut().unwrap();
                *last = x / g * y;
            }
        }

        stk.into_iter().map(|v| v as i32).collect()
    }
}
```

#### C#

```cs
public class Solution {
    public IList<int> ReplaceNonCoprimes(int[] nums) {
        long Gcd(long a, long b) {
            while (b != 0) {
                long t = a % b;
                a = b;
                b = t;
            }
            return a;
        }

        var stk = new List<long>();
        foreach (int num in nums) {
            stk.Add(num);
            while (stk.Count > 1) {
                long x = stk[stk.Count - 1];
                long y = stk[stk.Count - 2];
                long g = Gcd(x, y);
                if (g == 1) {
                    break;
                }
                stk.RemoveAt(stk.Count - 1);
                stk[stk.Count - 1] = x / g * y;
            }
        }

        var ans = new List<int>();
        foreach (long v in stk) {
            ans.Add((int)v);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
