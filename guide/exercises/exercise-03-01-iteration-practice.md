# Exercise 3.1: Iteration Practice

**Difficulty:** 🟡 Intermediate
**Time:** 30-40 minutes
**Chapter:** Effective Iteration Workflows
**Skills:** Code review, giving feedback, systematic refinement, testing

---

## Problem Statement

You're in a mock interview and asked an AI assistant to solve a classic problem. The AI gave you a solution that *looks* reasonable at first glance, but has multiple issues. Your task is to systematically refine this solution through 3-4 iterations, demonstrating the skills from Chapter 3.

---

## Learning Goals

This exercise helps you practice:
- Systematically reviewing AI-generated code for correctness, edge cases, and quality
- Giving specific, actionable feedback to AI
- Applying the Generate → Review → Refine → Validate cycle
- Knowing when to refine vs. when to start over
- Managing iteration time effectively
- Recognizing when a solution is "good enough"

---

## The Scenario

**Interview Question:**
> "Implement a function that merges two sorted linked lists into one sorted linked list. The merge should be done in-place (reusing the original nodes, not creating new ones)."

**Your Initial Prompt to AI:**
```
"Implement a function in Java that merges two sorted singly linked lists
into one sorted linked list. Use the existing nodes, don't create new ones."
```

**AI's First Output (Iteration 1):**

```java
/**
 * Definition for singly-linked list.
 */
class ListNode {
    int val;
    ListNode next;

    ListNode(int val) {
        this.val = val;
    }
}

public class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode current = dummy;

        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                current.next = l1;
                l1 = l1.next;
            } else {
                current.next = l2;
                l2 = l2.next;
            }
            current = current.next;
        }

        current.next = l1;

        return dummy.next;
    }

    // Test helper: create linked list from array
    public static ListNode createList(int[] values) {
        if (values.length == 0) return null;
        ListNode head = new ListNode(values[0]);
        ListNode current = head;
        for (int i = 1; i < values.length; i++) {
            current.next = new ListNode(values[i]);
            current = current.next;
        }
        return head;
    }

    // Test helper: print linked list
    public static void printList(ListNode head) {
        while (head != null) {
            System.out.print(head.val + " -> ");
            head = head.next;
        }
        System.out.println("null");
    }

    public static void main(String[] args) {
        Solution solution = new Solution();

        // Test case 1: Both lists have elements
        ListNode l1 = createList(new int[]{1, 2, 4});
        ListNode l2 = createList(new int[]{1, 3, 4});
        ListNode result = solution.mergeTwoLists(l1, l2);
        printList(result);  // Expected: 1 -> 1 -> 2 -> 3 -> 4 -> 4 -> null

        // Test case 2: One list is empty
        l1 = createList(new int[]{});
        l2 = createList(new int[]{0});
        result = solution.mergeTwoLists(l1, l2);
        printList(result);  // Expected: 0 -> null
    }
}
```

---

## Your Task

### Part 1: Review (5-7 minutes)

Carefully review the AI's code and identify ALL issues. Consider:

1. **Correctness:** Does the logic work for all cases?
2. **Edge Cases:** What about null inputs? Empty lists?
3. **Completeness:** Is anything missing from the merge logic?
4. **Testing:** Are the test cases comprehensive?

**Document your findings:**

**Critical Issues:**
- [ ] Issue 1: _________________________
- [ ] Issue 2: _________________________

**Edge Case Issues:**
- [ ] Issue 3: _________________________
- [ ] Issue 4: _________________________

**Test Coverage Issues:**
- [ ] Issue 5: _________________________

---

### Part 2: Iteration 2 - Fix Critical Bug (5 minutes)

Write a specific refinement prompt to fix the most critical bug(s).

**Your Refinement Prompt:**
```
[Write your prompt here]
```

**Expected Fix:**
What should change in the code?

---

### Part 3: Iteration 3 - Handle Edge Cases (5 minutes)

After the critical bug is fixed, write a prompt to handle missing edge cases.

**Your Refinement Prompt:**
```
[Write your prompt here]
```

**Expected Fix:**
What should be added?

---

### Part 4: Iteration 4 - Improve Test Coverage (5 minutes)

Write a prompt to add comprehensive test cases.

**Your Refinement Prompt:**
```
[Write your prompt here]
```

**Expected Tests:**
What test cases should be added?

---

### Part 5: Validation (5 minutes)

Mentally trace or actually run the final solution through these test cases:

