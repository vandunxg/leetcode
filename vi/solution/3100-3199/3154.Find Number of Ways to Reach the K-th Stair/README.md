---
comments: true
difficulty: Hard
rating: 2071
source: Weekly Contest 398 Q4
tags:
    - Bit Manipulation
    - Memoization
    - Math
    - Dynamic Programming
    - Combinatorics
---

<!-- problem:start -->

# [3154. Find Number of Ways to Reach the K-th Stair](https://leetcode.com/problems/find-number-of-ways-to-reach-the-k-th-stair)

[中文文档](/solution/3100-3199/3154.Find%20Number%20of%20Ways%20to%20Reach%20the%20K-th%20Stair/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <strong>không âm</strong> <code>k</code>. Có một cầu thang với vô hạn bậc, trong đó bậc <strong>thấp nhất</strong> được đánh số 0.</p>

<p>Alice có một số nguyên <code>jump</code>, với giá trị ban đầu là 0. Cô ấy bắt đầu ở bậc 1 và muốn đi đến bậc <code>k</code> bằng <strong>bất kỳ</strong> số lượng <strong>thao tác</strong> nào. Nếu đang ở bậc <code>i</code>, trong một <strong>thao tác</strong>, cô ấy có thể:</p>

<ul>
	<li>Đi xuống bậc <code>i - 1</code>. <strong>Không thể</strong> thực hiện thao tác này liên tiếp hoặc khi đang ở bậc 0.</li>
	<li>Đi lên bậc <code>i + 2<sup>jump</sup></code>. Sau đó, <code>jump</code> tăng lên <code>jump + 1</code>.</li>
</ul>

<p>Trả về <em>tổng số cách</em> Alice có thể đi đến bậc <code>k</code>.</p>

<p><strong>Lưu ý</strong> rằng Alice có thể đi đến bậc <code>k</code>, sau đó thực hiện một số thao tác để lại đi đến bậc <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 2 cách để đi đến bậc 0:</p>

<ul>
	<li>Alice bắt đầu ở bậc 1.
	<ul>
		<li>Thực hiện thao tác loại thứ nhất, cô ấy đi xuống 1 bậc để đến bậc 0.</li>
	</ul>
	</li>
	<li>Alice bắt đầu ở bậc 1.
	<ul>
		<li>Thực hiện thao tác loại thứ nhất, cô ấy đi xuống 1 bậc để đến bậc 0.</li>
		<li>Thực hiện thao tác loại thứ hai, cô ấy đi lên 2<sup>0</sup> bậc để đến bậc 1.</li>
		<li>Thực hiện thao tác loại thứ nhất, cô ấy đi xuống 1 bậc để đến bậc 0.</li>
	</ul>
	</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 4 cách để đi đến bậc 1:</p>

<ul>
	<li>Alice bắt đầu ở bậc 1. Alice đang ở bậc 1.</li>
	<li>Alice bắt đầu ở bậc 1.
	<ul>
		<li>Thực hiện thao tác loại thứ nhất, cô ấy đi xuống 1 bậc để đến bậc 0.</li>
		<li>Thực hiện thao tác loại thứ hai, cô ấy đi lên 2<sup>0</sup> bậc để đến bậc 1.</li>
	</ul>
	</li>
	<li>Alice bắt đầu ở bậc 1.
	<ul>
		<li>Thực hiện thao tác loại thứ hai, cô ấy đi lên 2<sup>0</sup> bậc để đến bậc 2.</li>
		<li>Thực hiện thao tác loại thứ nhất, cô ấy đi xuống 1 bậc để đến bậc 1.</li>
	</ul>
	</li>
	<li>Alice bắt đầu ở bậc 1.
	<ul>
		<li>Thực hiện thao tác loại thứ nhất, cô ấy đi xuống 1 bậc để đến bậc 0.</li>
		<li>Thực hiện thao tác loại thứ hai, cô ấy đi lên 2<sup>0</sup> bậc để đến bậc 1.</li>
		<li>Thực hiện thao tác loại thứ nhất, cô ấy đi xuống 1 bậc để đến bậc 0.</li>
		<li>Thực hiện thao tác loại thứ hai, cô ấy đi lên 2<sup>1</sup> bậc để đến bậc 2.</li>
		<li>Thực hiện thao tác loại thứ nhất, cô ấy đi xuống 1 bậc để đến bậc 1.</li>
	</ul>
	</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần ta có thể đi xuống một bậc (không được đi xuống hai lần liên tiếp) hoặc nhảy lên $2^{jump}$ bậc rồi tăng $jump$. Nếu tìm kiếm không giới hạn, số trạng thái sẽ tăng theo số lần nhảy.
>
> Các vị trí lớn hơn $k+1$ không thể quay lại. Số lần nhảy là $O(\log k)$, nên bộ ba $(i,j,jump)$ có kích thước nhỏ, trong đó $j$ ghi nhận việc vừa đi xuống.
>
> Ghi nhớ $dfs(i,j,jump)$: tính $1$ khi $i=k$, có thể tùy chọn đi xuống, và luôn thử lần nhảy tiếp theo. Nếu đi quá bậc cần tìm thì trả về $0$.

<!-- thinking:end -->

Ta xây dựng hàm `dfs(i, j, jump)`, biểu thị số cách đi đến bậc thứ $k$ khi hiện đang ở bậc thứ $i$, đã thực hiện $j$ thao tác loại 1 và `jump` thao tác loại 2. Đáp án là `dfs(1, 0, 0)`.

Quá trình tính hàm `dfs(i, j, jump)` như sau:

- Nếu $i > k + 1$, vì không thể đi xuống hai lần liên tiếp nên không thể lại đi đến bậc thứ $k$, trả về $0$;
- Nếu $i = k$, nghĩa là ta đã đi đến bậc thứ $k$. Khởi tạo đáp án bằng $1$, sau đó tiếp tục tính;
- Nếu $i > 0$ và $j = 0$, nghĩa là ta có thể đi xuống, tính đệ quy `dfs(i - 1, 1, jump)`;
- Tính đệ quy `dfs(i + 2^{jump}, 0, jump + 1)`, rồi cộng kết quả vào đáp án.

Để tránh tính toán lặp lại, ta sử dụng tìm kiếm có ghi nhớ để lưu các trạng thái đã tính.

Độ phức tạp thời gian là $O(\log^2 k)$, và độ phức tạp không gian là $O(\log^2 k)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def waysToReachStair(self, k: int) -> int:
        @cache
        def dfs(i: int, j: int, jump: int) -> int:
            if i > k + 1:
                return 0
            ans = int(i == k)
            if i > 0 and j == 0:
                ans += dfs(i - 1, 1, jump)
            ans += dfs(i + (1 << jump), 0, jump + 1)
            return ans

        return dfs(1, 0, 0)
```

#### Java

```java
class Solution {
    private Map<Long, Integer> f = new HashMap<>();
    private int k;

    public int waysToReachStair(int k) {
        this.k = k;
        return dfs(1, 0, 0);
    }

    private int dfs(int i, int j, int jump) {
        if (i > k + 1) {
            return 0;
        }
        long key = ((long) i << 32) | jump << 1 | j;
        if (f.containsKey(key)) {
            return f.get(key);
        }
        int ans = i == k ? 1 : 0;
        if (i > 0 && j == 0) {
            ans += dfs(i - 1, 1, jump);
        }
        ans += dfs(i + (1 << jump), 0, jump + 1);
        f.put(key, ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int waysToReachStair(int k) {
        unordered_map<long long, int> f;
        auto dfs = [&](this auto&& dfs, int i, int j, int jump) -> int {
            if (i > k + 1) {
                return 0;
            }
            long long key = ((long long) i << 32) | jump << 1 | j;
            if (f.contains(key)) {
                return f[key];
            }
            int ans = i == k ? 1 : 0;
            if (i > 0 && j == 0) {
                ans += dfs(i - 1, 1, jump);
            }
            ans += dfs(i + (1 << jump), 0, jump + 1);
            f[key] = ans;
            return ans;
        };
        return dfs(1, 0, 0);
    }
};
```

#### Go

```go
func waysToReachStair(k int) int {
	f := map[int]int{}
	var dfs func(i, j, jump int) int
	dfs = func(i, j, jump int) int {
		if i > k+1 {
			return 0
		}
		key := (i << 32) | jump<<1 | j
		if v, has := f[key]; has {
			return v
		}
		ans := 0
		if i == k {
			ans++
		}
		if i > 0 && j == 0 {
			ans += dfs(i-1, 1, jump)
		}
		ans += dfs(i+(1<<jump), 0, jump+1)
		f[key] = ans
		return ans
	}
	return dfs(1, 0, 0)
}
```

#### TypeScript

```ts
function waysToReachStair(k: number): number {
    const f: Map<bigint, number> = new Map();

    const dfs = (i: number, j: number, jump: number): number => {
        if (i > k + 1) {
            return 0;
        }

        const key: bigint = (BigInt(i) << BigInt(32)) | BigInt(jump << 1) | BigInt(j);
        if (f.has(key)) {
            return f.get(key)!;
        }

        let ans: number = 0;
        if (i === k) {
            ans++;
        }

        if (i > 0 && j === 0) {
            ans += dfs(i - 1, 1, jump);
        }

        ans += dfs(i + (1 << jump), 0, jump + 1);
        f.set(key, ans);
        return ans;
    };

    return dfs(1, 0, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
