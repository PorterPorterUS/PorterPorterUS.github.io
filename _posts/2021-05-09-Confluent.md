---
layout: post
author: Xusheng Ji
title: "Confluent coding"
tags: algorithm leetcode
categories: interview
---

{% include lib/mathjax.html %}


<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    extensions: [
      "MathMenu.js",
      "MathZoom.js",
      "AssistiveMML.js",
      "a11y/accessibility-menu.js"
    ],
    jax: ["input/TeX", "output/CommonHTML"],
    TeX: {
      extensions: [
        "AMSmath.js",
        "AMSsymbols.js",
        "noErrors.js",
        "noUndefined.js",
      ]
    }
  });
</script>


Confluent题目


### 44.wildcard matching
```java
class Solution {
public:
    bool isMatch(string s, string p) {
        int i = 0, j = 0, iStar = -1, jStar = -1, m = s.size(), n = p.size();
        while (i < m) {
            if (j < n && (s[i] == p[j] || p[j] == '?')) {
                ++i, ++j;//i，j向后瞬移
            } else if (j < n && p[j] == '*') {//记录如果之后序列匹配不成功时， i和j需要回溯到的位置
                iStar = i;//记录星号
                jStar = j++;//记录星号 并且j移到下一位 准备下个循环s[i]和p[j]的匹配
            } else if (iStar >= 0) {//发现字符不匹配且没有星号出现 但是istar>0 说明可能是*匹配的字符数量不对 这时回溯
                i = ++iStar;//i回溯到istar+1 因为上次从s串istar开始对*的尝试匹配已经被证明是不成功的（不然不会落入此分支） 所以需要从istar+1再开始试 同时inc istar 更新回溯位置
                j = jStar + 1;//j回溯到jstar+1 重新使用p串*后的部分开始对s串istar（这个istar在上一行已经inc过了）位置及之后字符的匹配 
            } else return false;
        }
        while (j < n && p[j] == '*') ++j;//去除多余星号
        return j == n;
    }
};

```


```java
// greedy solution with idea of DFS
// starj stores the position of last * in p
// last_match stores the position of the previous matched char in s after a *
// e.g. 
// s: a c d s c d
// p: * c d
// after the first match of the *, starj = 0 and last_match = 1
// when we come to i = 3 and j = 3, we know that the previous match of * is actually wrong, 
// (the first branch of DFS we take is wrong)
// then it resets j = starj + 1 
// since we already know i = last_match will give us the wrong answer
// so this time i = last_match+1 and we try to find a longer match of *
// then after another match we have starj = 0 and last_match = 4, which is the right solution
// since we don't know where the right match for * ends, we need to take a guess (one branch in DFS), 
// and store the information(starj and last_match) so we can always backup to the last correct place and take another guess.

 bool isMatch(string s, string p) {
        int i = 0, j = 0;
        int m = s.length(), n = p.length();
        int last_match = -1, starj = -1;
        while (i < m){
            if (j < n && (s[i] == p[j] || p[j] == '?')){
                i++; j++;
            }
            else if (j < n && p[j] == '*'){
                starj = j;
                j++;
                last_match = i;
            }
            else if (starj != -1){
                j = starj + 1;
                last_match++;
                i = last_match;
            }
            else return false;
        }
        while (p[j] == '*' && j <n) j++;
        return j == n;
    }

```



