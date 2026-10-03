---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [2143. Choose Numbers From Two Arrays in Range 🔒](https://leetcode.com/problems/choose-numbers-from-two-arrays-in-range)

[Tài liệu tiếng Trung](/solution/2100-2199/2143.Choose%20Numbers%20From%20Two%20Arrays%20in%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums1</code> và <code>nums2</code> có cùng độ dài <code>n</code>.</p>

<p>Một đoạn <code>[l, r]</code> (<strong>bao gồm hai đầu mút</strong>) với <code>0 &lt;= l &lt;= r &lt; n</code> được gọi là <strong>cân bằng</strong> nếu:</p>

<ul>
	<li>Với mọi <code>i</code> trong đoạn <code>[l, r]</code>, bạn chọn <code>nums1[i]</code> hoặc <code>nums2[i]</code>.</li>
	<li>Tổng các số bạn chọn từ <code>nums1</code> bằng tổng các số bạn chọn từ <code>nums2</code> (tổng được xem là <code>0</code> nếu bạn không chọn số nào từ một mảng).</li>
</ul>

<p>Hai đoạn <strong>cân bằng</strong> <code>[l<sub>1</sub>, r<sub>1</sub>]</code> và <code>[l<sub>2</sub>, r<sub>2</sub>]</code> được xem là <strong>khác nhau</strong> nếu ít nhất một trong các điều sau đúng:</p>

<ul>
	<li><code>l<sub>1</sub> != l<sub>2</sub></code></li>
	<li><code>r<sub>1</sub> != r<sub>2</sub></code></li>
	<li>Với ít nhất một <code>i</code>, đoạn thứ nhất chọn <code>nums1[i]</code> còn đoạn thứ hai chọn <code>nums2[i]</code>, hoặc <strong>ngược lại</strong>.</li>
</ul>

<p>Trả về <em>số lượng đoạn <strong>khác nhau</strong> cân bằng</em>. Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>lấy modulo</strong> <code>10<sup>9</sup> + 7</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,5], nums2 = [2,6,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các đoạn cân bằng là:
- [0, 1], trong đó ta chọn nums2[0] và nums1[1].
  Tổng các số được chọn từ nums1 bằng tổng các số được chọn từ nums2: 2 = 2.
- [0, 2], trong đó ta chọn nums1[0], nums2[1] và nums1[2].
  Tổng các số được chọn từ nums1 bằng tổng các số được chọn từ nums2: 1 + 5 = 6.
- [0, 2], trong đó ta chọn nums1[0], nums1[1] và nums2[2].
  Tổng các số được chọn từ nums1 bằng tổng các số được chọn từ nums2: 1 + 2 = 3.
Lưu ý rằng đoạn cân bằng thứ hai và thứ ba là khác nhau.
Trong đoạn cân bằng thứ hai, ta chọn nums2[1], còn trong đoạn cân bằng thứ ba, ta chọn nums1[1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [0,1], nums2 = [1,0]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các đoạn cân bằng là:
- [0, 0], trong đó ta chọn nums1[0].
  Tổng các số được chọn từ nums1 bằng tổng các số được chọn từ nums2: 0 = 0.
- [1, 1], trong đó ta chọn nums2[1].
  Tổng các số được chọn từ nums1 bằng tổng các số được chọn từ nums2: 0 = 0.
- [0, 1], trong đó ta chọn nums1[0] và nums2[1].
  Tổng các số được chọn từ nums1 bằng tổng các số được chọn từ nums2: 0 = 0.
- [0, 1], trong đó ta chọn nums2[0] và nums1[1].
  Tổng các số được chọn từ nums1 bằng tổng các số được chọn từ nums2: 1 = 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con cân bằng chọn một phía tại mỗi chỉ số sao cho hai tổng bằng nhau. Có $O(n^2)$ mảng con và số cách gán là lũy thừa. Các giá trị đủ nhỏ để ta dùng quy hoạch động trên hiệu của hai tổng.
>
> Gọi $f[i][j]$ là số đoạn cân bằng kết thúc tại $i$ có hiệu bằng $j$ (dịch bởi $s_2=\sum\textit{nums2}$). Phần tử $i$ có thể bắt đầu một đoạn mới bằng một trong hai phía, hoặc nối dài đoạn tại $i-1$ với hiệu thay đổi $+a$ hoặc $-b$.
>
> Tính tổng $f[i][s_2]$ theo mọi $i$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubranges(self, nums1: List[int], nums2: List[int]) -> int:
        n = len(nums1)
        s1, s2 = sum(nums1), sum(nums2)
        f = [[0] * (s1 + s2 + 1) for _ in range(n)]
        ans = 0
        mod = 10**9 + 7
        for i, (a, b) in enumerate(zip(nums1, nums2)):
            f[i][a + s2] += 1
            f[i][-b + s2] += 1
            if i:
                for j in range(s1 + s2 + 1):
                    if j >= a:
                        f[i][j] = (f[i][j] + f[i - 1][j - a]) % mod
                    if j + b < s1 + s2 + 1:
                        f[i][j] = (f[i][j] + f[i - 1][j + b]) % mod
            ans = (ans + f[i][s2]) % mod
        return ans
```

#### Java

```java
class Solution {
    public int countSubranges(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int s1 = Arrays.stream(nums1).sum();
        int s2 = Arrays.stream(nums2).sum();
        int[][] f = new int[n][s1 + s2 + 1];
        int ans = 0;
        final int mod = (int) 1e9 + 7;
        for (int i = 0; i < n; ++i) {
            int a = nums1[i], b = nums2[i];
            f[i][a + s2]++;
            f[i][-b + s2]++;
            if (i > 0) {
                for (int j = 0; j <= s1 + s2; ++j) {
                    if (j >= a) {
                        f[i][j] = (f[i][j] + f[i - 1][j - a]) % mod;
                    }
                    if (j + b <= s1 + s2) {
                        f[i][j] = (f[i][j] + f[i - 1][j + b]) % mod;
                    }
                }
            }
            ans = (ans + f[i][s2]) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countSubranges(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        int s1 = accumulate(nums1.begin(), nums1.end(), 0);
        int s2 = accumulate(nums2.begin(), nums2.end(), 0);
        int f[n][s1 + s2 + 1];
        memset(f, 0, sizeof(f));
        int ans = 0;
        const int mod = 1e9 + 7;
        for (int i = 0; i < n; ++i) {
            int a = nums1[i], b = nums2[i];
            f[i][a + s2]++;
            f[i][-b + s2]++;
            if (i) {
                for (int j = 0; j <= s1 + s2; ++j) {
                    if (j >= a) {
                        f[i][j] = (f[i][j] + f[i - 1][j - a]) % mod;
                    }
                    if (j + b <= s1 + s2) {
                        f[i][j] = (f[i][j] + f[i - 1][j + b]) % mod;
                    }
                }
            }
            ans = (ans + f[i][s2]) % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func countSubranges(nums1 []int, nums2 []int) (ans int) {
	n := len(nums1)
	s1, s2 := sum(nums1), sum(nums2)
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, s1+s2+1)
	}
	const mod int = 1e9 + 7
	for i, a := range nums1 {
		b := nums2[i]
		f[i][a+s2]++
		f[i][-b+s2]++
		if i > 0 {
			for j := 0; j <= s1+s2; j++ {
				if j >= a {
					f[i][j] = (f[i][j] + f[i-1][j-a]) % mod
				}
				if j+b <= s1+s2 {
					f[i][j] = (f[i][j] + f[i-1][j+b]) % mod
				}
			}
		}
		ans = (ans + f[i][s2]) % mod
	}
	return
}

func sum(nums []int) (ans int) {
	for _, x := range nums {
		ans += x
	}
	return
}
```

#### TypeScript

```ts
function countSubranges(nums1: number[], nums2: number[]): number {
    const n = nums1.length;
    const s1 = nums1.reduce((a, b) => a + b, 0);
    const s2 = nums2.reduce((a, b) => a + b, 0);
    const f: number[][] = Array(n)
        .fill(0)
        .map(() => Array(s1 + s2 + 1).fill(0));
    const mod = 1e9 + 7;
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        const [a, b] = [nums1[i], nums2[i]];
        f[i][a + s2]++;
        f[i][-b + s2]++;
        if (i) {
            for (let j = 0; j <= s1 + s2; ++j) {
                if (j >= a) {
                    f[i][j] = (f[i][j] + f[i - 1][j - a]) % mod;
                }
                if (j + b <= s1 + s2) {
                    f[i][j] = (f[i][j] + f[i - 1][j + b]) % mod;
                }
            }
        }
        ans = (ans + f[i][s2]) % mod;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
