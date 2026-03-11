# Exercise 4.1: Code Review Practice

**Difficulty:** 🟡 Intermediate
**Time:** 30-40 minutes
**Chapter:** Code Validation & Review
**Skills:** Systematic code review, bug detection, edge case analysis, security awareness, explaining code

---

## Problem Statement

You're in a mock interview where the AI assistant has generated solutions to four different problems. Each solution has multiple issues across different categories: correctness, edge cases, efficiency, readability, or security.

Your task is to review each solution using the systematic review checklist from Chapter 4, document all issues, and explain how to fix them.

---

## Learning Goals

This exercise helps you practice:
- Applying the 30-second review checklist systematically
- Identifying different types of issues (bugs, edge cases, security, efficiency)
- Prioritizing issues (critical vs. minor)
- Providing specific, actionable feedback
- Explaining code clearly as if to an interviewer

---

## Instructions

For each code sample below:

1. **Apply the Review Checklist** (30 seconds per sample)
   - Correctness: Logic bugs?
   - Edge Cases: Null, empty, boundaries?
   - Efficiency: Time/space complexity optimal?
   - Readability: Clear names, structure?
   - Security: Vulnerabilities?

2. **Document Issues** (1 minute per sample)
   - List all issues found
   - Mark severity: 🔴 Critical, 🟡 Minor, 🟢 Good

3. **Write Fix** (2 minutes per sample)
   - Provide corrected code
   - Explain what changed and why

4. **Explain to "Interviewer"** (1 minute per sample)
   - Practice verbal explanation
   - Use the framework: Summary → Algorithm → Complexity → Edge Cases

---

## Sample 1: Find Missing Number

**Problem:** Given an array containing n distinct numbers taken from 0 to n, find the missing number.

**Example:** `[3, 0, 1]` → `2` (missing number is 2)

**AI-Generated Code:**

```java
public class Solution {
    public static int missingNumber(int[] nums) {
        int n = nums.length;
        int expectedSum = (n * (n + 1)) / 2;
        int actualSum = 0;

        for (int num : nums) {
            actualSum += num;
        }

        return expectedSum - actualSum;
    }

    public static void main(String[] args) {
        System.out.println(missingNumber(new int[]{3, 0, 1}));  // Expected: 2
        System.out.println(missingNumber(new int[]{0, 1}));     // Expected: 2
        System.out.println(missingNumber(new int[]{9,6,4,2,3,5,7,0,1}));  // Expected: 8
    }
}
```

### Your Review

**Correctness Check:**
- [ ] Issue found: _______________

**Edge Cases Check:**
- [ ] Issue found: _______________
- [ ] Issue found: _______________

**Efficiency Check:**
- [ ] Issue found: _______________

**Readability Check:**
- [ ] Issue found: _______________

**Security Check:**
- [ ] Issue found: _______________

### Your Fixed Code

```java
// Write your corrected version here
```

### Your Explanation (Practice Out Loud)

> [Write what you would say to an interviewer explaining this code]

---

## Sample 2: Validate Email Address

**Problem:** Validate if a string is a valid email address (basic validation: must have @ symbol with text before and after, and domain must have a dot).

**AI-Generated Code:**

```java
public class EmailValidator {
    public static boolean isValidEmail(String email) {
        int atIndex = email.indexOf('@');
        int dotIndex = email.indexOf('.');

        if (atIndex > 0 && dotIndex > atIndex + 1 && dotIndex < email.length() - 1) {
            return true;
        }

        return false;
    }

    public static void main(String[] args) {
        System.out.println(isValidEmail("test@example.com"));  // Expected: true
        System.out.println(isValidEmail("invalid.email"));     // Expected: false
        System.out.println(isValidEmail("@example.com"));      // Expected: false
    }
}
```

### Your Review

**Correctness Check:**
- [ ] Issue found: _______________

