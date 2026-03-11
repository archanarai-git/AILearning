# Exercise 2.1: Prompt Refinement

**Difficulty:** 🟢→🟡 Beginner to Intermediate
**Time:** 20 minutes
**Chapter:** Prompt Engineering Fundamentals
**Skills:** Prompt writing, critical thinking, identifying gaps

---

## Problem Statement

You're in an interview and need to ask an AI assistant to implement a function. Below are 5 vague prompts that candidates often write. Your task is to refine each one into a clear, specific, effective prompt that minimizes iteration.

---

## Learning Goals

This exercise helps you practice:
- Identifying what's missing from vague prompts
- Knowing what context and constraints to specify
- Writing prompts that get better results on the first try
- Communicating clearly with AI tools

---

## Your Task

For each of the 5 vague prompts below:

1. **Identify the problems:** What's missing or unclear?
2. **Refine the prompt:** Rewrite it with proper context, task, and format
3. **Explain your improvements:** Why is your version better?

Take 15-20 minutes to work through all 5. Don't look at the solutions until you've tried!

---

## The 5 Vague Prompts

### Prompt 1
```
"Write a function to find duplicates in a list"
```

**Your refined version:**

[Your answer here]

---

### Prompt 2
```
"Convert this to be faster"

public static int sum(int[] arr) {
    int total = 0;
    for (int i = 0; i < arr.length; i++) {
        total += arr[i];
    }
    return total;
}
```

**Your refined version:**

[Your answer here]

---

### Prompt 3
```
"Add error handling to this method"

public class UserService {
    public User getUserById(String userId) {
        User user = database.query("SELECT * FROM users WHERE id = ?", userId);
        return user;
    }
}
```

**Your refined version:**

[Your answer here]

---

### Prompt 4
```
"This doesn't work"

public static boolean isPerfectSquare(int num) {
    for (int i = 1; i * i <= num; i++) {
        if (i * i == num) {
            return true;
        }
    }
    return false;
}
```

**Your refined version:**

[Your answer here]

---

### Prompt 5
```
"Refactor this code"

public class DataProcessor {
    public void processData(String[] data) {
        int[] processed = new int[data.length];
        for (int i = 0; i < data.length; i++) {
            processed[i] = Integer.parseInt(data[i]);
        }
        int s = 0;
        for (int i = 0; i < processed.length; i++) {
            s += processed[i];
        }
        int m = s / processed.length;
        System.out.println("Average: " + m);
    }
}
```

**Your refined version:**

[Your answer here]

---

## Hints

<details>
<summary>Hint 1: Think about missing details</summary>

For each vague prompt, ask:
- What language and version?
- What should the output look like?
- What constraints or edge cases matter?
- Why am I asking for this change?

</details>

<details>
<summary>Hint 2: Pattern matching</summary>

Refer back to the 6 prompt patterns from Chapter 2:
- Implementation: What should the function do?
- Debugging: What's the actual vs. expected behavior?
- Optimization: What's the current problem and target?
- Testing: What scenarios matter?
- Explanation: What's your audience?
- Refactoring: What specific issues need fixing?

</details>

<details>
<summary>Hint 3: Example structure</summary>

Good prompts usually include:
- **Context:** Relevant background or existing code
- **Task:** Specific action you want (implement, debug, etc.)
- **Format:** Language, style, requirements
- **Edge cases:** Special scenarios to handle

</details>

---

## Evaluation Criteria

Your refined prompts should:
- ✅ Be specific and clear (AI understands first time)
- ✅ Include necessary context (language, requirements, constraints)
- ✅ Avoid over-specifying (give AI room to optimize)
- ✅ State edge cases or special requirements
- ✅ Follow one of the 6 prompt patterns from Chapter 2

---

## Sample Solutions

<details>
<summary>Click to reveal solutions</summary>

### Prompt 1 Solution

**Original:** "Write a function to find duplicates in a list"

**Problems:**
- What language?
- What type of list? (integers, strings, objects?)
- Return format? (list of duplicates, count, indices?)
- Should it find first duplicate only or all?
- Handle null/empty inputs?

**Refined:**
```
"Implement a Java method that finds all duplicate integers in a List<Integer>.
Requirements:
- Input: List<Integer> (can be null or empty)
- Output: List<Integer> containing unique duplicate values (e.g., [3, 5] if 3 and 5 appear more than once)
- Edge cases: Handle null input (return empty list), empty list (return empty list), single element (return empty list)
- Constraints: Optimize for space—use a Set approach rather than nested loops
- Include Javadoc with complexity analysis"
```

**Why it's better:**
- Specifies Java and data structures
- Clear input/output format
- Defines what "duplicates" means
- Handles edge cases explicitly
- Suggests an approach (Set) without over-constraining
- Requests documentation

---

### Prompt 2 Solution

**Original:** "Convert this to be faster" + code

**Problems:**
- What metric? (Time, memory, readability?)
- Current performance unknown
- No target performance specified
- "Faster" is subjective

