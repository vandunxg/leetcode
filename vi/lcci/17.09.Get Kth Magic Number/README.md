---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [17.09. Get Kth Magic Number](https://leetcode.cn/problems/get-kth-magic-number-lcci)

## Mô tả

<!-- description:start -->

<p>Hãy thiết kế một thuật toán để tìm số thứ k sao cho các thừa số nguyên tố duy nhất của số đó là 3, 5 và 7. Lưu ý rằng số đó không nhất thiết phải có đủ các thừa số 3, 5 và 7, nhưng không được có bất kỳ thừa số nguyên tố nào khác. Ví dụ, một số bội đầu tiên theo thứ tự là (1, 3, 5, 7, 9, 15, 21).</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong>Đầu vào: </strong>k = 5

<strong>Đầu ra: </strong>9

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Số thứ $k$ có các thừa số nguyên tố thuộc $\{3,5,7\}$. Nhân từ $1$ sẽ tạo ra các giá trị trùng lặp; dùng một set rồi sort sẽ nặng hơn mức cần thiết.
>
> Min-heap lấy ra phần tử nhỏ nhất, nhân với $3,5,7$, và $vis$ ngăn việc đẩy các giá trị trùng lặp.
>
> Sau $k-1$ lần lấy ra, phần tử đầu heap là số thứ $k$. Mỗi giá trị được mở rộng nhiều nhất ba lần, với độ phức tạp $O(k\log k)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getKthMagicNumber(self, k: int) -> int:
        h = [1]
        vis = {1}
        for _ in range(k - 1):
            cur = heappop(h)
            for f in (3, 5, 7):
                if (nxt := cur * f) not in vis:
                    vis.add(nxt)
                    heappush(h, nxt)
        return h[0]
```

#### Java

```java
class Solution {
    private static final int[] FACTORS = new int[] {3, 5, 7};

    public int getKthMagicNumber(int k) {
        PriorityQueue<Long> q = new PriorityQueue<>();
        Set<Long> vis = new HashSet<>();
        q.offer(1L);
        vis.add(1L);
        while (--k > 0) {
            long cur = q.poll();
            for (int f : FACTORS) {
                long nxt = cur * f;
                if (!vis.contains(nxt)) {
                    q.offer(nxt);
                    vis.add(nxt);
                }
            }
        }
        long ans = q.poll();
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const vector<int> factors = {3, 5, 7};

    int getKthMagicNumber(int k) {
        priority_queue<long, vector<long>, greater<long>> q;
        unordered_set<long> vis;
        q.push(1l);
        vis.insert(1l);
        for (int i = 0; i < k - 1; ++i) {
            long cur = q.top();
            q.pop();
            for (int f : factors) {
                long nxt = cur * f;
                if (!vis.count(nxt)) {
                    vis.insert(nxt);
                    q.push(nxt);
                }
            }
        }
        return (int) q.top();
    }
};
```

#### Go

```go
func getKthMagicNumber(k int) int {
	q := hp{[]int{1}}
	vis := map[int]bool{1: true}
	for i := 0; i < k-1; i++ {
		cur := heap.Pop(&q).(int)
		for _, f := range []int{3, 5, 7} {
			nxt := cur * f
			if !vis[nxt] {
				vis[nxt] = true
				heap.Push(&q, nxt)
			}
		}
	}
	return q.IntSlice[0]
}

type hp struct{ sort.IntSlice }

func (h *hp) Push(v any) { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
```

#### TypeScript

```ts
function getKthMagicNumber(k: number): number {
    const dp = [1];
    const index = [0, 0, 0];
    while (dp.length < k) {
        const a = dp[index[0]] * 3;
        const b = dp[index[1]] * 5;
        const c = dp[index[2]] * 7;
        const num = Math.min(a, b, c);
        dp.push(num);
        if (a === num) {
            index[0]++;
        }
        if (b === num) {
            index[1]++;
        }
        if (c === num) {
            index[2]++;
        }
    }
    return dp[k - 1];
}
```

#### Rust

```rust
impl Solution {
    pub fn get_kth_magic_number(k: i32) -> i32 {
        let k = k as usize;
        let mut dp = vec![1];
        let mut index = [0, 0, 0];
        for _ in 1..k {
            let a = dp[index[0]] * 3;
            let b = dp[index[1]] * 5;
            let c = dp[index[2]] * 7;
            let num = a.min(b.min(c));
            dp.push(num);
            if a == num {
                index[0] += 1;
            }
            if b == num {
                index[1] += 1;
            }
            if c == num {
                index[2] += 1;
            }
        }
        dp[k - 1]
    }
}
```

#### C

```c
#define min(a, b) (((a) < (b)) ? (a) : (b))

int getKthMagicNumber(int k) {
    int* dp = (int*) malloc(sizeof(int) * k);
    dp[0] = 1;
    int index[3] = {0, 0, 0};
    for (int i = 1; i < k; i++) {
        int a = dp[index[0]] * 3;
        int b = dp[index[1]] * 5;
        int c = dp[index[2]] * 7;
        int num = min(a, min(b, c));
        dp[i] = num;
        if (a == num) {
            index[0]++;
        }
        if (b == num) {
            index[1]++;
        }
        if (c == num) {
            index[2]++;
        }
    }
    int res = dp[k - 1];
    free(dp);
    return res;
}
```

#### Swift

```swift
class Solution {
    private let factors = [3, 5, 7]

    func getKthMagicNumber(_ k: Int) -> Int {
        var heap: [Int] = [1]
        var seen = Set<Int>()
        seen.insert(1)

        var value = 1
        for _ in 1...k {
            value = heap.removeFirst()
            for factor in factors {
                let nextValue = value * factor
                if !seen.contains(nextValue) {
                    heap.append(nextValue)
                    seen.insert(nextValue)
                }
            }
            heap.sort()
        }
        return value
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Heap sẽ lưu nhiều bội số chưa được sử dụng, nên phải trả giá cả về không gian lẫn thừa số log.
>
> Ba con trỏ đánh dấu nguồn tiếp theo cần nhân với $3/5/7$. Lấy giá trị nhỏ nhất và tiến mọi con trỏ tạo ra giá trị đó, nhờ vậy mỗi giá trị được tạo đúng một lần trong thời gian tuyến tính.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getKthMagicNumber(self, k: int) -> int:
        dp = [1] * (k + 1)
        p3 = p5 = p7 = 1
        for i in range(2, k + 1):
            a, b, c = dp[p3] * 3, dp[p5] * 5, dp[p7] * 7
            v = min(a, b, c)
            dp[i] = v
            if v == a:
                p3 += 1
            if v == b:
                p5 += 1
            if v == c:
                p7 += 1
        return dp[k]
```

#### Java

```java
class Solution {
    public int getKthMagicNumber(int k) {
        int[] dp = new int[k + 1];
        Arrays.fill(dp, 1);
        int p3 = 1, p5 = 1, p7 = 1;
        for (int i = 2; i <= k; ++i) {
            int a = dp[p3] * 3, b = dp[p5] * 5, c = dp[p7] * 7;
            int v = Math.min(Math.min(a, b), c);
            dp[i] = v;
            if (v == a) {
                ++p3;
            }
            if (v == b) {
                ++p5;
            }
            if (v == c) {
                ++p7;
            }
        }
        return dp[k];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getKthMagicNumber(int k) {
        vector<int> dp(k + 1, 1);
        int p3 = 1, p5 = 1, p7 = 1;
        for (int i = 2; i <= k; ++i) {
            int a = dp[p3] * 3, b = dp[p5] * 5, c = dp[p7] * 7;
            int v = min(min(a, b), c);
            dp[i] = v;
            if (v == a) {
                ++p3;
            }
            if (v == b) {
                ++p5;
            }
            if (v == c) {
                ++p7;
            }
        }
        return dp[k];
    }
};
```

#### Go

```go
func getKthMagicNumber(k int) int {
	dp := make([]int, k+1)
	dp[1] = 1
	p3, p5, p7 := 1, 1, 1
	for i := 2; i <= k; i++ {
		a, b, c := dp[p3]*3, dp[p5]*5, dp[p7]*7
		v := min(min(a, b), c)
		dp[i] = v
		if v == a {
			p3++
		}
		if v == b {
			p5++
		}
		if v == c {
			p7++
		}
	}
	return dp[k]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
