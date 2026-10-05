---
comments: true
difficulty: Medium
rating: 1435
source: Biweekly Contest 180 Q3
tags:
    - Array
    - Math
    - Two Pointers
    - Binary Search
    - Number Theory
    - Sorting
---

<!-- problem:start -->

# [3896. Minimum Operations to Transform Array into Alternating Prime](https://leetcode.com/problems/minimum-operations-to-transform-array-into-alternating-prime)

[中文文档](/solution/3800-3899/3896.Minimum%20Operations%20to%20Transform%20Array%20into%20Alternating%20Prime/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Một mảng được gọi là <strong>mảng luân phiên số nguyên tố</strong> nếu:</p>

<ul>
	<li>Các phần tử ở <strong>chỉ số chẵn</strong> (đánh số từ 0) là <strong>số nguyên tố</strong>.</li>
	<li>Các phần tử ở <strong>chỉ số lẻ</strong> là <strong>số không nguyên tố</strong>.</li>
</ul>

<p>Trong một thao tác, bạn có thể <strong>tăng</strong> bất kỳ phần tử nào lên 1.</p>

<p>Hãy trả về số thao tác <strong>ít nhất</strong> cần thực hiện để biến <code>nums</code> thành một mảng <strong>luân phiên số nguyên tố</strong>.</p>

<p><strong>Số nguyên tố</strong> là số tự nhiên lớn hơn 1 và chỉ có đúng hai ước là 1 và chính nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Phần tử ở chỉ số 0 phải là số nguyên tố. Tăng <code>nums[0] = 1</code> lên 2, dùng 1 thao tác.</li>
	<li>Phần tử ở chỉ số 1 phải là số không nguyên tố. Tăng <code>nums[1] = 2</code> lên 4, dùng 2 thao tác.</li>
	<li>Phần tử ở chỉ số 2 đã là số nguyên tố.</li>
	<li>Phần tử ở chỉ số 3 đã là số không nguyên tố.</li>
</ul>

<p>Tổng số thao tác = <code>1 + 2 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,6,7,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các phần tử ở chỉ số 0 và 2 đã là số nguyên tố.</li>
	<li>Các phần tử ở chỉ số 1 và 3 đã là số không nguyên tố.</li>
</ul>

<p>Không cần thực hiện thao tác nào.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Phần tử ở chỉ số 0 phải là số nguyên tố. Tăng <code>nums[0] = 4</code> lên 5, dùng 1 thao tác.</li>
	<li>Phần tử ở chỉ số 1 đã là số không nguyên tố.</li>
</ul>

<p>Tổng số thao tác = 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Các chỉ số chẵn phải là số nguyên tố và các chỉ số lẻ phải là số hợp số; ta chỉ được cộng $1$. $n \le 10^5$ và các giá trị $\le 10^5$, nên cần sàng các số nguyên tố.
>
> Với chỉ số chẵn, đưa giá trị lên số nguyên tố nhỏ nhất $\ge x$, có thể tìm bằng tìm kiếm nhị phân trên danh sách số nguyên tố.
>
> Với chỉ số lẻ, nếu đã là số hợp số thì giữ nguyên; số nguyên tố $2$ cần cộng $+2$ để đạt $4$, các số nguyên tố khác chỉ cần cộng $+1$.
>
> Sàng Eratosthenes đến $2 \times 10^5$ để các giá trị sau khi tăng vẫn nằm trong bảng.

<!-- thinking:end -->

Trước hết, ta có thể tiền xử lý một danh sách số nguyên tố đủ lớn, ký hiệu là $\textit{primes}$, và một mảng boolean $\textit{isPrime}$, trong đó $\textit{isPrime}[i]$ cho biết $i$ có phải là số nguyên tố hay không.

Sau đó, ta duyệt qua từng phần tử trong mảng:

- Nếu chỉ số của phần tử hiện tại là chẵn, ta cần tăng nó lên số nguyên tố tiếp theo. Ta có thể dùng tìm kiếm nhị phân trên $\textit{primes}$ để tìm số nguyên tố đầu tiên lớn hơn hoặc bằng phần tử hiện tại, rồi cộng độ chênh lệch giữa hai giá trị vào đáp án.
- Nếu chỉ số của phần tử hiện tại là lẻ và phần tử hiện tại là số nguyên tố, ta cần tăng nó lên số không nguyên tố tiếp theo. Với số nguyên tố 2, cần tăng 2 lần để đạt số không nguyên tố tiếp theo là 4; với các số nguyên tố khác, chỉ cần tăng 1 lần.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log P)$, còn độ phức tạp không gian là $O(P)$. Trong đó, $n$ và $P$ lần lượt là độ dài của mảng và độ dài danh sách số nguyên tố được tiền xử lý.

