https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/?envType=study-plan-v2&envId=top-interview-150

### 목표
==**Preorder에서 현재 인덱스에 딱 맞는 자식 인덱스를 찾아내기** ==
- pre[0]은 Root
- preroder는 root -> left -> right 순서로 가기 때문에 preorder에서 왼쪽 노드를 모두 스킵하면 바로 오른쪽 Node를 알아낼 수 있음 
- inorder 순회의 경우엔 root를 찾아내면 그 전은 모두 왼쪽, 그 이후는 모두 오른쪽이라는 것을 알 수 있음 
- 현재 노드의 right 자식 노드의 인덱스는 preStart + numsOnLeft + 1 이 된다. 
- numsOnLeft = root - inStart 이다. 

**코드 구현** 
```swift
class Solution {
    func buildTree(_ preorder: [Int], _ inorder: [Int]) -> TreeNode? {
        build(0, 0, inorder.count-1, preorder,inorder)
    }

    private func build(_ preStart: Int, _ inStart: Int, _ inEnd: Int, _ preorder: [Int], _ inorder: [Int]) -> TreeNode? {
        if preStart > preorder.count - 1 || inStart > inEnd { return nil }
        let root = TreeNode(preorder[preStart])
        var inIndex = 0
        for i in inStart...inEnd {
            if inorder[i] == root.val {
                inIndex = i
                break
            }
        }
        root.left = build(preStart+1, inStart, inIndex - 1, preorder, inorder)
        root.right = build(preStart + inIndex - inStart + 1, inIndex + 1, inEnd, preorder, inorder)
        return root
    }
}

```