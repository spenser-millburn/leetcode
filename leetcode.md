# Leetcode

## Arrays

### Two Sum

```cpp
class Solution { // Define a Solution class
public: // Public access specifier for class members
    vector<int> twoSum(vector<int>& nums, int target)  // Function declaration that takes a vector of integers and a target integer, returns a vector of integers
    {
        for (int i = 0; i< nums.size(); i++) // Outer loop iterates through each element in the array
        {
            for (int j = i+1; j< nums.size(); j++) // Inner loop starts from i+1 to avoid duplicate pairs
            {
                if((nums[i] + nums[j]) == target) // Check if current pair sums to target
                {
                    return std::vector<int>{i,j}; // Return indices of the two numbers that add up to target
                }
            }
        }
        return std::vector<int>{}; // Return empty vector if no solution found
    }
};
```

## Two Pointers

```cpp
class Solution { // Define a Solution class
public: // Public access specifier for class members
    bool isPalindrome(string s) { // Function to check if a string is a palindrome
        int l = 0; // Initialize left pointer at the beginning of the string
        int r = s.size() - 1; // Initialize right pointer at the end of the string
        while (l < r) { // Continue until pointers meet or cross
            while (l < r && !isalnum(s[l])) l++; // Skip non-alphanumeric characters from left
            while (l < r && !isalnum(s[r])) r--; // Skip non-alphanumeric characters from right
            if (tolower(s[l]) != tolower(s[r])) { // Compare characters (case insensitive)
                return false; // Return false if characters don't match
            }
            l++; // Move left pointer to the right
            r--; // Move right pointer to the left
        }
        return true; // If the loop completes without finding differences, it's a palindrome
    }
};
```

## Binary Search

```cpp
class Solution { // Define a Solution class
public: // Public access specifier for class members
    int search(vector<int>& nums, int target) { // Function to search for target in a sorted array
        int left = 0; // Initialize left boundary index
        int right = nums.size() -1; // Initialize right boundary index
        int mid; // Declare variable for middle index

        while ( left <= right) // Continue search while valid search range exists
        {
            mid = (right+left)/2; // Calculate middle index

            if (nums[mid] == target) // Check if target found at middle index
            {
                return mid; // Return index where target was found
            }
            
            if (nums[mid] < target) // If middle element is less than target
            {
                left = mid+1; // Search right half
            }
            else // If middle element is greater than target
            {
                right = mid-1; // Search left half
            }
        }
        return -1; // Return -1 if target is not found
    }
};
```

## Stack

### Valid Parentheses
```cpp
class Solution { // Define a Solution class
public: // Public access specifier for class members
    bool isValid(string s) { // Function to validate parentheses matching
        std::stack<char> stack; // Stack to store opening brackets

        std::unordered_map<char, char> parens = // Map to store pairs of matching brackets
        {
            {'(', ')'}, // Round brackets mapping
            {'{', '}'}, // Curly brackets mapping
            {'[', ']'} // Square brackets mapping
        };


        for(auto c : s) // Iterate through each character in the string
        {
  
            if(parens.find(c) != parens.end()) // If current character is an opening bracket
            {
                stack.push(c); // Push it onto the stack
            }
            else // If current character is a closing bracket
            {
                if (stack.empty() || parens.at(stack.top()) != c ) // If stack is empty or top doesn't match current
                {
                    return false; // Return false for invalid parentheses
                }
                stack.pop(); // Remove the matching opening bracket from stack
            }

        }
        return stack.empty(); // Valid if all brackets were matched (stack is empty)
        
    }
};
```

### Generate Parentheses

