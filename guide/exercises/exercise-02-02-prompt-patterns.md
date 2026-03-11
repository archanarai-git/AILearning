# Exercise 2.2: Prompt Patterns

**Difficulty:** 🟡 Intermediate
**Time:** 30 minutes
**Chapter:** Prompt Engineering Fundamentals
**Skills:** Applying prompt patterns, scenario analysis, real-world prompting

---

## Problem Statement

You're in various interview scenarios and need to use the right prompt pattern to get what you need from an AI assistant. Below are 4 real interview scenarios. For each one, you must write an effective prompt using the appropriate pattern from Chapter 2.

---

## Learning Goals

This exercise helps you practice:
- Identifying which pattern fits which scenario
- Writing real-world prompts you can use in interviews
- Adapting patterns to specific situations
- Building confidence in AI interaction

---

## Your Task

For each of the 4 scenarios below:

1. **Identify the pattern:** Which of the 6 patterns (Implementation, Debugging, Optimization, Testing, Explanation, Refactoring) best fits?
2. **Write the prompt:** Create a clear, specific prompt using that pattern
3. **Explain your choice:** Why is this pattern the best fit?

Take 25-30 minutes total. Work on all 4 before checking solutions.

---

## Scenario 1: Building a Cache

**Interview situation:** You're asked to implement an LRU (Least Recently Used) cache during a live coding interview. You have a clear idea of the approach (hash map + doubly linked list) but want to focus on getting working code quickly rather than typing from scratch.

**Your task:** Write a prompt that uses the Implementation pattern to request an LRU cache in Java.

**Constraints mentioned by interviewer:**
- Must support get(key) and put(key, value) operations
- Must have a maximum capacity
- When capacity is exceeded, evict the least recently used item
- Both operations should be O(1) time complexity

**Your prompt:**

[Your answer here]

---

## Scenario 2: Debugging Test Failures

**Interview situation:** You submitted a solution for a coding problem. The code compiles and runs, but 2 out of 5 test cases are failing. You don't have time to debug manually, so you'll ask AI for help.

**Your task:** Write a prompt using the Debugging pattern.

**The code:**
```java
public class Solution {
    public static List<Integer> findTwoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return Arrays.asList(map.get(complement), i);
            }
            map.put(nums[i], i);
        }
        return new ArrayList<>();
    }
}
```

**Test failures:**
- Test case 1: Input: nums = [2, 7, 11, 15], target = 9 → Expected [0, 1], got [0, 1] ✅ PASS
- Test case 2: Input: nums = [3, 2, 4], target = 6 → Expected [1, 2], got empty list ❌ FAIL
- Test case 3: Input: nums = [3, 3], target = 6 → Expected [0, 1], got empty list ❌ FAIL

**Your prompt:**

[Your answer here]

---

## Scenario 3: Writing Comprehensive Tests

**Interview situation:** You've implemented a string manipulation method during the interview. It works for basic cases, but you want to make sure you haven't missed edge cases. You decide to ask AI to help generate comprehensive test cases.

**Your task:** Write a prompt using the Testing pattern.

**Your implementation:**
```java
public class StringUtils {
    /**
     * Reverses a string and removes all whitespace
     * @param str input string
     * @return reversed string without whitespace
     */
    public static String reverseAndTrim(String str) {
        if (str == null || str.isEmpty()) {
            return str;
        }
        String noWhitespace = str.replaceAll("\\s+", "");
        return new StringBuilder(noWhitespace).reverse().toString();
    }
}
```

**Test framework:** JUnit 5
**Coverage scenarios you want tested:**
- Normal strings with spaces
- Empty string
- Null string
- Strings with only whitespace
- Strings with multiple types of whitespace (spaces, tabs, newlines)
- Single character strings
- Strings with special characters

**Your prompt:**

[Your answer here]

---

## Scenario 4: Understanding an Algorithm

**Interview situation:** You're in the deep technical dive stage of an interview. The interviewer mentions a specific algorithm (consistent hashing) for a system design problem. You're not deeply familiar with it, so you quickly ask AI to explain it before you design the system.

