---
title: WhiteBoard
date: 2019-07-05 20:42:20
tags:
	- ZERO
	- Algorithm
---
#### 数据结构定义

##### 树节点定义

```C++
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
```

##### 链表节点定义

```C++

struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
};
```

<!--more-->

#### 树

##### 二叉树遍历迭代写法

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

##### [验证有效二叉搜索树（中序遍历有序）](https://leetcode.com/problems/validate-binary-search-tree/)

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

##### [删除二叉搜索树指定节点](https://leetcode.com/problems/delete-node-in-a-bst/)

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

##### [翻转二叉树](https://leetcode.com/problems/invert-binary-tree/)

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

##### [是否为平衡二叉树](https://leetcode.com/problems/balanced-binary-tree/)

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

#### 动态规划

##### [最大子数组和](https://leetcode.com/problems/maximum-subarray/)

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

##### [最长递增子序列](https://leetcode.com/problems/longest-increasing-subsequence/)

```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        vector<int> dp(nums.size(), 1);
        int mmax = 1;
        for (int i = 1; i < nums.size(); ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[i] > nums[j])
                    dp[i] = max(dp[i], dp[j]+1);
            }
            mmax = max(dp[i], mmax);
        }
        return mmax;
    }
};
```

##### [最长回文子串](https://leetcode.com/problems/longest-palindromic-substring/)

```C++
class Solution {
public:
    string longestPalindrome(string s) {
        int start = 0, end = 0, len=s.size();
        vector<vector<bool>> dp(len, vector<bool>(len, false));
        for (int i = len-1; i >= 0; --i) {
            for (int j = i; j < len; ++j) {
                if (s[i] == s[j] && (j-i<3 || dp[i+1][j-1])) {
                    dp[i][j] = true;
                    if (j - i > end - start) {
                        start = i;
                        end = j;
                    }
                }
            }
        }
        return s.substr(start, end-start+1);
    }
};
```

##### [最长公共子串](https://leetcode.com/problems/longest-common-subsequence/)

```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.size()+1, vector<int>(text2.size()+1, 0));
        for (int i = 1; i <= text1.size(); ++i) {
            for (int j = 1; j <= text2.size(); ++j) {
                if (text1[i-1] == text2[j-1]) dp[i][j] = dp[i-1][j-1]+1;
                else dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
            }
        }
        return dp[text1.size()][text2.size()];
    }
};
```

##### [最长回文子序列](https://leetcode.com/problems/longest-palindromic-subsequence/)

```C++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        vector<vector<int>> dp(s.size(), vector<int>(s.size(), 0));
        for (int i = s.size()-1; i >= 0; --i) {
            dp[i][i] = 1;
            for (int j = i+1; j < s.size(); ++j) {
                if (s[i] == s[j]) dp[i][j] = dp[i+1][j-1] + 2;
                else dp[i][j] = max(dp[i+1][j], dp[i][j-1]);
            }
        }
        return dp[0][s.size()-1];
    }
};
```



#### 排序

##### [找第k大元素](https://leetcode.com/problems/kth-largest-element-in-an-array/)

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

##### [中位数](https://www.lintcode.com/problem/median/description)

```C++
class Solution {
public:
    int median(vector<int> &nums) {
        if (nums.size() == 0) return 0;
        int lo = 0, hi = nums.size() - 1, mid = (lo + hi) / 2;
        while (lo < hi) {
            int p = partition(nums, lo, hi);
            if (p == mid) return nums[mid];
            else if (p > mid) hi = p - 1;
            else lo = p + 1;
        }
        return nums[mid];
    }
    
    int partition(vector<int> &nums, int lo, int hi) {
        int s = lo+1, e = hi;
        while (true) {
            while (s <= hi) if (nums[s] <= nums[lo]) ++s; else break;
            while (e >= lo) if (nums[e] > nums[lo]) --e; else break;
            if (s >= e) break;
            swap(nums, s, e);
        }
        swap(nums, lo, e);
        return e;
    }
    
    void swap(vector<int> &nums, int m, int n) {
        int tmp = nums[m];
        nums[m] = nums[n];
        nums[n] = tmp;
    }
};
```

