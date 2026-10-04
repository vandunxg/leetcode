---
comments: true
difficulty: Hard
rating: 2545
source: Weekly Contest 412 Q4
tags:
    - Array
    - Hash Table
    - Counting
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [3267. Count Almost Equal Pairs II](https://leetcode.com/problems/count-almost-equal-pairs-ii)

[中文文档](/solution/3200-3299/3267.Count%20Almost%20Equal%20Pairs%20II/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Lưu ý</strong>: Ở phiên bản này, số lần thực hiện thao tác đã được tăng lên <strong>hai lần</strong>.<!-- notionvc: 278e7cb2-3b05-42fa-8ae9-65f5fd6f7585 --></p>

<p>Cho một mảng gồm các số nguyên dương <code>nums</code>.</p>

<p>Hai số nguyên <code>x</code> và <code>y</code> được gọi là <strong>gần bằng nhau</strong> nếu cả hai số có thể trở nên bằng nhau sau khi thực hiện thao tác sau <strong>nhiều nhất <u>hai lần</u></strong>:</p>

<ul>
	<li>Chọn <strong>một trong hai</strong> số <code>x</code> hoặc <code>y</code>, rồi hoán đổi hai chữ số bất kỳ trong số đã chọn.</li>
</ul>

<p>Trả về số cặp chỉ số <code>i</code> và <code>j</code> trong <code>nums</code> với <code>i &lt; j</code> sao cho <code>nums[i]</code> và <code>nums[j]</code> là <strong>gần bằng nhau</strong>.</p>

<p><strong>Lưu ý</strong> rằng sau khi thực hiện thao tác, một số nguyên được phép có các số 0 ở đầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1023,2310,2130,213]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cặp phần tử gần bằng nhau là:</p>

<ul>
	<li>1023 và 2310. Hoán đổi các chữ số 1 và 2, sau đó hoán đổi các chữ số 0 và 3 trong 1023 để nhận được 2310.</li>
	<li>1023 và 213. Hoán đổi các chữ số 1 và 0, sau đó hoán đổi các chữ số 1 và 2 trong 1023 để nhận được 0213, tức là 213.</li>
	<li>2310 và 213. Hoán đổi các chữ số 2 và 0, sau đó hoán đổi các chữ số 3 và 2 trong 2310 để nhận được 0213, tức là 213.</li>
	<li>2310 và 2130. Hoán đổi các chữ số 3 và 1 trong 2310 để nhận được 2130.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,10,100]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các cặp phần tử gần bằng nhau là:</p>

<ul>
	<li>1 và 10. Hoán đổi các chữ số 1 và 0 trong 10 để nhận được 01, tức là 1.</li>
	<li>1 và 100. Hoán đổi số 0 thứ hai với chữ số 1 trong 100 để nhận được 001, tức là 1.</li>
	<li>10 và 100. Hoán đổi số 0 đầu tiên với chữ số 1 trong 100 để nhận được 010, tức là 10.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 5000</code></li>
	<li><code>1 &lt;= nums[i] &lt; 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Khác với bài I, ở đây ta có thể hoán đổi hai lần và $n\le 5000$. Ta vẫn sắp xếp để không bỏ sót trường hợp biến một số lớn hơn thành số nhỏ hơn khi hoán đổi. Phép đóng qua hai lần hoán đổi có độ phức tạp $O(\log^4 M)$ cho mỗi giá trị, nên nhân với $n$ vẫn phù hợp.
>
> Hoán đổi một lần, sau đó hoán đổi cặp chữ số thứ hai trên các chữ số còn lại, đưa mọi kết quả vào một set, rồi truy vấn số lần xuất hiện trước đó. Khi liệt kê ở vòng lặp bên trong, cần nhớ khôi phục trạng thái.

<!-- thinking:end -->

Ta có thể liệt kê từng số, và với mỗi số, liệt kê từng cặp chữ số khác nhau, sau đó hoán đổi hai chữ số này để thu được một số mới. Ta lưu số mới này vào hash table $\textit{vis}$, biểu diễn tất cả các số có thể thu được sau nhiều nhất một lần hoán đổi. Tiếp tục liệt kê từng cặp chữ số khác nhau, hoán đổi hai chữ số này để thu được một số mới, rồi lưu vào hash table $\textit{vis}$, biểu diễn tất cả các số có thể thu được sau nhiều nhất hai lần hoán đổi.

Cách liệt kê này có thể bỏ sót một số cặp số, chẳng hạn như $[100, 1]$, vì số thu được sau khi hoán đổi $100$ là $1$, nhưng các số đã liệt kê trước đó không bao gồm $1$, nên một số cặp sẽ bị bỏ qua. Ta chỉ cần sắp xếp mảng trước khi liệt kê để giải quyết vấn đề này.

Độ phức tạp thời gian là $O(n \times (\log n + \log^5 M))$, và độ phức tạp không gian là $O(n + \log^4 M)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$, còn $M$ là giá trị lớn nhất trong mảng $\textit{nums}$.

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
            m = len(s)
            for j in range(m):
                for i in range(j):
                    s[i], s[j] = s[j], s[i]
                    vis.add(int("".join(s)))
                    for q in range(i + 1, m):
                        for p in range(i + 1, q):
                            s[p], s[q] = s[q], s[p]
                            vis.add(int("".join(s)))
                            s[p], s[q] = s[q], s[p]
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
                    for (int q = i; q < s.length; ++q) {
                        for (int p = i; p < q; ++p) {
                            swap(s, p, q);
                            vis.add(Integer.parseInt(String.valueOf(s)));
                            swap(s, p, q);
                        }
                    }
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
                    for (int q = i + 1; q < s.length(); ++q) {
                        for (int p = i + 1; p < q; ++p) {
                            swap(s[p], s[q]);
                            vis.insert(stoi(s));
                            swap(s[p], s[q]);
                        }
                    }
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
				for q := i + 1; q < len(s); q++ {
					for p := i + 1; p < q; p++ {
						s[p], s[q] = s[q], s[p]
						z, _ := strconv.Atoi(string(s))
						vis[z] = struct{}{}
						s[p], s[q] = s[q], s[p]
					}
				}
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
                for (let q = i + 1; q < s.length; ++q) {
                    for (let p = i + 1; p < q; ++p) {
                        [s[p], s[q]] = [s[q], s[p]];
                        vis.add(+s.join(''));
                        [s[p], s[q]] = [s[q], s[p]];
                    }
                }
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