**Edge Cases Check:**
- [ ] Issue found: _______________
- [ ] Issue found: _______________
- [ ] Issue found: _______________

**Efficiency Check:**
- [ ] Issue found: _______________

**Readability Check:**
- [ ] Issue found: _______________

**Security Check:**
- [ ] Issue found: _______________

### Your Fixed Code

```java
// Write your corrected version here
```

### Your Explanation

> [Write what you would say to an interviewer]

---

## Sample 3: SQL Query Builder

**Problem:** Build a simple SQL query to find users by username.

**AI-Generated Code:**

```java
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class UserDatabase {
    private Connection connection;

    public UserDatabase(Connection connection) {
        this.connection = connection;
    }

    public List<String> findUsersByName(String name) throws SQLException {
        String query = "SELECT username FROM users WHERE username = '" + name + "'";
        Statement stmt = connection.createStatement();
        ResultSet rs = stmt.executeQuery(query);

        List<String> users = new ArrayList<>();
        while (rs.next()) {
            users.add(rs.getString("username"));
        }

        return users;
    }

    public static void main(String[] args) throws SQLException {
        // Test code (would need actual database connection)
        // UserDatabase db = new UserDatabase(connection);
        // List<String> users = db.findUsersByName("alice");
        // System.out.println(users);
    }
}
```

### Your Review

**Correctness Check:**
- [ ] Issue found: _______________

**Edge Cases Check:**
- [ ] Issue found: _______________

**Efficiency Check:**
- [ ] Issue found: _______________

**Readability Check:**
- [ ] Issue found: _______________

**Security Check:**
- [ ] Issue found: _______________
- [ ] Issue found: _______________

### Your Fixed Code

```java
// Write your corrected version here
```

### Your Explanation

> [Write what you would say to an interviewer, especially about the security fix]

---

## Sample 4: Merge Intervals

**Problem:** Given an array of intervals, merge overlapping intervals.

**Example:** `[[1,3],[2,6],[8,10],[15,18]]` → `[[1,6],[8,10],[15,18]]`

**AI-Generated Code:**

```java
import java.util.*;

public class Solution {
    public static int[][] merge(int[][] intervals) {
        List<int[]> result = new ArrayList<>();

        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

        int[] current = intervals[0];
        result.add(current);

        for (int i = 1; i < intervals.length; i++) {
            int[] interval = intervals[i];

            if (interval[0] <= current[1]) {
                current[1] = Math.max(current[1], interval[1]);
            } else {
                current = interval;
                result.add(current);
            }
        }

        return result.toArray(new int[result.size()][]);
    }

    public static void main(String[] args) {
        int[][] intervals1 = {{1,3},{2,6},{8,10},{15,18}};
        int[][] result1 = merge(intervals1);
        System.out.println(Arrays.deepToString(result1));  // Expected: [[1,6],[8,10],[15,18]]

        int[][] intervals2 = {{1,4},{4,5}};
        int[][] result2 = merge(intervals2);
        System.out.println(Arrays.deepToString(result2));  // Expected: [[1,5]]
    }
}
```

### Your Review

**Correctness Check:**
- [ ] Issue found: _______________

**Edge Cases Check:**
- [ ] Issue found: _______________
- [ ] Issue found: _______________

**Efficiency Check:**
- [ ] Issue found: _______________

**Readability Check:**
- [ ] Issue found: _______________

**Security Check:**
- [ ] Issue found: _______________

### Your Fixed Code

```java
// Write your corrected version here
```

### Your Explanation

> [Write what you would say to an interviewer]

---

## Hints

<details>
<summary>Hint 1: Sample 1 - Integer Overflow</summary>

Look at this line:
```java
int expectedSum = (n * (n + 1)) / 2;
```

What happens if `n` is very large, say 100,000?
`n * (n + 1)` could overflow before the division happens!

**Fix:** Use `long` for intermediate calculation or use the XOR approach.

</details>

