# Chapter 2: Prompt Engineering Fundamentals

**Difficulty:** 🟢→🟡 Beginner to Intermediate
**Time:** 2-3 hours
**Goal:** Master the art of crafting effective prompts that get you the code you need

---

## Learning Objectives

By the end of this chapter, you will be able to:
- [ ] Write clear, specific prompts that minimize iteration
- [ ] Use prompt patterns for common tasks (implement, debug, optimize, test)
- [ ] Balance specificity (not too vague, not too over-specified)
- [ ] Manage context effectively in prompts
- [ ] Iterate on prompts when initial results aren't satisfactory

---

## Introduction

Prompt engineering is the highest-leverage skill in AI-assisted development. A great prompt gets you 90% of the way to the solution in one shot. A poor prompt leads to endless iterations, wasted time, and frustration.

In an interview setting, prompt quality directly affects:
- ⏱️ **Speed:** Good prompts = less iteration time
- 🧠 **Mental load:** Clear requests = less debugging
- 💬 **Communication:** Well-framed prompts show clear thinking
- ✅ **Results:** Specific prompts = better code quality

This chapter teaches you the fundamentals of prompt engineering that work across all AI tools—Claude Code, Copilot, ChatGPT, Cursor, and beyond.

---

## Anatomy of an Effective Prompt

Every great prompt has three components:

```
┌───────────────────────────────────────────────────────┐
│  1. CONTEXT: What AI needs to know                   │
│     - Problem domain                                   │
│     - Constraints and requirements                     │
│     - Relevant background information                  │
├───────────────────────────────────────────────────────┤
│  2. TASK: What you want AI to do                     │
│     - Specific action (implement, debug, optimize)    │
│     - Clear description of desired outcome            │
│     - Success criteria                                 │
├───────────────────────────────────────────────────────┤
│  3. FORMAT: How you want the output                  │
│     - Language and framework                           │
│     - Code style preferences                           │
│     - Documentation requirements                       │
└───────────────────────────────────────────────────────┘
```

### Example: Bad vs. Good Prompts

**❌ BAD (Too Vague):**
```
"Write a function to sort a list"
```

Problems:
- What language?
- What kind of list (integers, strings, objects)?
- What algorithm (quicksort, mergesort, built-in)?
- Any constraints (in-place, stable, complexity)?

**✅ GOOD (Clear & Specific):**
```
"Implement a function in Java that sorts a list of integers
in ascending order using the merge sort algorithm. The function
should handle empty lists and lists with one element. Include
Javadoc with time/space complexity."
```

Result: AI knows exactly what you want.

---

## The Specificity Spectrum

Finding the right level of specificity is an art:

```
Too Vague ←──────────────────────────────────→ Too Specific

"Make it work"     "Fix the bug"     "Implement      "Write this
                   "Add validation"   function with    exact code
                                      these specs"     [500 lines]"

    ❌              ❌→⚠️              ✅              ⚠️→❌
```

### Too Vague
- "Build an API"
- "Fix this"
- "Make it faster"
- AI has too many interpretations

### Too Specific
- Providing the exact code you want
- Over-constraining the approach
- Specifying every variable name
- AI has no room for optimization

### Just Right ✅
- Clear requirements
- Key constraints specified
- Desired outcome defined
- Flexibility for AI to optimize

### Finding the Balance

**Ask yourself:**
- Do I have requirements? → State them
- Do I have constraints? → Specify them
- Do I care about the approach? → Guide it
- Am I unsure of details? → Let AI decide

---

## Prompt Patterns for Common Tasks

Master these patterns and adapt them to your needs.

### Pattern 1: Implementation

**Template:**
```
"Implement a [function/class/module] that [does X] with the following requirements:
- Input: [description]
- Output: [description]
- Constraints: [list constraints]
- Edge cases: [list edge cases to handle]
Use [language] and [any specific libraries/frameworks]."
```

**Example:**
```
"Implement a Java function that validates email addresses with the following requirements:
- Input: String (potential email)
- Output: boolean (true if valid, false otherwise)
- Constraints: Must follow RFC 5322 basic validation (user@domain.tld)
- Edge cases: Handle null strings, missing @ symbol, invalid domains
Use Java and avoid regex if possible."
```

### Pattern 2: Debugging

**Template:**
```
"Debug this [language] code that [intended behavior].
Current behavior: [what's happening]
Expected behavior: [what should happen]
Error message (if any): [error]

[CODE BLOCK]
"
```

