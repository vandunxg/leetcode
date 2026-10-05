---
comments: true
difficulty: Medium
rating: 2113
source: Weekly Contest 519 Q2
---

<!-- problem:start -->

# [4053. Minimum Operations to Make Every Element Palindromic](https://leetcode.com/problems/minimum-operations-to-make-every-element-palindromic)

[中文文档](/solution/4000-4099/4053.Minimum%20Operations%20to%20Make%20Every%20Element%20Palindromic/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Trong một <strong>thao tác</strong>, bạn có thể chọn một chỉ số <code>i</code> và tăng hoặc giảm <code>nums[i]</code> đi 2.</p>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để biến mọi phần tử trong <code>nums</code> thành một <strong>số nguyên dương</strong> <span data-keyword="palindrome-integer">palindrome</span>. Các phần tử khác nhau có thể được biến đổi thành các số nguyên palindrome khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,12,14,16]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một chuỗi thao tác tối ưu là:</p>

<ul>
	<li>Giảm <code>nums[0]</code> đi 2 một lần để đổi từ 10 thành 8.</li>
	<li>Giảm <code>nums[1]</code> đi 2 hai lần để đổi từ 12 thành 8.</li>
	<li>Giảm <code>nums[2]</code> đi 2 ba lần để đổi từ 14 thành 8.</li>
	<li>Tăng <code>nums[3]</code> đi 2 ba lần để đổi từ 16 thành 22.</li>
</ul>

<p>Sau <code>1 + 2 + 3 + 3 = 9</code> thao tác, <code>nums = [8, 8, 8, 22]</code>, và mọi phần tử đều là số nguyên palindrome dương.</p>

