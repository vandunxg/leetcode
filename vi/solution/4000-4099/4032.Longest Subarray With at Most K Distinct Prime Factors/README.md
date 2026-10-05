---
comments: true
difficulty: Medium
rating: 1758
source: Weekly Contest 516 Q3
---

<!-- problem:start -->

# [4032. Longest Subarray With at Most K Distinct Prime Factors](https://leetcode.com/problems/longest-subarray-with-at-most-k-distinct-prime-factors)

[中文文档](/solution/4000-4099/4032.Longest%20Subarray%20With%20at%20Most%20K%20Distinct%20Prime%20Factors/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> gồm các số nguyên dương và một số nguyên <code>k</code>.</p>

<p><strong>Tập hợp thừa số nguyên tố</strong> của một <span data-keyword="subarray-nonempty"><strong>mảng con</strong></span> là <strong>hợp</strong> của các <span data-keyword="prime-number"><strong>thừa số nguyên tố</strong></span> phân biệt của tất cả phần tử trong mảng con đó.</p>

<p>Trả về độ dài của mảng con <strong>dài nhất</strong> có tập hợp thừa số nguyên tố chứa <strong>không quá</strong> <code>k</code> thừa số nguyên tố phân biệt. Nếu không tồn tại mảng con nào như vậy, trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7,6,10,12,11], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xét mảng con <code>[6, 10, 12]</code>:</p>

<ul>
	<li>Các thừa số nguyên tố phân biệt của 6 là <code>{2, 3}</code>.</li>
	<li>Các thừa số nguyên tố phân biệt của 10 là <code>{2, 5}</code>.</li>
	<li>Các thừa số nguyên tố phân biệt của 12 là <code>{2, 3}</code>.</li>
	<li>Hợp của các tập hợp này là <code>{2, 3, 5}</code>, chứa 3 thừa số nguyên tố phân biệt.</li>
</ul>

<p>Không có mảng con dài hơn nào thỏa mãn điều kiện. Do đó, đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,6,9,18], k = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Xét toàn bộ mảng <code>[4, 6, 9, 18]</code>:</p>

<ul>
	<li>Các thừa số nguyên tố phân biệt của 4 là <code>{2}</code>.</li>
	<li>Các thừa số nguyên tố phân biệt của 6 là <code>{2, 3}</code>.</li>
	<li>Các thừa số nguyên tố phân biệt của 9 là <code>{3}</code>.</li>
	<li>Các thừa số nguyên tố phân biệt của 18 là <code>{2, 3}</code>.</li>
	<li>Hợp của các tập hợp này là <code>{2, 3}</code>, chứa 2 thừa số nguyên tố phân biệt.</li>
</ul>

<p>Vì <code>2 &lt;= 4</code>, toàn bộ mảng là hợp lệ. Do đó, đáp án là 4.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,10,15], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi mảng con có độ dài ít nhất 2 đều có tập hợp thừa số nguyên tố là <code>{2, 3, 5}</code>, chứa 3 thừa số nguyên tố phân biệt.</p>

<p>Vì <code>3 &gt; 2</code>, chỉ các mảng con có độ dài 1 là hợp lệ. Do đó, đáp án là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>2 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con hợp lệ khi và chỉ khi có không quá $k$ thừa số nguyên tố phân biệt. Điều kiện này có tính đơn điệu theo cửa sổ, vì vậy có thể áp dụng cửa sổ trượt.
>
> Nếu phân tích thừa số cho từng giá trị ngay khi xử lý, chi phí sẽ tăng theo tích của $n$ và $M=10^5$. Một sieve lưu danh sách thừa số nguyên tố trên đoạn $[2,M]$; cửa sổ cập nhật một bảng băm từ các danh sách này khi mở rộng hoặc thu hẹp.
>
> Mỗi khi số lượng thừa số nguyên tố phân biệt trở lại không quá $k$, ta cập nhật đáp án bằng độ dài cửa sổ.

<!-- thinking:end -->

Trước tiên, ta tiền xử lý danh sách các thừa số nguyên tố cho mỗi số trong $[2, 10^5]$ và lưu chúng vào $\textit{primes}$. Cụ thể, ta duyệt $i = 2, 3, \cdots, M$. Nếu $\textit{primes}[i]$ rỗng, thì $i$ là số nguyên tố, và ta thêm $i$ vào danh sách thừa số nguyên tố của mọi bội số của $i$.

Sau đó, ta dùng cửa sổ trượt để tìm mảng con hợp lệ dài nhất. Một bảng băm $\textit{cnt}$ ghi lại số lần xuất hiện của mỗi thừa số nguyên tố trong cửa sổ hiện tại. Khi con trỏ phải $r$ mở rộng, ta thêm tất cả thừa số nguyên tố của $\textit{nums}[r]$ vào cửa sổ. Khi số lượng thừa số nguyên tố phân biệt trong cửa sổ vượt quá $k$, con trỏ trái $l$ thu hẹp cửa sổ và ta xóa các thừa số nguyên tố của $\textit{nums}[l]$. Mỗi khi cửa sổ hợp lệ, ta cập nhật đáp án bằng độ dài cửa sổ.

Độ phức tạp thời gian là $O(M \log \log M + n \log M)$, và độ phức tạp không gian là $O(M \log \log M)$, trong đó $n$ là độ dài của $\textit{nums}$ và $M = 10^5$ là giá trị lớn nhất của các phần tử trong mảng.