**Example:**
```
"Debug this Java code that should calculate factorial recursively.
Current behavior: Throws StackOverflowError for input 1000
Expected behavior: Should handle large numbers efficiently
Error message: java.lang.StackOverflowError

public class Factorial {
    public static long factorial(int n) {
        if (n == 0) {
            return 1;
        }
        return n * factorial(n - 1);
    }
}
"
```

### Pattern 3: Optimization

**Template:**
```
"Optimize this [language] code for [metric: speed/memory/readability].
Current performance: [current stats if known]
Target: [desired improvement]
Constraints: [any limitations]

[CODE BLOCK]
"
```

**Example:**
```
"Optimize this Java code for speed.
Current performance: Takes 5 seconds for input size 10,000
Target: Should run in under 1 second
Constraints: Cannot use external libraries

public static List<Integer> findDuplicates(List<Integer> list) {
    List<Integer> duplicates = new ArrayList<>();
    for (int i = 0; i < list.size(); i++) {
        for (int j = i + 1; j < list.size(); j++) {
            if (list.get(i).equals(list.get(j)) && !duplicates.contains(list.get(i))) {
                duplicates.add(list.get(i));
            }
        }
    }
    return duplicates;
}
"
```

### Pattern 4: Testing

**Template:**
```
"Write [type] tests for this [language] [function/class]:
- Test framework: [JUnit/TestNG/etc.]
- Coverage: [list scenarios to test]
- Edge cases: [list edge cases]

[CODE TO TEST]
"
```

**Example:**
```
"Write unit tests for this Java method:
- Test framework: JUnit 5
- Coverage: Normal cases, empty list, single element, all duplicates
- Edge cases: null values, lists with one duplicate repeated multiple times

public static Integer findMostFrequent(List<Integer> list) {
    if (list == null || list.isEmpty()) {
        return null;
    }
    Map<Integer, Integer> counts = new HashMap<>();
    for (Integer item : list) {
        counts.put(item, counts.getOrDefault(item, 0) + 1);
    }
    return Collections.max(counts.entrySet(), Map.Entry.comparingByValue()).getKey();
}
"
```

### Pattern 5: Explanation

**Template:**
```
"Explain how this [language] code works, focusing on [specific aspect].
Audience: [who needs to understand this]
Detail level: [high-level/detailed/line-by-line]

[CODE BLOCK]
"
```

**Example:**
```
"Explain how this Java code works, focusing on the algorithm and complexity.
Audience: Someone familiar with programming but not this specific algorithm
Detail level: Detailed explanation with complexity analysis

public class BinarySearch {
    public static int binarySearch(int[] arr, int target) {
        int left = 0, right = arr.length - 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return -1;
    }
}
"
```

### Pattern 6: Refactoring

**Template:**
```
"Refactor this [language] code to improve [quality aspect].
Issues to address: [list specific problems]
Constraints: [preserve behavior, API compatibility, etc.]

[CODE BLOCK]
"
```

**Example:**
```
"Refactor this Java code to improve readability and follow Google Java Style Guide.
Issues to address: Long method, unclear variable names, no Javadoc
Constraints: Must preserve existing method signature and behavior

public class DataProcessor {
    public List<Integer> p(List<Integer> l, int t) {
        List<Integer> r = new ArrayList<>();
        for (int i = 0; i < l.size(); i++) {
            if (l.get(i) > t) {
                r.add(l.get(i));
            }
        }
        return r;
    }
}
"
```

---

## Context Management

How much context should you include in a prompt?

### The Context Triad

```
┌─────────────────────────────────────────────┐
│  ESSENTIAL CONTEXT                          │
│  - What AI absolutely needs to know         │
│  - Requirements and constraints             │
│  - Relevant code/data structures            │
├─────────────────────────────────────────────┤
│  HELPFUL CONTEXT                            │
│  - Background information                    │
│  - Why you're doing this                     │
│  - Preferred approaches                      │
├─────────────────────────────────────────────┤
│  NOISE                                      │
│  - Unrelated information                     │
│  - Over-explaining obvious things           │
│  - Entire codebase when you need one method │
└─────────────────────────────────────────────┘
```

### Include:
✅ Type definitions AI needs to know
✅ Method signatures it should match
✅ Constraints from requirements
✅ Error messages for debugging
✅ Relevant code snippets (< 50 lines)

### Exclude:
❌ Entire files when you need one method
❌ Unrelated code
❌ Redundant explanations
❌ Your thought process (unless asking for advice)

