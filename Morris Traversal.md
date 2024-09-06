一種不用額外遞迴空間的遍歷法，使用了引線二元樹的概念

```cpp

void morris(TreeNode *root){
	if (!root)return;
        TreeNode *cur=root,*pre,*parent=nullptr;
        while(cur){
            if(!cur->left){
                parent=cur;
                cur=cur->right;
            }else{
                pre=cur->left;
                while(pre->right && pre->right!=cur)pre=pre->right;
                if(!pre->right){
                    pre->right=cur;
                    cur=cur->left;
                }else{
                    pre->right=NULL;
                    parent=cur;
                    cur=cur->right;
                }
            }
        }
}


```