**Your task:** Write a prompt using the Explanation pattern.

**Your prompt:**

[Your answer here]

---

## Hints

<details>
<summary>Hint 1: Pattern Matching</summary>

- **Implementation:** Building something from scratch or from a description
- **Debugging:** Something isn't working as expected
- **Optimization:** Something works but needs to be better (faster/cleaner)
- **Testing:** Need to verify code works across many cases
- **Explanation:** Need to understand a concept or algorithm
- **Refactoring:** Code works but structure/quality needs improvement

</details>

<details>
<summary>Hint 2: Key pattern elements</summary>

Each pattern has specific elements:
- **Implementation:** Input/Output/Constraints/Edge cases
- **Debugging:** Current behavior/Expected behavior/Error messages
- **Testing:** Framework/Coverage scenarios/Edge cases
- **Explanation:** Audience level/Detail level/Specific focus
- **Optimization:** Current performance/Target/Constraints
- **Refactoring:** Issues to address/Constraints/Desired style

</details>

<details>
<summary>Hint 3: Time awareness</summary>

Remember you're in an interview with limited time:
- Be specific to avoid iterations
- Include all necessary context upfront
- Don't over-specify (let AI optimize)
- Validate the result quickly

</details>

---

## Evaluation Criteria

Your prompts should:
- ✅ Use the correct pattern for the scenario
- ✅ Be clear and specific (no ambiguity)
- ✅ Include necessary context (code, requirements, edge cases)
- ✅ Be concise (interview time is limited)
- ✅ Ask for explanation/justification when appropriate
- ✅ Follow Java conventions and syntax

---

## Sample Solutions

<details>
<summary>Click to reveal solutions</summary>

### Scenario 1 Solution: LRU Cache Implementation

**Pattern:** Implementation

**Explanation:** This is a classic implementation problem—you need to build something from scratch with clear requirements.

**Sample prompt:**
```
"Implement an LRU (Least Recently Used) Cache in Java with the following requirements:

Class structure:
public class LRUCache {
    public LRUCache(int capacity) { }
    public int get(int key) { }
    public void put(int key, int value) { }
}

Requirements:
- get(key): Return the value for a key if it exists, otherwise return -1
- put(key, value): Insert or update the key-value pair. If cache exceeds capacity, evict the least recently used item
- Both operations must be O(1) time complexity
- Accessing (get or put) a key makes it 'recently used'

Constraints:
- Use a HashMap for O(1) lookups
- Use a doubly linked list to track usage order
- Capacity is fixed and provided in constructor

Include:
- Javadoc for public methods
- Explanation of how the HashMap and linked list work together
- Time/space complexity analysis"
```

**Why this works:**
- Clear input/output (get/put operations)
- Specific constraints (O(1) complexity requirement)
- Hints at approach (HashMap + doubly linked list) without over-specifying
- Requests explanation and complexity analysis
- Follows Implementation pattern precisely

---

### Scenario 2 Solution: Debugging Test Failures

**Pattern:** Debugging

**Explanation:** Code is running but producing wrong results—classic debugging scenario.

**Sample prompt:**
```
"Debug this Java method that should find two indices in an array that sum to a target value.

Current behavior: Method passes test case [2, 7, 11, 15], target=9 but fails on:
- [3, 2, 4], target=6: Returns empty list instead of [1, 2]
- [3, 3], target=6: Returns empty list instead of [0, 1]

Expected behavior: Return indices [i, j] where nums[i] + nums[j] == target and i < j

The issue appears to be that the method doesn't find valid pairs in some cases.

Current code:
public static List<Integer> findTwoSum(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        int complement = target - nums[i];
        if (map.containsKey(complement)) {
            return Arrays.asList(map.get(complement), i);
        }
        map.put(nums[i], i);
    }
    return new ArrayList<>();
}

Please:
1. Identify why it fails for the specific test cases
2. Provide the corrected code
3. Explain the bug and the fix"
```