<p>Có thể chứng minh rằng không thể đạt được điều này với ít hơn 9 thao tác.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [9,10,11,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Giảm <code>nums[1]</code> và <code>nums[3]</code> đi 2, mỗi phần tử một lần.</p>

<p>Sau 2 thao tác, <code>nums = [9, 8, 11, 8]</code>, và mọi phần tử đều là số nguyên palindrome dương.</p>

<p>Cần ít nhất một thao tác cho mỗi trong hai phần tử này, nên số thao tác nhỏ nhất là 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [125]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Giảm <code>nums[0]</code> đi 2 hai lần để đổi từ 125 thành 121, là một số nguyên palindrome dương.</p>

<p>Một thao tác sẽ đổi nó thành 123 hoặc 127, cả hai đều không phải palindrome. Vì vậy, số thao tác nhỏ nhất là 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý Palindrome + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác cộng hoặc trừ $2$, nên tính chẵn lẻ không thay đổi: $\textit{nums}[i]$ chỉ có thể trở thành một palindrome dương có cùng tính chẵn lẻ. Các phần tử độc lập với nhau, và đáp án là tổng khoảng cách từ mỗi giá trị đến palindrome gần nhất có cùng tính chẵn lẻ, chia cho $2$.
>
> Với $n = 10^5$ và các giá trị lên tới $10^9$, việc bắt đầu từ $x$ rồi tăng từng bước $2$ cho đến khi gặp một palindrome là quá chậm.
>
> Mọi palindrome đều là kết quả đối xứng của một prefix. Liệt kê các prefix $1 \ldots 10^5$ và tạo cả palindrome độ dài chẵn lẫn độ dài lẻ sẽ bao phủ mọi palindrome quanh $10^9$. Chia chúng theo tính chẵn lẻ, sắp xếp từng danh sách, rồi tìm kiếm nhị phân palindrome gần nhất với mỗi $x$.

<!-- thinking:end -->

Một thao tác tăng hoặc giảm một phần tử đi $2$, nên tính chẵn lẻ của nó được bảo toàn và palindrome đích phải có cùng tính chẵn lẻ. Các phần tử độc lập với nhau: với mỗi $x$, tìm palindrome dương gần nhất $p$ có cùng tính chẵn lẻ rồi cộng thêm $\lvert x - p \rvert / 2$.

Trong bước tiền xử lý, liệt kê các prefix $i = 1, 2, \ldots, 10^5$ và gọi $s$ là biểu diễn thập phân của $i$:

- Palindrome độ dài chẵn: $s + \mathrm{reverse}(s)$
- Palindrome độ dài lẻ: $s + \mathrm{reverse}(s[:-1])$

Lưu chúng vào hai danh sách theo tính chẵn lẻ rồi sắp xếp từng danh sách. Phạm vi này bao phủ mọi palindrome có tối đa khoảng $12$ chữ số, đủ cho các giá trị lên tới $10^9$.

Với mỗi $x$, tìm kiếm nhị phân palindrome đầu tiên lớn hơn hoặc bằng $x$ trong danh sách có cùng tính chẵn lẻ, so sánh nó với palindrome đứng trước, rồi lấy khoảng cách nhỏ hơn chia cho $2$.

Gọi $M$ là số lượng palindrome (khoảng $2 \times 10^5$). Tiền xử lý mất $O(M \log M)$ và mỗi truy vấn mất $O(\log M)$. Độ phức tạp thời gian tổng thể là $O(M \log M + n \log M)$, còn độ phức tạp không gian là $O(M)$.

<!-- tabs:start -->

#### Python3

```python
ps = [[], []]
for i in range(1, 10**5 + 1):
    s = str(i)
    t1 = s[::-1]
    t2 = s[:-1][::-1]
    x = int(s + t1)
    ps[x & 1].append(x)
    y = int(s + t2)
    ps[y & 1].append(y)
for p in ps:
    p.sort()


class Solution:
    def minOperations(self, nums: list[int]) -> int:
        ans = 0
        for x in nums:
            p = ps[x & 1]
            i = bisect_left(p, x)
            t = inf
            if i < len(p):
                t = p[i] - x
            if i:
                t = min(t, x - p[i - 1])
            ans += t // 2
        return ans
```

#### Java

```java
class Solution {
    static List<Long>[] ps = new ArrayList[2];

    static {
        ps[0] = new ArrayList<>();
        ps[1] = new ArrayList<>();
        for (int i = 1; i <= 100000; i++) {
            String s = String.valueOf(i);
            String t1 = new StringBuilder(s).reverse().toString();
            String t2 = new StringBuilder(s.substring(0, s.length() - 1)).reverse().toString();
            long x = Long.parseLong(s + t1);
            ps[(int) (x & 1)].add(x);
            long y = Long.parseLong(s + t2);
            ps[(int) (y & 1)].add(y);
        }
        ps[0].sort(Long::compare);
        ps[1].sort(Long::compare);
    }

    public long minOperations(int[] nums) {
        long ans = 0;
        for (int x : nums) {
            List<Long> p = ps[x & 1];
            int l = 0, r = p.size();
            while (l < r) {
                int m = (l + r) >>> 1;
                if (p.get(m) < x) {
                    l = m + 1;
                } else {
                    r = m;
                }
            }
            long t = Long.MAX_VALUE;
            if (l < p.size()) {
                t = p.get(l) - x;
            }
            if (l > 0) {
                t = Math.min(t, (long) x - p.get(l - 1));
            }
            ans += t / 2;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minOperations(vector<int>& nums) {
        static vector<long long> ps[2];
        if (ps[0].empty()) {
            for (long long i = 1; i <= 100000; ++i) {
                string s = to_string(i);
                string t1 = s;
                reverse(t1.begin(), t1.end());
                string t2 = s.substr(0, s.size() - 1);
                reverse(t2.begin(), t2.end());
                long long x = stoll(s + t1);
                ps[x & 1].push_back(x);
                long long y = stoll(s + t2);
                ps[y & 1].push_back(y);
            }
            sort(ps[0].begin(), ps[0].end());
            sort(ps[1].begin(), ps[1].end());
        }
        long long ans = 0;
        for (int x : nums) {
            auto& p = ps[x & 1];
            auto it = lower_bound(p.begin(), p.end(), x);
            long long t = LLONG_MAX;
            if (it != p.end()) {
                t = *it - x;
            }
            if (it != p.begin()) {
                t = min(t, (long long) x - *prev(it));
            }
            ans += t / 2;
        }
        return ans;
    }
};
```

#### Go

```go
var ps [2][]int64

func init() {
	for i := int64(1); i <= 100000; i++ {
		s := strconv.FormatInt(i, 10)
		t1 := reverse(s)
		t2 := reverse(s[:len(s)-1])
		x, _ := strconv.ParseInt(s+t1, 10, 64)
		ps[x&1] = append(ps[x&1], x)
		y, _ := strconv.ParseInt(s+t2, 10, 64)
		ps[y&1] = append(ps[y&1], y)
	}
	sort.Slice(ps[0], func(a, b int) bool { return ps[0][a] < ps[0][b] })
	sort.Slice(ps[1], func(a, b int) bool { return ps[1][a] < ps[1][b] })
}

func reverse(s string) string {
	b := []byte(s)
	for i, j := 0, len(b)-1; i < j; i, j = i+1, j-1 {
		b[i], b[j] = b[j], b[i]
	}
	return string(b)
}

func minOperations(nums []int) int64 {
	var ans int64
	for _, x := range nums {
		p := ps[x&1]
		i := sort.Search(len(p), func(i int) bool { return p[i] >= int64(x) })
		t := int64(1 << 62)
		if i < len(p) {
			t = p[i] - int64(x)
		}
		if i > 0 {
			t = min(t, int64(x)-p[i-1])
		}
		ans += t / 2
	}
	return ans
}
```

#### TypeScript

```ts
const ps: number[][] = [[], []];

for (let i = 1; i <= 10 ** 5; i++) {
    const s = String(i);
    const t1 = [...s].reverse().join('');
    const t2 = [...s.slice(0, -1)].reverse().join('');
    const x = Number(s + t1);
    ps[x & 1].push(x);
    const y = Number(s + t2);
    ps[y & 1].push(y);
}
ps[0].sort((a, b) => a - b);
ps[1].sort((a, b) => a - b);

function minOperations(nums: number[]): number {
    let ans = 0;
    for (const x of nums) {
        const p = ps[x & 1];
        let l = 0;
        let r = p.length;
        while (l < r) {
            const m = (l + r) >> 1;
            if (p[m] < x) {
                l = m + 1;
            } else {
                r = m;
            }
        }
        let t = Number.MAX_SAFE_INTEGER;
        if (l < p.length) {
            t = p[l] - x;
        }
        if (l > 0) {
            t = Math.min(t, x - p[l - 1]);
        }
        ans += Math.floor(t / 2);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
