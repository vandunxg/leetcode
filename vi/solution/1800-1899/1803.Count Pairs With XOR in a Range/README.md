---
comments: true
difficulty: Hard
rating: 2479
source: Weekly Contest 233 Q4
tags:
    - Bit Manipulation
    - Trie
    - Array
---

<!-- problem:start -->

# [1803. Count Pairs With XOR in a Range](https://leetcode.com/problems/count-pairs-with-xor-in-a-range)

[中文文档](/solution/1800-1899/1803.Count%20Pairs%20With%20XOR%20in%20a%20Range/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> (đánh chỉ số từ <strong>0</strong>) và hai số nguyên <code>low</code>, <code>high</code>, hãy trả về <em>số lượng <strong>cặp tốt</strong></em>.</p>

<p>Một <strong>cặp tốt</strong> là một cặp <code>(i, j)</code> sao cho <code>0 &lt;= i &lt; j &lt; nums.length</code> và <code>low &lt;= (nums[i] XOR nums[j]) &lt;= high</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,2,7], low = 2, high = 6
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Tất cả cặp tốt là:
    - (0, 1): nums[0] XOR nums[1] = 5
    - (0, 2): nums[0] XOR nums[2] = 3
    - (0, 3): nums[0] XOR nums[3] = 6
    - (1, 2): nums[1] XOR nums[2] = 6
    - (1, 3): nums[1] XOR nums[3] = 3
    - (2, 3): nums[2] XOR nums[3] = 5
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [9,8,4,2,1], low = 5, high = 14
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Tất cả cặp tốt là:
​​​​​    - (0, 2): nums[0] XOR nums[2] = 13
&nbsp;   - (0, 3): nums[0] XOR nums[3] = 11
&nbsp;   - (0, 4): nums[0] XOR nums[4] = 8
&nbsp;   - (1, 2): nums[1] XOR nums[2] = 12
&nbsp;   - (1, 3): nums[1] XOR nums[3] = 10
&nbsp;   - (1, 4): nums[1] XOR nums[4] = 9
&nbsp;   - (2, 3): nums[2] XOR nums[3] = 6
&nbsp;   - (2, 4): nums[2] XOR nums[4] = 5</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= low &lt;= high &lt;= 2 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Trie 0-1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đếm các cặp có XOR nằm trong $[low,high]$. Kiểm tra mọi cặp tốn $O(n^2)$, không thể chạy được với $n \le 2 \times 10^4$.
>
> Số cặp trong khoảng bằng số cặp có XOR nhỏ hơn $high+1$ trừ số cặp có XOR nhỏ hơn $low$. Ta chèn các số đã xử lý vào Trie $0$-$1$ từ bit cao xuống bit thấp và lưu kích thước của mỗi cây con. Khi truy vấn một giới hạn, bit $1$ cho phép cộng toàn bộ cây con có cùng bit của $x$, sau đó đi sang nhánh đối diện; bit $0$ bắt buộc đi sang nhánh có cùng bit. Với mỗi $x$, hãy truy vấn trước khi chèn để một số không ghép cặp với chính nó.

<!-- thinking:end -->

Với dạng bài đếm trong khoảng $[low, high]$, ta có thể chuyển thành đếm trong $[0, high]$ và $[0, low - 1]$, rồi lấy hiệu của hai kết quả để có đáp án.

Trong bài này, ta đếm số cặp có giá trị XOR nhỏ hơn $high+1$, sau đó đếm số cặp có giá trị XOR nhỏ hơn $low$. Hiệu của hai số đếm này chính là số cặp có giá trị XOR nằm trong khoảng $[low, high]$.

Ngoài ra, với các bài toán đếm XOR của mảng, ta thường có thể dùng "Trie 0-1" để giải.

Định nghĩa node của Trie như sau:

- `children[0]` và `children[1]` lần lượt biểu diễn node con trái và node con phải của node hiện tại;
- `cnt` biểu diễn số lượng số kết thúc tại node hiện tại.

Trong Trie, ta định nghĩa thêm hai hàm sau:

Một hàm là $insert(x)$, dùng để chèn số $x$ vào Trie. Hàm này chèn số $x$ vào "Trie 0-1" theo thứ tự các bit nhị phân từ cao xuống thấp. Nếu bit nhị phân hiện tại là $0$, nó được chèn vào node con trái; nếu không, nó được chèn vào node con phải. Sau đó, giá trị đếm $cnt$ của node được tăng thêm $1$.

Một hàm khác là $search(x, limit)$, dùng để tìm số lượng số trong Trie có giá trị XOR với $x$ nhỏ hơn $limit$. Hàm bắt đầu từ node gốc `node` của Trie, duyệt các bit của $x$ từ cao xuống thấp, trong đó bit hiện tại của $x$ được ký hiệu là $v$. Nếu bit hiện tại của $limit$ là $1$, ta có thể cộng trực tiếp giá trị đếm $cnt$ của node con có cùng bit $v$ với $x$ vào đáp án, sau đó chuyển node hiện tại sang node con có bit khác với $v$ của $x$, tức là `node = node.children[v ^ 1]`. Tiếp tục duyệt bit kế tiếp. Nếu bit hiện tại của $limit$ là $0$, ta chỉ có thể chuyển node hiện tại sang node con có cùng bit $v$ với $x$, tức là `node = node.children[v]`. Tiếp tục duyệt bit kế tiếp. Sau khi duyệt xong các bit của $x$, trả về đáp án.