<details>
<summary>Hint 2: Sample 1 - Edge Cases</summary>

What happens if:
- `nums` is null? → NullPointerException
- `nums` is empty (`[]`)? → Works correctly (returns 0)

Don't forget null check!

</details>

<details>
<summary>Hint 3: Sample 2 - Edge Cases</summary>

Consider these test cases:
- `null` → NullPointerException on line 3
- `""` (empty string) → Returns false (handled correctly by conditions)
- `"test@domain.co.uk"` → Finds first dot after @, but might not validate correctly
- `"test@@example.com"` → Multiple @ symbols (should be invalid)
- `"test @example.com"` → Space before @ (should be invalid)

The validation is too simplistic!

</details>

<details>
<summary>Hint 4: Sample 3 - SQL Injection</summary>

🔴🔴🔴 **CRITICAL SECURITY VULNERABILITY**

Line 13:
```java
String query = "SELECT username FROM users WHERE username = '" + name + "'";
```

What if `name = "'; DROP TABLE users; --"`?

Resulting query:
```sql
SELECT username FROM users WHERE username = ''; DROP TABLE users; --'
```

This would **delete the entire users table**!

**Fix:** Use `PreparedStatement` with parameterized queries.

</details>

<details>
<summary>Hint 5: Sample 3 - Resource Management</summary>

Lines 14-15:
```java
Statement stmt = connection.createStatement();
ResultSet rs = stmt.executeQuery(query);
```

These resources are never closed! This causes memory leaks.

**Fix:** Use try-with-resources:
```java
try (PreparedStatement stmt = connection.prepareStatement(query);
     ResultSet rs = stmt.executeQuery()) {
    // ...
}
```

</details>

<details>
<summary>Hint 6: Sample 4 - Edge Cases</summary>

Line 8:
```java
int[] current = intervals[0];
```

What if `intervals` is:
- `null` → NullPointerException
- `[]` (empty array) → ArrayIndexOutOfBoundsException

**Fix:** Add null and empty checks before accessing index 0.

</details>

<details>
<summary>Hint 7: Sample 4 - Comparator Overflow</summary>

Line 7:
```java
Arrays.sort(intervals, (a, b) -> a[0] - b[0]);
```

If `a[0]` is `Integer.MAX_VALUE` and `b[0]` is `-1`, then `a[0] - b[0]` overflows!

**Fix:** Use `Integer.compare(a[0], b[0])` which handles overflow correctly.

</details>

---

## Solutions

<details>
<summary>Click to reveal solutions</summary>

## Sample 1 Solution: Find Missing Number

### Issues Found

**Correctness:**
🟡 **Potential integer overflow:** Line 4: `(n * (n + 1)) / 2` can overflow for large n
- Example: n = 100,000 → n * (n + 1) overflows int range
- Works for interview-size inputs, but worth mentioning

**Edge Cases:**
🔴 **No null check:** Line 5 crashes with NullPointerException if nums is null
🟢 **Empty array:** Works correctly (returns 0)

**Efficiency:**
🟢 **Time:** O(n) - optimal
🟢 **Space:** O(1) - optimal

**Readability:**
🟢 Clear variable names and logic

**Security:**
🟢 No security concerns for this problem

### Fixed Code