**Refined:**
```
"Optimize this Java method for time complexity.
Current performance: Takes 2+ seconds for an array of 1,000,000 integers
Target: Should complete in under 100ms
Constraints: Cannot use external libraries, must maintain same return type

public static int sum(int[] arr) {
    int total = 0;
    for (int i = 0; i < arr.length; i++) {
        total += arr[i];
    }
    return total;
}

Note: The current implementation is already O(n). If optimization is impossible, explain why and suggest alternative approaches for different scenarios (e.g., if data is immutable)."
```

**Why it's better:**
- Specifies the metric (time complexity)
- Provides concrete performance targets
- States constraints (no external libraries)
- Acknowledges this code might already be optimal
- Asks for explanation if optimization isn't possible

---

### Prompt 3 Solution

**Original:** "Add error handling to this method"

**Problems:**
- Which errors? (null input, database errors, parsing errors?)
- What kind of error handling? (try-catch, return null, throw exceptions?)
- How should errors be logged or communicated?

**Refined:**
```
"Add comprehensive error handling to this Java method:

public class UserService {
    public User getUserById(String userId) {
        User user = database.query("SELECT * FROM users WHERE id = ?", userId);
        return user;
    }
}

Specific error cases to handle:
- userId is null or empty → throw IllegalArgumentException with message
- database.query() throws SQLException → catch it, log error, return null
- user is null (not found in database) → return null (expected behavior)
- Any other unexpected exceptions → log and re-throw as RuntimeException

Requirements:
- Use try-catch blocks
- Add logging (assume a logger is available)
- Include Javadoc describing thrown exceptions
- Preserve the existing method signature where possible"
```

**Why it's better:**
- Specifies which exceptions matter
- States handling strategy for each (throw, log, return)
- Provides context (logger available)
- Explains expected behavior (null user)
- Clarifies signature preservation

---

### Prompt 4 Solution

**Original:** "This doesn't work" + code

**Problems:**
- No indication of what's wrong
- No error message provided
- Expected behavior unclear

**Refined:**
```
"Debug this Java method. It should return true for perfect squares and false otherwise.

Problem: It throws a StackOverflowError for large inputs like isPerfectSquare(1000000000)
Expected: Should return true/false without error
Failure case: isPerfectSquare(1000000000) should return false (1000000000 is not a perfect square)

Current code:
public static boolean isPerfectSquare(int num) {
    for (int i = 1; i * i <= num; i++) {
        if (i * i == num) {
            return true;
        }
    }
    return false;
}

Once fixed, provide the corrected method and explain what went wrong."
```

**Why it's better:**
- States expected vs. actual behavior
- Provides specific failing input
- Gives error type (StackOverflowError)
- Asks for explanation of the bug
- Clear definition of success

---

### Prompt 5 Solution

**Original:** "Refactor this code"

**Problems:**
- What aspects to refactor? (readability, performance, structure?)
- What code style to follow?
- Can behavior change?
- No guidance on what's most important

**Refined:**
```
"Refactor this Java class to improve readability and follow Google Java Style Guide.

Current issues to address:
- Method does multiple things (parsing, calculating, printing)—extract into separate methods
- Variable names are unclear (s = sum, m = mean)
- No Javadoc or inline comments
- Magic numbers without explanation
- System.out.println instead of proper logging
- No error handling for invalid input

Requirements:
- Preserve the original behavior (calculate average and output)
- Extract methods: parseData(), calculateAverage(), displayResult()
- Use descriptive variable names
- Add Javadoc for the class and methods
- Include error handling for non-numeric input
- Use a logger instead of System.out.println (assume SLF4J available)

public class DataProcessor {
    public void processData(String[] data) {
        int[] processed = new int[data.length];
        for (int i = 0; i < data.length; i++) {
            processed[i] = Integer.parseInt(data[i]);
        }
        int s = 0;
        for (int i = 0; i < processed.length; i++) {
            s += processed[i];
        }
        int m = s / processed.length;
        System.out.println("Average: " + m);
    }
}"
```

**Why it's better:**
- Specifies exact issues to address
- States constraints (preserve behavior)
- Provides clear improvement areas
- Mentions style guide
- Lists technical details (logger library)
- Includes the code for reference

---

## Key Takeaways

✅ **Be specific:** Vague prompts lead to iterations. Clear prompts get results fast.
✅ **Include context:** Type definitions, existing code, constraints all matter.
✅ **Define success:** State expected behavior or outcome clearly.
✅ **Handle edge cases:** Mention special scenarios upfront.
✅ **Match the pattern:** Use Implementation, Debugging, Optimization, etc. patterns.
✅ **Ask for explanations:** AI should justify its approach, not just code.

</details>

---

## Variations / Extensions

Want more practice? Try these:

1. **Create your own vague prompts** and refine them using the same process
2. **Find real code** you've written and write refinement prompts for improvements
3. **Compare your versions** with team members—discuss which is most effective
4. **Test with AI:** Actually send a vague prompt and your refined version to an AI tool, compare outputs

---

[← Back to Chapter 2](../chapters/chapter-02-prompt-engineering.md)