### Example: Right Amount of Context

**Scenario:** You need to add a method to an existing class.

**Too Little Context:**
```
"Add a method to calculate total price"
```

**Too Much Context:**
```
"Here's my entire 500-line e-commerce system [paste all code].
I need to add a method to calculate total price considering:
- Base price
- Quantity
- Discount
- Tax
- Shipping
[More paragraphs of explanation...]"
```

**Just Right:**
```
"Add a method `calculateTotal()` to this Order class that returns total price:

public class Order {
    private List<OrderItem> items;
    private double discount;  // Decimal, e.g., 0.1 for 10%
    private static final double TAX_RATE = 0.08;

    public Order(List<OrderItem> items, double discount) {
        this.items = items;
        this.discount = discount;
    }

    // Add calculateTotal() here
}

Requirements:
- Sum item prices (item.getPrice() * item.getQuantity())
- Apply discount to subtotal
- Add tax to discounted subtotal
- Return double rounded to 2 decimals"
```

---

## Multi-Turn Conversations vs. Single Prompts

When should you iterate with AI vs. crafting one perfect prompt?

### Single Prompt Strategy ✅

**When to use:**
- Clear requirements
- Standard implementations
- Time is critical (interviews!)
- You know exactly what you want

**Benefits:**
- Faster
- Less back-and-forth
- Shows clear thinking to interviewer

**Example:**
```
"Implement a binary search function in Java with type hints,
Javadoc, and handling for empty arrays. Include 3 test cases."
```
Result: Complete solution in one shot.

### Multi-Turn Strategy ✅

**When to use:**
- Exploring different approaches
- Unclear requirements
- Complex problems
- Learning new concepts

**Benefits:**
- Iterative refinement
- Can course-correct
- Explore multiple solutions

**Example:**
```
Turn 1: "What are good approaches for implementing a LRU cache?"
Turn 2: "Implement the hash map + doubly linked list approach in Java"
Turn 3: "Add a maxSize parameter and eviction logic"
Turn 4: "Optimize the get() operation"
```

### Interview Tip

**In timed interviews:** Favor single, well-crafted prompts.
**In take-homes:** Multi-turn is fine for exploration.

---

## Iterating on Prompts

What if your first prompt doesn't give you what you want?

### The Iteration Cycle

```
Initial Prompt → Review Output → Identify Gap → Refine Prompt → Repeat
```

### Common Reasons to Iterate

**1. Output is correct but not what you wanted**
- Refine requirements
- Add constraints
- Specify format

**2. Output has bugs**
- Point out specific issues
- Provide test cases that fail
- Request targeted fixes

**3. Output is inefficient**
- Request optimization
- Specify performance targets
- Suggest better approach

**4. Output is unclear/unreadable**
- Request refactoring
- Specify style preferences
- Ask for documentation

### Example: Prompt Refinement

**Attempt 1:**
```
Prompt: "Write a function to check if a string is a palindrome"

Output:
public static boolean isPalindrome(String s) {
    return s.equals(new StringBuilder(s).reverse().toString());
}
```

**Problem:** Doesn't handle case sensitivity or spaces.

**Attempt 2:**
```
Prompt: "Write a function to check if a string is a palindrome.
Ignore case and spaces."

Output:
public static boolean isPalindrome(String s) {
    s = s.toLowerCase().replaceAll(" ", "");
    return s.equals(new StringBuilder(s).reverse().toString());
}
```

**Better!** But still missing punctuation handling.

**Attempt 3:**
```
Prompt: "Write a function to check if a string is a palindrome.
Ignore case, spaces, and punctuation. Keep only alphanumeric characters."

Output:
public static boolean isPalindrome(String s) {
    String cleaned = s.toLowerCase().replaceAll("[^a-z0-9]", "");
    return cleaned.equals(new StringBuilder(cleaned).reverse().toString());
}
```

✅ **Perfect!**

### When to Start Over

Sometimes it's faster to start fresh than to iterate:
- After 3+ failed iterations
- When AI is confused by conversation history
- When you realize your approach was wrong
- When AI keeps making the same mistake

---

## AI-Agnostic Prompting Principles

These work across Claude Code, Copilot, ChatGPT, Cursor, and more:

### Universal Principles

**1. Be Specific** 🎯
- State what you want clearly
- Include constraints and requirements
- Specify language and framework