Với hai hàm trên, ta có thể giải bài toán.

Ta duyệt mảng `nums`. Với mỗi số $x$, trước tiên ta tìm trong Trie số lượng số có XOR với $x$ nhỏ hơn $high+1$, sau đó tìm số lượng cặp có XOR với $x$ nhỏ hơn $low$, rồi cộng hiệu của hai số đếm vào đáp án. Tiếp theo, chèn $x$ vào Trie. Lặp lại với số $x$ tiếp theo cho đến khi duyệt hết mảng `nums`. Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$ và độ phức tạp không gian là $O(n \times \log M)$. Trong đó, $n$ là độ dài mảng `nums`, còn $M$ là giá trị lớn nhất trong mảng `nums`. Trong bài này, ta lấy trực tiếp $\log M = 16$.

<!-- tabs:start -->

#### Python3

```python
class Trie:
    def __init__(self):
        self.children = [None] * 2
        self.cnt = 0

    def insert(self, x):
        node = self
        for i in range(15, -1, -1):
            v = x >> i & 1
            if node.children[v] is None:
                node.children[v] = Trie()
            node = node.children[v]
            node.cnt += 1

    def search(self, x, limit):
        node = self
        ans = 0
        for i in range(15, -1, -1):
            if node is None:
                return ans
            v = x >> i & 1
            if limit >> i & 1:
                if node.children[v]:
                    ans += node.children[v].cnt
                node = node.children[v ^ 1]
            else:
                node = node.children[v]
        return ans


class Solution:
    def countPairs(self, nums: List[int], low: int, high: int) -> int:
        ans = 0
        tree = Trie()
        for x in nums:
            ans += tree.search(x, high + 1) - tree.search(x, low)
            tree.insert(x)
        return ans
```

#### Java

```java
class Trie {
    private Trie[] children = new Trie[2];
    private int cnt;

    public void insert(int x) {
        Trie node = this;
        for (int i = 15; i >= 0; --i) {
            int v = (x >> i) & 1;
            if (node.children[v] == null) {
                node.children[v] = new Trie();
            }
            node = node.children[v];
            ++node.cnt;
        }
    }

    public int search(int x, int limit) {
        Trie node = this;
        int ans = 0;
        for (int i = 15; i >= 0 && node != null; --i) {
            int v = (x >> i) & 1;
            if (((limit >> i) & 1) == 1) {
                if (node.children[v] != null) {
                    ans += node.children[v].cnt;
                }
                node = node.children[v ^ 1];
            } else {
                node = node.children[v];
            }
        }
        return ans;
    }
}

class Solution {
    public int countPairs(int[] nums, int low, int high) {
        Trie trie = new Trie();
        int ans = 0;
        for (int x : nums) {
            ans += trie.search(x, high + 1) - trie.search(x, low);
            trie.insert(x);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Trie {
public:
    Trie()
        : children(2)
        , cnt(0) {}

    void insert(int x) {
        Trie* node = this;
        for (int i = 15; ~i; --i) {
            int v = x >> i & 1;
            if (!node->children[v]) {
                node->children[v] = new Trie();
            }
            node = node->children[v];
            ++node->cnt;
        }
    }

    int search(int x, int limit) {
        Trie* node = this;
        int ans = 0;
        for (int i = 15; ~i && node; --i) {
            int v = x >> i & 1;
            if (limit >> i & 1) {
                if (node->children[v]) {
                    ans += node->children[v]->cnt;
                }
                node = node->children[v ^ 1];
            } else {
                node = node->children[v];
            }
        }
        return ans;
    }

private:
    vector<Trie*> children;
    int cnt;
};

class Solution {
public:
    int countPairs(vector<int>& nums, int low, int high) {
        Trie* tree = new Trie();
        int ans = 0;
        for (int& x : nums) {
            ans += tree->search(x, high + 1) - tree->search(x, low);
            tree->insert(x);
        }
        return ans;
    }
};
```

#### Go

```go
type Trie struct {
	children [2]*Trie
	cnt      int
}

func newTrie() *Trie {
	return &Trie{}
}

func (this *Trie) insert(x int) {
	node := this
	for i := 15; i >= 0; i-- {
		v := (x >> i) & 1
		if node.children[v] == nil {
			node.children[v] = newTrie()
		}
		node = node.children[v]
		node.cnt++
	}
}

func (this *Trie) search(x, limit int) (ans int) {
	node := this
	for i := 15; i >= 0 && node != nil; i-- {
		v := (x >> i) & 1
		if (limit >> i & 1) == 1 {
			if node.children[v] != nil {
				ans += node.children[v].cnt
			}
			node = node.children[v^1]
		} else {
			node = node.children[v]
		}
	}
	return
}

func countPairs(nums []int, low int, high int) (ans int) {
	tree := newTrie()
	for _, x := range nums {
		ans += tree.search(x, high+1) - tree.search(x, low)
		tree.insert(x)
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
