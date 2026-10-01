---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.10. Living People](https://leetcode.cn/problems/living-people-lcci)

[中文文档](/lcci/16.10.Living%20People/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một danh sách những người cùng năm sinh và năm mất của họ, hãy triển khai một method để tính năm có nhiều người còn sống nhất. Có thể giả sử rằng tất cả mọi người đều sinh trong khoảng từ năm 1900 đến năm 2000 (tính cả hai đầu). Nếu một người còn sống trong bất kỳ khoảng thời gian nào của một năm, người đó được tính vào số người của năm đó. Ví dụ, Person (birth= 1908, death= 1909) được tính vào số người của cả năm 1908 và 1909.</p>

<p>Nếu có nhiều năm có cùng số người còn sống lớn nhất, hãy trả về năm nhỏ nhất.</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào: </strong>

birth = {1900, 1901, 1950}

death = {1948, 1951, 2000}

<strong>Đầu ra: </strong> 1901

</pre>

<p><strong>Lưu ý: </strong></p>

<ul>
	<li><code>0 &lt; birth.length == death.length &lt;= 10000</code></li>
	<li><code>birth[i] &lt;= death[i]</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Các năm nằm trong khoảng $1900$– $2000$; cần tìm năm sớm nhất có nhiều người còn sống nhất. Nếu duyệt qua tất cả mọi người cho từng năm thì độ phức tạp là $O(nC)$.
>
> Mỗi khoảng thời gian sống tương ứng với một lần tăng trên một đoạn. Mảng hiệu cập nhật các endpoint trong $O(1)$, sau đó tính tổng tiền tố để khôi phục số người của từng năm.
>
> Tăng một tại năm sinh, giảm một vào năm ngay sau năm mất (năm mất vẫn được tính). Tổng tiền tố của mảng hiệu theo dõi năm tốt nhất.

<!-- thinking:end -->

Bài toán thực chất là thực hiện các phép cộng và trừ trên một khoảng liên tục, sau đó tìm giá trị lớn nhất. Có thể giải quyết bằng mảng hiệu.

Vì khoảng năm trong bài toán là cố định, ta có thể dùng một mảng có độ dài $102$ để biểu diễn thay đổi dân số từ năm 1900 đến năm 2000. Mỗi phần tử trong mảng biểu diễn thay đổi dân số trong năm đó, trong đó số dương biểu thị số ca sinh và số âm biểu thị số ca tử vong.

Ta duyệt qua năm sinh và năm mất của từng người, rồi lần lượt cộng một và trừ một vào thay đổi dân số của các năm tương ứng. Sau đó, ta duyệt qua mảng hiệu và tìm giá trị lớn nhất của tổng tiền tố trong mảng hiệu. Năm tương ứng với giá trị lớn nhất chính là đáp án.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(C)$. Ở đây, $n$ là độ dài của các mảng năm sinh và năm mất, còn $C$ là khoảng năm.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxAliveYear(self, birth: List[int], death: List[int]) -> int:
        base = 1900
        d = [0] * 102
        for a, b in zip(birth, death):
            d[a - base] += 1
            d[b + 1 - base] -= 1
        s = mx = 0
        ans = 0
        for i, x in enumerate(d):
            s += x
            if mx < s:
                mx = s
                ans = base + i
        return ans
```

#### Java

```java
class Solution {
    public int maxAliveYear(int[] birth, int[] death) {
        int base = 1900;
        int[] d = new int[102];
        for (int i = 0; i < birth.length; ++i) {
            int a = birth[i] - base;
            int b = death[i] - base;
            ++d[a];
            --d[b + 1];
        }
        int s = 0, mx = 0;
        int ans = 0;
        for (int i = 0; i < d.length; ++i) {
            s += d[i];
            if (mx < s) {
                mx = s;
                ans = base + i;
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
    int maxAliveYear(vector<int>& birth, vector<int>& death) {
        int base = 1900;
        int d[102]{};
        for (int i = 0; i < birth.size(); ++i) {
            int a = birth[i] - base;
            int b = death[i] - base;
            ++d[a];
            --d[b + 1];
        }
        int s = 0, mx = 0;
        int ans = 0;
        for (int i = 0; i < 102; ++i) {
            s += d[i];
            if (mx < s) {
                mx = s;
                ans = base + i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxAliveYear(birth []int, death []int) (ans int) {
	base := 1900
	d := [102]int{}
	for i, a := range birth {
		a -= base
		b := death[i] - base
		d[a]++
		d[b+1]--
	}
	mx, s := 0, 0
	for i, x := range d {
		s += x
		if mx < s {
			mx = s
			ans = base + i
		}
	}
	return
}
```

#### TypeScript

```ts
function maxAliveYear(birth: number[], death: number[]): number {
    const base = 1900;
    const d: number[] = Array(102).fill(0);
    for (let i = 0; i < birth.length; ++i) {
        const [a, b] = [birth[i] - base, death[i] - base];
        ++d[a];
        --d[b + 1];
    }
    let [s, mx] = [0, 0];
    let ans = 0;
    for (let i = 0; i < d.length; ++i) {
        s += d[i];
        if (mx < s) {
            mx = s;
            ans = base + i;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_alive_year(birth: Vec<i32>, death: Vec<i32>) -> i32 {
        let n = birth.len();
        let mut d = vec![0; 102];
        let base = 1900;
        for i in 0..n {
            d[(birth[i] - base) as usize] += 1;
            d[(death[i] - base + 1) as usize] -= 1;
        }
        let mut ans = 0;
        let mut mx = 0;
        let mut s = 0;
        for i in 0..102 {
            s += d[i];
            if mx < s {
                mx = s;
                ans = base + (i as i32);
            }
        }
        ans
    }
}
```

#### Swift

```swift
class Solution {
    func maxAliveYear(_ birth: [Int], _ death: [Int]) -> Int {
        let base = 1900
        var delta = Array(repeating: 0, count: 102) // Array to hold the changes

        for i in 0..<birth.count {
            let start = birth[i] - base
            let end = death[i] - base
            delta[start] += 1
            if end + 1 < delta.count {
                delta[end + 1] -= 1
            }
        }

        var maxAlive = 0, currentAlive = 0, maxYear = 0
        for year in 0..<delta.count {
            currentAlive += delta[year]
            if currentAlive > maxAlive {
                maxAlive = currentAlive
                maxYear = year + base
            }
        }

        return maxYear
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