```java
public class Solution {
    /**
     * Finds the missing number in an array containing n distinct numbers from 0 to n.
     *
     * @param nums array of distinct numbers from 0 to n with one missing
     * @return the missing number
     * @throws IllegalArgumentException if nums is null
     *
     * Time: O(n), Space: O(1)
     *
     * Uses Gauss formula: sum of 0 to n = n*(n+1)/2
     */
    public static int missingNumber(int[] nums) {
        if (nums == null) {
            throw new IllegalArgumentException("Array cannot be null");
        }

        int n = nums.length;

        // Use long to prevent overflow for large n
        long expectedSum = (long) n * (n + 1) / 2;
        long actualSum = 0;

        for (int num : nums) {
            actualSum += num;
        }

        return (int) (expectedSum - actualSum);
    }

    public static void main(String[] args) {
        System.out.println(missingNumber(new int[]{3, 0, 1}));  // Expected: 2
        System.out.println(missingNumber(new int[]{0, 1}));     // Expected: 2
        System.out.println(missingNumber(new int[]{9,6,4,2,3,5,7,0,1}));  // Expected: 8

        // Edge cases
        System.out.println(missingNumber(new int[]{}));  // Expected: 0
        System.out.println(missingNumber(new int[]{0})); // Expected: 1

        // Test null (should throw exception)
        try {
            missingNumber(null);
        } catch (IllegalArgumentException e) {
            System.out.println("Correctly caught null input");
        }
    }
}
```

### Alternative Solution (XOR - No Overflow Risk)

```java
public static int missingNumber(int[] nums) {
    if (nums == null) {
        throw new IllegalArgumentException("Array cannot be null");
    }

    int xor = nums.length;  // Start with n

    for (int i = 0; i < nums.length; i++) {
        xor ^= i ^ nums[i];  // XOR with index and value
    }

    return xor;  // Missing number remains
}
```

**Why XOR works:** XOR is self-inverse (a ^ a = 0). XORing all indices (0 to n) with all array values cancels out present numbers, leaving only the missing one.

### Explanation to Interviewer

> "This function finds the missing number using the Gauss formula for sum of consecutive integers. We calculate the expected sum from 0 to n using n*(n+1)/2, then calculate the actual sum of array elements, and return the difference. I used `long` for intermediate calculations to prevent integer overflow if n is large. The time complexity is O(n) with a single pass, and space is O(1). Edge cases: I handle null input by throwing an exception, and empty array correctly returns 0 as the missing number. An alternative approach is using XOR, which avoids overflow entirely since XOR operations don't accumulate values."

---

## Sample 2 Solution: Validate Email Address

### Issues Found

**Correctness:**
🟡 **Too simplistic validation:** Doesn't catch many invalid formats
- Multiple @ symbols: `test@@example.com`
- Spaces: `test @example.com`
- Missing domain extension: `test@example` (has dot but in wrong place)

**Edge Cases:**
🔴 **No null check:** Line 3 crashes with NullPointerException if email is null
🟢 **Empty string:** Returns false correctly (indexOf returns -1 which fails atIndex > 0 check)
🟡 **Special characters not validated:** Allows `test@domain.c` (single char TLD)

**Efficiency:**
🟢 **Time:** O(n) - two indexOf calls scan string
🟡 **Could optimize:** Single pass validation would be better

**Readability:**
🟢 Clear logic, but could add comments

**Security:**
🟡 **No input sanitization:** If used in SQL context, could be vulnerable

### Fixed Code