<!-- tabs:start -->

#### Python3

```python
mx = 100001
primes = [[] for _ in range(mx)]
for i in range(2, mx):
    if not primes[i]:
        for j in range(i, mx, i):
            primes[j].append(i)


class Solution:
    def longestSubarray(self, nums: list[int], k: int) -> int:
        cnt = defaultdict(int)
        ans = l = 0
        for r, x in enumerate(nums):
            for y in primes[x]:
                cnt[y] += 1
            while len(cnt) > k:
                for y in primes[nums[l]]:
                    cnt[y] -= 1
                    if cnt[y] == 0:
                        cnt.pop(y)
                l += 1
            ans = max(ans, r - l + 1)
        return ans
```

#### Java

```java
class Solution {
    static final int MX = 100001;
    static List<Integer>[] primes = new ArrayList[MX];

    static {
        for (int i = 0; i < MX; i++) {
            primes[i] = new ArrayList<>();
        }

        for (int i = 2; i < MX; i++) {
            if (primes[i].isEmpty()) {
                for (int j = i; j < MX; j += i) {
                    primes[j].add(i);
                }
            }
        }
    }

    public int longestSubarray(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();

        int ans = 0;
        int l = 0;

        for (int r = 0; r < nums.length; r++) {
            for (int p : primes[nums[r]]) {
                cnt.merge(p, 1, Integer::sum);
            }

            while (cnt.size() > k) {
                for (int p : primes[nums[l]]) {
                    if (cnt.merge(p, -1, Integer::sum) == 0) {
                        cnt.remove(p);
                    }
                }
                l++;
            }

            ans = Math.max(ans, r - l + 1);
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums, int k) {
        const int MX = 100001;

        static vector<vector<int>> primes(MX);

        static bool initialized = false;
        if (!initialized) {
            initialized = true;

            for (int i = 2; i < MX; i++) {
                if (primes[i].empty()) {
                    for (int j = i; j < MX; j += i) {
                        primes[j].push_back(i);
                    }
                }
            }
        }

        unordered_map<int, int> cnt;

        int ans = 0;
        int l = 0;

        for (int r = 0; r < nums.size(); r++) {
            for (int p : primes[nums[r]]) {
                cnt[p]++;
            }

            while (cnt.size() > k) {
                for (int p : primes[nums[l]]) {
                    if (--cnt[p] == 0) {
                        cnt.erase(p);
                    }
                }
                l++;
            }

            ans = max(ans, r - l + 1);
        }

        return ans;
    }
};
```

#### Go

```go
var primes [100001][]int

func init() {
	for i := 2; i < 100001; i++ {
		if len(primes[i]) == 0 {
			for j := i; j < 100001; j += i {
				primes[j] = append(primes[j], i)
			}
		}
	}
}

func longestSubarray(nums []int, k int) int {
	cnt := map[int]int{}

	ans := 0
	l := 0

	for r, x := range nums {

		for _, p := range primes[x] {
			cnt[p]++
		}

		for len(cnt) > k {
			for _, p := range primes[nums[l]] {
				cnt[p]--
				if cnt[p] == 0 {
					delete(cnt, p)
				}
			}
			l++
		}

		ans = max(ans, r-l+1)
	}

	return ans
}
```

#### TypeScript

```ts
const MX = 100001;

const primes: number[][] = Array.from({ length: MX }, () => []);

for (let i = 2; i < MX; i++) {
    if (primes[i].length === 0) {
        for (let j = i; j < MX; j += i) {
            primes[j].push(i);
        }
    }
}

function longestSubarray(nums: number[], k: number): number {
    const cnt = new Map<number, number>();

    let ans = 0;
    let l = 0;

    for (let r = 0; r < nums.length; r++) {
        for (const p of primes[nums[r]]) {
            cnt.set(p, (cnt.get(p) ?? 0) + 1);
        }

        while (cnt.size > k) {
            for (const p of primes[nums[l]]) {
                cnt.set(p, cnt.get(p)! - 1);

                if (cnt.get(p) === 0) {
                    cnt.delete(p);
                }
            }
            l++;
        }

        ans = Math.max(ans, r - l + 1);
    }

    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;
use std::sync::OnceLock;

impl Solution {
    pub fn longest_subarray(nums: Vec<i32>, k: i32) -> i32 {
        static PRIMES: OnceLock<Vec<Vec<i32>>> = OnceLock::new();

        let primes = PRIMES.get_or_init(|| {
            let mut primes = vec![Vec::<i32>::new(); 100001];

            for i in 2..100001 {
                if primes[i].is_empty() {
                    let mut j = i;
                    while j < 100001 {
                        primes[j].push(i as i32);
                        j += i;
                    }
                }
            }

            primes
        });

        let mut cnt: HashMap<i32, i32> = HashMap::new();

        let mut ans = 0;
        let mut l = 0usize;

        for r in 0..nums.len() {
            for &p in &primes[nums[r] as usize] {
                *cnt.entry(p).or_insert(0) += 1;
            }

            while cnt.len() > k as usize {
                for &p in &primes[nums[l] as usize] {
                    let v = cnt.get_mut(&p).unwrap();
                    *v -= 1;

                    if *v == 0 {
                        cnt.remove(&p);
                    }
                }

                l += 1;
            }

            ans = ans.max((r - l + 1) as i32);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