```java
// Test 1: Normal case
mergeTwoLists([1,2,4], [1,3,4]) → Expected: [1,1,2,3,4,4]

// Test 2: One empty list
mergeTwoLists([], [0]) → Expected: [0]

// Test 3: Both empty lists
mergeTwoLists([], []) → Expected: []

// Test 4: One null list
mergeTwoLists(null, [1,2]) → Expected: [1,2]

// Test 5: Both null lists
mergeTwoLists(null, null) → Expected: null

// Test 6: Different lengths
mergeTwoLists([1,2,3,4,5], [6]) → Expected: [1,2,3,4,5,6]

// Test 7: All elements from l1 smaller
mergeTwoLists([1,2,3], [4,5,6]) → Expected: [1,2,3,4,5,6]

// Test 8: Single elements
mergeTwoLists([2], [1]) → Expected: [1,2]
```

**Check:** Does your final solution pass all these tests?

---

## Hints

<details>
<summary>Hint 1: What's the critical bug?</summary>

Look at line 26:
```java
current.next = l1;
```

What if `l2` still has elements remaining? The code only attaches `l1`. This means any remaining elements in `l2` will be lost!

**The bug:** After the while loop, we need to attach whichever list still has elements (could be `l1` OR `l2`).

</details>

<details>
<summary>Hint 2: What edge cases are missing?</summary>

The current code doesn't handle:
1. **Null inputs:** What if `l1` or `l2` is null?
2. **Both empty:** The test case exists but uses `new int[]{}` which creates an empty array, but `createList` might not handle it correctly.

Check what happens when you pass null to `mergeTwoLists`.

</details>

<details>
<summary>Hint 3: What's wrong with the test helper?</summary>

In `createList`:
```java
if (values.length == 0) return null;
```

This is good! But what if `values` itself is null? This will throw `NullPointerException`.

Also, there's no null check in `printList`.

</details>

<details>
<summary>Hint 4: Refinement prompt structure</summary>

Use this structure:
```
"There's a bug on line [X]: [what's wrong]
Expected behavior: [correct behavior]
Fix by: [specific change]"
```

Example:
```
"Line 26 has a logic error: current.next = l1 only attaches the remaining
elements of l1, but l2 might also have remaining elements.

Expected behavior: After the while loop, attach whichever list (l1 or l2)
still has elements.

Fix by: Change line 26 to:
if (l1 != null) {
    current.next = l1;
} else {
    current.next = l2;
}"
```

Wait—can we simplify this even further?

</details>

<details>
<summary>Hint 5: Even simpler fix</summary>

Since exactly one of `l1` or `l2` (or both) will be null after the while loop, you can use:
```java
current.next = (l1 != null) ? l1 : l2;
```

Or even simpler, since if `l1` is not null, we want `l1`, otherwise we want `l2` (which could also be null):
```java
current.next = l1 != null ? l1 : l2;
```

Actually, the ternary is unnecessary here! If both are null, either assignment is fine (result is null). So we can just do:
```java
// Attach remaining elements (one or both lists may be null)
if (l1 != null) {
    current.next = l1;
} else {
    current.next = l2;
}
```

Or most elegantly:
```java
current.next = (l1 != null) ? l1 : l2;
```

</details>

---

## Solution Walkthrough

### Iteration 1: Review (Initial Analysis)

Let's systematically review the AI's code:

**Critical Issues Found:**

🔴 **Bug on line 26:** `current.next = l1;`
- **Problem:** Assumes l1 has remaining elements, but l2 might have remaining elements
- **Impact:** If l2 has elements left, they'll be lost
- **Example:** `mergeTwoLists([5], [1,2,3])` would return `[1,2,3,5]` but lose elements after 5

**Edge Case Issues Found:**

🟡 **No null checks for l1/l2 inputs**
- **Problem:** `mergeTwoLists(null, [1,2])` might cause issues
- **Impact:** Unclear behavior for null inputs

🟡 **createList helper doesn't handle null array**
- **Problem:** `createList(null)` will throw NullPointerException
- **Impact:** Can't test with null easily

**Test Coverage Issues:**

🟡 **Missing test cases:**
- Both lists null
- One list null
- Both lists empty
- Lists of very different lengths
- All elements from one list smaller than the other

---

### Iteration 2: Fix Critical Bug

