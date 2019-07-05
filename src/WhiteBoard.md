---
title: WhiteBoard
date: 2019-07-05 20:42:20
tags:
	- ZERO
---

#### 树节点定义

```C++
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
```

#### 二叉树遍历迭代写法

- [先序](https://leetcode.com/problems/binary-tree-preorder-traversal/)
- [中序](https://leetcode.com/problems/binary-tree-inorder-traversal/)
- [后序](https://leetcode.com/problems/binary-tree-postorder-traversal/)

```C++
class Solution {
private:
    stack<TreeNode*> s;
    vector<int> res;
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (root == NULL) return res;
        s.push(root);
        while (!s.empty()) {
            TreeNode *tmp = s.top();
            s.pop();
            res.push_back(tmp->val);
            if (tmp->right != NULL) s.push(tmp->right);
            if (tmp->left != NULL) s.push(tmp->left);
        }
        return res;
    }
};

class Solution {
private:
    stack<TreeNode*> s;
    vector<int> res;
public:
    vector<int> inorderTraversal(TreeNode* root) {
        while (!s.empty() || root != NULL) {
            while (root != NULL) {
                s.push(root);
                root = root->left;
            }
            TreeNode* tmp = s.top();
            s.pop();
            res.push_back(tmp->val);
            root = tmp->right;
        }
        return res;
    }
};

class Solution {
private:
    stack<TreeNode*> s;
    vector<int> res;
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if (root == NULL) return res;
        s.push(root);
        while (!s.empty()) {
            TreeNode* tmp = s.top();
            s.pop();
            res.insert(res.begin(), tmp->val);
            if (tmp->left != NULL) s.push(tmp->left);
            if (tmp->right != NULL) s.push(tmp->right);
        }
        return res;
    }
};
```

#### [验证有效二叉搜索树（中序遍历有序）](https://leetcode.com/problems/validate-binary-search-tree/)

```C++
class Solution {
private:
    long long val = LLONG_MIN;
public:
    bool isValidBST(TreeNode* root) {
        bool res = true;
        if (root == NULL) return true;
        res = res && isValidBST(root->left);
        if (root->val <= val) return false;
        val = root->val;
        res = res && isValidBST(root->right);
        return res;
    }
};
```

#### [删除二叉搜索树指定节点](https://leetcode.com/problems/delete-node-in-a-bst/)

```C++
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == NULL) return NULL;
        if (root->val == key) {
            if (root->left == NULL || root->right == NULL)
                root = root->left == NULL ? root->right : root->left;
            else {
                TreeNode* tmp1 = root->left, *tmp2;
                while (tmp1->right != NULL) {
                    tmp2 = tmp1->right;
                    if (tmp2->right == NULL) {
                        tmp1->right = tmp2->left;
                        tmp1 = tmp2;
                    }
                    tmp1 = tmp2;
                }
                if (root->left->right != NULL) tmp1->left = root->left;
                tmp1->right = root->right;
                root = tmp1;
            }
        } else if (root->val > key)
            root->left = deleteNode(root->left, key);
        else
            root->right = deleteNode(root->right, key);
        return root;
    }
};
```

#### [翻转二叉树](https://leetcode.com/problems/invert-binary-tree/)

```C++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) return root;
        TreeNode* tmp = root->left;
        root->left = root->right;
        root->right = tmp;
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }
};
```

#### [是否为平衡二叉树](https://leetcode.com/problems/balanced-binary-tree/)

```C++
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if (root == NULL) return true;
        int diff = abs(getDepth(root->left) - getDepth(root->right));
        return diff > 1 ? false : (isBalanced(root->left) && isBalanced(root->right));
    }
    
    int getDepth(TreeNode* root) {
        if (root == NULL) return 0;
        return 1 + max(getDepth(root->left), getDepth(root->right));
    }
};
```

---

#### [最大子数组和](https://leetcode.com/problems/maximum-subarray/)

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        int maxSum = nums[0], prev = nums[0];
        for (int i = 1; i < nums.size(); ++i) {
            prev = max(prev+nums[i], nums[i]);
            maxSum = max(maxSum, prev);
        }
        return maxSum;
    }
};
```

