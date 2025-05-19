# Leetcode

## Arrays

### Two Sum

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) 
    {
        for (int i = 0; i< nums.size(); i++)
        {
            for (int j = i+1; j< nums.size(); j++)
            {
                if((nums[i] + nums[j]) == target)
                {
                    return std::vector<int>{i,j};
                }
            }
        }
        return std::vector<int>{};
    }
};
```

## Two Pointers

```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        int l = 0;
        int r = s.size() - 1;
        while (l < r) {
            while (l < r && !isalnum(s[l])) l++;
            while (l < r && !isalnum(s[r])) r--;
            if (tolower(s[l]) != tolower(s[r])) {
                return false;
            }
            l++;
            r--;
        }
        return true;
    }
};
```

## Binary Search

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() -1;
        int mid;

        while ( left <= right)
        {
            mid = (right+left)/2;

            if (nums[mid] == target)
            {
                return mid;
            }
            
            if (nums[mid] < target)
            {
                left = mid+1;
            }
            else
            {
                right = mid-1;
            }
        }
        return -1;
    }
};
```

## Stack

### Valid Parentheses
```cpp
class Solution {
public:
    bool isValid(string s) {
        std::stack<char> stack;

        std::unordered_map<char, char> parens = 
        {
            {'(', ')'},
            {'{', '}'},
            {'[', ']'}
        };


        for(auto c : s)
        {
  
            if(parens.find(c) != parens.end())
            {
                stack.push(c);
            }
            else
            {
                if (stack.empty() || parens.at(stack.top()) != c )
                {
                    return false;
                }
                stack.pop();
            }

        }
        return stack.empty();
        
    }
};
```

### Generate Parentheses

```cpp
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> result;
        string subset;
        int closed =0;
        int open =0;
        dfs(result, subset, n, closed, open);
        return result;
    }
private:
    void dfs(vector<string>& result, string& subset, int n, int closed, int open )
    {
        if(closed == n && open ==n)
        {
            result.push_back(subset);
            return;
        }
        if(open <n )
        {
            subset+="(";
            dfs(result, subset, n, closed, open+1);
            subset.pop_back();
        }

        if(closed < open)
        {
            subset+=")";
            dfs(result, subset, n, closed +1, open);
            subset.pop_back();

        }


    }
};
```

## Linked List

### Reverse Linked List
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */

class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode * next = nullptr;
        ListNode * cur = head;
        ListNode * prev = nullptr;

        while (cur != nullptr)
        {
            next = cur->next;
            cur->next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
        
    }
};
```

## Trees

### Tree DFS
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void inorderTraversalHelper(TreeNode* root, vector<int>& ret)
    {
        if (root == nullptr)
        {
            return;
        }
        inorderTraversalHelper(root->left, ret);
        ret.push_back(root->val);
        inorderTraversalHelper(root->right, ret);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> ret;
        inorderTraversalHelper(root, ret);
        return ret;
        
    }
};
```

## Backtracking



### Subsets

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) 
    {
        vector<vector<int>> res;
        vector<int> subset;
        dfs(nums, subset, res,0);
        return res;
        
    }

    void dfs (vector<int>& nums, vector<int>& subset, vector<vector<int>>& res, int i)
    {
        if(i >= nums.size())
        {
            res.push_back(subset);
            return;
        }

        subset.push_back(nums[i]);
        dfs(nums, subset, res, i+1);
        subset.pop_back();
        dfs(nums, subset, res, i+1);
    }
};

```

### Permutations
```cpp
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) 
    {
        if ( nums.size() == 0)
        {
            return {{}};
        }

        vector<int> tmp = vector<int>(nums.begin()+1, nums.end());
        vector<vector<int>> res;
        vector<vector<int>> perms = permute(tmp);

        for (auto perm : perms)
        {
            for (int i = 0 ; i <= perm.size() ; i++)
            {
                vector<int> perm_copy  = perm;
                perm_copy.insert(perm_copy.begin() + i , nums[0]);
                res.push_back(perm_copy);

            }
        }

        return res;
    }
};


```

## Graphs

### Matrix DFS

```cpp
    vector<pair<int,int>> directions = {{-1,0}, {1,0}, {0,-1},{0,1}};
    void dfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int r , int c)
    {
        int cols  = grid[0].size();
        int rows = grid.size();

        visited[r][c] = true;

        cout << "Visiting:" << r << "," << c  << " = "  << grid[r][c] <<  std::endl;
        
        for (auto& dir : directions)
        {
            int newr  = r + dir.first;
            int newc  = c+ dir.second;

            if(
                newr>= 0  
                && newc >= 0 
                && newr < rows 
                &&newc < cols 
                && !visited[newr][newc]
                && grid[newr][newc] == '1'
            )
            {
                dfs(grid, visited, newr, newc);
            }
        }
    }

```

### Matrix BFS - Shortest Path

```cpp
{
       int rows = grid.size();
       int cols = grid[0].size();
       vector<pair<int, int>> dirs = {{1,0}, {-1,0},{0, 1},{0,-1},{1,1},{-1,-1},{1,-1},{-1,1}};
       queue<pair<int,int>> q;
       set<pair<int,int>> visited;

       if (grid[0][0] !=0 || grid[rows-1][cols-1] != 0)
       {
        return -1;
       }

       q.push({0,0});
       visited.insert({0,0});
       int length  =1;

       while (!q.empty())
       {
            int size = q.size();
            for(int i = 0 ; i < size; i++ )
            {
                int r = q.front().first;
                int c = q.front().second;
                q.pop();
                if( r == rows-1 && c == cols -1)
                {
                    return length;
                }

                for(auto dir: dirs)
                {
                    auto new_r = r+ dir.first;
                    auto new_c = c + dir.second;
                    auto new_p = pair<int, int>{new_r, new_c};

                    if(
                        new_r >= 0
                        && new_c >= 0 
                        && new_r < rows
                        && new_c < cols
                        && grid[new_r][new_c] == 0
                        && (visited.find(new_p) == visited.end())
                    )
                    {
                        q.push(new_p);
                        visited.insert(new_p);
                    }
                }
            }
            length++;
       }
       return -1;
    }

```

## Dynamic Programming

### Climbing Stairs
```cpp
class Solution {
public:
    int climbStairs(int n) {
        if(n<=2)
         return n;

        if(n>2)
            return climbStairs(n-1) + climbStairs(n-2);
        
    }
};

```