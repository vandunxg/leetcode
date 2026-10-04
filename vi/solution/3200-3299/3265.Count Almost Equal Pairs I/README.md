---
comments: true
difficulty: Medium
rating: 1661
source: Weekly Contest 412 Q2
tags:
    - Array
    - Hash Table
    - Counting
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [3265. Count Almost Equal Pairs I](https://leetcode.com/problems/count-almost-equal-pairs-i)

[中文文档](/solution/3200-3299/3265.Count%20Almost%20Equal%20Pairs%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên dương.</p>

<p>Trong bài toán này, ta gọi hai số nguyên <code>x</code> và <code>y</code> là <strong>gần bằng nhau</strong> nếu cả hai có thể trở nên bằng nhau sau khi thực hiện thao tác sau <strong>nhiều nhất một lần</strong>:</p>

<ul>
	<li>Chọn <strong>một trong hai</strong> số <code>x</code> hoặc <code>y</code> và hoán đổi hai chữ số bất kỳ trong số đã chọn.</li>
</ul>

<p>Trả về số cặp chỉ số <code>i</code> và <code>j</code> trong <code>nums</code> với <code>i &lt; j</code> sao cho <code>nums[i]</code> và <code>nums[j]</code> là <strong>gần bằng nhau</strong>.</p>

<p><strong>Lưu ý</strong> rằng sau khi thực hiện thao tác, một số nguyên có thể có các chữ số 0 ở đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,12,30,17,21]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cặp phần tử gần bằng nhau là:</p>

<ul>
	<li>3 và 30. Hoán đổi 3 và 0 trong 30, ta được 3.</li>
	<li>12 và 21. Hoán đổi 1 và 2 trong 12, ta được 21.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,1,1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi cặp phần tử trong mảng đều gần bằng nhau.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [123,231]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta không thể hoán đổi hai chữ số bất kỳ của 123 hoặc 231 để được số còn lại.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp được tính nếu hai số trở nên bằng nhau sau nhiều nhất một lần hoán đổi chữ số. $n\le 100$ và $M\le 10^6$ cho phép kiểm tra từng cặp, nhưng một số nhỏ hơn không thể tạo thêm chữ số 0 ở đầu bằng cách hoán đổi, nên việc liệt kê một chiều sẽ bỏ sót một số cặp.
>
> Trước tiên, ta sắp xếp mảng. Với mỗi giá trị, ta sinh tất cả các số có được sau nhiều nhất một lần hoán đổi rồi truy vấn số lần xuất hiện của các giá trị trước đó. Các số nhỏ hơn nằm trong bảng; các số lớn hơn có thể được hoán đổi để giảm xuống và khớp với chúng.

<!-- thinking:end -->

Ta có thể liệt kê từng số; với mỗi số, ta liệt kê từng cặp chữ số khác nhau, rồi hoán đổi hai chữ số đó để tạo ra một số mới. Ta lưu số mới này trong hash table $s$, biểu diễn tất cả các số có thể tạo ra sau nhiều nhất một lần hoán đổi. Sau đó, ta đếm có bao nhiêu số đã liệt kê trước đó xuất hiện trong hash table $s$ và cộng số lượng này vào đáp án. Tiếp theo, ta thêm số hiện tại vào hash table $\textit{cnt}$, biểu diễn số lần xuất hiện của số hiện tại.

Phương pháp liệt kê này có thể bỏ sót một số cặp, chẳng hạn $[100, 1]$, vì số nhận được khi hoán đổi các chữ số trong $100$ là $1$, còn các số được liệt kê trước đó không chứa $1$, nên một số cặp bị bỏ sót. Ta có thể giải quyết vấn đề này bằng cách sắp xếp mảng trước khi liệt kê.

Độ phức tạp thời gian là $O(n \times (\log n + \log^3 M))$, và độ phức tạp không gian là $O(n + \log^2 M)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$, còn $M$ là giá trị lớn nhất trong mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPairs(self, nums: List[int]) -> int:
        nums.sort()
        ans = 0
        cnt = defaultdict(int)
        for x in nums:
            vis = {x}
            s = list(str(x))
            for j in range(len(s)):
                for i in range(j):
                    s[i], s[j] = s[j], s[i]
                    vis.add(int("".join(s)))
                    s[i], s[j] = s[j], s[i]
            ans += sum(cnt[x] for x in vis)
            cnt[x] += 1
        return ans
```

#### Java

```java
class Solution {
    public int countPairs(int[] nums) {
        Arrays.sort(nums);
        int ans = 0;
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            Set<Integer> vis = new HashSet<>();
            vis.add(x);
            char[] s = String.valueOf(x).toCharArray();
            for (int j = 0; j < s.length; ++j) {
                for (int i = 0; i < j; ++i) {
                    swap(s, i, j);
                    vis.add(Integer.parseInt(String.valueOf(s)));
                    swap(s, i, j);
                }
            }
            for (int y : vis) {
                ans += cnt.getOrDefault(y, 0);
            }
            cnt.merge(x, 1, Integer::sum);
        }
        return ans;
    }

    private void swap(char[] s, int i, int j) {
        char t = s[i];
        s[i] = s[j];
        s[j] = t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countPairs(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int ans = 0;
        unordered_map<int, int> cnt;

        for (int x : nums) {
            unordered_set<int> vis = {x};
            string s = to_string(x);

            for (int j = 0; j < s.length(); ++j) {
                for (int i = 0; i < j; ++i) {
                    swap(s[i], s[j]);
                    vis.insert(stoi(s));
                    swap(s[i], s[j]);
                }
            }

            for (int y : vis) {
                ans += cnt[y];
            }
            cnt[x]++;
        }

        return ans;
    }
};
```

#### Go

```go
func countPairs(nums []int) (ans int) {
	sort.Ints(nums)
	cnt := make(map[int]int)

	for _, x := range nums {
		vis := make(map[int]struct{})
		vis[x] = struct{}{}
		s := []rune(strconv.Itoa(x))

		for j := 0; j < len(s); j++ {
			for i := 0; i < j; i++ {
				s[i], s[j] = s[j], s[i]
				y, _ := strconv.Atoi(string(s))
				vis[y] = struct{}{}
				s[i], s[j] = s[j], s[i]
			}
		}

		for y := range vis {
			ans += cnt[y]
		}
		cnt[x]++
	}

	return
}
```

#### TypeScript

```ts
function countPairs(nums: number[]): number {
    nums.sort((a, b) => a - b);
    let ans = 0;
    const cnt = new Map<number, number>();

    for (const x of nums) {
        const vis = new Set<number>();
        vis.add(x);
        const s = x.toString().split('');

        for (let j = 0; j < s.length; j++) {
            for (let i = 0; i < j; i++) {
                [s[i], s[j]] = [s[j], s[i]];
                vis.add(+s.join(''));
                [s[i], s[j]] = [s[j], s[i]];
            }
        }

        for (const y of vis) {
            ans += cnt.get(y) || 0;
        }
        cnt.set(x, (cnt.get(x) || 0) + 1);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