<!-- tabs:start -->

#### Python3

```python
MX = 200000
is_prime = [True] * (MX + 1)
is_prime[0] = is_prime[1] = False

for i in range(2, int(MX**0.5) + 1):
    if is_prime[i]:
        for j in range(i * i, MX + 1, i):
            is_prime[j] = False

primes = [i for i in range(2, MX + 1) if is_prime[i]]


class Solution:
    def minOperations(self, nums: list[int]) -> int:
        ans = 0
        for i, x in enumerate(nums):
            if i % 2 == 0:
                j = bisect_left(primes, x)
                ans += primes[j] - x
            else:
                if is_prime[x]:
                    ans += 2 if x == 2 else 1
        return ans
```

#### Java

```java
class Solution {
    private static final int MX = 200000;
    private static final boolean[] IS_PRIME = new boolean[MX + 1];
    private static final List<Integer> PRIMES = new ArrayList<>();

    static {
        Arrays.fill(IS_PRIME, true);
        IS_PRIME[0] = false;
        IS_PRIME[1] = false;
        for (int i = 2; i <= MX / i; ++i) {
            if (IS_PRIME[i]) {
                for (int j = i * i; j <= MX; j += i) {
                    IS_PRIME[j] = false;
                }
            }
        }
        for (int i = 2; i <= MX; ++i) {
            if (IS_PRIME[i]) {
                PRIMES.add(i);
            }
        }
    }

    public int minOperations(int[] nums) {
        int ans = 0;
        for (int i = 0; i < nums.length; ++i) {
            int x = nums[i];
            if ((i & 1) == 0) {
                int j = Collections.binarySearch(PRIMES, x);
                if (j < 0) {
                    j = -j - 1;
                }
                ans += PRIMES.get(j) - x;
            } else if (IS_PRIME[x]) {
                ans += x == 2 ? 2 : 1;
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
    int minOperations(vector<int>& nums) {
        int ans = 0;
        for (int i = 0; i < nums.size(); ++i) {
            int x = nums[i];
            if ((i & 1) == 0) {
                auto it = lower_bound(primes.begin(), primes.end(), x);
                ans += *it - x;
            } else if (isPrime[x]) {
                ans += x == 2 ? 2 : 1;
            }
        }
        return ans;
    }

private:
    static constexpr int MX = 200000;
    inline static vector<bool> isPrime = [] {
        vector<bool> p(MX + 1, true);
        p[0] = p[1] = false;
        for (int i = 2; i <= MX / i; ++i) {
            if (p[i]) {
                for (int j = i * i; j <= MX; j += i) {
                    p[j] = false;
                }
            }
        }
        return p;
    }();

    inline static vector<int> primes = [] {
        vector<int> res;
        for (int i = 2; i <= MX; ++i) {
            if (isPrime[i]) {
                res.push_back(i);
            }
        }
        return res;
    }();
};
```

#### Go

```go
const MX = 200000

var isPrime = func() []bool {
	p := make([]bool, MX+1)
	for i := range p {
		p[i] = true
	}
	p[0], p[1] = false, false
	for i := 2; i <= MX/i; i++ {
		if p[i] {
			for j := i * i; j <= MX; j += i {
				p[j] = false
			}
		}
	}
	return p
}()

var primes = func() []int {
	var res []int
	for i := 2; i <= MX; i++ {
		if isPrime[i] {
			res = append(res, i)
		}
	}
	return res
}()

func minOperations(nums []int) (ans int) {
	for i, x := range nums {
		if i%2 == 0 {
			j := sort.SearchInts(primes, x)
			ans += primes[j] - x
		} else if isPrime[x] {
			if x == 2 {
				ans += 2
			} else {
				ans++
			}
		}
	}
	return
}
```

#### TypeScript

```ts
const MX = 200000;

const isPrime: boolean[] = (() => {
    const p = Array<boolean>(MX + 1).fill(true);
    p[0] = p[1] = false;
    for (let i = 2; i <= Math.floor(MX / i); ++i) {
        if (p[i]) {
            for (let j = i * i; j <= MX; j += i) {
                p[j] = false;
            }
        }
    }
    return p;
})();

const primes: number[] = Array.from({ length: MX - 1 }, (_, i) => i + 2).filter(i => isPrime[i]);

function minOperations(nums: number[]): number {
    let ans = 0;
    for (let i = 0; i < nums.length; ++i) {
        const x = nums[i];
        if ((i & 1) === 0) {
            const j = _.sortedIndex(primes, x);
            ans += primes[j] - x;
        } else if (isPrime[x]) {
            ans += x === 2 ? 2 : 1;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
