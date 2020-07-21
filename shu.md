# 树

## 知识点

### 定义

```java
public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}
```

### 二叉树遍历

#### 遍历方式

深度优先：

前序（Pre-order）：根-左-右

中序（In-order）：左-根-右

后序（Post-order）：左-右-根

注意：  
以根访问顺序决定是什么遍历  
左子树总是优先右子树

广度优先：层序遍历

#### 代码模板

递归

前序

```java
    private void preorder(TreeNode node) {
        // terminate condition
        if (node == null) {
            return;
        }

        // process current node
        res.add(node.val);

        // drill down
        preorder(node.left);
        preorder(node.right);
    }
```

中序

```java
    private void inorder(TreeNode node) {
        // terminate condition
        if (node == null) {
            return;
        }

        // drill down left
        inorder(node.left);

        // process current node
        res.add(node.val);

        // drill down right
        inorder(node.right);
    }
```

后序

```java
    private void postorder(TreeNode node) {
        // terminate condition
        if (node == null) {
            return;
        }

        // drill down left
        postorder(node.left);

        // drill down right
        postorder(node.right);

        // process current node
        res.add(node.val);
    }
```

非递归

前序

思路：栈

```java
    private void preoder(TreeNode root) {
        // create a stack
        Deque<TreeNode> stack = new LinkedList<>();

        // push root to stack
        stack.addFirst(root);

        while (!stack.isEmpty()) {
            // pop node from stack
            TreeNode node = stack.removeFirst();

            // skip condition
            if (node == null) {
                continue;
            }

            // process node
            res.add(node.val);

            // push children to stack
            stack.addFirst(node.right); // !!! right first
            stack.addFirst(node.left);
        }
    }
```

中序

思路：栈+指针

当前节点不为null时压入节点，并且指向其左子节点

当前节点为null时，从栈中弹出节点，**处理此节点**，然后指向其右节点

```java
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();

        Deque<TreeNode> stack = new LinkedList<>();
        TreeNode curr = root;

        while (!stack.isEmpty() || curr != null) {
            while (curr != null) {
                stack.addFirst(curr);
                curr = curr.left;
            }
            
            curr = stack.removeFirst();

            // process node
            res.add(curr.val);

            curr = curr.right;
        }
        
        return res;
    }
```

后序

思路：两个栈（前序+翻转\)

```java
    private void postorder(TreeNode root) {
        // create two stack
        Deque<TreeNode> stack = new LinkedList<>();
        Deque<TreeNode> anotherStack = new LinkedList<>();

        // push root to stack
        stack.addFirst(root);

        while (!stack.isEmpty()) {
            // pop node from stack
            TreeNode node = stack.removeFirst();

            // skip condition
            if (node == null) {
                continue;
            }

            // !!! push node to another stack
            anotherStack.addFirst(node);

            // push children to stack
            stack.addFirst(node.left); // !!! left first
            stack.addFirst(node.right);
        }

        while (!anotherStack.isEmpty()) {
            // pop
            TreeNode node = anotherStack.removeFirst();

            // process
            res.add(node.val);
        }
    }
```

层序

思路：队列

当需要单独返回每一层结果时，在入队和出队时，需要增加一个层数信息

```java
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> res=new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<Node> queue = new LinkedList<>();
        Queue<Integer> levelQue = new LinkedList<>();
        queue.offer(root);
        levelQue.offer(0);
        while (!queue.isEmpty()) {
            Node node = queue.poll();
            int level = levelQue.poll();
            if (res.size() == level) {
                res.add(new ArrayList<>());
            }
            res.get(level).add(node.val);

            for (Node child : node.children) {
                queue.offer(child);
                levelQue.offer(level + 1);
            }
        }
        return res;
    }
```

### 二叉搜索树

1. 左子树上所有结点的值均小于它的根结点的值；
2. 右子树上所有结点的值均大于它的根结点的值；
3. 以此类推：左、右子树也分别为二叉查找树。 

中序遍历：升序排列