##### 快速排序

```C++
class Solution {
public:
    void sort(vector<int> &nums) {
        if (nums.size() == 0) return;
        int lo = 0, hi = nums.size() - 1;
        sort(nums, lo, hi);
    }
    
    void sort(vector<int> &nums, int lo, int hi) {
        if (lo >= hi) return;
        int pos = partition(nums, lo, hi);
        sort(nums, lo, pos-1);
        sort(nums, pos+1, hi);
    }
    
    int partition(vector<int> &nums, int lo, int hi) {
        int s = lo+1, e = hi;
        while (true) {
            while (s <= hi) if (nums[s] <= nums[lo]) ++s; else break;
            while (e >= lo) if (nums[e] > nums[lo]) --e; else break;
            if (s >= e) break;
            swap(nums, s, e);
        }
        swap(nums, lo, e);
        return e;
    }
    
    void swap(vector<int> &nums, int m, int n) {
        int tmp = nums[m];
        nums[m] = nums[n];
        nums[n] = tmp;
    }
};
```

#### 链表

##### [合并k个有序链表](https://leetcode.com/problems/merge-k-sorted-lists/)

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

##### [链表表示的两个数求和](https://leetcode.com/problems/add-two-numbers/)

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

##### [链表表示的两个数求和II](https://leetcode.com/problems/add-two-numbers-ii/)

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

##### [翻转链表](https://leetcode.com/problems/reverse-linked-list/)

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

##### [翻转指定位置的链表](https://leetcode.com/problems/reverse-linked-list-ii/)

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

##### [旋转链表](https://leetcode.com/problems/rotate-list/)

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

##### [删除倒数第N个节点](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

```C++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *newHead = new ListNode(0), *prev = newHead;
        ListNode *p1 = head, *p2 = head;
        for (int i = 0; i < n; ++i)
            p2 = p2->next;
        while (p2 != NULL) {
            prev->next = p1;
            p1 = p1->next;
            p2 = p2->next;
            prev = prev->next;
        }
        prev->next = p1->next;
        return newHead->next;
    }
};
```
#### 排列组合

##### k-子数组

```C++
#include <vector>
#include <algorithm>
#include <iostream>
using namespace std;

void combination(vector<int> &arr, vector<vector<int>> &res, vector<int> &path, int k, int pos) {
  if (path.size() == k) {
    vector<int> tmp(path.begin(), path.end());
    res.push_back(tmp);
    return;
  }
  int prev = arr[0] - 1;
  for (int i = pos; i <= arr.size() - (k - path.size()); ++i) {
    if (arr[i] != prev) {
      path.push_back(arr[i]);
      prev = arr[i];
      combination(arr, res, path, k, i + 1);
      path.pop_back();
    }
  }
}

vector<vector<int>> combination(vector<int> arr, int k) {
  vector<vector<int>> res;
  vector<int> path;
  if (arr.size() < k)
    return res;
  sort(arr.begin(), arr.end());
  combination(arr, res, path, k, 0);
  return res;
}

int main() {
  int arrs[] = {1, 1, 2, 3, 5, 6};
  vector<int> arr(arrs, arrs + sizeof(arrs) / sizeof(int));
  vector<vector<int>> res = combination(arr, 3);

  for (vector<int> path : res) {
    for (int num : path)
      cout << num << " ";
    cout << endl;
  }
  return 0;
}
```

##### [有重复全排列](https://leetcode.com/problems/permutations-ii/)

```C++
class Solution {
private:
    vector<vector<int>> res;
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        permuteUnique(nums, 0);
        return res;
    }
    
    void permuteUnique(vector<int>& nums, int pos) {
        if (pos == nums.size()) {
            vector<int> tmp(nums.begin(), nums.end());
            res.push_back(tmp);
            return;
        }
        set<int> flag;
        for (int i = pos; i < nums.size(); ++i) {
            if (flag.find(nums[i]) == flag.end()) {
                flag.insert(nums[i]);
                swap(nums, pos, i);
                permuteUnique(nums, pos+1);
                swap(nums, pos, i);
            }
        }
    }
    
    void swap(vector<int>& nums, int m, int n) {
        int tmp = nums[m];
        nums[m] = nums[n];
        nums[n] = tmp;
    }
};
```

