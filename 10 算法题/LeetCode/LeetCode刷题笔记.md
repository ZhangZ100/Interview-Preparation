# Tree-tag

#### [99. 恢复二叉搜索树](https://leetcode-cn.com/problems/recover-binary-search-tree/)

**解法一**

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public void recoverTree(TreeNode root) {
        List<TreeNode> list = new ArrayList<TreeNode>();
        dfs(root,list);
        TreeNode x = null;
        TreeNode y = null;
        //扫面遍历的结果，找出可能存在错误交换的节点x和y
        for(int i=0;i<list.size()-1;++i) {
            if(list.get(i).val>list.get(i+1).val) {
                y = list.get(i+1);
                if(x==null) {
                    x = list.get(i);
                }else{
					break;
                }
            }
        }
        //如果x和y不为空，则交换这两个节点值，恢复二叉搜索树
        if(x!=null && y!=null) {
            int tmp = x.val;
            x.val = y.val;
            y.val = tmp;
        }
    }

    //中序遍历二叉树，并将遍历的结果保存到list中        
    private void dfs(TreeNode node,List<TreeNode> list) {
        if(node==null) {
            return;
        }
        dfs(node.left,list);
        list.add(node);
        dfs(node.right,list);
    }
}

```

- 首先，BST的最主要特征是中序遍历序列为升序
- 所以，在此基础上，我们先得到中序序列，然后记录错位的两个结点的下标或者值，以便后面交换
  - 这里注意题目，题目说了只有两个结点错位。

**一般情况**

......error one | a ... b| error two ......

...处都是升序的序列，是符合BST的。

不符合升序的有两种情况，error one >a ,b>error two。

- 1  **9** 4 5 6 **2**	我们只需检索出 9>4,	6>2，然后记录下9和2即可。
- 1  **4 2** 5 6 9    比较特殊的情况，两者相邻。



**解法二**

上述的解法中序遍历的时候空间复杂度是O(n)，但是可以优化到O(1)，因为只需要记录两个结点。

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    //用两个变量x，y来记录需要交换的节点
    private TreeNode x = null;
    private TreeNode y = null;
    private TreeNode pre = null;
    public void recoverTree(TreeNode root) {
        dfs(root);
        //如果x和y都不为空，说明二叉搜索树出现错误的节点，将其交换
        if(x!=null && y!=null) {
            int tmp = x.val;
            x.val = y.val;
            y.val = tmp;
        }
    }
	
    //中序遍历二叉树，并比较上一个节点(pre)和当前节点的值，如果pre的值大于当前节点值，则记录下这两个节点
    private void dfs(TreeNode node) {
        if(node==null) {
            return;
        }

        dfs(node.left);

        if(pre==null) {
            pre = node;
        }else {
            if(pre.val>node.val) {
                y = node;
                if(x==null) {
                    x = pre;
                }
            }
            pre = node;//pre指向当前中序遍历的结点
        }

        dfs(node.right);
    }
}


~~~

- 定义全局变量Node x,	y，用来记录两个可能出错的结点。
- 定义Node 变量 pre指向当前中序遍历的结点。
- 总是先比较pre和当前node的val值大小，如果是降序，则记录y为node(如果x为null,则更新x为pre，x总是只会被更新一次)
- 总是在每次比较更新完y的值之后更新pre的值指向node，指向当前中序遍历的结点node。（根据题意，如果出错，y最多/一般会被更新两次，x总是被更新一次）
- 空间复杂度优化到了O(1)，其实也不算严格的O(1)，因为用到了递归，如果需要严格的O(1)，则可以 **莫里斯遍历**



# 数组-tag

#### [1846. 减小和重新排列数组后的最大元素](https://leetcode-cn.com/problems/maximum-element-after-decreasing-and-rearranging/)

![image-20220105200903476](LeetCode%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20220105200903476.png)

~~~java
class Solution {
    public int maximumElementAfterDecrementingAndRearranging(int[] arr) {
        int n = arr.length;
        Arrays.sort(arr);
        arr[0] = 1;
        for (int i = 1; i < n; i++) {
            if (arr[i] - arr[i - 1] > 1) {
                arr[i] = arr[i - 1] + 1;
            }
        }
        return arr[n - 1];
    }
}
~~~









#### [1995. 统计特殊四元组](https://leetcode-cn.com/problems/count-special-quadruplets/)

![image-20211229200414508](LeetCode%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20211229200414508.png)

![image-20211229200426392](LeetCode%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20211229200426392.png)

- 注意审题，四元组的下标 a b c d是严格的 a<b<c<d，所以不能排序。



**解法一	暴力法**

~~~java
class Solution {
    public int countQuadruplets(int[] nums) {
        int n = nums.length;
        int ans = 0;
        for (int a = 0; a < n; ++a) {
            for (int b = a + 1; b < n; ++b) {
                for (int c = b + 1; c < n; ++c) {
                    for (int d = c + 1; d < n; ++d) {
                        if (nums[a] + nums[b] + nums[c] == nums[d]) {
                            ++ans;
                        }
                    }
                }
            }
        }
        return ans;
    }
}

~~~

- 直接枚举a b c d四个下标进行判断



**解法二	使用哈希表记录nums[d]**

~~~java
class Solution {
public:
    int countQuadruplets(vector<int>& nums) {
        int n = nums.size();
        int ans = 0;
        unordered_map<int, int> cnt;
        for (int c = n - 2; c >= 2; --c) {
            ++cnt[nums[c + 1]];
            for (int a = 0; a < c; ++a) {
                for (int b = a + 1; b < c; ++b) {
                    if (int sum = nums[a] + nums[b] + nums[c]; cnt.count(sum)) {
                        ans += cnt[sum];
                    }
                }
            }
        }
        return ans;
    }
};

~~~

- 这样每次枚举c的时候，就不用在[c+1,n-1]里面枚举一遍d



**解法三	使用哈希表记录d-c**

~~~java
class Solution {
    public int countQuadruplets(int[] nums) {
        int n = nums.length;
        int ans = 0;
        Map<Integer, Integer> cnt = new HashMap<Integer, Integer>();
        for (int b = n - 3; b >= 1; --b) {
            for (int d = b + 2; d < n; ++d) {
                cnt.put(nums[d] - nums[b + 1], cnt.getOrDefault(nums[d] - nums[b + 1], 0) + 1);
            }
            for (int a = 0; a < b; ++a) {
                ans += cnt.getOrDefault(nums[a] + nums[b], 0);
            }
        }
        return ans;
    }
}

~~~

- 解法三中每次枚举b+1的时候，b+1也是新的，所以需要把对于b+1这个新的c，在[b+1,n-1]里面记录上所有的d-c
- 本质是节省了枚举b的时候，需要在[b+1,n-1]里面枚举c和d
- 无法理解的时候，关注一下下标会比较容易理解。



# 贪心-tag



#### [846. 一手顺子](https://leetcode-cn.com/problems/hand-of-straights/)

![image-20211230010422732](LeetCode%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20211230010422732.png)

![image-20211230010432320](LeetCode%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20211230010432320.png)

- 注意审题，很自然地想到数组的 len对groupSize取模必须为0，否则放回false；然而紧接着才是这个题的重点，这个题要求groupSize的分组里面数字是连续的，所以我们自然想到了用map来统计个数，然后依次取出各个组。
- 思路进行不下去的原因在于，知道从最小的开始取，但是一些细节，比如从数组中找出来后，怎么处理
- 比如 123 234，排序后是 122334，从最小的开始取，取出123，哈希表怎么才能取，数组下标又怎么弄这些问题

答案是用贪心，注意细节的地方

~~~java
class Solution {
    public boolean isNStraightHand(int[] hand, int groupSize) {
        int n = hand.length;
        if (n % groupSize != 0) {
            return false;
        }
        Arrays.sort(hand);
        Map<Integer, Integer> cnt = new HashMap<Integer, Integer>();
        for (int x : hand) {
            cnt.put(x, cnt.getOrDefault(x, 0) + 1);
        }
        for (int x : hand) {
            if (!cnt.containsKey(x)) {
                continue;
            }
            for (int j = 0; j < groupSize; j++) {
                int num = x + j;
                if (!cnt.containsKey(num)) {
                    return false;
                }
                cnt.put(num, cnt.get(num) - 1);
                if (cnt.get(num) == 0) {
                    cnt.remove(num);
                }
            }
        }
        return true;
    }
}
~~~



# 滑动窗口-tag



#### [30. 串联所有单词的子串](https://leetcode-cn.com/problems/substring-with-concatenation-of-all-words/)

![image-20211230012109765](LeetCode%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20211230012109765.png)

![image-20211230012117814](LeetCode%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20211230012117814.png)

- 注意审题，题目中说明了word的length是相等的
- 所以，用hashmap来记录，再比较两个hashmap是否相等即可
- 进一步优化的，可以用滑动窗口来维护s中我们搜索的字符串，可以优化时间复杂度
- 但是再进一步优化就比较复杂，不考虑



**解法一	比较hashmap是否相等**

~~~java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> res = new ArrayList<>();
        if (s == null || s.length() == 0 || words == null || words.length == 0) return res;
        HashMap<String, Integer> map = new HashMap<>();
        int one_word = words[0].length();
        int word_num = words.length;
        int all_len = one_word * word_num;
        for (String word : words) {
            map.put(word, map.getOrDefault(word, 0) + 1);
        }
        for (int i = 0; i < s.length() - all_len + 1; i++) {
            String tmp = s.substring(i, i + all_len);
            HashMap<String, Integer> tmp_map = new HashMap<>();
            for (int j = 0; j < all_len; j += one_word) {
                String w = tmp.substring(j, j + one_word);
                tmp_map.put(w, tmp_map.getOrDefault(w, 0) + 1);
            }
            if (map.equals(tmp_map)) res.add(i);
        }
        return res;
    }
}
~~~



**解法二	滑动窗口**

~~~java
class Solution {
    public List<Integer> findSubstring(String s, String[] words) {
        List<Integer> res = new ArrayList<>();
        if (s == null || s.length() == 0 || words == null || words.length == 0) return res;
        HashMap<String, Integer> map = new HashMap<>();
        int one_word = words[0].length();
        int word_num = words.length;
        int all_len = one_word * word_num;
        for (String word : words) {
            map.put(word, map.getOrDefault(word, 0) + 1);
        }
        for (int i = 0; i < one_word; i++) {
            int left = i, right = i, count = 0;
            HashMap<String, Integer> tmp_map = new HashMap<>();
            while (right + one_word <= s.length()) {
                String w = s.substring(right, right + one_word);
                tmp_map.put(w, tmp_map.getOrDefault(w, 0) + 1);
                right += one_word;
                count++;
                while (tmp_map.getOrDefault(w, 0) > map.getOrDefault(w, 0)) {
                    String t_w = s.substring(left, left + one_word);
                    count--;
                    tmp_map.put(t_w, tmp_map.getOrDefault(t_w, 0) - 1);
                    left += one_word;
                }
                if (count == word_num) res.add(left);

            }
        }

        return res;
    }
}
~~~



# 几何、数学-tag

#### [223. 矩形面积](https://leetcode-cn.com/problems/rectangle-area/)

![image-20220105201519083](LeetCode%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20220105201519083.png)

~~~java
class Solution {
    public int computeArea(int ax1, int ay1, int ax2, int ay2, int bx1, int by1, int bx2, int by2) {
        int area1 = (ax2 - ax1) * (ay2 - ay1), area2 = (bx2 - bx1) * (by2 - by1);
        int overlapWidth = Math.min(ax2, bx2) - Math.max(ax1, bx1);
        int overlapHeight = Math.min(ay2, by2) - Math.max(ay1, by1);
        int overlapArea = Math.max(overlapWidth, 0) * Math.max(overlapHeight, 0);
        return area1 + area2 - overlapArea;
    }
}
~~~

- 不失一般性的解答





# 动态规划-tag

#### [1220. 统计元音字母序列的数目](https://leetcode-cn.com/problems/count-vowels-permutation/)

![image-20220117004700601](LeetCode%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20220117004700601.png)



首先，分析出5种情况，aeiou，动态规划

dp\[i][j] 代表以字母j结尾的长度为i的合法字符串个数

其中 j 的 0 1 2 3 4 分别代表a e i o u



原题意等价为

![image-20220117005100139](LeetCode%E5%88%B7%E9%A2%98%E7%AC%94%E8%AE%B0.assets/image-20220117005100139.png)



~~~java
class Solution {
    public int countVowelPermutation(int n) {
        long mod = 1000000007;
        long[] dp = new long[5];
        long[] ndp = new long[5];
        for (int i = 0; i < 5; ++i) {
            dp[i] = 1;
        }
        for (int i = 2; i <= n; ++i) {
            /* a前面可以为e,u,i */
            ndp[0] = (dp[1] + dp[2] + dp[4]) % mod;
            /* e前面可以为a,i */
            ndp[1] = (dp[0] + dp[2]) % mod;
            /* i前面可以为e,o */
            ndp[2] = (dp[1] + dp[3]) % mod;
            /* o前面可以为i */
            ndp[3] = dp[2];
            /* u前面可以为i,o */
            ndp[4] = (dp[2] + dp[3]) % mod;
            // System.arraycopy(ndp, 0, dp, 0, 5);
            for(int k=0;k<5;k++){
                dp[k]=ndp[k];
            }
        }
        long ans = 0;
        for (int i = 0; i < 5; ++i) {
            ans = (ans + dp[i]) % mod;
        }
        return (int)ans;
    }
}
~~~

