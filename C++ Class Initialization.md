太常忘記了，做個筆記

以常用的LeetCode TreeNode當例子
```cpp

class TreeNode{

    public:
        int val;
        TreeNode* left;
        TreeNode* right;
        TreeNode():val(0),left(nullptr),right(nullptr) {}
        TreeNode(int x):val(x),left(nullptr),right(nullptr){}
        TreeNode(int x ,TreeNode* lptr,TreeNode* rptr):val(x),left(lptr),right(rptr){}

};

```