**Refinement Prompt:**
```
"There's a critical bug on line 26. Currently:
    current.next = l1;

This only attaches remaining elements from l1, but l2 might also have
remaining elements after the while loop ends.

Fix: Change line 26 to attach whichever list still has elements:
    current.next = (l1 != null) ? l1 : l2;

This works because after the while loop, at least one of l1 or l2 is null,
and we want to attach whichever one isn't null."
```

**Expected Fixed Code (Iteration 2):**
```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;

    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }

    current.next = (l1 != null) ? l1 : l2;  // ✅ FIXED

    return dummy.next;
}
```

**Validation:**
```
mergeTwoLists([5], [1,2,3])
→ Should now return [1,2,3,5] ✅
```

---

### Iteration 3: Handle Edge Cases

**Refinement Prompt:**
```
"The merge logic is now correct, but missing edge case handling:

1. If l1 is null, the function should return l2
2. If l2 is null, the function should return l1
3. If both are null, return null

Add these checks at the very beginning of mergeTwoLists, before creating
the dummy node:

if (l1 == null) return l2;
if (l2 == null) return l1;

Also, fix the createList helper method:
- In createList, add null check: if (values == null || values.length == 0) return null;
"
```

**Expected Fixed Code (Iteration 3):**
```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    // Handle null inputs
    if (l1 == null) return l2;
    if (l2 == null) return l1;

    ListNode dummy = new ListNode(0);
    ListNode current = dummy;

    while (l1 != null && l2 != null) {
        if (l1.val < l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }

    current.next = (l1 != null) ? l1 : l2;

    return dummy.next;
}

public static ListNode createList(int[] values) {
    if (values == null || values.length == 0) return null;  // ✅ FIXED
    ListNode head = new ListNode(values[0]);
    ListNode current = head;
    for (int i = 1; i < values.length; i++) {
        current.next = new ListNode(values[i]);
        current = current.next;
    }
    return head;
}

public static void printList(ListNode head) {
    while (head != null) {
        System.out.print(head.val + " -> ");
        head = head.next;
    }
    System.out.println("null");
}
```

**Validation:**
```
mergeTwoLists(null, [1,2]) → [1,2] ✅
mergeTwoLists([1,2], null) → [1,2] ✅
mergeTwoLists(null, null) → null ✅
```

---

### Iteration 4: Comprehensive Testing

**Refinement Prompt:**
```
"Add comprehensive test cases to main() to cover all edge cases:

1. Both lists null
2. One list null (both directions)
3. Both lists empty
4. Very different lengths: [1,2,3,4,5] and [6]
5. All elements from l1 smaller than l2: [1,2,3] and [4,5,6]
6. All elements from l2 smaller than l1: [4,5,6] and [1,2,3]
7. Single element lists: [2] and [1]
8. Duplicate values: [1,3,5] and [1,3,5]

Use clear comments for each test case showing expected output."
```

**Expected Final Code (Iteration 4):**
```java
public static void main(String[] args) {
    Solution solution = new Solution();

    System.out.println("Test 1: Normal merge");
    ListNode l1 = createList(new int[]{1, 2, 4});
    ListNode l2 = createList(new int[]{1, 3, 4});
    printList(solution.mergeTwoLists(l1, l2));
    // Expected: 1 -> 1 -> 2 -> 3 -> 4 -> 4 -> null

    System.out.println("\nTest 2: One list empty");
    l1 = createList(new int[]{});
    l2 = createList(new int[]{0});
    printList(solution.mergeTwoLists(l1, l2));
    // Expected: 0 -> null

    System.out.println("\nTest 3: Both lists empty");
    l1 = createList(new int[]{});
    l2 = createList(new int[]{});
    printList(solution.mergeTwoLists(l1, l2));
    // Expected: null

    System.out.println("\nTest 4: First list null");
    l1 = null;
    l2 = createList(new int[]{1, 2});
    printList(solution.mergeTwoLists(l1, l2));
    // Expected: 1 -> 2 -> null

    System.out.println("\nTest 5: Second list null");
    l1 = createList(new int[]{1, 2});
    l2 = null;
    printList(solution.mergeTwoLists(l1, l2));
    // Expected: 1 -> 2 -> null

    System.out.println("\nTest 6: Both lists null");
    printList(solution.mergeTwoLists(null, null));
    // Expected: null

    System.out.println("\nTest 7: Different lengths");
    l1 = createList(new int[]{1, 2, 3, 4, 5});
    l2 = createList(new int[]{6});
    printList(solution.mergeTwoLists(l1, l2));
    // Expected: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> null

    System.out.println("\nTest 8: All l1 elements smaller");
    l1 = createList(new int[]{1, 2, 3});
    l2 = createList(new int[]{4, 5, 6});
    printList(solution.mergeTwoLists(l1, l2));
    // Expected: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> null

    System.out.println("\nTest 9: All l2 elements smaller");
    l1 = createList(new int[]{4, 5, 6});
    l2 = createList(new int[]{1, 2, 3});
    printList(solution.mergeTwoLists(l1, l2));
    // Expected: 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> null

    System.out.println("\nTest 10: Single elements");
    l1 = createList(new int[]{2});
    l2 = createList(new int[]{1});
    printList(solution.mergeTwoLists(l1, l2));
    // Expected: 1 -> 2 -> null

    System.out.println("\nTest 11: Duplicate values");
    l1 = createList(new int[]{1, 3, 5});
    l2 = createList(new int[]{1, 3, 5});
    printList(solution.mergeTwoLists(l1, l2));
    // Expected: 1 -> 1 -> 3 -> 3 -> 5 -> 5 -> null
}
```

