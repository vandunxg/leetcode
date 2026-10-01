---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [17.22. Word Transformer](https://leetcode.cn/problems/word-transformer-lcci)

[Tài liệu tiếng Trung](/lcci/17.22.Word%20Transformer/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai từ có cùng độ dài và đều nằm trong một từ điển, hãy viết một method để biến đổi từ này thành từ kia bằng cách chỉ thay đổi một chữ cái trong mỗi lần. Từ mới nhận được ở mỗi bước phải nằm trong từ điển.</p>

<p>Hãy viết code để trả về một chuỗi biến đổi khả dĩ. Nếu có nhiều hơn một chuỗi, trả về chuỗi bất kỳ là được.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong>Đầu vào:</strong>

beginWord = &quot;hit&quot;,

endWord = &quot;cog&quot;,

wordList = [&quot;hot&quot;,&quot;dot&quot;,&quot;dog&quot;,&quot;lot&quot;,&quot;log&quot;,&quot;cog&quot;]



<strong>Đầu ra:</strong>

[&quot;hit&quot;,&quot;hot&quot;,&quot;dot&quot;,&quot;lot&quot;,&quot;log&quot;,&quot;cog&quot;]

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong>Đầu vào:</strong>

beginWord = &quot;hit&quot;

endWord = &quot;cog&quot;

wordList = [&quot;hot&quot;,&quot;dot&quot;,&quot;dog&quot;,&quot;lot&quot;,&quot;log&quot;]



<strong>Đầu ra: </strong>[]



<strong>Giải thích:</strong>&nbsp;<em>endWord</em> &quot;cog&quot; không nằm trong từ điển, nên không có chuỗi biến đổi khả dĩ.</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Một đường đi từ $begin$ đến $end$ bằng cách thay đổi từng chữ cái một lần trong danh sách từ. Danh sách đủ nhỏ để có thể tìm kiếm mà không cần dựng graph trước.
>
> DFS thử từng từ chưa dùng khác tại đúng một vị trí, giữ lại path nếu thành công và backtrack nếu không.
>
> `check` đếm số vị trí khác nhau; $vis$ ngăn việc dùng lại. Khi chạm tới $endWord$, trả về $ans$, nếu không thì trả về mảng rỗng. Chấp nhận bất kỳ path nào.

<!-- thinking:end -->

Trước tiên, ta định nghĩa một mảng đáp án `ans`, ban đầu chỉ chứa `beginWord`. Sau đó, ta định nghĩa một mảng `vis` để đánh dấu các từ trong `wordList` đã được duyệt hay chưa.

Tiếp theo, ta thiết kế hàm `dfs(s)`, biểu thị liệu có thể chuyển đổi thành công từ `s` tới `endWord` bắt đầu từ `s` hay không. Nếu thành công, trả về `True`, ngược lại trả về `False`.

Triển khai cụ thể của hàm `dfs(s)` như sau:

1. Nếu `s` bằng `endWord`, quá trình chuyển đổi thành công, trả về `True`;
2. Nếu không, ta duyệt từng từ `t` trong `wordList`. Nếu `t` chưa được duyệt và `s` với `t` chỉ khác nhau ở một ký tự, ta đánh dấu `t` đã được duyệt, thêm `t` vào `ans`, rồi gọi đệ quy `dfs(t)`. Nếu trả về `True`, quá trình chuyển đổi thành công, ta trả về `True`; nếu không, ta xóa `t` khỏi `ans` và tiếp tục duyệt từ tiếp theo;
3. Nếu đã duyệt toàn bộ các từ trong `wordList` mà không tìm thấy từ có thể chuyển đổi, quá trình chuyển đổi thất bại, ta trả về `False`.

Cuối cùng, ta gọi `dfs(beginWord)`. Nếu trả về `True`, quá trình chuyển đổi thành công, ta trả về `ans`, nếu không thì trả về một mảng rỗng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLadders(
        self, beginWord: str, endWord: str, wordList: List[str]
    ) -> List[str]:
        def check(s: str, t: str) -> bool:
            return len(s) == len(t) and sum(a != b for a, b in zip(s, t)) == 1

        def dfs(s: str) -> bool:
            if s == endWord:
                return True
            for i, t in enumerate(wordList):
                if not vis[i] and check(s, t):
                    vis[i] = True
                    ans.append(t)
                    if dfs(t):
                        return True
                    ans.pop()
            return False

        ans = [beginWord]
        vis = [False] * len(wordList)
        return ans if dfs(beginWord) else []
```

#### Java

```java
class Solution {
    private List<String> ans = new ArrayList<>();
    private List<String> wordList;
    private String endWord;
    private boolean[] vis;

    public List<String> findLadders(String beginWord, String endWord, List<String> wordList) {
        this.wordList = wordList;
        this.endWord = endWord;
        ans.add(beginWord);
        vis = new boolean[wordList.size()];
        return dfs(beginWord) ? ans : List.of();
    }

    private boolean dfs(String s) {
        if (s.equals(endWord)) {
            return true;
        }
        for (int i = 0; i < wordList.size(); ++i) {
            String t = wordList.get(i);
            if (vis[i] || !check(s, t)) {
                continue;
            }
            vis[i] = true;
            ans.add(t);
            if (dfs(t)) {
                return true;
            }
            ans.remove(ans.size() - 1);
        }
        return false;
    }

    private boolean check(String s, String t) {
        if (s.length() != t.length()) {
            return false;
        }
        int cnt = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) != t.charAt(i)) {
                ++cnt;
            }
        }
        return cnt == 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        this->endWord = move(endWord);
        this->wordList = move(wordList);
        vis.resize(this->wordList.size(), false);
        ans.push_back(beginWord);
        if (dfs(beginWord)) {
            return ans;
        }
        return {};
    }

