---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [17.26. Sparse Similarity](https://leetcode.cn/problems/sparse-similarity-lcci)

[中文文档](/lcci/17.26.Sparse%20Similarity/README.md)

## Mô tả

<!-- description:start -->

<p>Độ tương đồng của hai tài liệu (mỗi tài liệu chứa các từ không trùng nhau) được định nghĩa là kích thước của giao chia cho kích thước của hợp. Ví dụ, nếu các tài liệu gồm các số nguyên, độ tương đồng của {1, 5, 3} và {1, 7, 2, 3} là 0.4, vì giao có kích thước 2 và hợp có kích thước 5.&nbsp;Chúng ta có một danh sách dài các tài liệu (các giá trị trong mỗi tài liệu là khác nhau và mỗi tài liệu có một ID đi kèm), trong đó độ tương đồng được cho là &quot;thưa&quot;. Nghĩa là, hai tài liệu bất kỳ được chọn ngẫu nhiên rất có khả năng có độ tương đồng bằng 0. Hãy thiết kế một thuật toán trả về danh sách các cặp ID tài liệu cùng độ tương đồng tương ứng.</p>
<p>Đầu vào là một mảng 2D&nbsp;<code>docs</code>, trong đó&nbsp;<code>docs[i]</code>&nbsp;là tài liệu có id&nbsp;<code>i</code>. Trả về một mảng chuỗi, trong đó mỗi chuỗi biểu diễn một cặp tài liệu có độ tương đồng lớn hơn 0. Chuỗi phải có định dạng&nbsp; <code>{id1},{id2}: {similarity}</code>, trong đó&nbsp;<code>id1</code>&nbsp;là ID nhỏ hơn trong hai tài liệu, còn&nbsp;<code>similarity</code> là độ tương đồng được làm tròn đến bốn chữ số thập phân. Bạn có thể trả về mảng theo bất kỳ thứ tự nào.</p>
<p><strong>Ví dụ:</strong></p>
<pre>

<strong>Đầu vào:</strong>

<code>[

&nbsp; [14, 15, 100, 9, 3],

&nbsp; [32, 1, 9, 3, 5],

&nbsp; [15, 29, 2, 6, 8, 7],

&nbsp; [7, 10]

]</code>

<strong>Đầu ra:</strong>

[

&nbsp; &quot;0,1: 0.2500&quot;,

&nbsp; &quot;0,2: 0.1000&quot;,

&nbsp; &quot;2,3: 0.1429&quot;

]</pre>

<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>docs.length &lt;= 500</code></li>
	<li><code>docs[i].length &lt;= 500</code></li>
	<li>Số cặp tài liệu có độ tương đồng lớn hơn 0 không vượt quá 1000.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cách tiếp cận trực tiếp là liệt kê mọi cặp tài liệu, tạo một set cho mỗi tài liệu, rồi tính giao và hợp. Số lượng tài liệu và độ dài tài liệu đều có thể đạt $500$, trong khi độ tương đồng của hai tài liệu bất kỳ gần bằng $0$, nên phần lớn công việc sẽ tạo và duyệt các giao rỗng.
>
> Có nhiều nhất $1000$ cặp thực sự cần tính độ tương đồng. Thời gian chạy chỉ nên xử lý những tài liệu có chung ít nhất một từ.
>
> Những tài liệu chứa cùng một từ sẽ tạo thành các cặp có giao chứa ít nhất từ đó. Cộng các đóng góp này sẽ cho kích thước của giao. Độ dài của mỗi tài liệu đã biết, nên hợp bằng tổng độ dài của hai tài liệu trừ đi kích thước giao, và không cần duyệt lại các từ.
>
> Vì vậy, chúng ta nhóm các ID tài liệu theo từ. Các ID được thêm theo thứ tự từ nhỏ đến lớn, nên mỗi inverted list đều được sắp xếp và ID nhỏ hơn đã đứng trước. Hash map chỉ lưu các cặp có giao khác rỗng, và độ tương đồng được tính từ key đó.

<!-- thinking:end -->

Chúng ta sử dụng một hash map $d$ để ghi lại các ID tài liệu chứa mỗi từ. Các tài liệu được duyệt theo thứ tự ID tăng dần, và các số nguyên trong một tài liệu là khác nhau, nên các ID trong $d[x]$ tăng nghiêm ngặt.

Hai tài liệu có độ tương đồng lớn hơn $0$ khi và chỉ khi chúng có ít nhất một từ chung. Với mỗi danh sách ID trong $d$, chúng ta liệt kê các cặp ID và cộng dồn chúng trong một hash map $cnt$. Key là cặp $(i, j)$ với $i < j$, còn value là kích thước của giao. Kích thước của hợp là $|docs[i]| + |docs[j]| - |\cap|$. Một tài liệu rỗng không bao giờ xuất hiện trong một danh sách, nên được bỏ qua.

Khi duyệt qua $cnt$, độ tương đồng là tỉ số giữa giao và hợp. Phép chia số thực đôi khi có thể cho kết quả nhỏ hơn một chút so với giá trị thực, nên chúng ta cộng thêm $10^{-9}$ trước khi format kết quả đến bốn chữ số thập phân. Mỗi inverted list đã được sắp xếp theo ID, nên ID nhỏ hơn là thành phần đầu tiên của key.

Độ phức tạp thời gian là $O(m \times n^2)$ và độ phức tạp không gian là $O(S)$, trong đó $n$ là số tài liệu, $m$ là độ dài lớn nhất của một tài liệu, còn $S$ là tổng số từ. Có nhiều nhất $1000$ cặp có độ tương đồng lớn hơn $0$, và các vòng lặp bên trong chạy một lần cho mỗi phần tử trong các giao đó, nên thời gian chạy trên các input đã cho nhỏ hơn cận này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def computeSimilarities(self, docs: List[List[int]]) -> List[str]:
        eps = 1e-9
        d = defaultdict(list)
        for i, v in enumerate(docs):
            for x in v:
                d[x].append(i)
        cnt = Counter()
        for ids in d.values():
            n = len(ids)
            for i in range(n):
                for j in range(i + 1, n):
                    cnt[(ids[i], ids[j])] += 1
        ans = []
        for (i, j), v in cnt.items():
            tot = len(docs[i]) + len(docs[j]) - v
            x = v / tot + eps
            ans.append(f'{i},{j}: {x:.4f}')
        return ans
```

#### Java

```java
class Solution {
    public List<String> computeSimilarities(int[][] docs) {
        int n = docs.length;
        Map<Integer, List<Integer>> d = new HashMap<>();
        for (int i = 0; i < n; ++i) {
            for (int x : docs[i]) {
                d.computeIfAbsent(x, k -> new ArrayList<>()).add(i);
            }
        }
        Map<Long, Integer> cnt = new HashMap<>();
        for (List<Integer> ids : d.values()) {
            int m = ids.size();
            for (int i = 0; i < m; ++i) {
                for (int j = i + 1; j < m; ++j) {
                    long key = 1L * ids.get(i) * n + ids.get(j);
                    cnt.merge(key, 1, Integer::sum);
                }
            }
        }
        List<String> ans = new ArrayList<>();
        for (var e : cnt.entrySet()) {
            long key = e.getKey();
            int v = e.getValue();
            int i = (int) (key / n), j = (int) (key % n);
            int tot = docs[i].length + docs[j].length - v;
            double x = (double) v / tot + 1e-9;
            ans.add(String.format("%d,%d: %.4f", i, j, x));
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> computeSimilarities(vector<vector<int>>& docs) {
        using pii = pair<int, int>;
        double eps = 1e-9;
        unordered_map<int, vector<int>> d;
        for (int i = 0; i < docs.size(); ++i) {
            for (int v : docs[i]) {
                d[v].push_back(i);
            }
        }
        map<pii, int> cnt;
        for (auto& [_, ids] : d) {
            int n = ids.size();
            for (int i = 0; i < n; ++i) {
                for (int j = i + 1; j < n; ++j) {
                    cnt[{ids[i], ids[j]}]++;
                }
            }
        }
        vector<string> ans;
        for (auto& [k, v] : cnt) {
            auto [i, j] = k;
            int tot = docs[i].size() + docs[j].size() - v;
            double x = (double) v / tot + eps;
            char t[20];
            sprintf(t, "%d,%d: %0.4lf", i, j, x);
            ans.push_back(t);
        }
        return ans;
    }
};
```

#### Go

```go
func computeSimilarities(docs [][]int) []string {
	d := map[int][]int{}
	for i, v := range docs {
		for _, x := range v {
			d[x] = append(d[x], i)
		}
	}
	type pair struct{ i, j int }
	cnt := map[pair]int{}
	for _, ids := range d {
		n := len(ids)
		for i := 0; i < n; i++ {
			for j := i + 1; j < n; j++ {
				k := pair{ids[i], ids[j]}
				cnt[k]++
			}
		}
	}
	ans := []string{}
	for k, v := range cnt {
		i, j := k.i, k.j
		tot := len(docs[i]) + len(docs[j]) - v
		x := float64(v)/float64(tot) + 1e-9
		ans = append(ans, fmt.Sprintf("%d,%d: %.4f", i, j, x))
	}
	return ans
}
```

#### TypeScript

```ts
function computeSimilarities(docs: number[][]): string[] {
    const n = docs.length;
    const d = new Map<number, number[]>();
    for (let i = 0; i < n; ++i) {
        for (const x of docs[i]) {
            if (!d.has(x)) {
                d.set(x, []);
            }
            d.get(x)!.push(i);
        }
    }
    const cnt = new Map<number, number>();
    for (const ids of d.values()) {
        const m = ids.length;
        for (let i = 0; i < m; ++i) {
            for (let j = i + 1; j < m; ++j) {
                const key = ids[i] * n + ids[j];
                cnt.set(key, (cnt.get(key) ?? 0) + 1);
            }
        }
    }
    const ans: string[] = [];
    for (const [key, v] of cnt) {
        const i = Math.floor(key / n);
        const j = key % n;
        const tot = docs[i].length + docs[j].length - v;
        const x = v / tot + 1e-9;
        ans.push(`${i},${j}: ${x.toFixed(4)}`);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
