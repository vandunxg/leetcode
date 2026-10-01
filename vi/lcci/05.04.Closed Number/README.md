---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [05.04. Closed Number](https://leetcode.cn/problems/closed-number-lcci)

[中文文档](/lcci/05.04.Closed%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương, hãy in ra số nhỏ nhất tiếp theo và số lớn nhất tiếp theo có cùng số bit 1 trong biểu diễn nhị phân.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong> Đầu vào</strong>: num = 2 (0b10)

<strong> Đầu ra</strong>: [4, 1] ([0b100, 0b1])

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong> Đầu vào</strong>: num = 1

<strong> Đầu ra</strong>: [2, -1]

</pre>
<p><strong>Lưu ý:</strong></p>
<ol>
	<li><code>1 &lt;= num &lt;=&nbsp;2147483647</code></li>
	<li>Nếu không tồn tại số nhỏ nhất tiếp theo hoặc số lớn nhất tiếp theo, hãy xuất -1.</li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số nguyên lớn hơn và nhỏ hơn tiếp theo có cùng popcount. Việc tăng dần rồi đếm lại các bit 1 có thể phải bỏ qua một khoảng trống rất dài.
>
> Giá trị lớn hơn tiếp theo thay `01` bằng `10` rồi dồn các bit 1 còn lại về xa bên phải nhất có thể; giá trị nhỏ hơn tiếp theo cũng tương tự với `10`.
>
> Hai hướng dùng chung một vòng lặp trên các mẫu bit liền kề $(a,b)$: đổi cặp hợp lệ thấp nhất, sau đó tập hợp các bit bên dưới nó. Nếu không có số lân cận, kết quả vẫn là `-1`.

<!-- thinking:end -->

Trước hết, hãy xem cách tìm số đầu tiên lớn hơn `num` và có cùng số bit `1` trong biểu diễn nhị phân của nó.

Ta có thể duyệt qua từng cặp bit nhị phân liền kề của `num` từ thấp đến cao. Nếu bit thấp hơn là `1` và bit cao hơn liền kề là `0`, ta đã tìm được vị trí có thể đổi `0` ở vị trí này thành `1` và đổi `1` ở vị trí này thành `0`. Sau đó, ta chuyển tất cả các bit `1` còn lại ở phía thấp hơn về bit thấp nhất, từ đó nhận được một số lớn hơn `num` và có cùng số bit `1` trong biểu diễn nhị phân.

Tương tự, ta có thể tìm số đầu tiên nhỏ hơn `num` và có cùng số bit `1` trong biểu diễn nhị phân của nó.

Ta có thể duyệt qua từng cặp bit nhị phân liền kề của `num` từ thấp đến cao. Nếu bit thấp hơn là `0` và bit cao hơn liền kề là `1`, ta đã tìm được vị trí có thể đổi `1` ở vị trí này thành `0` và đổi `0` ở vị trí này thành `1`. Sau đó, ta chuyển tất cả các bit `0` còn lại ở phía thấp hơn về bit thấp nhất, từ đó nhận được một số nhỏ hơn `num` và có cùng số bit `1` trong biểu diễn nhị phân.

Trong phần cài đặt, ta có thể dùng một đoạn code để xử lý thống nhất hai trường hợp trên.

Độ phức tạp thời gian là $O(\log n)$, trong đó `n` là kích thước của `num`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findClosedNumbers(self, num: int) -> List[int]:
        ans = [-1] * 2
        dirs = (0, 1, 0)
        for p in range(2):
            a, b = dirs[p], dirs[p + 1]
            x = num
            for i in range(1, 31):
                if (x >> i & 1) == a and (x >> (i - 1) & 1) == b:
                    x ^= 1 << i
                    x ^= 1 << (i - 1)
                    j, k = 0, i - 2
                    while j < k:
                        while j < k and (x >> j & 1) == b:
                            j += 1
                        while j < k and (x >> k & 1) == a:
                            k -= 1
                        if j < k:
                            x ^= 1 << j
                            x ^= 1 << k
                    ans[p] = x
                    break
        return ans
```

#### Java

```java
class Solution {
    public int[] findClosedNumbers(int num) {
        int[] ans = {-1, -1};
        int[] dirs = {0, 1, 0};
        for (int p = 0; p < 2; ++p) {
            int a = dirs[p], b = dirs[p + 1];
            int x = num;
            for (int i = 1; i < 31; ++i) {
                if ((x >> i & 1) == a && (x >> (i - 1) & 1) == b) {
                    x ^= 1 << i;
                    x ^= 1 << (i - 1);
                    int j = 0, k = i - 2;
                    while (j < k) {
                        while (j < k && (x >> j & 1) == b) {
                            ++j;
                        }
                        while (j < k && (x >> k & 1) == a) {
                            --k;
                        }
                        if (j < k) {
                            x ^= 1 << j;
                            x ^= 1 << k;
                        }
                    }
                    ans[p] = x;
                    break;
                }
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
    vector<int> findClosedNumbers(int num) {
        vector<int> ans(2, -1);
        int dirs[3] = {0, 1, 0};
        for (int p = 0; p < 2; ++p) {
            int a = dirs[p], b = dirs[p + 1];
            int x = num;
            for (int i = 1; i < 31; ++i) {
                if ((x >> i & 1) == a && (x >> (i - 1) & 1) == b) {
                    x ^= 1 << i;
                    x ^= 1 << (i - 1);
                    int j = 0, k = i - 2;
                    while (j < k) {
                        while (j < k && (x >> j & 1) == b) {
                            ++j;
                        }
                        while (j < k && (x >> k & 1) == a) {
                            --k;
                        }
                        if (j < k) {
                            x ^= 1 << j;
                            x ^= 1 << k;
                        }
                    }
                    ans[p] = x;
                    break;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findClosedNumbers(num int) []int {
	ans := []int{-1, -1}
	dirs := [3]int{0, 1, 0}
	for p := 0; p < 2; p++ {
		a, b := dirs[p], dirs[p+1]
		x := num
		for i := 1; i < 31; i++ {
			if x>>i&1 == a && x>>(i-1)&1 == b {
				x ^= 1 << i
				x ^= 1 << (i - 1)
				j, k := 0, i-2
				for j < k {
					for j < k && x>>j&1 == b {
						j++
					}
					for j < k && x>>k&1 == a {
						k--
					}
					if j < k {
						x ^= 1 << j
						x ^= 1 << k
					}
				}
				ans[p] = x
				break
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findClosedNumbers(num: number): number[] {
    const ans: number[] = [-1, -1];
    const dirs: number[] = [0, 1, 0];
    for (let p = 0; p < 2; ++p) {
        const [a, b] = [dirs[p], dirs[p + 1]];
        let x = num;
        for (let i = 1; i < 31; ++i) {
            if (((x >> i) & 1) === a && ((x >> (i - 1)) & 1) === b) {
                x ^= 1 << i;
                x ^= 1 << (i - 1);
                let [j, k] = [0, i - 2];
                while (j < k) {
                    while (j < k && ((x >> j) & 1) === b) {
                        ++j;
                    }
                    while (j < k && ((x >> k) & 1) === a) {
                        --k;
                    }
                    if (j < k) {
                        x ^= 1 << j;
                        x ^= 1 << k;
                    }
                }
                ans[p] = x;
                break;
            }
        }
    }
    return ans;
}
```

#### Swift

```swift
class Solution {
    func findClosedNumbers(_ num: Int) -> [Int] {
        var ans = [-1, -1]
        let dirs = [0, 1, 0]

        for p in 0..<2 {
            let a = dirs[p], b = dirs[p + 1]
            var x = num
            var found = false

            for i in 1..<31 {
                if ((x >> i) & 1) == a && ((x >> (i - 1)) & 1) == b {
                    x ^= (1 << i)
                    x ^= (1 << (i - 1))

                    var j = 0, k = i - 2
                    while j < k {
                        while j < k && ((x >> j) & 1) == b {
                            j += 1
                        }
                        while j < k && ((x >> k) & 1) == a {
                            k -= 1
                        }
                        if j < k {
                            x ^= (1 << j)
                            x ^= (1 << k)
                        }
                    }
                    ans[p] = x
                    found = true
                    break
                }
            }
            if !found {
                ans[p] = -1
            }
        }

        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