private:
    vector<string> ans;
    vector<bool> vis;
    string endWord;
    vector<string> wordList;

    bool check(string& s, string& t) {
        if (s.size() != t.size()) {
            return false;
        }
        int cnt = 0;
        for (int i = 0; i < s.size(); ++i) {
            cnt += s[i] != t[i];
        }
        return cnt == 1;
    }

    bool dfs(string& s) {
        if (s == endWord) {
            return true;
        }
        for (int i = 0; i < wordList.size(); ++i) {
            string& t = wordList[i];
            if (!vis[i] && check(s, t)) {
                vis[i] = true;
                ans.push_back(t);
                if (dfs(t)) {
                    return true;
                }
                ans.pop_back();
            }
        }
        return false;
    }
};
```

#### Go

```go
func findLadders(beginWord string, endWord string, wordList []string) []string {
	ans := []string{beginWord}
	vis := make([]bool, len(wordList))
	check := func(s, t string) bool {
		if len(s) != len(t) {
			return false
		}
		cnt := 0
		for i := range s {
			if s[i] != t[i] {
				cnt++
			}
		}
		return cnt == 1
	}
	var dfs func(s string) bool
	dfs = func(s string) bool {
		if s == endWord {
			return true
		}
		for i, t := range wordList {
			if !vis[i] && check(s, t) {
				vis[i] = true
				ans = append(ans, t)
				if dfs(t) {
					return true
				}
				ans = ans[:len(ans)-1]
			}
		}
		return false
	}
	if dfs(beginWord) {
		return ans
	}
	return []string{}
}
```

#### TypeScript

```ts
function findLadders(beginWord: string, endWord: string, wordList: string[]): string[] {
    const ans: string[] = [beginWord];
    const vis: boolean[] = Array(wordList.length).fill(false);
    const check = (s: string, t: string): boolean => {
        if (s.length !== t.length) {
            return false;
        }
        let cnt = 0;
        for (let i = 0; i < s.length; ++i) {
            if (s[i] !== t[i]) {
                ++cnt;
            }
        }
        return cnt === 1;
    };
    const dfs = (s: string): boolean => {
        if (s === endWord) {
            return true;
        }
        for (let i = 0; i < wordList.length; ++i) {
            const t: string = wordList[i];
            if (!vis[i] && check(s, t)) {
                vis[i] = true;
                ans.push(t);
                if (dfs(t)) {
                    return true;
                }
                ans.pop();
            }
        }
        return false;
    };
    return dfs(beginWord) ? ans : [];
}
```

#### Swift

```swift
class Solution {
    private var ans: [String] = []
    private var wordList: [String] = []
    private var endWord: String = ""
    private var vis: [Bool] = []

    func findLadders(_ beginWord: String, _ endWord: String, _ wordList: [String]) -> [String] {
        self.wordList = wordList
        self.endWord = endWord
        ans.append(beginWord)
        vis = Array(repeating: false, count: wordList.count)
        return dfs(beginWord) ? ans : []
    }

    private func dfs(_ s: String) -> Bool {
        if s == endWord {
            return true
        }
        for i in 0..<wordList.count {
            let t = wordList[i]
            if vis[i] || !check(s, t) {
                continue
            }
            vis[i] = true
            ans.append(t)
            if dfs(t) {
                return true
            }
            ans.removeLast()
        }
        return false
    }

    private func check(_ s: String, _ t: String) -> Bool {
        if s.count != t.count {
            return false
        }
        var cnt = 0
        for (sc, tc) in zip(s, t) {
            if sc != tc {
                cnt += 1
                if cnt > 1 {
                    return false
                }
            }
        }
        return cnt == 1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