**Why this works:**
- Clearly states current vs. expected behavior
- Provides specific failing inputs
- Shows the code being debugged
- Asks for explanation of the root cause
- Follows Debugging pattern with all required elements

---

### Scenario 3 Solution: Writing Comprehensive Tests

**Pattern:** Testing

**Explanation:** You have working code and want to ensure it's well-tested—textbook Testing pattern.

**Sample prompt:**
```
"Write comprehensive unit tests for this Java method using JUnit 5:

public class StringUtils {
    public static String reverseAndTrim(String str) {
        if (str == null || str.isEmpty()) {
            return str;
        }
        String noWhitespace = str.replaceAll(\"\\\\s+\", \"\");
        return new StringBuilder(noWhitespace).reverse().toString();
    }
}

Test framework: JUnit 5

Test scenarios to cover:
1. Normal case: String with spaces → should reverse and remove spaces
   Example: \"hello world\" → \"dlrowolleh\"

2. Edge cases:
   - Empty string → should return empty string
   - Null string → should return null
   - String with only whitespace → should return empty string
   - String with mixed whitespace (spaces, tabs, newlines) → should remove all

3. Special characters:
   - String with punctuation → should preserve and reverse
   - String with numbers → should preserve and reverse

4. Single element: Single character string → should return same character

Requirements:
- Use @Test annotations
- Include both positive and negative test cases
- Provide meaningful test method names
- Include assertions with helpful messages
- Assume the method behavior is correct; just write the tests"
```

**Why this works:**
- Specifies JUnit 5 framework
- Lists comprehensive coverage scenarios
- Provides example inputs/outputs
- States edge cases explicitly
- Follows Testing pattern with all necessary elements
- Includes assumption about correctness

---

### Scenario 4 Solution: Understanding Consistent Hashing

**Pattern:** Explanation

**Explanation:** You need to understand a concept/algorithm—pure Explanation pattern.

**Sample prompt:**
```
"Explain consistent hashing in the context of distributed systems, particularly for:

Audience: A software engineer familiar with basic hashing and distributed systems concepts, but not familiar with consistent hashing specifically

Detail level: Detailed with practical implications

Specific focus:
1. How it differs from simple hash modulo approach
2. Why it's useful for distributed caches (like Memcached) and load balancing
3. The virtual nodes concept and why it matters
4. Practical example: How would consistent hashing work if you have 3 cache servers and need to add/remove one?

Please include:
- A clear explanation of the core concept
- Why simple modulo hashing fails at scale
- How consistent hashing solves this problem
- Time/space complexity considerations
- Real-world use cases where it's applied

After explaining, provide a simple pseudocode example (not full implementation, just the algorithm) to illustrate the concept."
```

**Why this works:**
- Specifies audience knowledge level
- Requests detailed explanation
- Lists specific focus areas
- Asks for practical example
- Requests pseudocode (not full implementation)
- Follows Explanation pattern comprehensively
- Good for quick learning before system design

---

## Key Takeaways

✅ **Match the pattern to the situation:** Each pattern solves a specific problem type
✅ **Implementation:** Use when building from scratch with clear requirements
✅ **Debugging:** Use when something isn't working as expected
✅ **Testing:** Use to verify code works across scenarios
✅ **Explanation:** Use to understand concepts before implementing
✅ **Optimization:** Use to improve working code
✅ **Refactoring:** Use to improve code quality/structure
✅ **Interview tip:** Identify which pattern you need BEFORE writing the prompt

</details>

---

## Variations / Extensions

Want more practice? Try these:

1. **Real interview code:** Apply these patterns to code you've written in practice
2. **Multiple patterns:** Sometimes you need more than one—try combining (e.g., explain then implement)
3. **Tool variations:** Write the same prompt for Claude Code, Copilot, and ChatGPT—notice differences
4. **Peer review:** Exchange prompts with a study partner, give feedback on clarity and completeness

---

[← Back to Chapter 2](../chapters/chapter-02-prompt-engineering.md)