```cpp
class Solution { // Define a Solution class
public: // Public access specifier for class members
    vector<string> generateParenthesis(int n) { // Function to generate all combinations of well-formed parentheses
        vector<string> result; // Vector to store the result
        string subset; // Current string being built
        int closed =0; // Count of closed parentheses
        int open =0; // Count of open parentheses
        dfs(result, subset, n, closed, open); // Call helper DFS function
        return result; // Return the result vector
    }
private: // Private access specifier for helper methods
    void dfs(vector<string>& result, string& subset, int n, int closed, int open ) // DFS backtracking helper function
    {
        if(closed == n && open ==n) // Base case: if we've used all parentheses
        {
            result.push_back(subset); // Add the current string to results
            return; // Return from this branch
        }
        if(open <n ) // If we can still add more open parentheses
        {
            subset+="("; // Add an open parenthesis
            dfs(result, subset, n, closed, open+1); // Recursively explore this path
            subset.pop_back(); // Backtrack by removing the last character
        }

        if(closed < open) // If we can add a closing parenthesis (must have matching open)
        {
            subset+=")"; // Add a closing parenthesis
            dfs(result, subset, n, closed +1, open); // Recursively explore this path
            subset.pop_back(); // Backtrack by removing the last character

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

class Solution { // Define a Solution class
public: // Public access specifier for class members
    ListNode* reverseList(ListNode* head) { // Function to reverse a linked list
        ListNode * next = nullptr; // Pointer to store the next node
        ListNode * cur = head; // Pointer to the current node, starting with head
        ListNode * prev = nullptr; // Pointer to store the previous node

        while (cur != nullptr) // Continue until we reach the end of the list
        {
            next = cur->next; // Save the next node
            cur->next = prev; // Reverse the pointer of current node
            prev = cur; // Move prev to current node
            cur = next; // Move current to next node
        }
        return prev; // Return the new head (last node of original list)
        
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
class Solution { // Define a Solution class
public: // Public access specifier for class members
    void inorderTraversalHelper(TreeNode* root, vector<int>& ret) // Helper function for inorder traversal
    {
        if (root == nullptr) // Base case: if current node is null
        {
            return; // Return from this branch
        }
        inorderTraversalHelper(root->left, ret); // Recursively traverse left subtree
        ret.push_back(root->val); // Visit current node: add its value to result
        inorderTraversalHelper(root->right, ret); // Recursively traverse right subtree
    }
    vector<int> inorderTraversal(TreeNode* root) { // Main function for inorder traversal
        vector<int> ret; // Vector to store the traversal result
        inorderTraversalHelper(root, ret); // Call helper function
        return ret; // Return the result vector
        
    }
};
```

## Backtracking



### Subsets

```cpp
class Solution { // Define a Solution class
public: // Public access specifier for class members
    vector<vector<int>> subsets(vector<int>& nums) // Function to generate all possible subsets
    {
        vector<vector<int>> res; // Vector to store all subsets
        vector<int> subset; // Current subset being built
        dfs(nums, subset, res,0); // Call recursive helper with starting index 0
        return res; // Return all subsets
        
    }

    void dfs (vector<int>& nums, vector<int>& subset, vector<vector<int>>& res, int i) // DFS backtracking helper function
    {
        if(i >= nums.size()) // Base case: reached end of array
        {
            res.push_back(subset); // Add current subset to results
            return; // Return from this branch
        }

        subset.push_back(nums[i]); // Include current element
        dfs(nums, subset, res, i+1); // Recursively generate subsets with current element
        subset.pop_back(); // Backtrack by removing current element
        dfs(nums, subset, res, i+1); // Recursively generate subsets without current element
    }
};

```

### Permutations
```cpp
class Solution { // Define a Solution class
public: // Public access specifier for class members
    vector<vector<int>> permute(vector<int>& nums) // Function to generate all permutations
    {
        if ( nums.size() == 0) // Base case: empty array
        {
            return {{}}; // Return a single empty permutation
        }

        vector<int> tmp = vector<int>(nums.begin()+1, nums.end()); // Create subarray without first element
        vector<vector<int>> res; // Vector to store all permutations
        vector<vector<int>> perms = permute(tmp); // Recursively get permutations of subarray

        for (auto perm : perms) // For each permutation of the subarray
        {
            for (int i = 0 ; i <= perm.size() ; i++) // Try inserting first element at each position
            {
                vector<int> perm_copy  = perm; // Make a copy of current permutation
                perm_copy.insert(perm_copy.begin() + i , nums[0]); // Insert first element at position i
                res.push_back(perm_copy); // Add new permutation to results

            }
        }

        return res; // Return all permutations
    }
};


```