```java
public class EmailValidator {
    /**
     * Validates if a string is a valid email address.
     *
     * Basic validation checks:
     * - Not null or empty
     * - Contains exactly one @ symbol
     * - Has text before @
     * - Has domain after @ with at least one dot
     * - Has text after final dot (TLD)
     *
     * @param email the email string to validate
     * @return true if valid, false otherwise
     */
    public static boolean isValidEmail(String email) {
        // Handle null and empty
        if (email == null || email.trim().isEmpty()) {
            return false;
        }

        email = email.trim();

        // Check for exactly one @ symbol
        int atIndex = email.indexOf('@');
        int lastAtIndex = email.lastIndexOf('@');

        if (atIndex <= 0 || atIndex != lastAtIndex) {
            return false;  // No @, or @ at start, or multiple @
        }

        // Check for domain with dot
        String domain = email.substring(atIndex + 1);
        int dotIndex = domain.indexOf('.');

        if (dotIndex <= 0 || dotIndex >= domain.length() - 1) {
            return false;  // No dot, or dot at start/end of domain
        }

        // Optional: Check for spaces (invalid in emails)
        if (email.contains(" ")) {
            return false;
        }

        return true;
    }

    public static void main(String[] args) {
        // Valid emails
        System.out.println(isValidEmail("test@example.com"));     // true
        System.out.println(isValidEmail("user.name@domain.co.uk")); // true

        // Invalid emails
        System.out.println(isValidEmail("invalid.email"));        // false (no @)
        System.out.println(isValidEmail("@example.com"));         // false (nothing before @)
        System.out.println(isValidEmail("test@"));                // false (nothing after @)
        System.out.println(isValidEmail("test@@example.com"));    // false (multiple @)
        System.out.println(isValidEmail("test @example.com"));    // false (space)
        System.out.println(isValidEmail("test@example"));         // false (no dot in domain)
        System.out.println(isValidEmail("test@.com"));            // false (dot at start of domain)
        System.out.println(isValidEmail("test@example."));        // false (dot at end)
        System.out.println(isValidEmail(null));                   // false (null)
        System.out.println(isValidEmail(""));                     // false (empty)
        System.out.println(isValidEmail("  "));                   // false (blank)
    }
}
```

### Explanation to Interviewer

> "This function validates email addresses using basic checks. First, I handle null and empty strings by returning false. Then I verify there's exactly one @ symbol, not at the beginning, by comparing indexOf and lastIndexOf. I extract the domain part after @ and check for a dot that's not at the start or end. I also reject emails with spaces. The time complexity is O(n) where n is the email length, and space is O(n) for the substring. For production, I'd use regex or a proper email validation library, but this demonstrates understanding of the validation logic and edge case handling. The main edge cases are: null, empty, multiple @, missing @, no domain, spaces, and incorrect dot placement."

---

## Sample 3 Solution: SQL Query Builder

### Issues Found

**Correctness:**
🟢 Logic is correct for happy path

**Edge Cases:**
🟡 **No null check for name:** Should validate input
🟡 **Empty string:** Would generate valid query but might not be intended

**Efficiency:**
🟢 Query itself is simple, efficient

**Readability:**
🟡 **No Javadoc:** Should document parameters and exceptions
🟡 **Resource leak:** stmt and rs never closed

**Security:**
🔴🔴🔴 **CRITICAL: SQL Injection vulnerability** on line 13
- Concatenating user input directly into SQL query
- Attacker could pass: `name = "'; DROP TABLE users; --"`
- Would execute: `SELECT username FROM users WHERE username = ''; DROP TABLE users; --'`
- **This could delete the entire database!**

### Fixed Code

```java
import java.sql.*;
import java.util.ArrayList;
import java.util.List;

public class UserDatabase {
    private Connection connection;

    public UserDatabase(Connection connection) {
        if (connection == null) {
            throw new IllegalArgumentException("Connection cannot be null");
        }
        this.connection = connection;
    }

    /**
     * Finds users by username using a parameterized SQL query.
     *
     * @param name the username to search for (can be null or empty)
     * @return list of matching usernames (empty list if none found)
     * @throws SQLException if database error occurs
     * @throws IllegalArgumentException if name is null
     *
     * Security: Uses PreparedStatement to prevent SQL injection
     */
    public List<String> findUsersByName(String name) throws SQLException {
        if (name == null) {
            throw new IllegalArgumentException("Name cannot be null");
        }

        // ✅ Use parameterized query to prevent SQL injection
        String query = "SELECT username FROM users WHERE username = ?";
        List<String> users = new ArrayList<>();

        // ✅ Use try-with-resources to automatically close resources
        try (PreparedStatement stmt = connection.prepareStatement(query)) {
            stmt.setString(1, name);  // ✅ Safely set parameter

            try (ResultSet rs = stmt.executeQuery()) {
                while (rs.next()) {
                    users.add(rs.getString("username"));
                }
            }
        }

        return users;
    }

    public static void main(String[] args) throws SQLException {
        // Test code (would need actual database connection)
        // UserDatabase db = new UserDatabase(connection);

        // Safe: Even malicious input is treated as literal string
        // List<String> users = db.findUsersByName("'; DROP TABLE users; --");
        // This searches for username literally equal to "'; DROP TABLE users; --"
        // Does NOT execute the DROP command!
    }
}
```

