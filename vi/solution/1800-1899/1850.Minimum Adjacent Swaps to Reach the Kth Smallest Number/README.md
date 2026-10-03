---
comments: true
difficulty: Medium
rating: 2073
source: Weekly Contest 239 Q3
tags:
    - Greedy
    - Two Pointers
    - String
---

<!-- problem:start -->

# [1850. Minimum Adjacent Swaps to Reach the Kth Smallest Number](https://leetcode.com/problems/minimum-adjacent-swaps-to-reach-the-kth-smallest-number)

[中文文档](/solution/1800-1899/1850.Minimum%20Adjacent%20Swaps%20to%20Reach%20the%20Kth%20Smallest%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>num</code> biểu diễn một số nguyên lớn và một số nguyên <code>k</code>.</p>

<p>Một số nguyên được gọi là <strong>đặc biệt</strong> nếu nó là một <strong>hoán vị</strong> của các chữ số trong <code>num</code> và có <strong>giá trị lớn hơn</strong> <code>num</code>. Có thể có rất nhiều số nguyên đặc biệt. Tuy nhiên, ta chỉ quan tâm đến những số có <strong>giá trị nhỏ nhất</strong>.</p>

<ul>
<li>Ví dụ, với <code>num = &quot;5489355142&quot;</code>:

    <ul>
    <li>Số nguyên đặc biệt nhỏ thứ <sup>1</sup> là <code>&quot;5489355214&quot;</code>.</li>
    <li>Số nguyên đặc biệt nhỏ thứ <sup>2</sup> là <code>&quot;5489355241&quot;</code>.</li>
    <li>Số nguyên đặc biệt nhỏ thứ <sup>3</sup> là <code>&quot;5489355412&quot;</code>.</li>
    <li>Số nguyên đặc biệt nhỏ thứ <sup>4</sup> là <code>&quot;5489355421&quot;</code>.</li>
    </ul>
    </li>

</ul>

<p>Trả về <em><strong>số lần hoán đổi các chữ số liền kề nhỏ nhất</strong> cần thực hiện trên </em><code>num</code><em> để thu được số nguyên đặc biệt nhỏ thứ </em><code>k<sup>th</sup></code><em><strong>.</strong></em></p>

<p>Các bộ kiểm thử được tạo sao cho số nguyên đặc biệt nhỏ thứ <code>k<sup>th</sup></code>&nbsp; luôn tồn tại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;5489355142&quot;, k = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Số nguyên đặc biệt nhỏ thứ 4<sup>th</sup> là &quot;5489355421&quot;. Để thu được số này:
- Hoán đổi chỉ số 7 với chỉ số 8: &quot;5489355<u>14</u>2&quot; -&gt; &quot;5489355<u>41</u>2&quot;
- Hoán đổi chỉ số 8 với chỉ số 9: &quot;54893554<u>12</u>&quot; -&gt; &quot;54893554<u>21</u>&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;11112&quot;, k = 4
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Số nguyên đặc biệt nhỏ thứ 4<sup>th</sup> là &quot;21111&quot;. Để thu được số này:
- Hoán đổi chỉ số 3 với chỉ số 4: &quot;111<u>12</u>&quot; -&gt; &quot;111<u>21</u>&quot;
- Hoán đổi chỉ số 2 với chỉ số 3: &quot;11<u>12</u>1&quot; -&gt; &quot;11<u>21</u>1&quot;
- Hoán đổi chỉ số 1 với chỉ số 2: &quot;1<u>12</u>11&quot; -&gt; &quot;1<u>21</u>11&quot;
- Hoán đổi chỉ số 0 với chỉ số 1: &quot;<u>12</u>111&quot; -&gt; &quot;<u>21</u>111&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = &quot;00123&quot;, k = 1
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Số nguyên đặc biệt nhỏ thứ 1<sup>st</sup> là &quot;00132&quot;. Để thu được số này:
- Hoán đổi chỉ số 3 với chỉ số 4: &quot;001<u>23</u>&quot; -&gt; &quot;001<u>32</u>&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= num.length &lt;= 1000</code></li>
	<li><code>1 &lt;= k &lt;= 1000</code></li>
	<li><code>num</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hoán vị kế tiếp + Cặp nghịch thế

<!-- thinking:start -->

> **Tư duy**
>
> Trước tiên, ta tìm hoán vị kế tiếp thứ $k$, sau đó tính số lần hoán đổi liền kề để biến chuỗi ban đầu thành chuỗi đó. Vì có các chữ số trùng nhau nên không thể xem các vị trí như một hoán vị tùy ý.
>
> Áp dụng next-permutation $k$ lần để thu được $s$. Ghi lại các chỉ số ban đầu của từng chữ số theo thứ tự, rồi lần lượt gán chúng khi duyệt $s$. Số lần hoán đổi liền kề bằng số nghịch thế của dãy chỉ số đó.

<!-- thinking:end -->

Ta có thể gọi hàm `next_permutation` $k$ lần để thu được hoán vị nhỏ thứ $k$ là $s$.

Tiếp theo, ta chỉ cần tính số lần hoán đổi cần thiết để biến $num$ thành $s$.

Trước hết, xét trường hợp đơn giản khi mọi chữ số trong $num$ đều khác nhau. Khi đó, ta có thể ánh xạ trực tiếp các ký tự chữ số trong $num$ tới các chỉ số. Ví dụ, nếu $num$ là `"54893"` và $s$ là `"98345"`, ta ánh xạ mỗi chữ số trong $num$ tới một chỉ số như sau:

$$
\begin{aligned}
num[0] &= 5 &\rightarrow& \quad 0 \\
num[1] &= 4 &\rightarrow& \quad 1 \\
num[2] &= 8 &\rightarrow& \quad 2 \\
num[3] &= 9 &\rightarrow& \quad 3 \\
num[4] &= 3 &\rightarrow& \quad 4 \\
\end{aligned}
$$

Sau đó, ánh xạ từng chữ số trong $s$ tới chỉ số tương ứng sẽ cho kết quả `"32410"`. Như vậy, số lần hoán đổi cần thiết để biến $num$ thành $s$ bằng số cặp nghịch thế trong mảng chỉ số sau khi ánh xạ $s$.

Nếu $num$ có các chữ số trùng nhau, ta có thể dùng mảng $d$ để lưu các chỉ số xuất hiện của từng chữ số, trong đó $d[i]$ là danh sách các chỉ số mà chữ số $i$ xuất hiện. Để số lần hoán đổi là nhỏ nhất, khi ánh xạ $s$ thành mảng chỉ số, ta chỉ cần tham lam chọn lần lượt các chỉ số của chữ số tương ứng trong $d$.

Cuối cùng, ta có thể dùng hai vòng lặp để tính số cặp nghịch thế hoặc tối ưu bằng Binary Indexed Tree.

Độ phức tạp thời gian là $O(n \times (k + n))$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $num$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMinSwaps(self, num: str, k: int) -> int:
        def next_permutation(nums: List[str]) -> bool:
            n = len(nums)
            i = n - 2
            while i >= 0 and nums[i] >= nums[i + 1]:
                i -= 1
            if i < 0:
                return False
            j = n - 1
            while j >= 0 and nums[j] <= nums[i]:
                j -= 1
            nums[i], nums[j] = nums[j], nums[i]
            nums[i + 1 : n] = nums[i + 1 : n][::-1]
            return True

        s = list(num)
        for _ in range(k):
            next_permutation(s)
        d = [[] for _ in range(10)]
        idx = [0] * 10
        n = len(s)
        for i, c in enumerate(num):
            j = ord(c) - ord("0")
            d[j].append(i)
        arr = [0] * n
        for i, c in enumerate(s):
            j = ord(c) - ord("0")
            arr[i] = d[j][idx[j]]
            idx[j] += 1
        return sum(arr[j] > arr[i] for i in range(n) for j in range(i))
```

#### Java

```java
class Solution {
    public int getMinSwaps(String num, int k) {
        char[] s = num.toCharArray();
        for (int i = 0; i < k; ++i) {
            nextPermutation(s);
        }
        List<Integer>[] d = new List[10];
        Arrays.setAll(d, i -> new ArrayList<>());
        int n = s.length;
        for (int i = 0; i < n; ++i) {
            d[num.charAt(i) - '0'].add(i);
        }
        int[] idx = new int[10];
        int[] arr = new int[n];
        for (int i = 0; i < n; ++i) {
            arr[i] = d[s[i] - '0'].get(idx[s[i] - '0']++);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (arr[j] > arr[i]) {
                    ++ans;
                }
            }
        }
        return ans;
    }

    private boolean nextPermutation(char[] nums) {
        int n = nums.length;
        int i = n - 2;
        while (i >= 0 && nums[i] >= nums[i + 1]) {
            --i;
        }
        if (i < 0) {
            return false;
        }
        int j = n - 1;
        while (j >= 0 && nums[i] >= nums[j]) {
            --j;
        }
        swap(nums, i++, j);
        for (j = n - 1; i < j; ++i, --j) {
            swap(nums, i, j);
        }
        return true;
    }

    private void swap(char[] nums, int i, int j) {
        char t = nums[i];
        nums[i] = nums[j];
        nums[j] = t;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getMinSwaps(string num, int k) {
        string s = num;
        for (int i = 0; i < k; ++i) {
            next_permutation(begin(s), end(num));
        }
        vector<int> d[10];
        int n = num.size();
        for (int i = 0; i < n; ++i) {
            d[num[i] - '0'].push_back(i);
        }
        int idx[10]{};
        vector<int> arr(n);
        for (int i = 0; i < n; ++i) {
            arr[i] = d[s[i] - '0'][idx[s[i] - '0']++];
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (arr[j] > arr[i]) {
                    ++ans;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getMinSwaps(num string, k int) (ans int) {
	s := []byte(num)
	for ; k > 0; k-- {
		nextPermutation(s)
	}
	d := [10][]int{}
	for i, c := range num {
		j := int(c - '0')
		d[j] = append(d[j], i)
	}
	idx := [10]int{}
	n := len(s)
	arr := make([]int, n)
	for i, c := range s {
		j := int(c - '0')
		arr[i] = d[j][idx[j]]
		idx[j]++
	}
	for i := 0; i < n; i++ {
		for j := 0; j < i; j++ {
			if arr[j] > arr[i] {
				ans++
			}
		}
	}
	return
}

func nextPermutation(nums []byte) bool {
	n := len(nums)
	i := n - 2
	for i >= 0 && nums[i] >= nums[i+1] {
		i--
	}
	if i < 0 {
		return false
	}
	j := n - 1
	for j >= 0 && nums[j] <= nums[i] {
		j--
	}
	nums[i], nums[j] = nums[j], nums[i]
	for i, j = i+1, n-1; i < j; i, j = i+1, j-1 {
		nums[i], nums[j] = nums[j], nums[i]
	}
	return true
}
```

#### TypeScript

```ts
function getMinSwaps(num: string, k: number): number {
    const n = num.length;
    const s = num.split('');
    for (let i = 0; i < k; ++i) {
        nextPermutation(s);
    }
    const d: number[][] = Array.from({ length: 10 }, () => []);
    for (let i = 0; i < n; ++i) {
        d[+num[i]].push(i);
    }
    const idx: number[] = Array(10).fill(0);
    const arr: number[] = [];
    for (let i = 0; i < n; ++i) {
        arr.push(d[+s[i]][idx[+s[i]]++]);
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            if (arr[j] > arr[i]) {
                ans++;
            }
        }
    }
    return ans;
}

function nextPermutation(nums: string[]): boolean {
    const n = nums.length;
    let i = n - 2;
    while (i >= 0 && nums[i] >= nums[i + 1]) {
        i--;
    }
    if (i < 0) {
        return false;
    }
    let j = n - 1;
    while (j >= 0 && nums[i] >= nums[j]) {
        j--;
    }
    [nums[i], nums[j]] = [nums[j], nums[i]];
    for (i = i + 1, j = n - 1; i < j; ++i, --j) {
        [nums[i], nums[j]] = [nums[j], nums[i]];
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