## Graphs

### Matrix DFS

```cpp
    vector<pair<int,int>> directions = {{-1,0}, {1,0}, {0,-1},{0,1}}; // Define the four possible movement directions (up, down, left, right)
    void dfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int r , int c) // DFS function for grid traversal
    {
        int cols  = grid[0].size(); // Get number of columns
        int rows = grid.size(); // Get number of rows

        visited[r][c] = true; // Mark current cell as visited

        cout << "Visiting:" << r << "," << c  << " = "  << grid[r][c] <<  std::endl; // Debug print current cell
        
        for (auto& dir : directions) // Iterate through all four directions
        {
            int newr  = r + dir.first; // Calculate new row coordinate
            int newc  = c+ dir.second; // Calculate new column coordinate

            if( // Check if the new position is valid for exploration:
                newr>= 0  // New row is within lower bound
                && newc >= 0 // New column is within lower bound
                && newr < rows // New row is within upper bound
                &&newc < cols // New column is within upper bound
                && !visited[newr][newc] // Cell hasn't been visited yet
                && grid[newr][newc] == '1' // Cell contains value '1' (land in island counting problem)
            )
            {
                dfs(grid, visited, newr, newc); // Recursively explore this cell
            }
        }
    }

```

### Matrix BFS - Shortest Path

```cpp
{
       int rows = grid.size(); // Get number of rows
       int cols = grid[0].size(); // Get number of columns
       vector<pair<int, int>> dirs = {{1,0}, {-1,0},{0, 1},{0,-1},{1,1},{-1,-1},{1,-1},{-1,1}}; // Define 8 directions (including diagonals)
       queue<pair<int,int>> q; // Queue for BFS
       set<pair<int,int>> visited; // Set to track visited cells

       if (grid[0][0] !=0 || grid[rows-1][cols-1] != 0) // Check if start or end cells are obstacles
       {
        return -1; // Return -1 if path is impossible
       }

       q.push({0,0}); // Start BFS from top-left cell
       visited.insert({0,0}); // Mark start cell as visited
       int length  =1; // Initialize path length to 1

       while (!q.empty()) // Continue BFS until queue is empty
       {
            int size = q.size(); // Get current level size
            for(int i = 0 ; i < size; i++ ) // Process all cells at current level
            {
                int r = q.front().first; // Get row of current cell
                int c = q.front().second; // Get column of current cell
                q.pop(); // Remove cell from queue
                if( r == rows-1 && c == cols -1) // Check if reached destination
                {
                    return length; // Return path length if destination reached
                }

                for(auto dir: dirs) // Explore all 8 directions
                {
                    auto new_r = r+ dir.first; // Calculate new row
                    auto new_c = c + dir.second; // Calculate new column
                    auto new_p = pair<int, int>{new_r, new_c}; // Create pair for new position

                    if( // Check if new position is valid:
                        new_r >= 0 // New row within lower bound
                        && new_c >= 0 // New column within lower bound
                        && new_r < rows // New row within upper bound
                        && new_c < cols // New column within upper bound
                        && grid[new_r][new_c] == 0 // Cell is not an obstacle
                        && (visited.find(new_p) == visited.end()) // Cell hasn't been visited
                    )
                    {
                        q.push(new_p); // Add cell to queue
                        visited.insert(new_p); // Mark cell as visited
                    }
                }
            }
            length++; // Increment path length after processing current level
       }
       return -1; // Return -1 if no path found
    }

```

## Dynamic Programming

### Climbing Stairs
```cpp
class Solution { // Define a Solution class
public: // Public access specifier for class members
    int climbStairs(int n) { // Function to calculate ways to climb n stairs
        if(n<=2) // Base case: if 1 or 2 stairs
         return n; // Return n (1 way for 1 stair, 2 ways for 2 stairs)

        if(n>2) // Recursive case: if more than 2 stairs
            return climbStairs(n-1) + climbStairs(n-2); // Recurrence relation: ways to climb n stairs equals ways to climb (n-1) + ways to climb (n-2)
        
    }
};

```