---

### Final Validation

**Complete Solution (After All Iterations):**

```java
class ListNode {
    int val;
    ListNode next;

    ListNode(int val) {
        this.val = val;
    }
}

public class Solution {
    /**
     * Merges two sorted linked lists into one sorted linked list.
     *
     * @param l1 First sorted linked list (can be null)
     * @param l2 Second sorted linked list (can be null)
     * @return Merged sorted linked list
     *
     * Time Complexity: O(n + m) where n and m are lengths of l1 and l2
     * Space Complexity: O(1) - only uses a few pointers
     */
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        // Handle null inputs
        if (l1 == null) return l2;
        if (l2 == null) return l1;

        ListNode dummy = new ListNode(0);
        ListNode current = dummy;

        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                current.next = l1;
                l1 = l1.next;
            } else {
                current.next = l2;
                l2 = l2.next;
            }
            current = current.next;
        }

        // Attach remaining elements (exactly one of l1/l2 is null here)
        current.next = (l1 != null) ? l1 : l2;

        return dummy.next;
    }

    public static ListNode createList(int[] values) {
        if (values == null || values.length == 0) return null;
        ListNode head = new ListNode(values[0]);
        ListNode current = head;
        for (int i = 1; i < values.length; i++) {
            current.next = new ListNode(values[i]);
            current = current.next;
        }
        return head;
    }

    public static void printList(ListNode head) {
        while (head != null) {
            System.out.print(head.val + " -> ");
            head = head.next;
        }
        System.out.println("null");
    }

    public static void main(String[] args) {
        Solution solution = new Solution();

        System.out.println("Test 1: Normal merge");
        ListNode l1 = createList(new int[]{1, 2, 4});
        ListNode l2 = createList(new int[]{1, 3, 4});
        printList(solution.mergeTwoLists(l1, l2));

        System.out.println("\nTest 2: One list empty");
        l1 = createList(new int[]{});
        l2 = createList(new int[]{0});
        printList(solution.mergeTwoLists(l1, l2));

        System.out.println("\nTest 3: Both lists empty");
        l1 = createList(new int[]{});
        l2 = createList(new int[]{});
        printList(solution.mergeTwoLists(l1, l2));

        System.out.println("\nTest 4: First list null");
        l1 = null;
        l2 = createList(new int[]{1, 2});
        printList(solution.mergeTwoLists(l1, l2));

        System.out.println("\nTest 5: Second list null");
        l1 = createList(new int[]{1, 2});
        l2 = null;
        printList(solution.mergeTwoLists(l1, l2));

        System.out.println("\nTest 6: Both lists null");
        printList(solution.mergeTwoLists(null, null));

        System.out.println("\nTest 7: Different lengths");
        l1 = createList(new int[]{1, 2, 3, 4, 5});
        l2 = createList(new int[]{6});
        printList(solution.mergeTwoLists(l1, l2));

        System.out.println("\nTest 8: All l1 elements smaller");
        l1 = createList(new int[]{1, 2, 3});
        l2 = createList(new int[]{4, 5, 6});
        printList(solution.mergeTwoLists(l1, l2));

        System.out.println("\nTest 9: All l2 elements smaller");
        l1 = createList(new int[]{4, 5, 6});
        l2 = createList(new int[]{1, 2, 3});
        printList(solution.mergeTwoLists(l1, l2));

        System.out.println("\nTest 10: Single elements");
        l1 = createList(new int[]{2});
        l2 = createList(new int[]{1});
        printList(solution.mergeTwoLists(l1, l2));

        System.out.println("\nTest 11: Duplicate values");
        l1 = createList(new int[]{1, 3, 5});
        l2 = createList(new int[]{1, 3, 5});
        printList(solution.mergeTwoLists(l1, l2));
    }
}
```

