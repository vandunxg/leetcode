---
comments: true
difficulty: Medium
rating: 1737
source: Weekly Contest 506 Q2
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3960. Frequency Balance Subarray](https://leetcode.com/problems/frequency-balance-subarray)

[中文文档](/solution/3900-3999/3960.Frequency%20Balance%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên ​​​​​​​<code>nums</code>.</p>

<p>Định nghĩa <strong><span data-keyword="subarray-nonempty">mảng con</span> cân bằng tần suất</strong> như sau:</p>

<ul>
	<li>Nếu mảng con chỉ chứa một giá trị phân biệt, nó cân bằng tần suất.</li>
	<li>Nếu không, phải tồn tại một số nguyên dương <code>f</code> sao cho mỗi giá trị phân biệt trong mảng con xuất hiện <code>f</code> hoặc <code>2 * f</code> lần, đồng thời cả hai <span data-keyword="frequency-array">tần suất</span> đều xuất hiện trong các giá trị phân biệt.</li>
</ul>

<p>Trả về một số nguyên biểu thị độ dài của mảng con cân bằng tần suất <strong>dài nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2,1,2,3,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng con cân bằng tần suất dài nhất là <code>[2, 1, 2, 3, 3]</code>.</li>
	<li>Các phần tử xuất hiện nhiều nhất là 2 và 3, mỗi phần tử xuất hiện hai lần.</li>
	<li>Phần tử còn lại 1 xuất hiện một lần, thỏa mãn yêu cầu.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,5,5,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Mảng con cân bằng tần suất dài nhất là <code>[5, 5, 5, 5]</code>.</li>
	<li>Phần tử xuất hiện nhiều nhất là 5.</li>
	<li>Không có phần tử nào khác thỏa mãn yêu cầu.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì mọi phần tử chỉ xuất hiện một lần, độ dài của mảng con cân bằng tần suất dài nhất là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>​​​​​​​3</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>​​​​​​​9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 10^3$, vì vậy duyệt cả hai đầu mút trong $O(n^2)$ là chấp nhận được. Cân bằng có nghĩa là hoặc chỉ có một giá trị, hoặc có đúng hai giá trị với tần suất theo tỉ lệ $2:1$.
>
> Cố định $l$ và mở rộng $r$, duy trì số lần xuất hiện của mỗi giá trị trong $\textit{cnt}$ và “có bao nhiêu giá trị có tần suất này” trong $\textit{freq}$. Mỗi lần mở rộng kiểm tra hai dạng trên trong $O(1)$ và cập nhật độ dài lớn nhất.

<!-- thinking:end -->

Ta có thể duyệt đầu trái $l$ của mảng con trong đoạn $[0, n)$, sau đó duyệt đầu phải $r$ từ trái sang phải, bắt đầu từ $l$. Trong quá trình duyệt, ta sử dụng hai hash table $\textit{cnt}$ và $\textit{freq}$ để lần lượt ghi lại tần suất của mỗi phần tử trong mảng con và số lượng giá trị có mỗi tần suất.

Khi một trong các điều kiện sau được thỏa mãn, cập nhật đáp án $\textit{ans} = \max(\textit{ans}, r - l + 1)$:

- Chỉ có một phần tử phân biệt trong hash table $\textit{cnt}$, tức là độ dài của $\textit{cnt}$ bằng $1$;
- Chỉ có hai giá trị tần suất phân biệt trong hash table $\textit{freq}$, tức là độ dài của $\textit{freq}$ bằng $2$, và một giá trị tần suất đúng bằng hai lần giá trị còn lại;

Sau khi duyệt xong, trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getLength(self, nums: List[int]) -> int:
        n = len(nums)
        ans = 1
        for l in range(n):
            cnt = Counter()
            freq = Counter()
            for r in range(l, n):
                x = nums[r]
                if freq[cnt[x]]:
                    freq[cnt[x]] -= 1
                    if freq[cnt[x]] == 0:
                        freq.pop(cnt[x])
                cnt[x] += 1
                freq[cnt[x]] += 1
                if (len(cnt) == 1) or (len(freq) == 2 and (freq[cnt[x] * 2] or (cnt[x] % 2 == 0 and freq[cnt[x] // 2]))):
                    ans = max(ans, r - l + 1)
        return ans
```

#### Java

```java
class Solution {
    public int getLength(int[] nums) {
        int n = nums.length;
        int ans = 1;
        for (int l = 0; l < n; l++) {
            Map<Integer, Integer> cnt = new HashMap<>();
            Map<Integer, Integer> freq = new HashMap<>();
            for (int r = l; r < n; r++) {
                int x = nums[r];
                int c = cnt.getOrDefault(x, 0);
                if (freq.getOrDefault(c, 0) > 0) {
                    freq.put(c, freq.get(c) - 1);
                    if (freq.get(c) == 0) {
                        freq.remove(c);
                    }
                }
                cnt.put(x, c + 1);
                freq.merge(cnt.get(x), 1, Integer::sum);
                int cx = cnt.get(x);
                if (cnt.size() == 1
                    || (freq.size() == 2
                        && (freq.getOrDefault(cx * 2, 0) > 0
                            || (cx % 2 == 0 && freq.getOrDefault(cx / 2, 0) > 0)))) {
                    ans = Math.max(ans, r - l + 1);
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
    int getLength(vector<int>& nums) {
        int n = nums.size();
        int ans = 1;

        for (int l = 0; l < n; ++l) {
            unordered_map<int, int> cnt;
            unordered_map<int, int> freq;

            for (int r = l; r < n; ++r) {
                int x = nums[r];
                int c = cnt[x];

                if (freq.contains(c)) {
                    if (--freq[c] == 0) {
                        freq.erase(c);
                    }
                }

                ++cnt[x];
                ++freq[cnt[x]];

                if (cnt.size() == 1 || (freq.size() == 2 && (freq.contains(cnt[x] * 2) || (cnt[x] % 2 == 0 && freq.contains(cnt[x] / 2))))) {
                    ans = max(ans, r - l + 1);
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func getLength(nums []int) int {
	n := len(nums)
	ans := 1
	for l := 0; l < n; l++ {
		cnt := make(map[int]int)
		freq := make(map[int]int)
		for r := l; r < n; r++ {
			x := nums[r]
			c := cnt[x]
			if freq[c] > 0 {
				freq[c]--
				if freq[c] == 0 {
					delete(freq, c)
				}
			}
			cnt[x] = c + 1
			freq[cnt[x]]++
			cx := cnt[x]
			if len(cnt) == 1 || (len(freq) == 2 && (freq[cx*2] > 0 || (cx%2 == 0 && freq[cx/2] > 0))) {
				ans = max(ans, r-l+1)
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function getLength(nums: number[]): number {
    const n = nums.length;
    let ans = 1;

    for (let l = 0; l < n; l++) {
        const cnt = new Map<number, number>();
        const freq = new Map<number, number>();

        for (let r = l; r < n; r++) {
            const x = nums[r];
            const c = cnt.get(x) ?? 0;

            if ((freq.get(c) ?? 0) > 0) {
                const f = (freq.get(c) ?? 0) - 1;
                if (f === 0) {
                    freq.delete(c);
                } else {
                    freq.set(c, f);
                }
            }

            cnt.set(x, c + 1);
            freq.set(c + 1, (freq.get(c + 1) ?? 0) + 1);

            const cur = c + 1;

            if (
                cnt.size === 1 ||
                (freq.size === 2 &&
                    ((freq.get(cur * 2) ?? 0) > 0 ||
                        (cur % 2 === 0 && (freq.get(cur / 2) ?? 0) > 0)))
            ) {
                ans = Math.max(ans, r - l + 1);
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
