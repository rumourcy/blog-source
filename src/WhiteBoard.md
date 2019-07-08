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

#### 链表节点定义

```C++

struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
};
```

---

<!--more-->

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

---

#### [找第k大元素](https://leetcode.com/problems/kth-largest-element-in-an-array/)

```C++
class Comp {
public:
    bool operator()(int num1, int num2) {
        return num1 > num2;
    }
};

class Solution {
private:
    priority_queue<int, vector<int>, Comp> q;
public:
    int findKthLargest(vector<int>& nums, int k) {
        for (int num: nums) {
            q.push(num);
            if (q.size() > k)
                q.pop();
        }
        return q.top();
    }
};
```

#### [合并k个有序链表](https://leetcode.com/problems/merge-k-sorted-lists/)

```C++
class Compare {
public:
    bool operator()(const ListNode* l1, const ListNode* l2) {
        return l1->val > l2->val;
    }
};

class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode* res = new ListNode(0), *cur = res;
        priority_queue<ListNode*, vector<ListNode*>, Compare> q;
        if (lists.size() == 0) return res->next;
        
        for (ListNode* l: lists)
            if (l != NULL) q.push(l);
        
        while (!q.empty()) {
            cur->next = q.top();
            cur = cur->next;
            q.pop();
            if (cur->next != NULL) q.push(cur->next);
        }
        
        return res->next;
    }
};
```

---

#### [链表表示的两个数求和](https://leetcode.com/problems/add-two-numbers/)

```C++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int prev = 0, cur;
        ListNode *head = new ListNode(0), *tmp = head;
        while (l1 != NULL || l2 != NULL) {
            cur = (l1 != NULL) ? l1->val : 0;
            cur += (l2 != NULL) ? l2->val : 0;
            cur += prev;
            prev = cur / 10;
            cur = cur % 10;
            tmp->next = (l1 != NULL) ? l1 : l2;
            if (l1 != NULL) {
                l1->val = cur;
                l1 = l1->next;
            }
            if (l2 != NULL) {
                l2->val = cur;
                l2 = l2->next;
            }
            tmp = tmp->next;
        }
        tmp->next = NULL;
        if (prev != 0)
            tmp->next = new ListNode(prev);
        return head->next;
    }
};
```

#### [链表表示的两个数求和II](https://leetcode.com/problems/add-two-numbers-ii/)

```C++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<int> s1, s2;
        while (l1 != NULL) {
            s1.push(l1->val);
            l1 = l1->next;
        }
        while (l2 != NULL) {
            s2.push(l2->val);
            l2 = l2->next;
        }
        ListNode *head = new ListNode(0);
        int cur = 0;
        while (!s1.empty() || !s2.empty()) {
            if (!s1.empty()) {
                cur += s1.top();
                s1.pop();
            }
            if (!s2.empty()) {
                cur += s2.top();
                s2.pop();
            }
            head->val = cur % 10;
            cur = cur / 10;
            ListNode* tmp = new ListNode(cur);
            tmp->next = head;
            head = tmp;
        }
        return head->val != 0 ? head : head->next;
    }
};
```

#### [翻转链表](https://leetcode.com/problems/reverse-linked-list/)

```C++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode *prev = NULL, *cur = head;
        while (cur != NULL) {
            ListNode *tmp = cur->next;
            cur->next = prev;
            prev = cur;
            cur = tmp;
        }
        return prev;
    }
};

class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        return reverseList(head, NULL);
    }
    
    ListNode* reverseList(ListNode* cur, ListNode* prev) {
        if (cur == NULL) return prev;
        ListNode* tmp = cur->next;
        cur->next = prev;
        prev = cur;
        cur = tmp;
        return reverseList(cur, prev);
    }
};
```

#### [翻转指定位置的链表](https://leetcode.com/problems/reverse-linked-list-ii/)

```C++
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        ListNode *pHead = new ListNode(0), *p1 = pHead;
        for (int i = 1; i < m; ++i) {
            p1->next = head;
            head = head->next;
            p1 = p1->next;
        }
        ListNode *p2 = head, *prev = NULL, *cur = head;
        for (int i = 0; i <= n-m; ++i) {
            ListNode* tmp = cur->next;
            cur->next = prev;
            prev = cur;
            cur = tmp;
        }
        p2->next = cur;
        p1->next = prev;
        return pHead->next;
    }
};
```

#### [旋转链表](https://leetcode.com/problems/rotate-list/)

```C++
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (head == NULL) return NULL;
        ListNode *tail=head, *newHead;
        int len = 1;
        while (tail->next != NULL) {
            tail = tail->next;
            ++len;
        }
        tail->next = head;
        if ((k = k % len) != 0)
            for (int i = 0; i < len - k; ++i)
                tail = tail->next;
        newHead = tail->next;
        tail->next = NULL;
        return newHead;
    }
};
```
