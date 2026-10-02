---
comments: true
difficulty: Medium
rating: 1797
source: Weekly Contest 222 Q2
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1711. Count Good Meals](https://leetcode.com/problems/count-good-meals)

[中文文档](/solution/1700-1799/1711.Count%20Good%20Meals/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Bữa ăn hợp lệ</strong> là bữa ăn chứa <strong>chính xác hai món ăn khác nhau</strong>, có tổng độ ngon bằng một lũy thừa của hai.</p>

<p>Bạn có thể chọn <strong>bất kỳ</strong> hai món ăn khác nhau để tạo thành một bữa ăn hợp lệ.</p>

<p>Cho mảng số nguyên <code>deliciousness</code>, trong đó <code>deliciousness[i]</code> là độ ngon của món ăn thứ <code>i<sup>​​​​​​th</sup>​​​​</code>​​​​. Hãy trả về <em>số <strong>bữa ăn hợp lệ</strong> khác nhau có thể tạo từ danh sách này, lấy modulo</em> <code>10<sup>9</sup> + 7</code>.</p>

<p>Lưu ý rằng các phần tử có chỉ số khác nhau được xem là khác nhau dù có cùng độ ngon.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> deliciousness = [1,3,5,7,9]
<strong>Output:</strong> 4
<strong>Explanation: </strong>Các bữa ăn hợp lệ là (1,3), (1,7), (3,5) và (7,9).
Tổng tương ứng là 4, 8, 8 và 16, đều là lũy thừa của 2.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> deliciousness = [1,1,1,3,3,3,7]
<strong>Output:</strong> 15
<strong>Explanation: </strong>Có 3 cách chọn (1,1), 9 cách chọn (1,3) và 3 cách chọn (1,7).</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= deliciousness.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= deliciousness[i] &lt;= 2<sup>20</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Enumeration of Powers of Two

<!-- thinking:start -->

> **Tư duy**
>
> Liệt kê mọi cặp chỉ số để đếm các cặp không có thứ tự có tổng là lũy thừa của hai sẽ tốn $O(n^2)$ và không đáp ứng được khi $n\le 10^5$.
>
> Các giá trị không vượt quá $2^{20}$, nên chỉ có $O(\log M)$ lũy thừa cần xét. Với mỗi $d$ đã gặp, ta liệt kê một lũy thừa $s$ rồi tra $s-d$ trong hash map.
>
> Chèn $d$ vào bộ đếm sau khi tra cứu để mỗi cặp chỉ được đếm một lần với các phần tử đứng trước. Lấy kết quả modulo $10^9+7$.

<!-- thinking:end -->

Theo đề bài, ta cần đếm số cặp trong mảng có tổng là lũy thừa của $2$. Liệt kê trực tiếp mọi cặp có độ phức tạp $O(n^2)$ và chắc chắn sẽ quá thời gian.

Ta có thể duyệt mảng và dùng hash table $cnt$ để lưu số lần xuất hiện của mỗi phần tử $d$.

Với mỗi phần tử, ta liệt kê các lũy thừa của hai $s$ từ nhỏ đến lớn, rồi cộng số lần xuất hiện của $s - d$ trong hash table vào kết quả. Sau đó tăng số lần xuất hiện của phần tử hiện tại $d$ lên một.

Sau khi duyệt xong, trả về kết quả.

Độ phức tạp thời gian là $O(n \times \log M)$, trong đó $n$ là độ dài mảng `deliciousness`, còn $M$ là cận trên của các phần tử. Với bài này, $M=2^{20}$.

Ta cũng có thể dùng hash table $cnt$ để đếm số lần xuất hiện của từng phần tử trước.

Sau đó, ta liệt kê các lũy thừa của hai $s$ từ nhỏ đến lớn. Với mỗi $s$, duyệt từng cặp khóa-giá trị $(a, m)$ trong hash table. Nếu $s - a$ cũng có trong hash table và $s - a \neq a$, cộng $m \times cnt[s - a]$ vào kết quả; nếu $s - a = a$, cộng $m \times (m - 1)$.

Cuối cùng, chia kết quả cho $2$, lấy modulo $10^9 + 7$ rồi trả về.

Độ phức tạp thời gian giống phương pháp trên.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPairs(self, deliciousness: List[int]) -> int:
        mod = 10**9 + 7
        mx = max(deliciousness) << 1
        cnt = Counter()
        ans = 0
        for d in deliciousness:
            s = 1
            while s <= mx:
                ans = (ans + cnt[s - d]) % mod
                s <<= 1
            cnt[d] += 1
        return ans
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int countPairs(int[] deliciousness) {
        int mx = Arrays.stream(deliciousness).max().getAsInt() << 1;
        int ans = 0;
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int d : deliciousness) {
            for (int s = 1; s <= mx; s <<= 1) {
                ans = (ans + cnt.getOrDefault(s - d, 0)) % MOD;
            }
            cnt.merge(d, 1, Integer::sum);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;

    int countPairs(vector<int>& deliciousness) {
        int mx = *max_element(deliciousness.begin(), deliciousness.end()) << 1;
        unordered_map<int, int> cnt;
        int ans = 0;
        for (auto& d : deliciousness) {
            for (int s = 1; s <= mx; s <<= 1) {
                ans = (ans + cnt[s - d]) % mod;
            }
            ++cnt[d];
        }
        return ans;
    }
};
```

#### Go

```go
func countPairs(deliciousness []int) (ans int) {
	mx := slices.Max(deliciousness) << 1
	const mod int = 1e9 + 7
	cnt := map[int]int{}
	for _, d := range deliciousness {
		for s := 1; s <= mx; s <<= 1 {
			ans = (ans + cnt[s-d]) % mod
		}
		cnt[d]++
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tra cứu ngay trong lúc duyệt. Ta có thể đếm toàn bộ tần suất trước, sau đó liệt kê từng lũy thừa $s$ và từng khóa $a$, ghép với $cnt[s-a]$.
>
> Dùng $m(m-1)$ khi $a=s-a$ và $m\cdot cnt[s-a]$ trong trường hợp còn lại. Mỗi cặp bị đếm hai lần, nên dịch phải một lần trước khi lấy modulo. Độ phức tạp vẫn là $O(n\log M)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPairs(self, deliciousness: List[int]) -> int:
        mod = 10**9 + 7
        cnt = Counter(deliciousness)
        ans = 0
        for i in range(22):
            s = 1 << i
            for a, m in cnt.items():
                if (b := s - a) in cnt:
                    ans += m * (m - 1) if a == b else m * cnt[b]
        return (ans >> 1) % mod
```

#### Java

```java
class Solution {
    private static final int MOD = (int) 1e9 + 7;

    public int countPairs(int[] deliciousness) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int d : deliciousness) {
            cnt.put(d, cnt.getOrDefault(d, 0) + 1);
        }
        long ans = 0;
        for (int i = 0; i < 22; ++i) {
            int s = 1 << i;
            for (var x : cnt.entrySet()) {
                int a = x.getKey(), m = x.getValue();
                int b = s - a;
                if (!cnt.containsKey(b)) {
                    continue;
                }
                ans += 1L * m * (a == b ? m - 1 : cnt.get(b));
            }
        }
        ans >>= 1;
        return (int) (ans % MOD);
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;

    int countPairs(vector<int>& deliciousness) {
        unordered_map<int, int> cnt;
        for (int& d : deliciousness) ++cnt[d];
        long long ans = 0;
        for (int i = 0; i < 22; ++i) {
            int s = 1 << i;
            for (auto& [a, m] : cnt) {
                int b = s - a;
                if (!cnt.count(b)) continue;
                ans += 1ll * m * (a == b ? (m - 1) : cnt[b]);
            }
        }
        ans >>= 1;
        return ans % mod;
    }
};
```

#### Go

```go
func countPairs(deliciousness []int) (ans int) {
	cnt := map[int]int{}
	for _, d := range deliciousness {
		cnt[d]++
	}
	const mod int = 1e9 + 7
	for i := 0; i < 22; i++ {
		s := 1 << i
		for a, m := range cnt {
			b := s - a
			if n, ok := cnt[b]; ok {
				if a == b {
					ans += m * (m - 1)
				} else {
					ans += m * n
				}
			}
		}
	}
	ans >>= 1
	return ans % mod
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