### Explanation to Interviewer

> "The original code has a **critical security vulnerability**—SQL injection on line 13 where user input is concatenated directly into the SQL query. An attacker could pass something like `'; DROP TABLE users; --` which would delete the entire users table. My fix uses `PreparedStatement` with parameterized queries. The `?` placeholder is safely replaced by the `setString()` method, which automatically escapes special characters, treating the input as a literal string value, not SQL code. I also added try-with-resources blocks to automatically close the `PreparedStatement` and `ResultSet`, preventing resource leaks. Finally, I added null validation for the name parameter and Javadoc. This is a crucial security practice—never concatenate user input into SQL queries; always use parameterized statements."

---

## Sample 4 Solution: Merge Intervals

### Issues Found

**Correctness:**
🟡 **Comparator overflow risk:** Line 7: `a[0] - b[0]` can overflow
- If a[0] = Integer.MAX_VALUE and b[0] = -1
- Subtraction overflows and produces wrong ordering

**Edge Cases:**
🔴 **No null check:** Line 8 crashes with NullPointerException if intervals is null
🔴 **Empty array check:** Line 8 crashes with ArrayIndexOutOfBoundsException if intervals is empty
🟡 **Single interval:** Works correctly (returns single interval as-is)

**Efficiency:**
🟢 **Time:** O(n log n) for sorting + O(n) for merging = O(n log n) - optimal
🟢 **Space:** O(n) for result list - optimal

**Readability:**
🟡 Variable names are okay but could add comments

**Security:**
🟢 No security concerns

### Fixed Code

```java
import java.util.*;

public class Solution {
    /**
     * Merges overlapping intervals.
     *
     * @param intervals array of intervals where intervals[i] = [start, end]
     * @return array of merged intervals
     * @throws IllegalArgumentException if intervals is null
     *
     * Time: O(n log n) for sorting + O(n) for merging = O(n log n)
     * Space: O(n) for result list
     *
     * Example: [[1,3],[2,6],[8,10]] → [[1,6],[8,10]]
     */
    public static int[][] merge(int[][] intervals) {
        if (intervals == null) {
            throw new IllegalArgumentException("Intervals cannot be null");
        }

        if (intervals.length == 0) {
            return new int[0][];  // ✅ Return empty array for empty input
        }

        // ✅ Use Integer.compare to avoid overflow
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        List<int[]> result = new ArrayList<>();
        int[] current = intervals[0];
        result.add(current);

        for (int i = 1; i < intervals.length; i++) {
            int[] interval = intervals[i];

            // If overlapping or adjacent, merge
            if (interval[0] <= current[1]) {
                // Extend current interval's end to the max of both ends
                current[1] = Math.max(current[1], interval[1]);
            } else {
                // No overlap, add new interval
                current = interval;
                result.add(current);
            }
        }

        return result.toArray(new int[result.size()][]);
    }

    public static void main(String[] args) {
        // Normal cases
        int[][] intervals1 = {{1,3},{2,6},{8,10},{15,18}};
        System.out.println(Arrays.deepToString(merge(intervals1)));
        // Expected: [[1,6],[8,10],[15,18]]

        int[][] intervals2 = {{1,4},{4,5}};
        System.out.println(Arrays.deepToString(merge(intervals2)));
        // Expected: [[1,5]]

        // Edge cases
        System.out.println(Arrays.deepToString(merge(new int[][]{})));
        // Expected: []

        System.out.println(Arrays.deepToString(merge(new int[][]{{1,5}})));
        // Expected: [[1,5]]

        // Test null
        try {
            merge(null);
        } catch (IllegalArgumentException e) {
            System.out.println("Correctly caught null input");
        }
    }
}
```

