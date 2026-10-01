---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [04.09. BST Sequences](https://leetcode.cn/problems/bst-sequences-lcci)

[Phiên bản tiếng Trung](/lcci/04.09.BST%20Sequences/README.md)

## Mô tả

<!-- description:start -->

<p>Một binary search tree được tạo bằng cách duyệt một mảng từ trái sang phải và chèn từng phần tử. Cho một binary search tree có các phần tử phân biệt, hãy in ra tất cả các mảng có thể đã tạo ra cây này.</p>
<p><strong>Ví dụ:</strong><br />
Cho cây sau:</p>
<pre>

        2

       / \

      1   3

</pre>
<p>Đầu ra:</p>
<pre>

[

[2,1,3],

[2,3,1]

]

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đan xen các subsequence bằng đệ quy

<!-- thinking:start -->

> **Tư duy**
>
> Liệt kê các hoán vị của các giá trị node rồi mô phỏng thao tác chèn sẽ phải kiểm tra $n!$ thứ tự, không thể mở rộng khi kích thước tăng. Root phải được chèn đầu tiên, nên mọi mảng hợp lệ đều bắt đầu bằng root. Các sequence của cây con trái và phải độc lập với nhau, ngoại trừ việc mỗi sequence phải giữ nguyên thứ tự tương đối của chính nó. Ta đệ quy để lấy mọi sequence hợp lệ của từng cây con, sau đó đan xen chúng phía sau prefix chứa root: ở mỗi bước, lấy giá trị chưa dùng tiếp theo từ một phía. Cây rỗng tạo ra một sequence rỗng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Swift

```swift
/* class TreeNode {
*    var val: Int
*    var left: TreeNode?
*    var right: TreeNode?
*
*    init(_ val: Int, _ left: TreeNode? = nil, _ right: TreeNode? = nil) {
*        self.val = val
*        self.left = left
*        self.right = right
*    }
* }
*/

class Solution {
    func BSTSequences(_ root: TreeNode?) -> [[Int]] {
        guard let root = root else { return [[]] }

        var result = [[Int]]()
        let prefix = [root.val]
        let leftSeq = BSTSequences(root.left)
        let rightSeq = BSTSequences(root.right)

        for left in leftSeq {
            for right in rightSeq {
                var weaved = [[Int]]()
                weaveLists(left, right, &weaved, prefix)
                result.append(contentsOf: weaved)
            }
        }
        return result
    }

    private func weaveLists(_ first: [Int], _ second: [Int], _ results: inout [[Int]], _ prefix: [Int]) {
        if first.isEmpty || second.isEmpty {
            var result = prefix
            result.append(contentsOf: first)
            result.append(contentsOf: second)
            results.append(result)
            return
        }

        var prefixWithFirst = prefix
        prefixWithFirst.append(first.first!)
        weaveLists(Array(first.dropFirst()), second, &results, prefixWithFirst)

        var prefixWithSecond = prefix
        prefixWithSecond.append(second.first!)
        weaveLists(first, Array(second.dropFirst()), &results, prefixWithSecond)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
