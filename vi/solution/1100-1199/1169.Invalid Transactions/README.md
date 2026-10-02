---
comments: true
difficulty: Medium
rating: 1658
source: Weekly Contest 151 Q1
tags:
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [1169. Invalid Transactions](https://leetcode.com/problems/invalid-transactions)

[中文文档](/solution/1100-1199/1169.Invalid%20Transactions/README.md)

## Mô tả

<!-- description:start -->

<p>Một giao dịch có thể không hợp lệ nếu:</p>

<ul>
	<li>số tiền vượt quá <code>$1000</code>; hoặc</li>
	<li>giao dịch diễn ra trong vòng <code>60</code> phút (tính cả mốc 60 phút) so với một giao dịch khác có <strong>cùng tên</strong> nhưng ở <strong>thành phố khác</strong>.</li>
</ul>

<p>Cho mảng chuỗi <code>transaction</code>, trong đó <code>transactions[i]</code> gồm các giá trị phân tách bằng dấu phẩy, lần lượt biểu thị tên, thời gian (tính bằng phút), số tiền và thành phố của giao dịch.</p>

<p>Trả về danh sách các <code>transactions</code> có thể không hợp lệ. Bạn có thể trả lời theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> transactions = [&quot;alice,20,800,mtv&quot;,&quot;alice,50,100,beijing&quot;]
<strong>Đầu ra:</strong> [&quot;alice,20,800,mtv&quot;,&quot;alice,50,100,beijing&quot;]
<strong>Giải thích:</strong> Giao dịch đầu tiên không hợp lệ vì giao dịch thứ hai cách không quá 60 phút, có cùng tên nhưng ở thành phố khác. Tương tự, giao dịch thứ hai cũng không hợp lệ.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> transactions = [&quot;alice,20,800,mtv&quot;,&quot;alice,50,1200,mtv&quot;]
<strong>Đầu ra:</strong> [&quot;alice,50,1200,mtv&quot;]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> transactions = [&quot;alice,20,800,mtv&quot;,&quot;bob,50,1200,mtv&quot;]
<strong>Đầu ra:</strong> [&quot;bob,50,1200,mtv&quot;]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>transactions.length &lt;= 1000</code></li>
	<li>Mỗi <code>transactions[i]</code> có dạng <code>&quot;{name},{time},{amount},{city}&quot;</code></li>
	<li>Mỗi <code>{name}</code> và <code>{city}</code> chỉ gồm chữ cái tiếng Anh viết thường, có độ dài từ <code>1</code> đến <code>10</code>.</li>
	<li>Mỗi <code>{time}</code> chỉ gồm chữ số và biểu thị một số nguyên từ <code>0</code> đến <code>1000</code>.</li>
	<li>Mỗi <code>{amount}</code> chỉ gồm chữ số và biểu thị một số nguyên từ <code>0</code> đến <code>2000</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Giao dịch không hợp lệ khi và chỉ khi số tiền vượt $1000$, hoặc cùng tên xuất hiện ở thành phố khác trong vòng $60$ phút. Gom nhóm $(\textit{time},\textit{city},\textit{index})$ theo tên rồi so sánh giao dịch mới với các giao dịch trong nhóm để đánh dấu cả hai giao dịch vi phạm. Quy tắc về số tiền được xét riêng. $n$ đủ nhỏ để có thể kiểm tra từng cặp giao dịch cùng tên.

<!-- thinking:end -->

Duyệt danh sách giao dịch. Với mỗi giao dịch, nếu số tiền lớn hơn 1000, hoặc có giao dịch cùng tên ở thành phố khác với khoảng cách thời gian không quá 60 phút, thì thêm giao dịch đó vào đáp án.

Cụ thể, dùng hash table `d` để lưu các giao dịch, trong đó key là tên giao dịch và value là một danh sách. Mỗi phần tử trong danh sách là tuple `(time, city, index)`, cho biết giao dịch có chỉ số `index` diễn ra ở thành phố `city` tại thời điểm `time`. Đồng thời, dùng hash table `idx` để lưu chỉ số các giao dịch thuộc đáp án.

Duyệt danh sách giao dịch. Với mỗi giao dịch, trước tiên thêm giao dịch đó vào hash table `d`, rồi kiểm tra số tiền có lớn hơn 1000 hay không. Nếu có, thêm chỉ số của nó vào đáp án. Tiếp theo, duyệt các giao dịch cùng tên trong `d`. Nếu tên giống nhau, thành phố khác nhau và khoảng cách thời gian không quá 60 phút, thêm chỉ số của giao dịch kia vào đáp án.

Cuối cùng, duyệt các chỉ số trong đáp án và thêm những giao dịch tương ứng vào danh sách kết quả.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số giao dịch.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def invalidTransactions(self, transactions: List[str]) -> List[str]:
        d = defaultdict(list)
        idx = set()
        for i, x in enumerate(transactions):
            name, time, amount, city = x.split(",")
            time, amount = int(time), int(amount)
            d[name].append((time, city, i))
            if amount > 1000:
                idx.add(i)
            for t, c, j in d[name]:
                if c != city and abs(time - t) <= 60:
                    idx.add(i)
                    idx.add(j)
        return [transactions[i] for i in idx]
```

#### Java

```java
class Solution {
    public List<String> invalidTransactions(String[] transactions) {
        Map<String, List<Item>> d = new HashMap<>();
        Set<Integer> idx = new HashSet<>();
        for (int i = 0; i < transactions.length; ++i) {
            var e = transactions[i].split(",");
            String name = e[0];
            int time = Integer.parseInt(e[1]);
            int amount = Integer.parseInt(e[2]);
            String city = e[3];
            d.computeIfAbsent(name, k -> new ArrayList<>()).add(new Item(time, city, i));
            if (amount > 1000) {
                idx.add(i);
            }
            for (Item item : d.get(name)) {
                if (!city.equals(item.city) && Math.abs(time - item.t) <= 60) {
                    idx.add(i);
                    idx.add(item.i);
                }
            }
        }
        List<String> ans = new ArrayList<>();
        for (int i : idx) {
            ans.add(transactions[i]);
        }
        return ans;
    }
}

class Item {
    int t;
    String city;
    int i;

    Item(int t, String city, int i) {
        this.t = t;
        this.city = city;
        this.i = i;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> invalidTransactions(vector<string>& transactions) {
        unordered_map<string, vector<tuple<int, string, int>>> d;
        unordered_set<int> idx;
        for (int i = 0; i < transactions.size(); ++i) {
            vector<string> e = split(transactions[i], ',');
            string name = e[0];
            int time = stoi(e[1]);
            int amount = stoi(e[2]);
            string city = e[3];
            d[name].push_back({time, city, i});
            if (amount > 1000) {
                idx.insert(i);
            }
            for (auto& [t, c, j] : d[name]) {
                if (c != city && abs(time - t) <= 60) {
                    idx.insert(i);
                    idx.insert(j);
                }
            }
        }
        vector<string> ans;
        for (int i : idx) {
            ans.emplace_back(transactions[i]);
        }
        return ans;
    }

    vector<string> split(string& s, char delim) {
        stringstream ss(s);
        string item;
        vector<string> res;
        while (getline(ss, item, delim)) {
            res.emplace_back(item);
        }
        return res;
    }
};
```

#### Go

```go
func invalidTransactions(transactions []string) (ans []string) {
	d := map[string][]tuple{}
	idx := map[int]bool{}
	for i, x := range transactions {
		e := strings.Split(x, ",")
		name := e[0]
		time, _ := strconv.Atoi(e[1])
		amount, _ := strconv.Atoi(e[2])
		city := e[3]
		d[name] = append(d[name], tuple{time, city, i})
		if amount > 1000 {
			idx[i] = true
		}
		for _, item := range d[name] {
			if city != item.city && abs(time-item.t) <= 60 {
				idx[i], idx[item.i] = true, true
			}
		}
	}
	for i := range idx {
		ans = append(ans, transactions[i])
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}

type tuple struct {
	t    int
	city string
	i    int
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
