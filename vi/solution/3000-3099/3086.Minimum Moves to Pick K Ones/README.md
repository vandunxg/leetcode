---
comments: true
difficulty: Hard
rating: 2672
source: Weekly Contest 389 Q4
tags:
    - Greedy
    - Array
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [3086. Minimum Moves to Pick K Ones](https://leetcode.com/problems/minimum-moves-to-pick-k-ones)

[中文文档](/solution/3000-3099/3086.Minimum%20Moves%20to%20Pick%20K%20Ones/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng nhị phân <code>nums</code> có độ dài <code>n</code>, một số nguyên <strong>dương</strong> <code>k</code> và một số nguyên <strong>không âm</strong> <code>maxChanges</code>.</p>

<p>Alice chơi một trò chơi với mục tiêu nhặt <code>k</code> số 1 từ <code>nums</code> bằng số lượng <strong>nước đi</strong> <strong>ít nhất</strong>. Khi trò chơi bắt đầu, Alice chọn một chỉ số bất kỳ <code>aliceIndex</code> trong khoảng <code>[0, n - 1]</code> và đứng tại đó. Nếu <code>nums[aliceIndex] == 1</code>, Alice nhặt số 1 đó và <code>nums[aliceIndex]</code> trở thành <code>0</code> (việc này <strong>không</strong> được tính là một nước đi). Sau đó, Alice có thể thực hiện <strong>bất kỳ</strong> số lượng <strong>nước đi nào</strong> (<strong>kể cả</strong> <strong>0</strong>), trong đó ở mỗi nước đi Alice phải thực hiện <strong>chính xác</strong> một trong các hành động sau:</p>

<ul>
	<li>Chọn một chỉ số bất kỳ <code>j != aliceIndex</code> sao cho <code>nums[j] == 0</code> rồi đặt <code>nums[j] = 1</code>. Hành động này được thực hiện <strong>tối</strong> <strong>đa</strong> <code>maxChanges</code> lần.</li>
	<li>Chọn hai chỉ số kề nhau <code>x</code> và <code>y</code> (<code>|x - y| == 1</code>) sao cho <code>nums[x] == 1</code>, <code>nums[y] == 0</code>, sau đó hoán đổi giá trị của chúng (đặt <code>nums[y] = 1</code> và <code>nums[x] = 0</code>). Nếu <code>y == aliceIndex</code>, Alice nhặt số 1 sau nước đi này và <code>nums[y]</code> trở thành <code>0</code>.</li>
</ul>

<p>Trả về <em>số lượng nước đi <strong>ít nhất</strong> cần thiết để Alice nhặt được <strong>chính xác </strong></em><code>k</code> <em>số 1</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">nums = [1,1,0,0,0,1,1,0,0,1], k = 3, maxChanges = 1</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">3</span></p>

<p><strong>Giải thích:</strong> Alice có thể nhặt <code>3</code> số 1 trong <code>3</code> nước đi nếu thực hiện các hành động sau ở mỗi nước đi khi đứng tại <code>aliceIndex == 1</code>:</p>

<ul>
	<li>Khi trò chơi bắt đầu, Alice nhặt số 1 và <code>nums[1]</code> trở thành <code>0</code>. <code>nums</code> trở thành <code>[1,<strong><u>0</u></strong>,0,0,0,1,1,0,0,1]</code>.</li>
	<li>Chọn <code>j == 2</code> và thực hiện hành động loại 1. <code>nums</code> trở thành <code>[1,<strong><u>0</u></strong>,1,0,0,1,1,0,0,1]</code></li>
	<li>Chọn <code>x == 2</code> và <code>y == 1</code>, rồi thực hiện hành động loại 2. <code>nums</code> trở thành <code>[1,<strong><u>1</u></strong>,0,0,0,1,1,0,0,1]</code>. Vì <code>y == aliceIndex</code>, Alice nhặt số 1 và <code>nums</code> trở thành <code>[1,<strong><u>0</u></strong>,0,0,0,1,1,0,0,1]</code>.</li>
	<li>Chọn <code>x == 0</code> và <code>y == 1</code>, rồi thực hiện hành động loại 2. <code>nums</code> trở thành <code>[0,<strong><u>1</u></strong>,0,0,0,1,1,0,0,1]</code>. Vì <code>y == aliceIndex</code>, Alice nhặt số 1 và <code>nums</code> trở thành <code>[0,<strong><u>0</u></strong>,0,0,0,1,1,0,0,1]</code>.</li>
</ul>

<p>Lưu ý rằng Alice có thể nhặt <code>3</code> số 1 bằng một chuỗi <code>3</code> nước đi khác.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">nums = [0,0,0,0], k = 2, maxChanges = 3</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">4</span></p>

<p><strong>Giải thích:</strong> Alice có thể nhặt <code>2</code> số 1 trong <code>4</code> nước đi nếu thực hiện các hành động sau ở mỗi nước đi khi đứng tại <code>aliceIndex == 0</code>:</p>

<ul>
	<li>Chọn <code>j == 1</code> và thực hiện hành động loại 1. <code>nums</code> trở thành <code>[<strong><u>0</u></strong>,1,0,0]</code>.</li>
	<li>Chọn <code>x == 1</code> và <code>y == 0</code>, rồi thực hiện hành động loại 2. <code>nums</code> trở thành <code>[<strong><u>1</u></strong>,0,0,0]</code>. Vì <code>y == aliceIndex</code>, Alice nhặt số 1 và <code>nums</code> trở thành <code>[<strong><u>0</u></strong>,0,0,0]</code>.</li>
	<li>Chọn lại <code>j == 1</code> và thực hiện hành động loại 1. <code>nums</code> trở thành <code>[<strong><u>0</u></strong>,1,0,0]</code>.</li>
	<li>Chọn lại <code>x == 1</code> và <code>y == 0</code>, rồi thực hiện hành động loại 2. <code>nums</code> trở thành <code>[<strong><u>1</u></strong>,0,0,0]</code>. Vì <code>y == aliceIndex</code>, Alice nhặt số 1 và <code>nums</code> trở thành <code>[<strong><u>0</u></strong>,0,0,0]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= maxChanges &lt;= 10<sup>5</sup></code></li>
	<li><code>maxChanges + sum(nums) &gt;= k</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Prefix Sum + Binary Search

<!-- thinking:start -->

> **Tư duy**
>
> Ta thu thập $k$ số 1 khi đứng tại $i$, sử dụng các số 1 kề bên, tối đa $\textit{maxChanges}$ số 1 được tạo ra ở các ô lân cận, hoặc các số 1 ở xa hơn. $n \le 10^5$.
>
> Vị trí đứng là giá trị cần duyệt. Ta tham lam sử dụng các số 1 kề bên và ngân sách thay đổi; các số 1 ở xa hơn nên được đưa đến theo thứ tự gần nhất trước, đồng thời cả số lượng và chi phí của chúng đều có thể được truy vấn bằng tổng tiền tố trong một bán kính tìm kiếm nhị phân.
>
> Với mỗi $i$, ta lấy ô hiện tại và các ô lân cận, sử dụng hạn mức, sau đó tìm kiếm nhị phân bán kính $d$ sao cho các số 1 trong $[i-d,i-2]\cup[i+2,i+d]$ đáp ứng phần còn thiếu, với chi phí là một tổng tiền tố có trọng số.

<!-- thinking:end -->

Ta xét tất cả vị trí đứng $i$ của Alice. Với mỗi $i$, ta thực hiện chiến lược sau:

- Trước tiên, nếu số ở vị trí $i$ là $1$, ta có thể nhặt trực tiếp một số $1$ mà không cần nước đi nào.
- Sau đó, ta nhặt số $1$ ở hai phía của vị trí $i$, tức là thực hiện hành động $2$: di chuyển số $1$ từ vị trí $i-1$ đến vị trí $i$, rồi nhặt nó; di chuyển số $1$ từ vị trí $i+1$ đến vị trí $i$, rồi nhặt nó. Mỗi lần nhặt một số $1$ cần $1$ nước đi.
- Tiếp theo, ta tối đa hóa việc chuyển các số $0$ ở vị trí $i-1$ hoặc $i+1$ thành số $1$ bằng hành động $1$, sau đó di chuyển chúng đến vị trí $i$ bằng hành động $2$ để nhặt. Quá trình này tiếp tục cho đến khi số lượng số $1$ đã nhặt đạt $k$ hoặc số lần sử dụng hành động $1$ đạt $\textit{maxChanges}$. Giả sử hành động $1$ được sử dụng $c$ lần, tổng số nước đi cần thiết là $2c$.
- Sau khi sử dụng hành động $1$, nếu số lượng số $1$ đã nhặt chưa đạt $k$, ta cần tiếp tục xét việc di chuyển các số $1$ đến vị trí $i$ từ các đoạn $[1,..i-2]$ và $[i+2,..n]$ bằng hành động $2$ để nhặt chúng. Ta có thể dùng tìm kiếm nhị phân để xác định kích thước đoạn sao cho số lượng số $1$ đã nhặt đạt $k$. Cụ thể, ta tìm kiếm nhị phân kích thước đoạn $d$, rồi trong các đoạn $[i-d,..i-2]$ và $[i+2,..i+d]$, thực hiện hành động $2$ để di chuyển các số $1$ đến vị trí $i$ và nhặt chúng. Khi số lượng số $1$ đã nhặt đạt $k$, ta cập nhật đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumMoves(self, nums: List[int], k: int, maxChanges: int) -> int:
        n = len(nums)
        cnt = [0] * (n + 1)
        s = [0] * (n + 1)
        for i, x in enumerate(nums, 1):
            cnt[i] = cnt[i - 1] + x
            s[i] = s[i - 1] + i * x
        ans = inf
        max = lambda x, y: x if x > y else y
        min = lambda x, y: x if x < y else y
        for i, x in enumerate(nums, 1):
            t = 0
            need = k - x
            for j in (i - 1, i + 1):
                if need > 0 and 1 <= j <= n and nums[j - 1] == 1:
                    need -= 1
                    t += 1
            c = min(need, maxChanges)
            need -= c
            t += c * 2
            if need <= 0:
                ans = min(ans, t)
                continue
            l, r = 2, max(i - 1, n - i)
            while l <= r:
                mid = (l + r) >> 1
                l1, r1 = max(1, i - mid), max(0, i - 2)
                l2, r2 = min(n + 1, i + 2), min(n, i + mid)
                c1 = cnt[r1] - cnt[l1 - 1]
                c2 = cnt[r2] - cnt[l2 - 1]
                if c1 + c2 >= need:
                    t1 = c1 * i - (s[r1] - s[l1 - 1])
                    t2 = s[r2] - s[l2 - 1] - c2 * i
                    ans = min(ans, t + t1 + t2)
                    r = mid - 1
                else:
                    l = mid + 1
        return ans
```

#### Java

```java
class Solution {
    public long minimumMoves(int[] nums, int k, int maxChanges) {
        int n = nums.length;
        int[] cnt = new int[n + 1];
        long[] s = new long[n + 1];
        for (int i = 1; i <= n; ++i) {
            cnt[i] = cnt[i - 1] + nums[i - 1];
            s[i] = s[i - 1] + i * nums[i - 1];
        }
        long ans = Long.MAX_VALUE;
        for (int i = 1; i <= n; ++i) {
            long t = 0;
            int need = k - nums[i - 1];
            for (int j = i - 1; j <= i + 1; j += 2) {
                if (need > 0 && 1 <= j && j <= n && nums[j - 1] == 1) {
                    --need;
                    ++t;
                }
            }
            int c = Math.min(need, maxChanges);
            need -= c;
            t += c * 2;
            if (need <= 0) {
                ans = Math.min(ans, t);
                continue;
            }
            int l = 2, r = Math.max(i - 1, n - i);
            while (l <= r) {
                int mid = (l + r) >> 1;
                int l1 = Math.max(1, i - mid), r1 = Math.max(0, i - 2);
                int l2 = Math.min(n + 1, i + 2), r2 = Math.min(n, i + mid);
                int c1 = cnt[r1] - cnt[l1 - 1];
                int c2 = cnt[r2] - cnt[l2 - 1];
                if (c1 + c2 >= need) {
                    long t1 = 1L * c1 * i - (s[r1] - s[l1 - 1]);
                    long t2 = s[r2] - s[l2 - 1] - 1L * c2 * i;
                    ans = Math.min(ans, t + t1 + t2);
                    r = mid - 1;
                } else {
                    l = mid + 1;
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
    long long minimumMoves(vector<int>& nums, int k, int maxChanges) {
        int n = nums.size();
        vector<int> cnt(n + 1, 0);
        vector<long long> s(n + 1, 0);

        for (int i = 1; i <= n; ++i) {
            cnt[i] = cnt[i - 1] + nums[i - 1];
            s[i] = s[i - 1] + 1LL * i * nums[i - 1];
        }

        long long ans = LLONG_MAX;

        for (int i = 1; i <= n; ++i) {
            long long t = 0;
            int need = k - nums[i - 1];

            for (int j = i - 1; j <= i + 1; j += 2) {
                if (need > 0 && 1 <= j && j <= n && nums[j - 1] == 1) {
                    --need;
                    ++t;
                }
            }

            int c = min(need, maxChanges);
            need -= c;
            t += c * 2;

            if (need <= 0) {
                ans = min(ans, t);
                continue;
            }

            int l = 2, r = max(i - 1, n - i);

            while (l <= r) {
                int mid = (l + r) / 2;
                int l1 = max(1, i - mid), r1 = max(0, i - 2);
                int l2 = min(n + 1, i + 2), r2 = min(n, i + mid);

                int c1 = cnt[r1] - cnt[l1 - 1];
                int c2 = cnt[r2] - cnt[l2 - 1];

                if (c1 + c2 >= need) {
                    long long t1 = 1LL * c1 * i - (s[r1] - s[l1 - 1]);
                    long long t2 = s[r2] - s[l2 - 1] - 1LL * c2 * i;
                    ans = min(ans, t + t1 + t2);
                    r = mid - 1;
                } else {
                    l = mid + 1;
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func minimumMoves(nums []int, k int, maxChanges int) int64 {
	n := len(nums)
	cnt := make([]int, n+1)
	s := make([]int, n+1)

	for i := 1; i <= n; i++ {
		cnt[i] = cnt[i-1] + nums[i-1]
		s[i] = s[i-1] + i*nums[i-1]
	}

	ans := math.MaxInt64

	for i := 1; i <= n; i++ {
		t := 0
		need := k - nums[i-1]

		for _, j := range []int{i - 1, i + 1} {
			if need > 0 && 1 <= j && j <= n && nums[j-1] == 1 {
				need--
				t++
			}
		}

		c := min(need, maxChanges)
		need -= c
		t += c * 2

		if need <= 0 {
			ans = min(ans, t)
			continue
		}

		l, r := 2, max(i-1, n-i)

		for l <= r {
			mid := (l + r) >> 1
			l1, r1 := max(1, i-mid), max(0, i-2)
			l2, r2 := min(n+1, i+2), min(n, i+mid)

			c1 := cnt[r1] - cnt[l1-1]
			c2 := cnt[r2] - cnt[l2-1]

			if c1+c2 >= need {
				t1 := c1*i - (s[r1] - s[l1-1])
				t2 := s[r2] - s[l2-1] - c2*i
				ans = min(ans, t+t1+t2)
				r = mid - 1
			} else {
				l = mid + 1
			}
		}
	}

	return int64(ans)
}
```

#### TypeScript

```ts
function minimumMoves(nums: number[], k: number, maxChanges: number): number {
    const n = nums.length;
    const cnt = Array(n + 1).fill(0);
    const s = Array(n + 1).fill(0);

    for (let i = 1; i <= n; i++) {
        cnt[i] = cnt[i - 1] + nums[i - 1];
        s[i] = s[i - 1] + i * nums[i - 1];
    }

    let ans = Infinity;
    for (let i = 1; i <= n; i++) {
        let t = 0;
        let need = k - nums[i - 1];

        for (let j of [i - 1, i + 1]) {
            if (need > 0 && 1 <= j && j <= n && nums[j - 1] === 1) {
                need--;
                t++;
            }
        }

        const c = Math.min(need, maxChanges);
        need -= c;
        t += c * 2;

        if (need <= 0) {
            ans = Math.min(ans, t);
            continue;
        }

        let l = 2,
            r = Math.max(i - 1, n - i);

        while (l <= r) {
            const mid = (l + r) >> 1;
            const [l1, r1] = [Math.max(1, i - mid), Math.max(0, i - 2)];
            const [l2, r2] = [Math.min(n + 1, i + 2), Math.min(n, i + mid)];

            const c1 = cnt[r1] - cnt[l1 - 1];
            const c2 = cnt[r2] - cnt[l2 - 1];

            if (c1 + c2 >= need) {
                const t1 = c1 * i - (s[r1] - s[l1 - 1]);
                const t2 = s[r2] - s[l2 - 1] - c2 * i;
                ans = Math.min(ans, t + t1 + t2);
                r = mid - 1;
            } else {
                l = mid + 1;
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
