---
comments: true
difficulty: Medium
tags:
    - Tree
    - Depth-First Search
    - Array
    - Hash Table
    - Binary Tree
---

<!-- problem:start -->

# [666. Path Sum IV 🔒](https://leetcode.com/problems/path-sum-iv)

[中文文档](/solution/0600-0699/0666.Path%20Sum%20IV/README.md)

## Mô tả

<!-- description:start -->

<p>Nếu độ sâu của cây nhỏ hơn <code>5</code>, ta có thể biểu diễn cây bằng một mảng các số nguyên có ba chữ số. Cho mảng <code>nums</code> được <strong>sắp xếp tăng dần </strong>gồm các số nguyên có ba chữ số biểu diễn một binary tree có độ sâu nhỏ hơn <code>5</code>. Với mỗi số nguyên:</p>

<ul>
	<li>Chữ số hàng trăm biểu thị độ sâu <code>d</code> của node, với <code>1 &lt;= d &lt;= 4</code>.</li>
	<li>Chữ số hàng chục biểu thị vị trí <code>p</code> của node trong level, với <code>1 &lt;= p &lt;= 8</code>, tương ứng với vị trí của node trong một <strong>full binary tree</strong>.</li>
	<li>Chữ số hàng đơn vị biểu thị giá trị <code>v</code> của node, với <code>0 &lt;= v &lt;= 9</code>.</li>
</ul>

<p>Trả về <strong>tổng</strong> của <strong>tất cả các đường đi</strong> từ <strong>root</strong> đến các <strong>leaf</strong>.</p>

<p>Đảm bảo mảng đã cho biểu diễn một binary tree liên thông hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0666.Path%20Sum%20IV/images/pathsum4-1-tree.jpg" style="width: 212px; height: 183px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [113,215,221]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cây được biểu diễn bởi danh sách được minh họa bên trên.<br />
Tổng các giá trị trên đường đi là (3 + 5) + (3 + 1) = 12.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0666.Path%20Sum%20IV/images/pathsum4-2-tree.jpg" style="width: 132px; height: 183px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [113,221]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cây được biểu diễn bởi danh sách được minh họa bên trên.&nbsp;<br />
Tổng các giá trị trên đường đi là (3 + 1) = 4.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 15</code></li>
	<li><code>110 &lt;= nums[i] &lt;= 489</code></li>
	<li><code>nums</code> biểu diễn một binary tree hợp lệ có độ sâu nhỏ hơn <code>5</code>.</li>
	<li><code>nums</code> được sắp xếp theo thứ tự tăng dần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các node được mã hóa bằng độ sâu, vị trí và giá trị. Không cần dựng cây tường minh.
>
> Ánh xạ $depth\times 10+pos$ sang giá trị node. Bắt đầu DFS từ $11$ theo công thức tính chỉ số node con; khi cả hai node con đều không tồn tại, cộng tổng đường đi vào kết quả.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pathSum(self, nums: List[int]) -> int:
        def dfs(node, t):
            if node not in mp:
                return
            t += mp[node]
            d, p = divmod(node, 10)
            l = (d + 1) * 10 + (p * 2) - 1
            r = l + 1
            nonlocal ans
            if l not in mp and r not in mp:
                ans += t
                return
            dfs(l, t)
            dfs(r, t)

        ans = 0
        mp = {num // 10: num % 10 for num in nums}
        dfs(11, 0)
        return ans
```

#### Java

```java
class Solution {
    private int ans;
    private Map<Integer, Integer> mp;

    public int pathSum(int[] nums) {
        ans = 0;
        mp = new HashMap<>(nums.length);
        for (int num : nums) {
            mp.put(num / 10, num % 10);
        }
        dfs(11, 0);
        return ans;
    }

    private void dfs(int node, int t) {
        if (!mp.containsKey(node)) {
            return;
        }
        t += mp.get(node);
        int d = node / 10, p = node % 10;
        int l = (d + 1) * 10 + (p * 2) - 1;
        int r = l + 1;
        if (!mp.containsKey(l) && !mp.containsKey(r)) {
            ans += t;
            return;
        }
        dfs(l, t);
        dfs(r, t);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int ans;
    unordered_map<int, int> mp;

    int pathSum(vector<int>& nums) {
        ans = 0;
        mp.clear();
        for (int num : nums) mp[num / 10] = num % 10;
        dfs(11, 0);
        return ans;
    }

    void dfs(int node, int t) {
        if (!mp.count(node)) return;
        t += mp[node];
        int d = node / 10, p = node % 10;
        int l = (d + 1) * 10 + (p * 2) - 1;
        int r = l + 1;
        if (!mp.count(l) && !mp.count(r)) {
            ans += t;
            return;
        }
        dfs(l, t);
        dfs(r, t);
    }
};
```

#### Go

```go
func pathSum(nums []int) int {
	ans := 0
	mp := make(map[int]int)
	for _, num := range nums {
		mp[num/10] = num % 10
	}
	var dfs func(node, t int)
	dfs = func(node, t int) {
		if v, ok := mp[node]; ok {
			t += v
			d, p := node/10, node%10
			l := (d+1)*10 + (p * 2) - 1
			r := l + 1
			if _, ok1 := mp[l]; !ok1 {
				if _, ok2 := mp[r]; !ok2 {
					ans += t
					return
				}
			}
			dfs(l, t)
			dfs(r, t)
		}
	}
	dfs(11, 0)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
