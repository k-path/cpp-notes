# Solving Tree Problems
## DFS
### Recursively
- It's usually better write the DFS using a helper function. The helper function makes it's recursive calls within that function. In the other function is where you call the helper function just once with the initial value arguments ("root" for example as an initial value argument).
- You can use global variables to keep track of values that need to be updated and processed again, or you can choose to define it as one of the parameters passed into your function (**recommended**).
  - It's usually frowned upon to use global variables because of thread safety reasons and among other reasons, it's just bad practice
  - In a problem where you need to track a value across all recursive calls that gets modified, it's necessary to pass by reference instead of by value (when you pass by value it creates a copy creates a copy of the argument being passed to the function)

    In this example, we keep track of a counter so the next recursive call needs access to the modified value
    ```cpp
    kthSmallestHelper(TreeNode* node, int k, int& counter) {
    if (!node) { 
            return -1; 
        }
        int kth_smallest = getKthSmallest(node->left, k, counter);
        if (kth_smallest != -1) { 
            return kth_smallest;
        }

        counter++;
        if (counter == k) { 
            return node->val;
        }

        kth_smallest = getKthSmallest(node->right, k, counter);
        if (kth_smallest != -1) {
            return kth_smallest;
        }

        return -1;
    }
    ```
  - In a problem where a parameter represents the current state for that specific recursive call, you'd want to pass by value.
    In this example, each path down the tree needs its own independent maximum value: Left path might have different maximums than right path, each recursive call represents a different path, and changes to path_max in one branch shouldn't affect other branches.
    ```cpp
    int goodNodesHelper(TreeNode* node, int path_max) {
        if (!node) {
            return 0;
        }
        int left_count = 0;
        int right_count = 0;
        int curr_node_count = 0;
        if (node->val >= path_max) {
            left_count = goodNodesHelper(node->left, node->val);
            right_count = goodNodesHelper(node->right, node->val);
            curr_node_count++;
        } else {
            left_count = goodNodesHelper(node->left, path_max);
            right_count = goodNodesHelper(node->right, path_max);
        }
    
        return left_count + right_count + curr_node_count;
    }
    ```
    - So, the key question to ask: "Does this value need to be shared and modified across all recursive calls, or is it specific to the current call?"
- Depending on what the problem is asking, you usually do some form of DFS tree traversal (preorder (DLR), inorder (LDR), or postorder (LRD)) to solve the problem.
  - Sometimes we may want process a node or do some operation on a node then do our left and right recurses, or left recurse first then process or operate on a node then recurse right, OR left recurse then right recurse then do some operation on the node.
  - For example, in Good Nodes question we use a preorder approach (we check the node before recursing left and right), in Kth Smallest question, we use a inorder approach (usually the way to go for BST because it gets a sorted traversal), we recurse left, increment counter, then recurse right.

#### Recursion in a Nutshell
- Anything that is called before the recursive statement in a recursive function happens right away.
  - This is often called the "winding" phase, or what happens "on the way down" the call stack. As each function is called, it executes these initial instructions before pausing to call the next function deeper in the chain. (This corresponds to pre-order operations).
- Anything that is called after the recursive statement only happens after the recursion hits a base case. But instead of happening in the order of that it was called by the recursion, it gets processed in reverse order. So the first thing in the path gets its call processed last if it is called after the recursive statement.
  - This is the "unwinding" phase, or what happens "on the way up" the call stack. Recall that in a **Last-In-First-Out (LIFO)** stack, things are processed in the reverse order that they came in. (So this corresponds to post-order operations).

### Iteratively
- It usually requires a stack. In my experience, it's usually more straightforward and simpler to just do it recursively

## BFS
### Iteratively (Queue)
- BFS is usually implemented using queue as it is much more straightforward than a recursive approach.
- The queue holds each level of the tree at a time. First holding the root, then use the root to push it's children and pop root, children pushes it's children and pops until the queue is empty.
- It may be good to keep track of the level size, which is the current size of the queue. With 1 always being the level size for the root, and 2 for it's children and so on.

