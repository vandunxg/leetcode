---
comments: true
difficulty: Easy
rating: 1413
source: Weekly Contest 307 Q1
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [2383. Minimum Hours of Training to Win a Competition](https://leetcode.com/problems/minimum-hours-of-training-to-win-a-competition)

[中文文档](/solution/2300-2399/2383.Minimum%20Hours%20of%20Training%20to%20Win%20a%20Competition/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn tham gia một cuộc thi và được cho hai số nguyên <strong>dương</strong> <code>initialEnergy</code> và <code>initialExperience</code>, lần lượt biểu thị năng lượng và kinh nghiệm ban đầu của bạn.</p>

<p>Bạn cũng được cho hai mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>energy</code> và <code>experience</code>, cả hai đều có độ dài <code>n</code>.</p>

<p>Bạn sẽ lần lượt đối đầu với <code>n</code> đối thủ <strong>theo thứ tự</strong>. Năng lượng và kinh nghiệm của đối thủ thứ <code>i<sup>th</sup></code> lần lượt là <code>energy[i]</code> và <code>experience[i]</code>. Khi đối đầu với một đối thủ, bạn cần có cả kinh nghiệm và năng lượng <strong>lớn hơn nghiêm ngặt</strong> họ để đánh bại và chuyển sang đối thủ tiếp theo nếu còn.</p>

<p>Đánh bại đối thủ thứ <code>i<sup>th</sup></code> sẽ <strong>tăng</strong> kinh nghiệm của bạn thêm <code>experience[i]</code>, nhưng <strong>giảm</strong> năng lượng của bạn đi <code>energy[i]</code>.</p>

<p>Trước khi bắt đầu cuộc thi, bạn có thể luyện tập trong một số giờ. Sau mỗi giờ luyện tập, bạn có thể <strong>chọn một trong hai</strong>: tăng kinh nghiệm ban đầu thêm một hoặc tăng năng lượng ban đầu thêm một.</p>

<p>Hãy trả về <em>số giờ luyện tập <strong>ít nhất</strong> cần thiết để đánh bại tất cả </em><code>n</code><em> đối thủ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> initialEnergy = 5, initialExperience = 3, energy = [1,4,3,2], experience = [2,6,3,1]
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Bạn có thể tăng năng lượng lên 11 sau 6 giờ luyện tập và tăng kinh nghiệm lên 5 sau 2 giờ luyện tập.
Bạn đối đầu với các đối thủ theo thứ tự sau:
- Bạn có năng lượng và kinh nghiệm lớn hơn đối thủ 0<sup>th</sup> nên bạn thắng.
  Năng lượng của bạn trở thành 11 - 1 = 10 và kinh nghiệm trở thành 5 + 2 = 7.
- Bạn có năng lượng và kinh nghiệm lớn hơn đối thủ 1<sup>st</sup> nên bạn thắng.
  Năng lượng của bạn trở thành 10 - 4 = 6 và kinh nghiệm trở thành 7 + 6 = 13.
- Bạn có năng lượng và kinh nghiệm lớn hơn đối thủ 2<sup>nd</sup> nên bạn thắng.
  Năng lượng của bạn trở thành 6 - 3 = 3 và kinh nghiệm trở thành 13 + 3 = 16.
- Bạn có năng lượng và kinh nghiệm lớn hơn đối thủ 3<sup>rd</sup> nên bạn thắng.
  Năng lượng của bạn trở thành 3 - 2 = 1 và kinh nghiệm trở thành 16 + 1 = 17.
Tổng cộng bạn đã luyện tập 6 + 2 = 8 giờ trước cuộc thi, nên đáp án là 8.
Có thể chứng minh rằng không tồn tại đáp án nhỏ hơn.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> initialEnergy = 2, initialExperience = 4, energy = [1], experience = [3]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Bạn không cần bổ sung năng lượng hay kinh nghiệm để thắng cuộc thi, nên đáp án là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == energy.length == experience.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= initialEnergy, initialExperience, energy[i], experience[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các đối thủ phải được đánh bại theo đúng thứ tự, đồng thời cả năng lượng và kinh nghiệm đều phải lớn hơn nghiêm ngặt tại thời điểm bắt đầu trận đấu. $n \le 100$, nên ta có thể mô phỏng và bổ sung khi thiếu.
>
> Nếu không đủ năng lượng, ta luyện tập để tăng lên thành năng lượng của đối thủ cộng một; tương tự với kinh nghiệm. Tổng lượng thiếu hụt chính là số giờ luyện tập.

<!-- thinking:end -->

Gọi năng lượng hiện tại là $x$ và kinh nghiệm hiện tại là $y$.

Tiếp theo, ta duyệt qua từng đối thủ. Với đối thủ thứ $i$, gọi năng lượng và kinh nghiệm của họ lần lượt là $dx$ và $dy$.

- Nếu $x \leq dx$, ta cần luyện tập trong $dx + 1 - x$ giờ để tăng năng lượng lên $dx + 1$.
- Nếu $y \leq dy$, ta cần luyện tập trong $dy + 1 - y$ giờ để tăng kinh nghiệm lên $dy + 1$.
- Sau đó, ta trừ $dx$ khỏi năng lượng và cộng $dy$ vào kinh nghiệm.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số đối thủ. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minNumberOfHours(
        self, x: int, y: int, energy: List[int], experience: List[int]
    ) -> int:
        ans = 0
        for dx, dy in zip(energy, experience):
            if x <= dx:
                ans += dx + 1 - x
                x = dx + 1
            if y <= dy:
                ans += dy + 1 - y
                y = dy + 1
            x -= dx
            y += dy
        return ans
```

#### Java

```java
class Solution {
    public int minNumberOfHours(int x, int y, int[] energy, int[] experience) {
        int ans = 0;
        for (int i = 0; i < energy.length; ++i) {
            int dx = energy[i], dy = experience[i];
            if (x <= dx) {
                ans += dx + 1 - x;
                x = dx + 1;
            }
            if (y <= dy) {
                ans += dy + 1 - y;
                y = dy + 1;
            }
            x -= dx;
            y += dy;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minNumberOfHours(int x, int y, vector<int>& energy, vector<int>& experience) {
        int ans = 0;
        for (int i = 0; i < energy.size(); ++i) {
            int dx = energy[i], dy = experience[i];
            if (x <= dx) {
                ans += dx + 1 - x;
                x = dx + 1;
            }
            if (y <= dy) {
                ans += dy + 1 - y;
                y = dy + 1;
            }
            x -= dx;
            y += dy;
        }
        return ans;
    }
};
```

#### Go

```go
func minNumberOfHours(x int, y int, energy []int, experience []int) (ans int) {
	for i, dx := range energy {
		dy := experience[i]
		if x <= dx {
			ans += dx + 1 - x
			x = dx + 1
		}
		if y <= dy {
			ans += dy + 1 - y
			y = dy + 1
		}
		x -= dx
		y += dy
	}
	return
}
```

#### TypeScript

```ts
function minNumberOfHours(x: number, y: number, energy: number[], experience: number[]): number {
    let ans = 0;
    for (let i = 0; i < energy.length; ++i) {
        const [dx, dy] = [energy[i], experience[i]];
        if (x <= dx) {
            ans += dx + 1 - x;
            x = dx + 1;
        }
        if (y <= dy) {
            ans += dy + 1 - y;
            y = dy + 1;
        }
        x -= dx;
        y += dy;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_number_of_hours(
        mut x: i32,
        mut y: i32,
        energy: Vec<i32>,
        experience: Vec<i32>,
    ) -> i32 {
        let mut ans = 0;

        for (&dx, &dy) in energy.iter().zip(experience.iter()) {
            if x <= dx {
                ans += dx + 1 - x;
                x = dx + 1;
            }
            if y <= dy {
                ans += dy + 1 - y;
                y = dy + 1;
            }
            x -= dx;
            y += dy;
        }

        ans
    }
}
```

#### C

```c
int minNumberOfHours(int x, int y, int* energy, int energySize, int* experience, int experienceSize) {
    int ans = 0;
    for (int i = 0; i < energySize; ++i) {
        int dx = energy[i], dy = experience[i];
        if (x <= dx) {
            ans += dx + 1 - x;
            x = dx + 1;
        }
        if (y <= dy) {
            ans += dy + 1 - y;
            y = dy + 1;
        }
        x -= dx;
        y += dy;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