**Summary of Iterations:**

| Iteration | Focus | Changes Made | Time |
|-----------|-------|--------------|------|
| 1 (Initial) | Generate solution | Initial working code | N/A |
| 2 | Fix critical bug | Fixed line 26 to handle both remaining lists | ~5 min |
| 3 | Handle edge cases | Added null checks to main function and helpers | ~5 min |
| 4 | Comprehensive testing | Added 11 test cases covering all scenarios | ~5 min |
| **Total** | | **Complete, production-ready solution** | **~15 min** |

---

## Key Takeaways

✅ **Systematic review catches issues:** Don't just accept AI output—review for correctness, edge cases, and completeness

✅ **Specific feedback gets results:** "Fix line 26 to attach whichever list remains" is better than "Fix the bug"

✅ **Prioritize iterations:** Critical bugs first, then edge cases, then testing

✅ **Small, focused changes:** Each iteration addressed one category of issues

✅ **Testing validates completeness:** Comprehensive test cases prove the solution works

✅ **Time management:** 3 iterations in ~15 minutes is reasonable for interviews

✅ **Know when you're done:** After iteration 4, the solution is complete—no need for more refinement

---

## Reflection Questions

1. **What would happen if you tried to fix everything in one iteration?**
   - Likely overwhelming prompt, unclear changes, new bugs introduced

2. **Why was the iteration order important (bug → edge cases → testing)?**
   - Must have correct core logic before handling edge cases
   - Can't write good tests if logic is broken

3. **When would you have started over instead of iterating?**
   - If the algorithm approach was fundamentally wrong (e.g., used sorting when not needed)
   - If after 3 iterations, bugs kept appearing

4. **How did specific feedback speed up refinement?**
   - AI knew exactly what to change (line 26)
   - No ambiguity, no back-and-forth

---

## Variations / Extensions

Want more practice? Try these variations:

### Variation 1: Different Problem
Apply the same iteration workflow to this problem:
```
"Implement a function that removes duplicates from a sorted linked list"
```

Generate initial solution, identify issues, iterate 3-4 times.

### Variation 2: Performance Optimization
Start with an O(n²) solution and iterate to optimize to O(n):
```
"Find two numbers in an array that sum to a target"
```

### Variation 3: Time-Constrained Practice
Set a timer for 20 minutes total:
- 5 min: Generate + Review
- 10 min: 2-3 iterations
- 5 min: Final validation

Can you complete a full iteration cycle in 20 minutes?

### Variation 4: Real AI Practice
Actually use an AI tool (Claude, ChatGPT, Copilot) for this exercise:
1. Send the initial prompt
2. Review the output
3. Send your refinement prompts
4. Compare the AI's responses to the expected fixes

Did the AI make the changes you expected?

---

## Additional Challenge: Start Over Scenario

**Challenge:** What if the AI had generated this solution instead?

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    List<Integer> values = new ArrayList<>();

    while (l1 != null) {
        values.add(l1.val);
        l1 = l1.next;
    }
    while (l2 != null) {
        values.add(l2.val);
        l2 = l2.next;
    }

    Collections.sort(values);

    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    for (int val : values) {
        current.next = new ListNode(val);
        current = current.next;
    }

    return dummy.next;
}
```

**Question:** Would you iterate to fix this, or start over? Why?

<details>
<summary>Answer</summary>

**Start over!**

**Why:**
- Violates the requirement: "Use existing nodes, don't create new ones"
- Creates O(n) extra space (ArrayList)
- Uses sorting which is O(n log n), not optimal
- Fundamental approach is wrong

**Better prompt:**
```
"Start over. The requirement is to merge in-place using existing nodes,
not create new nodes. Use a two-pointer approach to build the merged list
by reusing l1 and l2 nodes. Time: O(n+m), Space: O(1)."
```

This is a case where iterating would waste time—the approach is fundamentally wrong.

</details>

---

[← Back to Chapter 3](../chapters/chapter-03-iteration-workflows.md)
