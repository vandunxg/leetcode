---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Simulation
---

<!-- problem:start -->

# [735. Asteroid Collision](https://leetcode.com/problems/asteroid-collision)

[中文文档](/solution/0700-0799/0735.Asteroid%20Collision/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>asteroids</code> biểu diễn các asteroid nằm trên một hàng. Chỉ số của asteroid trong mảng thể hiện vị trí tương đối của chúng trong không gian.</p>

<p>Với mỗi asteroid, trị tuyệt đối biểu thị kích thước, còn dấu biểu thị hướng di chuyển (số dương đi sang phải, số âm đi sang trái). Tất cả asteroid di chuyển với cùng tốc độ.</p>

<p>Hãy xác định trạng thái của các asteroid sau khi mọi va chạm kết thúc. Khi hai asteroid gặp nhau, asteroid nhỏ hơn sẽ nổ. Nếu chúng có cùng kích thước, cả hai đều nổ. Hai asteroid di chuyển cùng hướng sẽ không bao giờ gặp nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> asteroids = [5,10,-5]
<strong>Đầu ra:</strong> [5,10]
<strong>Giải thích:</strong> 10 và -5 va chạm, asteroid 10 còn lại. 5 và 10 không bao giờ va chạm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> asteroids = [8,-8]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> 8 và -8 va chạm rồi nổ cả hai.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> asteroids = [10,2,-5]
<strong>Đầu ra:</strong> [10]
<strong>Giải thích:</strong> 2 và -5 va chạm, asteroid -5 còn lại. 10 và -5 va chạm, asteroid 10 còn lại.
</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong> asteroids = [3,5,-6,2,-1,4]​​​​​​​
<strong>Đầu ra:</strong> [-6,2,4]
<strong>Giải thích:</strong> Asteroid -6 khiến asteroid 3 và 5 nổ rồi tiếp tục đi sang trái. Ở phía còn lại, asteroid 2 phá hủy -1. Vì 2 và 4 đều di chuyển sang phải nên chúng không va chạm.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= asteroids.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-1000 &lt;= asteroids[i] &lt;= 1000</code></li>
	<li><code>asteroids[i] != 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack

<!-- thinking:start -->

> **Tư duy**
>
> Va chạm chỉ xảy ra khi asteroid đi sang phải gặp asteroid đi sang trái. $n\le 10^4$. Stack lưu prefix các asteroid còn tồn tại, phù hợp với thứ tự của dãy cuối cùng.
>
> Asteroid dương không thể va chạm với asteroid nào ở bên trái nên được push vào stack. Asteroid âm va chạm với phần tử dương trên top: nếu top nhỏ hơn thì nó nổ; nếu bằng nhau thì cả hai cùng nổ; nếu top lớn hơn thì asteroid mới bị phá hủy.
>
> Mỗi asteroid được push và pop nhiều nhất một lần nên phép duyệt có độ phức tạp $O(n)$.

<!-- thinking:end -->

Ta duyệt từng asteroid $x$ từ trái sang phải. Vì một asteroid có thể va chạm với nhiều asteroid đứng trước nó, ta dùng stack để lưu các asteroid đang còn tồn tại.

- Nếu asteroid hiện tại thỏa $x>0$, nó chắc chắn không va chạm với asteroid đứng trước, nên ta push $x$ vào stack.
- Nếu không, khi stack không rỗng, phần tử trên top lớn hơn $0$ và nhỏ hơn $-x$, asteroid ở top sẽ nổ. Ta tiếp tục pop top cho đến khi điều kiện này không còn đúng. Sau đó, nếu top bằng $-x$, hai asteroid nổ cùng nhau nên chỉ cần pop top. Nếu stack rỗng hoặc top nhỏ hơn $0$, asteroid hiện tại không va chạm nữa và ta push $x$ vào stack. Nếu top lớn hơn $-x$, asteroid hiện tại bị phá hủy và không được thêm vào stack.

Cuối cùng, trả về các phần tử còn lại trong stack.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài của mảng $asteroids$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def asteroidCollision(self, asteroids: List[int]) -> List[int]:
        stk = []
        for x in asteroids:
            if x > 0:
                stk.append(x)
            else:
                while stk and stk[-1] > 0 and stk[-1] < -x:
                    stk.pop()
                if stk and stk[-1] == -x:
                    stk.pop()
                elif not stk or stk[-1] < 0:
                    stk.append(x)
        return stk
```

#### Java

```java
class Solution {
    public int[] asteroidCollision(int[] asteroids) {
        Deque<Integer> stk = new ArrayDeque<>();
        for (int x : asteroids) {
            if (x > 0) {
                stk.offerLast(x);
            } else {
                while (!stk.isEmpty() && stk.peekLast() > 0 && stk.peekLast() < -x) {
                    stk.pollLast();
                }
                if (!stk.isEmpty() && stk.peekLast() == -x) {
                    stk.pollLast();
                } else if (stk.isEmpty() || stk.peekLast() < 0) {
                    stk.offerLast(x);
                }
            }
        }
        return stk.stream().mapToInt(Integer::valueOf).toArray();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {
        vector<int> stk;
        for (int x : asteroids) {
            if (x > 0) {
                stk.push_back(x);
            } else {
                while (stk.size() && stk.back() > 0 && stk.back() < -x) {
                    stk.pop_back();
                }
                if (stk.size() && stk.back() == -x) {
                    stk.pop_back();
                } else if (stk.empty() || stk.back() < 0) {
                    stk.push_back(x);
                }
            }
        }
        return stk;
    }
};
```

#### Go

```go
func asteroidCollision(asteroids []int) (stk []int) {
	for _, x := range asteroids {
		if x > 0 {
			stk = append(stk, x)
		} else {
			for len(stk) > 0 && stk[len(stk)-1] > 0 && stk[len(stk)-1] < -x {
				stk = stk[:len(stk)-1]
			}
			if len(stk) > 0 && stk[len(stk)-1] == -x {
				stk = stk[:len(stk)-1]
			} else if len(stk) == 0 || stk[len(stk)-1] < 0 {
				stk = append(stk, x)
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function asteroidCollision(asteroids: number[]): number[] {
    const stk: number[] = [];
    for (const x of asteroids) {
        if (x > 0) {
            stk.push(x);
        } else {
            while (stk.length && stk.at(-1) > 0 && stk.at(-1) < -x) {
                stk.pop();
            }
            if (stk.length && stk.at(-1) === -x) {
                stk.pop();
            } else if (!stk.length || stk.at(-1) < 0) {
                stk.push(x);
            }
        }
    }
    return stk;
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn asteroid_collision(asteroids: Vec<i32>) -> Vec<i32> {
        let mut stk = Vec::new();
        for &x in &asteroids {
            if x > 0 {
                stk.push(x);
            } else {
                while !stk.is_empty() && *stk.last().unwrap() > 0 && *stk.last().unwrap() < -x {
                    stk.pop();
                }
                if !stk.is_empty() && *stk.last().unwrap() == -x {
                    stk.pop();
                } else if stk.is_empty() || *stk.last().unwrap() < 0 {
                    stk.push(x);
                }
            }
        }
        stk
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