### Explanation to Interviewer

> "This function merges overlapping intervals. First, I validate for null and empty inputs. Then I sort intervals by start time using `Integer.compare` instead of subtraction to avoid integer overflow—if you subtract Integer.MAX_VALUE and a negative number, the result overflows. After sorting, I iterate through intervals with a two-pointer approach: maintain a `current` interval in the result list, and for each new interval, check if it overlaps (start <= current end). If so, extend the current interval by taking the max of both ends. If not, add the new interval to results. Time complexity is O(n log n) dominated by sorting, and space is O(n) for the result. Edge cases: null returns exception, empty array returns empty result, single interval returns as-is, and adjacent intervals like [1,4] and [4,5] correctly merge to [1,5]."

</details>

---

## Key Takeaways

✅ **Systematic review catches issues:** The checklist ensures you don't miss critical bugs

✅ **Security matters in interviews:** Showing awareness of SQL injection, XSS, etc. demonstrates professionalism

✅ **Edge cases are everywhere:** Null, empty, overflow—always check these

✅ **Resource management matters:** Use try-with-resources for auto-closing

✅ **Prioritize fixes:** Critical security/correctness bugs before readability improvements

✅ **Explain clearly:** Demonstrate understanding by articulating the problem and fix

✅ **Know common pitfalls:** Overflow in comparators, SQL injection, null checks—these appear frequently

---

## Self-Reflection Questions

1. **Which category of issues did you find most challenging to spot?**
   - Correctness bugs (logic errors)?
   - Edge cases (null, empty, boundaries)?
   - Security vulnerabilities?
   - Efficiency problems?

2. **How long did it take you to review each sample?**
   - Under 30 seconds? → Excellent, you're developing intuition
   - 30-60 seconds? → Good, keep practicing
   - Over 60 seconds? → Review the checklist again, practice more

3. **Did you catch the critical bugs first?**
   - SQL injection (Sample 3)?
   - Array access without null/empty check (Sample 4)?
   - Prioritizing critical issues is essential in interviews

4. **Could you explain each fix clearly?**
   - Practice explaining out loud as if to an interviewer
   - Use the framework: Problem → Fix → Why it matters

---

## Variations / Extensions

Want more practice? Try these:

### Variation 1: Time-Boxed Challenge
Set a timer for 5 minutes per sample. Can you:
- Find all critical issues
- Document them
- Propose fixes
- Explain clearly

All in 5 minutes?

### Variation 2: Real AI Practice
Use an AI tool (Claude, ChatGPT, Copilot) to generate solutions to these problems:
1. "Find the kth largest element in an array"
2. "Implement a basic LRU cache"
3. "Check if a binary tree is balanced"

Review the AI's output using the checklist. What issues do you find?

### Variation 3: Peer Review
Exchange code with a study partner:
1. Each person writes a solution to a problem
2. Intentionally introduce 2-3 bugs
3. Swap code and review using the checklist
4. Compare findings

### Variation 4: Progressive Difficulty
Start with easier problems, work up to harder:
1. **Easy:** "Reverse a string"
2. **Medium:** "Implement a stack with min() operation"
3. **Hard:** "Design a thread-safe singleton"

Review AI-generated solutions for each.

---

## Additional Practice Problems

Here are more problems to practice the review checklist on:

1. **Implement strStr()** - Find substring in string
2. **Valid Parentheses** - Check if brackets are balanced
3. **Rotate Array** - Rotate array by k positions
4. **Product of Array Except Self** - Without division operator
5. **Group Anagrams** - Group strings that are anagrams

Generate solutions with AI, then review using the checklist!

---

[← Back to Chapter 4](../chapters/chapter-04-code-validation.md)
