---
comments: true
difficulty: Hard
tags:
    - Array
    - Hash Table
    - Math
    - Binary Search
    - Sorting
    - Randomized
---

<!-- problem:start -->

# [710. Random Pick with Blacklist](https://leetcode.com/problems/random-pick-with-blacklist)

[中文文档](/solution/0700-0799/0710.Random%20Pick%20with%20Blacklist/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code> và mảng các số nguyên <strong>khác nhau</strong> <code>blacklist</code>. Hãy thiết kế thuật toán chọn ngẫu nhiên một số nguyên trong khoảng <code>[0, n - 1]</code> và <strong>không</strong> nằm trong <code>blacklist</code>. Mọi số nguyên thuộc khoảng đã nêu và không có trong <code>blacklist</code> phải có <strong>khả năng được chọn như nhau</strong>.</p>

<p>Tối ưu thuật toán để giảm số lần gọi hàm random <strong>có sẵn</strong> trong ngôn ngữ của bạn.</p>

<p>Hãy triển khai class <code>Solution</code>:</p>

<ul>
	<li><code>Solution(int n, int[] blacklist)</code> Khởi tạo object với số nguyên <code>n</code> và các số nguyên bị cấm trong <code>blacklist</code>.</li>
	<li><code>int pick()</code> Trả về một số nguyên ngẫu nhiên trong khoảng <code>[0, n - 1]</code> và không nằm trong <code>blacklist</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Solution&quot;, &quot;pick&quot;, &quot;pick&quot;, &quot;pick&quot;, &quot;pick&quot;, &quot;pick&quot;, &quot;pick&quot;, &quot;pick&quot;]
[[7, [2, 3, 5]], [], [], [], [], [], [], []]
<strong>Đầu ra</strong>
[null, 0, 4, 1, 6, 1, 0, 4]

<strong>Giải thích</strong>
Solution solution = new Solution(7, [2, 3, 5]);
solution.pick(); // return 0, any integer from [0,1,4,6] should be ok. Note that for every call of pick,
                 // 0, 1, 4, and 6 must be equally likely to be returned (i.e., with probability 1/4).
solution.pick(); // return 4
solution.pick(); // return 1
solution.pick(); // return 6
solution.pick(); // return 1
solution.pick(); // return 0
solution.pick(); // return 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= blacklist.length &lt;= min(10<sup>5</sup>, n - 1)</code></li>
	<li><code>0 &lt;= blacklist[i] &lt; n</code></li>
	<li>Tất cả giá trị trong <code>blacklist</code> đều <strong>khác nhau</strong>.</li>
	<li><code>pick</code> được gọi nhiều nhất <code>2 * 10<sup>4</sup></code> lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chọn đều ngẫu nhiên trong tập $[0, n)$ sau khi loại các giá trị bị cấm. $n$ có thể rất lớn, nên quét tuần tự hoặc rejection sampling có thể gặp giá trị bị cấm nhiều lần.
>
> Có chính xác $k=n-|B|$ số hợp lệ. Nếu ánh xạ mỗi chỉ số bị cấm trong $[0, k)$ sang một giá trị hợp lệ trong $[k, n)$, ta chỉ cần chọn đều từ $[0, k)$ rồi thay thế các giá trị bị cấm được chọn.
>
> Tạo set từ blacklist, duyệt $i$ bắt đầu từ $k$ và bỏ qua các số bị cấm, rồi ánh xạ mỗi $b<k$ sang $i$ trống tiếp theo. $\textit{pick}$ chọn $x$ trong $[0, k)$ và trả về $d.get(x, x)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def __init__(self, n: int, blacklist: List[int]):
        self.k = n - len(blacklist)
        self.d = {}
        i = self.k
        black = set(blacklist)
        for b in blacklist:
            if b < self.k:
                while i in black:
                    i += 1
                self.d[b] = i
                i += 1

    def pick(self) -> int:
        x = randrange(self.k)
        return self.d.get(x, x)


# Your Solution object will be instantiated and called as such:
# obj = Solution(n, blacklist)
# param_1 = obj.pick()
```

#### Java

```java
class Solution {
    private Map<Integer, Integer> d = new HashMap<>();
    private Random rand = new Random();
    private int k;

    public Solution(int n, int[] blacklist) {
        k = n - blacklist.length;
        int i = k;
        Set<Integer> black = new HashSet<>();
        for (int b : blacklist) {
            black.add(b);
        }
        for (int b : blacklist) {
            if (b < k) {
                while (black.contains(i)) {
                    ++i;
                }
                d.put(b, i++);
            }
        }
    }

    public int pick() {
        int x = rand.nextInt(k);
        return d.getOrDefault(x, x);
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(n, blacklist);
 * int param_1 = obj.pick();
 */
```

#### C++

```cpp
class Solution {
public:
    unordered_map<int, int> d;
    int k;

    Solution(int n, vector<int>& blacklist) {
        k = n - blacklist.size();
        int i = k;
        unordered_set<int> black(blacklist.begin(), blacklist.end());
        for (int& b : blacklist) {
            if (b < k) {
                while (black.count(i)) ++i;
                d[b] = i++;
            }
        }
    }

    int pick() {
        int x = rand() % k;
        return d.count(x) ? d[x] : x;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(n, blacklist);
 * int param_1 = obj->pick();
 */
```

#### Go

```go
type Solution struct {
	d map[int]int
	k int
}

func Constructor(n int, blacklist []int) Solution {
	k := n - len(blacklist)
	i := k
	black := map[int]bool{}
	for _, b := range blacklist {
		black[b] = true
	}
	d := map[int]int{}
	for _, b := range blacklist {
		if b < k {
			for black[i] {
				i++
			}
			d[b] = i
			i++
		}
	}
	return Solution{d, k}
}

func (this *Solution) Pick() int {
	x := rand.Intn(this.k)
	if v, ok := this.d[x]; ok {
		return v
	}
	return x
}

/**
 * Your Solution object will be instantiated and called as such:
 * obj := Constructor(n, blacklist);
 * param_1 := obj.Pick();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