```java

/**
 * General Idea: Credit: https://leetcode.com/problems/wildcard-matching/discuss/370736/Detailed-Intuition-From-Brute-force-to-Bottom-up-DP 
 * The idea is pretty straightforward : scan S and P while there is a match between the current character of S and the current character of P. 
 * If we reach the end of both strings while there is still a match, return True, otherwise return False. 
 * The scan is done by having a pointer in S and a pointer in P.

	Example: S="code"
	The character 'c' of S matches the first character of P if the first character of P is:

	'c'
	'?'
	'*'

	Case 1:
		When the first character of P is a lowercase letter different from 'c', return False.
	Case 2:
		If the first character of P is 'c' or '?', we move both pointers one step to the right.
	Case 3:
		If the first character of P is '*', we have 2 possibilities:

			- '*' matches 0 character : in this case we move the pointer in P one step, ie will ignore the whole pattern
			- '*' matches 1 or more characters : in this case we move the pointer in S one step, ie we consider pattern
	And we continue like this for each two positions taken by the two pointers.

	- If we reach the end of P but there is still characters from S, simply return .. False !
	- If we reach the end of S and there is still characters from P, the only case when there is a match is that all the remaining characters in P are '*', 
	  in this case these stars will be matched with the empty string.
 ****/

/*******************
 * Approach 1: TLE: using recursion
 * 
 * Time Complexity : 
 * 
 * Space Complexity : O(M^2+N^2)
 **********************************/

public boolean isMatchApproach1(String text, String pattern) {
	return isMatchApproach1Helper(0, 0, text, pattern);
}

public boolean isMatchApproach1Helper(int tIdx, int pIdx, String text, String pattern) {

	// reached the end of both S and P
	if (tIdx == text.length() && pIdx == pattern.length()) {
		return true;
	}
	// there are still characters in S => there is no match
	else if (pIdx == pattern.length()) {
		return false; // Can't have a non-empty s match an empty p.
	}
	// if we reached end of text and pattern is still left. 
	// Try to see if p at or after this stage is only * or ** or *** etc. Only way to match an empty text.
	else if (tIdx == text.length()) {
		return pattern.charAt(pIdx) == '*' && isMatchApproach1Helper(tIdx, pIdx + 1, text, pattern);
	}
	// Here cuz text & pattern match atleast a char
	else if (text.charAt(tIdx) == pattern.charAt(pIdx) || pattern.charAt(pIdx) == '?') {

		// Match here if strs from next index onward also are a match. Delegate job to recursive func for latter.
		return isMatchApproach1Helper(tIdx + 1, pIdx + 1, text, pattern);

	}
	// star either matches 0 or >=1 character
	else if (pattern.charAt(pIdx) == '*') {
		// 1: When * matches an empty seq, it's work is done. Hence, the next stage to check match for is w/o *.
		// 	  Also, there could be *s in line. So, consuming this *, could exhibit new p with next fresh *.
		// 2: '*' can match seq of chars. Hence, * kept. Further rec stages could use it to match more chars/empty.
		return isMatchApproach1Helper(tIdx, pIdx + 1, text, pattern)
				|| isMatchApproach1Helper(tIdx + 1, pIdx, text, pattern);
	}

	return false;
}

/*******************
 * Approach 2: Top Down Memoization + Recursion
 * Top-down the smallest problem is (len(s), len(p)),
 * 
 * Time Complexity : O(m * n)
 * 
 * Space Complexity : O(m * n)
 **********************************/
Boolean[][] memo;

public boolean isMatchApproach2(String text, String pattern) {
	memo = new Boolean[text.length() + 1][pattern.length() + 1];
	return dpMemo(0, 0, text, pattern);
}

public boolean dpMemo(int tIdx, int pIdx, String text, String pattern) {
	if (memo[tIdx][pIdx] != null) {
		return memo[tIdx][pIdx];
	}
	boolean match = false; // False picked up in memo @ end for else case when chars don't match.

	// reached the end of both S and P
	if (tIdx == text.length() && pIdx == pattern.length()) {
		match = true;

	}
	// there are still characters in S => there is no match
	else if (pIdx == pattern.length()) {

		match = false; // Can't have a non-empty s match an empty p.
	}
	// if we reached end of text and pattern is still left. 
	// Try to see if p at or after this stage is only * or ** or *** etc. Only way to match an empty text.
	else if (tIdx == text.length()) {
		match = pattern.charAt(pIdx) == '*' && dpMemo(tIdx, pIdx + 1, text, pattern);

	}
	//  Here cuz text & pattern match atleast a char
	else if (text.charAt(tIdx) == pattern.charAt(pIdx) || pattern.charAt(pIdx) == '?') {

		// Match here if strs from next index onward also are a match. Delegate job to recursive func for latter.
		match = dpMemo(tIdx + 1, pIdx + 1, text, pattern);

	} 
	// star either matches 0 or >=1 character
	else if (pattern.charAt(pIdx) == '*') {
		// 1: When * matches an empty seq, it's work is done. Hence, the next stage to check match for is w/o *.
		// 	  Also, there could be *s in line. So, consuming this *, could exhibit new p with next fresh *.
		// 2: '*' can match seq of chars. Hence, * kept. Further rec stages could use it to match more chars/empty.
		match = dpMemo(tIdx, pIdx + 1, text, pattern) || dpMemo(tIdx + 1, pIdx, text, pattern);
	}

	return memo[tIdx][pIdx] = match;
}

/*******************
 * Approach 3: Bottom Up Tabulation: https://www.youtube.com/watch?v=3ZDZ-N0EPV0
 * Bottom-up the smallest is (0, 0)
 * 
 * 				|	dp[i-1][j-1]  if str[i] == pattern[j] || pattern[j] == '?'
 * 				|					
 * 				|	if pattern[j-1] == '*'
 * dp[i][j] = 	|		dp[i][j-1] || dp[i-1][j]
 * 				|					
 * 				|	False
 * 
 * Time Complexity : O(m * n)
 * Space Complexity : O(m * n)
 **********************************/

public boolean isMatchApproach3(String text, String pattern) {

	int m = text.length();
	int n = pattern.length();
	boolean[][] dp = new boolean[m + 1][n + 1];

	for (int tIdx = 0; tIdx <= m; tIdx++) {
		for (int pIdx = 0; pIdx <= n; pIdx++) {

			// empty strings match
			if (tIdx == 0 && pIdx == 0)
				dp[tIdx][pIdx] = true;

			// fill first row, empty text can match all * pattern
			else if (tIdx == 0)
				dp[tIdx][pIdx] = pattern.charAt(pIdx - 1) == '*' && dp[tIdx][pIdx - 1] == true;

			// Can't have a non-empty text match an empty pattern.
			else if (pIdx == 0)
				dp[tIdx][pIdx] = false;

			// star either matches 0 or >=1 character
			else if (pattern.charAt(pIdx - 1) == '*')
				dp[tIdx][pIdx] = dp[tIdx][pIdx - 1] || dp[tIdx - 1][pIdx];

			//  Here cuz text & pattern match atleast a char
			else if (text.charAt(tIdx - 1) == pattern.charAt(pIdx - 1) || pattern.charAt(pIdx - 1) == '?')
				dp[tIdx][pIdx] = dp[tIdx - 1][pIdx - 1];
		}
	}

	return dp[m][n];
}

/*
 * Approach 4: Linear: Very tricky: https://leetcode.com/problems/wildcard-matching/discuss/17810/Linear-runtime-and-constant-space-solution
 */
public boolean isMatchApproach4(String text, String pattern) {
	int tIdx = 0, pIdx = 0, match = 0, starIdx = -1;

	while (tIdx < text.length()) {
		// advancing both pointers
		if (pIdx < pattern.length() && (pattern.charAt(pIdx) == '?' || text.charAt(tIdx) == pattern.charAt(pIdx))) {
			tIdx++;
			pIdx++;
		}
		// '*' found, only advancing pattern pointer
		else if (pIdx < pattern.length() && pattern.charAt(pIdx) == '*') {
			starIdx = pIdx;
			match = tIdx;
			pIdx++;
		}
		// last pattern pointer was *, advancing string pointer
		else if (starIdx != -1) {
			pIdx = starIdx + 1;
			match++;
			tIdx = match;
		}
		//current pattern pointer is not star, last patter pointer was not *
		//characters do not match
		else
			return false;
	}

	//check for remaining characters in pattern
	while (pIdx < pattern.length() && pattern.charAt(pIdx) == '*')
		pIdx++;

	return pIdx == pattern.length();
}

```