#### 字符串

##### [atoi](https://leetcode.com/problems/string-to-integer-atoi/)

```C++
class Solution {
public:
    int myAtoi(string str) {
        long long res = 0;
        int sign = 1, i = 0;
        for (; i < str.size(); ++i)
            if (str[i] != ' ')
                break;
        if (i < str.size() && (str[i] == '+' || str[i] == '-')) {
            if (str[i] == '-') sign = -1;
            ++i;
        }
        for (; i < str.size(); ++i) {
            if (str[i] > '9' || str[i] < '0')
                break;
            res = res * 10 + (str[i] - '0');
            if (res * sign >= INT_MAX) return INT_MAX;
            if (res * sign <= INT_MIN) return INT_MIN;
        }
        return res * sign;
    }
};
```

#### 其他

##### [稀疏矩阵乘法](https://www.lintcode.com/problem/sparse-matrix-multiplication/description)

```C++
class Solution {
public:
    vector<vector<int>> multiply(vector<vector<int>> &A, vector<vector<int>> &B) {
        int a = A.size(), b = A[0].size(), c = B[0].size();
        vector<vector<int>> res(a, vector<int>(c, 0));
        for (int i = 0; i < a; ++i) {
            for (int j = 0; j < c; ++j) {
                for (int k = 0; k < b; ++k) {
                    if (A[i][k] == 0 || B[k][j] == 0) continue;
                    res[i][j] += A[i][k] * B[k][j];
                }
            }
        }
        return res;
    }
};
```

##### 最长连续线段长

> 在x轴上给一些线段，求出这些线段组成的最长连续线段长
>
> 比如： 
> 1 4 
> 2 3 
> 3 6 
> 7 11 
> 12 15
>
> 则答案是：5（由1 4和3 6组成的1 6线段，长为5）

```C++
#include <bits/stdc++.h>
using namespace std;

bool compare(pair<int, int> p1, pair<int, int> p2) {
    return p1.first < p2.first;
}

int main() {
    vector<pair<int, int>> lines;
    string line;
    while (getline(cin, line)) {
        istringstream iss(line);
        string s, e;
        iss >> s >> e;
        lines.emplace_back(stoi(s), stoi(e));
    }
    sort(lines.begin(), lines.end(), compare);
    int start = lines[0].first, end = lines[0].second, res = 0, cur = end - start;
    for (size_t i = 1; i < lines.size(); ++i) {
        if (lines[i].first <= end) {
            if (lines[i].second - end > 0) {
                cur += (lines[i].second - end);
                end = lines[i].second;
            }
        } else {
            res = max(res, cur);
            start = lines[i].first, end = lines[i].second;
            cur = end - start;
        }
    }
    res = max(res, cur);
    cout << res << endl;
    return 0;
}
```

##### [最长连续序列](https://leetcode.com/problems/longest-consecutive-sequence/)

```C++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int res = 0;
        unordered_map<int, int> map;
        for (auto num: nums) {
            if (map.find(num) == map.end()) {
                int left = (map.find(num-1) != map.end()) ? map[num-1] : 0;
                int right = (map.find(num+1) != map.end()) ? map[num+1] : 0;
                int cur = left + right + 1;
                res = max(cur, res);
                map[num] = cur;
                map[num-left] = cur;
                map[num+right] = cur;
            }
        }
        return res;
    }
};
```

##### [最长连续递增子序列](https://leetcode.com/problems/longest-continuous-increasing-subsequence/)

```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int res = 0, cur = 0, prev = INT_MIN;
        for (auto num: nums) {
            if (num > prev) {
                res = max(res, ++cur);
            } else {
                cur = 1;
            }
            prev = num;
        }
        return res;
    }
};
```

