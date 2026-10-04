---
comments: true
difficulty: Medium
rating: 1787
source: Weekly Contest 428 Q2
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Array
    - String
---

<!-- problem:start -->

# [3387. Maximize Amount After Two Days of Conversions](https://leetcode.com/problems/maximize-amount-after-two-days-of-conversions)

[中文文档](/solution/3300-3399/3387.Maximize%20Amount%20After%20Two%20Days%20of%20Conversions/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>initialCurrency</code> và bắt đầu với <code>1.0</code> đơn vị <code>initialCurrency</code>.</p>

<p>Bạn cũng được cho bốn mảng chứa các cặp tiền tệ (chuỗi) và tỷ giá (số thực):</p>

<ul>
	<li><code>pairs1[i] = [startCurrency<sub>i</sub>, targetCurrency<sub>i</sub>]</code> biểu thị rằng bạn có thể đổi từ <code>startCurrency<sub>i</sub></code> sang <code>targetCurrency<sub>i</sub></code> với tỷ giá <code>rates1[i]</code> vào <strong>ngày 1</strong>.</li>
	<li><code>pairs2[i] = [startCurrency<sub>i</sub>, targetCurrency<sub>i</sub>]</code> biểu thị rằng bạn có thể đổi từ <code>startCurrency<sub>i</sub></code> sang <code>targetCurrency<sub>i</sub></code> với tỷ giá <code>rates2[i]</code> vào <strong>ngày 2</strong>.</li>
	<li>Ngoài ra, mỗi <code>targetCurrency</code> có thể được đổi ngược lại thành <code>startCurrency</code> tương ứng với tỷ giá <code>1 / rate</code>.</li>
</ul>

<p>Bạn có thể thực hiện <strong>bất kỳ</strong> số lần đổi nào, <strong>kể cả không đổi lần nào</strong>, sử dụng <code>rates1</code> vào ngày 1, <strong>sau đó</strong> thực hiện thêm bất kỳ số lần đổi nào, <strong>kể cả không đổi lần nào</strong>, sử dụng <code>rates2</code> vào ngày 2.</p>

<p>Hãy trả về lượng <code>initialCurrency</code> <strong>lớn nhất</strong> mà bạn có thể có sau khi thực hiện bất kỳ số lần đổi nào trong cả hai ngày <strong>theo đúng thứ tự</strong>.</p>

<p><strong>Lưu ý: </strong>Các tỷ giá chuyển đổi là hợp lệ và sẽ không có mâu thuẫn trong tỷ giá của mỗi ngày. Tỷ giá của hai ngày độc lập với nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">initialCurrency = &quot;EUR&quot;, pairs1 = [[&quot;EUR&quot;,&quot;USD&quot;],[&quot;USD&quot;,&quot;JPY&quot;]], rates1 = [2.0,3.0], pairs2 = [[&quot;JPY&quot;,&quot;USD&quot;],[&quot;USD&quot;,&quot;CHF&quot;],[&quot;CHF&quot;,&quot;EUR&quot;]], rates2 = [4.0,5.0,6.0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">720.00000</span></p>

<p><strong>Giải thích:</strong></p>

<p>Để nhận được lượng <strong>EUR</strong> lớn nhất, bắt đầu với 1.0 <strong>EUR</strong>:</p>

<ul>
	<li>Ngày 1:
	<ul>
		<li>Đổi <strong>EUR </strong>sang <strong>USD</strong> để nhận được 2.0 <strong>USD</strong>.</li>
		<li>Đổi <strong>USD</strong> sang <strong>JPY</strong> để nhận được 6.0 <strong>JPY</strong>.</li>
	</ul>
	</li>
	<li>Ngày 2:
	<ul>
		<li>Đổi <strong>JPY</strong> sang <strong>USD</strong> để nhận được 24.0 <strong>USD</strong>.</li>
		<li>Đổi <strong>USD</strong> sang <strong>CHF</strong> để nhận được 120.0 <strong>CHF</strong>.</li>
		<li>Cuối cùng, đổi <strong>CHF</strong> sang <strong>EUR</strong> để nhận được 720.0 <strong>EUR</strong>.</li>
	</ul>
	</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">initialCurrency = &quot;NGN&quot;, pairs1 = </span>[[&quot;NGN&quot;,&quot;EUR&quot;]]<span class="example-io">, rates1 = </span>[9.0]<span class="example-io">, pairs2 = </span>[[&quot;NGN&quot;,&quot;EUR&quot;]]<span class="example-io">, rates2 = </span>[6.0]</p>

<p><strong>Đầu ra:</strong> 1.50000</p>

<p><strong>Giải thích:</strong></p>

<p>Đổi <strong>NGN</strong> sang <strong>EUR</strong> vào ngày 1 rồi đổi <strong>EUR</strong> sang <strong>NGN</strong> bằng tỷ giá nghịch đảo vào ngày 2 sẽ cho lượng tiền lớn nhất.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">initialCurrency = &quot;USD&quot;, pairs1 = [[&quot;USD&quot;,&quot;EUR&quot;]], rates1 = [1.0], pairs2 = [[&quot;EUR&quot;,&quot;JPY&quot;]], rates2 = [10.0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1.00000</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong ví dụ này, không cần thực hiện bất kỳ lần đổi nào trong cả hai ngày.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= initialCurrency.length &lt;= 3</code></li>
	<li><code>initialCurrency</code> chỉ bao gồm các chữ cái tiếng Anh viết hoa.</li>
	<li><code>1 &lt;= n == pairs1.length &lt;= 10</code></li>
	<li><code>1 &lt;= m == pairs2.length &lt;= 10</code></li>
	<li><code>pairs1[i] == [startCurrency<sub>i</sub>, targetCurrency<sub>i</sub>]</code><!-- notionvc: c31b5bb8-4df6-4987-9bcd-6dff8a5f7cd4 --></li>
	<li><code>pairs2[i] == [startCurrency<sub>i</sub>, targetCurrency<sub>i</sub>]</code><!--{C}%3C!%2D%2D%20notionvc%3A%20c31b5bb8-4df6-4987-9bcd-6dff8a5f7cd4%20%2D%2D%3E--></li>
	<li><code>1 &lt;= startCurrency<sub>i</sub>.length, targetCurrency<sub>i</sub>.length &lt;= 3</code></li>
	<li><code>startCurrency<sub>i</sub></code> và <code>targetCurrency<sub>i</sub></code> chỉ bao gồm các chữ cái tiếng Anh viết hoa.</li>
	<li><code>rates1.length == n</code></li>
	<li><code>rates2.length == m</code></li>
	<li><code>1.0 &lt;= rates1[i], rates2[i] &lt;= 10.0</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho không có mâu thuẫn hoặc chu kỳ trong đồ thị chuyển đổi của mỗi ngày.</li>
	<li>Dữ liệu đầu vào được tạo sao cho kết quả <strong>không vượt quá</strong> <code>5 * 10<sup>10</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai ngày tương ứng với hai đồ thị tỷ giá; ta có thể tự do đổi tiền và muốn thu về lượng tiền ban đầu lớn nhất. Số loại tiền rất nhỏ, nên dùng DFS từ loại tiền ban đầu để tìm lượng lớn nhất của mọi loại tiền trong ngày đó.
>
> Mỗi tỷ giá đều là hai chiều, với $r$ và $1/r$. Đầu vào không có chu kỳ arbitrage, nên một lần duyệt sẽ cho hệ số nhân lớn nhất từ loại tiền ban đầu.
>
> Ngày đầu tiên tạo ra $d1[a]$; ngày thứ hai đổi $a$ trở lại với $1/d2[a]$. Đáp án là giá trị lớn nhất của $d1[a]/d2[a]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxAmount(
        self,
        initialCurrency: str,
        pairs1: List[List[str]],
        rates1: List[float],
        pairs2: List[List[str]],
        rates2: List[float],
    ) -> float:
        d1 = self.build(pairs1, rates1, initialCurrency)
        d2 = self.build(pairs2, rates2, initialCurrency)
        return max(d1.get(a, 0) / r2 for a, r2 in d2.items())

    def build(
        self, pairs: List[List[str]], rates: List[float], init: str
    ) -> Dict[str, float]:
        def dfs(a: str, v: float):
            d[a] = v
            for b, r in g[a]:
                if b not in d:
                    dfs(b, v * r)

        g = defaultdict(list)
        for (a, b), r in zip(pairs, rates):
            g[a].append((b, r))
            g[b].append((a, 1 / r))
        d = {}
        dfs(init, 1)
        return d
```

#### Java

```java
class Solution {
    public double maxAmount(String initialCurrency, List<List<String>> pairs1, double[] rates1,
        List<List<String>> pairs2, double[] rates2) {
        Map<String, Double> d1 = build(pairs1, rates1, initialCurrency);
        Map<String, Double> d2 = build(pairs2, rates2, initialCurrency);
        double ans = 0;
        for (Map.Entry<String, Double> entry : d2.entrySet()) {
            String currency = entry.getKey();
            double rate = entry.getValue();
            if (d1.containsKey(currency)) {
                ans = Math.max(ans, d1.get(currency) / rate);
            }
        }
        return ans;
    }

    private Map<String, Double> build(List<List<String>> pairs, double[] rates, String init) {
        Map<String, List<Pair<String, Double>>> g = new HashMap<>();
        Map<String, Double> d = new HashMap<>();
        for (int i = 0; i < pairs.size(); ++i) {
            String a = pairs.get(i).get(0);
            String b = pairs.get(i).get(1);
            double r = rates[i];
            g.computeIfAbsent(a, k -> new ArrayList<>()).add(new Pair<>(b, r));
            g.computeIfAbsent(b, k -> new ArrayList<>()).add(new Pair<>(a, 1 / r));
        }
        dfs(g, d, init, 1.0);
        return d;
    }

    private void dfs(
        Map<String, List<Pair<String, Double>>> g, Map<String, Double> d, String a, double v) {
        if (d.containsKey(a)) {
            return;
        }

        d.put(a, v);
        for (Pair<String, Double> pair : g.getOrDefault(a, List.of())) {
            String b = pair.getKey();
            double r = pair.getValue();
            if (!d.containsKey(b)) {
                dfs(g, d, b, v * r);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    double maxAmount(string initialCurrency, vector<vector<string>>& pairs1, vector<double>& rates1, vector<vector<string>>& pairs2, vector<double>& rates2) {
        unordered_map<string, double> d1 = build(pairs1, rates1, initialCurrency);
        unordered_map<string, double> d2 = build(pairs2, rates2, initialCurrency);
        double ans = 0;
        for (const auto& [currency, rate] : d2) {
            if (d1.find(currency) != d1.end()) {
                ans = max(ans, d1[currency] / rate);
            }
        }
        return ans;
    }

private:
    unordered_map<string, double> build(vector<vector<string>>& pairs, vector<double>& rates, const string& init) {
        unordered_map<string, vector<pair<string, double>>> g;
        unordered_map<string, double> d;
        for (int i = 0; i < pairs.size(); ++i) {
            const string& a = pairs[i][0];
            const string& b = pairs[i][1];
            double r = rates[i];
            g[a].push_back({b, r});
            g[b].push_back({a, 1 / r});
        }

        auto dfs = [&](this auto&& dfs, const string& a, double v) -> void {
            if (d.find(a) != d.end()) {
                return;
            }

            d[a] = v;
            for (const auto& [b, r] : g[a]) {
                if (d.find(b) == d.end()) {
                    dfs(b, v * r);
                }
            }
        };
        dfs(init, 1.0);
        return d;
    }
};
```

#### Go

```go
type Pair struct {
	Key   string
	Value float64
}

func maxAmount(initialCurrency string, pairs1 [][]string, rates1 []float64, pairs2 [][]string, rates2 []float64) (ans float64) {
	d1 := build(pairs1, rates1, initialCurrency)
	d2 := build(pairs2, rates2, initialCurrency)
	for currency, rate := range d2 {
		if val, found := d1[currency]; found {
			ans = max(ans, val/rate)
		}
	}
	return
}

func build(pairs [][]string, rates []float64, init string) map[string]float64 {
	g := make(map[string][]Pair)
	d := make(map[string]float64)

	for i := 0; i < len(pairs); i++ {
		a := pairs[i][0]
		b := pairs[i][1]
		r := rates[i]
		g[a] = append(g[a], Pair{Key: b, Value: r})
		g[b] = append(g[b], Pair{Key: a, Value: 1.0 / r})
	}

	dfs(g, d, init, 1.0)
	return d
}

func dfs(g map[string][]Pair, d map[string]float64, a string, v float64) {
	if _, found := d[a]; found {
		return
	}

	d[a] = v
	for _, pair := range g[a] {
		b := pair.Key
		r := pair.Value
		if _, found := d[b]; !found {
			dfs(g, d, b, v*r)
		}
	}
}
```

#### TypeScript

```ts
class Pair {
    constructor(
        public key: string,
        public value: number,
    ) {}
}

function maxAmount(
    initialCurrency: string,
    pairs1: string[][],
    rates1: number[],
    pairs2: string[][],
    rates2: number[],
): number {
    const d1 = build(pairs1, rates1, initialCurrency);
    const d2 = build(pairs2, rates2, initialCurrency);
    let ans = 0;
    for (const [currency, rate] of Object.entries(d2)) {
        if (currency in d1) {
            ans = Math.max(ans, d1[currency] / rate);
        }
    }
    return ans;
}

function build(pairs: string[][], rates: number[], init: string): { [key: string]: number } {
    const g: { [key: string]: Pair[] } = {};
    const d: { [key: string]: number } = {};
    for (let i = 0; i < pairs.length; ++i) {
        const a = pairs[i][0];
        const b = pairs[i][1];
        const r = rates[i];
        if (!g[a]) g[a] = [];
        if (!g[b]) g[b] = [];
        g[a].push(new Pair(b, r));
        g[b].push(new Pair(a, 1 / r));
    }
    dfs(g, d, init, 1.0);
    return d;
}

function dfs(
    g: { [key: string]: Pair[] },
    d: { [key: string]: number },
    a: string,
    v: number,
): void {
    if (a in d) {
        return;
    }

    d[a] = v;
    for (const pair of g[a] || []) {
        const b = pair.key;
        const r = pair.value;
        if (!(b in d)) {
            dfs(g, d, b, v * r);
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
