请实现两个函数，分别用来序列化和反序列化二叉树。

示例: 

你可以将以下二叉树：
```
    1
   / \
  2   3
     / \
    4   5
```
序列化为 `"[1,2,3,null,null,4,5]"`

<!--more-->

## 1. 按层序列化

我们结合着例子来讲解, 这样会更容易理解一些

``` 
        1
      /   \
     2      3
    /\     / \
   N  N   4   5
         / \  / \
        6  7 8   9
```

首先序列化是比较简单的, 使用一个队列即可

首先1入队.

1出队到list中, 1的左右子节点2, 3 入队,

2出队到list中, **然后因为2的子节点是null, 这时仍然要入队, 因为在反序列化的时候需要通过null占位来保证当前层数的节点数量为上一层非空节点数量乘2. 下面解释一下**

> 如果null添加到队列中, 序列化结束之后变成 [1,2,3,null,null,4,5,6,7,8,9]
>
> 我们先选取第0个节点1, 所以当前层的非null节点数量为1, 所以接下来的`1 * 2`个节点就为下一层的节点[2, 3]
>
> 选取第二层的节点[2, 3], **第二层的非null节点数量为2, 所以接下来的`2 * 2`个节点就为下一层的节点[null,null,4,5], 这里如果没有null的话就无法判断[2,3]的下一层节点到底是[4,5] 还是[4,5,6]还是[4,5,6,7]**
>
> 选取第三层的节点[null,null,4,5], 第三次的**非null节点数量**为2, 所以接下来的`2 * 2`个节点就为下一层的节点[6,7,8,9].
>
> 选取第四层的节点[[6,7,8,9]], 第四层的非null节点数量为4, 所以接下来的`4 * 2`个节点就为下一层的节点, 但是接下来没有更多的节点了, 所以只能是null了, 二叉树构造到此结束

然后3出队到list, 此时队列中该null出队了, null要出队到队列中, 原因如上已经解释了, 但是null出队之后就不用再往队列中添加null了.

最后直到队列为空, 结束

这样产生的list最后会有许多null, 即[1,2,3,null,null,4,5,6,7,8,9,null,null,null,null,null,null,null,null]

把尾部的null去掉即可[1,2,3,null,null,4,5,6,7,8,9]



其实上面也讲了反序列化的方法

> 我们先选取第0个节点1, 所以当前层的非null节点数量为1, 所以接下来的`1 * 2`个节点就为下一层的节点[2, 3]
>
> 选取第二层的节点[2, 3], **第二层的非null节点数量为2, 所以接下来的`2 * 2`个节点就为下一层的节点[null,null,4,5], 这里如果没有null的话就无法判断[2,3]的下一层节点到底是[4,5] 还是[4,5,6]还是[4,5,6,7]**
>
> 选取第三层的节点[null,null,4,5], 第三次的**非null节点数量**为2, 所以接下来的`2 * 2`个节点就为下一层的节点[6,7,8,9].
>
> 选取第四层的节点[[6,7,8,9]], 第四层的非null节点数量为4, 所以接下来的`4 * 2`个节点就为下一层的节点, 但是接下来没有更多的节点了, 所以只能是null了, 二叉树构造到此结束

