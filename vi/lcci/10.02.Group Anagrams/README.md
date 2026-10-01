---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [10.02. Group Anagrams](https://leetcode.cn/problems/group-anagrams-lcci)

[中文文档](/lcci/10.02.Group%20Anagrams/README.md)

## Mô tả

<!-- description:start -->

<p>Viết một phương thức để sắp xếp một mảng chuỗi sao cho tất cả các từ đảo chữ nằm trong cùng một nhóm.</p>

<p><b>Lưu ý:&nbsp;</b>Bài toán này hơi khác so với bài gốc trong sách.</p>

<p><strong>Ví dụ:</strong></p>

<pre>

<strong>Đầu vào:</strong> <code>[&quot;eat&quot;, &quot;tea&quot;, &quot;tan&quot;, &quot;ate&quot;, &quot;nat&quot;, &quot;bat&quot;]</code>,

<strong>Đầu ra:</strong>

[

  [&quot;ate&quot;,&quot;eat&quot;,&quot;tea&quot;],

  [&quot;nat&quot;,&quot;tan&quot;],

  [&quot;bat&quot;]

]</pre>

<p><strong>Ghi chú: </strong></p>

<ul>
	<li>Tất cả đầu vào đều là chữ thường.</li>
	<li>Thứ tự đầu ra không quan trọng.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các từ đảo chữ có chung một key sau khi sắp xếp. Kiểm tra từng cặp có phải là từ đảo chữ hay không tốn $O(n^2 k)$.
>
> Băm chuỗi đã sắp xếp sẽ tự động gom các từ ban đầu thành từng nhóm.
>
> Với mỗi $s$, nối nó vào $d[''.join(sorted(s))]$ rồi xuất các danh sách value. Mỗi lần sắp xếp $O(k\log k)$ cho một từ đổi lấy một lần chèn vào hash table.

<!-- thinking:end -->

1. Duyệt mảng chuỗi, sắp xếp từng chuỗi theo **thứ tự từ điển của ký tự**, rồi nhận được một chuỗi mới.
2. Dùng chuỗi mới làm `key` và `[str]` làm `value`, rồi lưu chúng vào hash table (`HashMap<String, List<String>>`).
3. Khi gặp lại `key` giống nhau trong các lần duyệt tiếp theo, thêm chuỗi đó vào `value` tương ứng.

Lấy `strs = ["eat", "tea", "tan", "ate", "nat", "bat"]` làm ví dụ. Sau khi duyệt xong, trạng thái của hash table là:

| key     | value                   |
| ------- | ----------------------- |
| `"aet"` | `["eat", "tea", "ate"]` |
| `"ant"` | `["tan", "nat"] `       |
| `"abt"` | `["bat"] `              |

Cuối cùng, trả về danh sách `value` của hash table.

Độ phức tạp thời gian là $O(n\times k\times \log k)$, trong đó $n$ và $k$ lần lượt là độ dài của mảng chuỗi và độ dài lớn nhất của một chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        d = defaultdict(list)
        for s in strs:
            k = ''.join(sorted(s))
            d[k].append(s)
        return list(d.values())
```

#### Java

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> d = new HashMap<>();
        for (String s : strs) {
            char[] t = s.toCharArray();
            Arrays.sort(t);
            String k = String.valueOf(t);
            d.computeIfAbsent(k, key -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(d.values());
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> d;
        for (auto& s : strs) {
            string k = s;
            sort(k.begin(), k.end());
            d[k].emplace_back(s);
        }
        vector<vector<string>> ans;
        for (auto& [_, v] : d) ans.emplace_back(v);
        return ans;
    }
};
```

#### Go

```go
func groupAnagrams(strs []string) (ans [][]string) {
	d := map[string][]string{}
	for _, s := range strs {
		t := []byte(s)
		sort.Slice(t, func(i, j int) bool { return t[i] < t[j] })
		k := string(t)
		d[k] = append(d[k], s)
	}
	for _, v := range d {
		ans = append(ans, v)
	}
	return
}
```

#### TypeScript

```ts
function groupAnagrams(strs: string[]): string[][] {
    const d: Map<string, string[]> = new Map();
    for (const s of strs) {
        const k = s.split('').sort().join('');
        if (!d.has(k)) {
            d.set(k, []);
        }
        d.get(k)!.push(s);
    }
    return Array.from(d.values());
}
```

#### Swift

```swift
class Solution {
    func groupAnagrams(_ strs: [String]) -> [[String]] {
        var d = [String: [String]]()
        for s in strs {
            let t = String(s.sorted())
            d[t, default: []].append(s)
        }
        return Array(d.values)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Counting

<!-- thinking:start -->

> **Tư duy**
>
> Với một alphabet nhỏ, sorting key có thể chuyển thành tuple tần suất, loại bỏ thừa số $O(k\log k)$.
>
> Dùng một bộ đếm độ dài $26$ (hoặc một tuple tạo từ nó) làm key; cách gom nhóm không đổi và mỗi từ có độ phức tạp $O(k+C)$.

<!-- thinking:end -->

Ta cũng có thể đổi phần sắp xếp trong Lời giải 1 thành đếm, tức là dùng các ký tự trong mỗi chuỗi $s$ và số lần xuất hiện của chúng làm `key`, rồi dùng chuỗi $s$ làm `value` để lưu vào hash table.

Độ phức tạp thời gian là $O(n\times (k + C))$, trong đó $n$ và $k$ lần lượt là độ dài của mảng chuỗi và độ dài lớn nhất của một chuỗi, còn $C$ là kích thước của tập ký tự. Trong bài này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        d = defaultdict(list)
        for s in strs:
            cnt = [0] * 26
            for c in s:
                cnt[ord(c) - ord('a')] += 1
            d[tuple(cnt)].append(s)
        return list(d.values())
```

#### Java

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> d = new HashMap<>();
        for (String s : strs) {
            int[] cnt = new int[26];
            for (int i = 0; i < s.length(); ++i) {
                ++cnt[s.charAt(i) - 'a'];
            }
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i < 26; ++i) {
                if (cnt[i] > 0) {
                    sb.append((char) ('a' + i)).append(cnt[i]);
                }
            }
            String k = sb.toString();
            d.computeIfAbsent(k, key -> new ArrayList<>()).add(s);
        }
        return new ArrayList<>(d.values());
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> d;
        for (auto& s : strs) {
            int cnt[26] = {0};
            for (auto& c : s) ++cnt[c - 'a'];
            string k;
            for (int i = 0; i < 26; ++i) {
                if (cnt[i]) {
                    k += 'a' + i;
                    k += to_string(cnt[i]);
                }
            }
            d[k].emplace_back(s);
        }
        vector<vector<string>> ans;
        for (auto& [_, v] : d) ans.emplace_back(v);
        return ans;
    }
};
```

#### Go

```go
func groupAnagrams(strs []string) (ans [][]string) {
	d := map[[26]int][]string{}
	for _, s := range strs {
		cnt := [26]int{}
		for _, c := range s {
			cnt[c-'a']++
		}
		d[cnt] = append(d[cnt], s)
	}
	for _, v := range d {
		ans = append(ans, v)
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