**2. Provide Context** 📚
- Essential information only
- Relevant code snippets
- Type definitions

**3. Show Examples** 📖
- Input/output examples
- Edge cases
- Expected behavior

**4. Request Explanation** 💭
- Ask AI to explain its approach
- Request complexity analysis
- Have it identify edge cases

**5. Validate Everything** ✅
- Review all output
- Test edge cases
- Verify correctness

### Tool-Specific Nuances

While principles are universal, each tool has quirks:

**Claude Code (CLI):**
- Has full codebase context
- Can make file edits directly
- Good at multi-file changes

**GitHub Copilot:**
- Inline autocomplete
- Context from current file
- Great for completing patterns

**ChatGPT/Claude (Web):**
- No codebase context
- Need to provide all code
- Good for conceptual help

**Cursor:**
- IDE integration
- Can reference multiple files
- Good for refactoring

**Adapt your prompts to the tool's strengths.**

---

## Common Prompt Anti-Patterns

Avoid these mistakes:

### ❌ Anti-Pattern 1: The Vague Request
```
"Fix this code"
```
**Problem:** AI doesn't know what's broken

✅ **Better:**
```
"This code throws an NullPointerException on line 5. Fix it and handle edge case of null input."
```

### ❌ Anti-Pattern 2: The Novel
```
[Three paragraphs of backstory about your project]
"...so anyway, can you write a function to parse JSON?"
```
**Problem:** Too much noise, buried request

✅ **Better:**
```
"Write a Java function to parse JSON from a file. Handle FileNotFoundException and ParseException."
```

### ❌ Anti-Pattern 3: The Mind Reader
```
"Make it better"
```
**Problem:** AI doesn't know what "better" means

✅ **Better:**
```
"Optimize this for readability. Extract magic numbers to constants and add Javadoc."
```

### ❌ Anti-Pattern 4: The Copy-Paste
```
[Pastes 500 lines of code]
"What does this do?"
```
**Problem:** Too much code, no focus

✅ **Better:**
```
"Explain this specific algorithm [20 lines] focusing on the caching strategy."
```

### ❌ Anti-Pattern 5: The Assumption
```
"Add error handling"
```
**Problem:** Doesn't specify which errors or how to handle

✅ **Better:**
```
"Add try-catch blocks to handle NullPointerException and IllegalArgumentException. Log errors and return null on failure."
```

---

## Practical Application

### Exercise 2.1: Prompt Refinement

**Difficulty:** 🟢→🟡 Beginner to Intermediate
**Time:** 20 minutes
**Goal:** Practice refining vague prompts into specific, effective ones

See [Exercise 2.1](../exercises/exercise-02-01-prompt-refinement.md)

### Exercise 2.2: Prompt Patterns

**Difficulty:** 🟡 Intermediate
**Time:** 30 minutes
**Goal:** Apply prompt patterns to real scenarios

See [Exercise 2.2](../exercises/exercise-02-02-prompt-patterns.md)

---

## Key Takeaways

✅ Effective prompts have three parts: context, task, format
✅ Balance specificity—not too vague, not too over-constrained
✅ Master common patterns: implement, debug, optimize, test, explain, refactor
✅ Include essential context only, avoid noise
✅ Favor single prompts in interviews, iterate when exploring
✅ These principles work across all AI tools
✅ Always review and validate AI output

---

## Self-Assessment

Rate your understanding (1-5):
- [ ] I can write specific, clear prompts that minimize iteration
- [ ] I can use prompt patterns for common tasks
- [ ] I know when to provide more vs. less context
- [ ] I can iterate effectively when prompts don't work

**If you rated 4+ on all:** Move to Chapter 3
**If any 3 or below:** Complete additional exercises, review patterns

---

## Additional Resources

- [Prompt Engineering Guide](https://www.promptingguide.ai/)
- [Claude Prompt Library](https://docs.anthropic.com/claude/prompt-library)
- [OpenAI Prompt Examples](https://platform.openai.com/examples)

---

## Next Steps

- ✅ Complete Exercises 2.1 and 2.2
- ✅ Build your personal prompt library for common tasks
- ✅ Practice with your preferred AI tool
- ✅ Move to [Chapter 3: Effective Iteration Workflows](chapter-03-iteration-workflows.md)

---

[← Previous Chapter](chapter-01-introduction.md) | [↑ Part 1 Overview](../PART1-FOUNDATIONS.md) | [Next Chapter →](chapter-03-iteration-workflows.md)