注意每一次循环都要首先确定当前层的元素个数, 然后根据当前层的非null节点数确定下一层的元素个数. 然后再构造.

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {
    private static final TreeNode FAKENULL = new TreeNode(-2147483648);
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        List<Integer> ans = new ArrayList<>();
        if(root == null){
            return ans.toString();
        }
        Queue<TreeNode> q = new ArrayDeque<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode curr = q.poll();
            if(curr == FAKENULL){
                ans.add(null);
            }
            else{
                q.offer(curr.left == null ? FAKENULL : curr.left);
                q.offer(curr.right == null ? FAKENULL : curr.right);
                ans.add(curr.val);
            }
        }
        while(ans.get(ans.size() - 1) == null){
            ans.remove(ans.size() - 1);
        }
        
        return list2String(ans);
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        List<Integer> list = string2List(data);
        if(list.size() == 0){
            return null;
        }
        List<TreeNode> nodeList = new ArrayList<>(list.size());
        for(int i = 0; i < list.size(); ++i){
            if(list.get(i) != null){
                nodeList.add(new TreeNode(list.get(i)));
            }
            else{
                nodeList.add(null);
            }
        }

        int currentLayerSize = 1;
        int currentBegin = 0;
        int currentEnd = 1;
        while(currentBegin < list.size()){
            int nextLayerSize = 0;
            for(int i = currentBegin; i < currentEnd; ++i){
                if(list.get(i) != null){
                    nextLayerSize++;
                    int iLeftIndex = currentEnd + 2 * (nextLayerSize - 1);
                    int iRightIndex = currentEnd + 2 * (nextLayerSize - 1) + 1;
                    nodeList.get(i).left = (iLeftIndex < list.size()) ? nodeList.get(iLeftIndex) : null;
                    nodeList.get(i).right = (iRightIndex < list.size()) ? nodeList.get(iRightIndex) : null;
                }
            }
            currentBegin = currentEnd;
            currentEnd = Math.min(currentBegin + 2 * nextLayerSize, list.size());
        }
        return nodeList.get(0);
    }

    private static String list2String(List<Integer> list){
        StringBuilder sb = new StringBuilder();
        sb.append("[");
        for(int i = 0; i < list.size(); ++i){
            if(i != 0){
                sb.append(",");
            }
            sb.append(list.get(i));
        }
        sb.append("]");
        return sb.toString();
    }

    private static List<Integer> string2List(String listString){
        //remove "[" and "]"
        listString = listString.substring(1,listString.length() - 1);
        String[] strings = listString.split(",");

        List<Integer> list = new ArrayList<>();
        for(String str : strings){
            if("null".equals(str) || "".equals(str)){
                list.add(null);
            }
            else{
                list.add(Integer.valueOf(str));
            }
        }
        return list;
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

## 2. 按前序遍历序列化

剑指offer上使用的是前序遍历的序列化, 首先序列化根节点, 然后序列化左子树, 然后序列化右子树. 如果碰到null, 就把这个null写入进去, 并且停止递归.

例如,  设`sub(i)`为以i为根节点的子树的序列化, sub(null) 永远为null

```
      1
     / \
    2   3
   /   / \
  4   5   6
```

则会序列化为[1, sub(2), sub(3)]

而sub(2) = [2, sub(4), sub(null)]

sub(4) = [4, sub(null), sub(null)] = [4, null, null]

所以sub(2) = [2, 4, null, null, null]

sub(3) = [3, sub(5), sub(6)] = [3, 5, null, null, 6, null, null]

所以整个树序列化后就为[1, 2, 4, null, null, null, 3, 5, null, null, 6, null, null]



对于反序列化, 也可以按照这样的方式, 创建根节点, 反序列化左子树, 反序列化右子树, 把左右子树连接到根节点这4步操作来.

**问题是如何保证左子树反序列化完了呢?**

**这可以根据从流中读出的数据来判断, 如果是null, 那么这个子树就到这里就结束了, 接下来的值不可能作为null的子节点了, 停止递归.**



```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Codec {
    
    // Encodes a tree to a single string.
    public String serialize(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        serialize(root, list);
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < list.size(); ++i){
            if(i != 0){
                sb.append(",");
            }
            sb.append((list.get(i) == null) ? "null" : list.get(i));
        }
        return sb.toString();
    }
    private void serialize(TreeNode root, List<Integer> list){
        if(root == null){
            list.add(null);
        }
        else{
            list.add(root.val);
            serialize(root.left, list);
            serialize(root.right, list);
        }
    }

    // Decodes your encoded data to tree.
    public TreeNode deserialize(String data) {
        // transfer string to input stream, use a queue to simulate the stream
        Queue<String> q = new ArrayDeque<>(Arrays.asList(data.split(",")));

        return deserialize(q);
        
    }
    private TreeNode deserialize(Queue<String> q){
        String curr = q.poll();
        if("null".equals(curr)){
            return null;
        }
        else{
            TreeNode ans = new TreeNode(Integer.valueOf(curr));
            ans.left = deserialize(q);
            ans.right = deserialize(q);
            return ans;
        }
    }
}

// Your Codec object will be instantiated and called as such:
// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```

