# Chapter 5: Common Anti-Patterns & Pitfalls

**Difficulty:** 🟡 Intermediate
**Time:** 2-3 hours
**Goal:** Learn to recognize and avoid common mistakes that candidates make when using AI coding assistants in interviews

---

## Learning Objectives

By the end of this chapter, you will be able to:
- [ ] Identify and avoid the seven deadly anti-patterns of AI-assisted development
- [ ] Recognize when you're falling into iteration traps and recover quickly
- [ ] Balance AI assistance with genuine problem-solving demonstration
- [ ] Apply recovery strategies when things go wrong
- [ ] Distinguish between productive and counterproductive AI usage patterns

---

## Introduction

You've learned prompt engineering, iteration workflows, and code validation. You understand the mechanics of working with AI. But knowing the right approach is only half the battle. You also need to recognize and avoid the wrong approaches.

**The harsh reality:** Most candidates who fail AI-assisted interviews don't fail because they lack coding skills. They fail because they fall into predictable anti-patterns—behavioral traps that waste time, demonstrate poor judgment, or signal lack of understanding to the interviewer.

This chapter is your field guide to failure modes. We'll examine real case studies of candidate mistakes, understand why these patterns are problematic, and learn concrete recovery strategies. Think of this as defensive driving for technical interviews: knowing what can go wrong is the first step in ensuring it doesn't happen to you.

**Critical insight:** Interviewers are explicitly watching for these anti-patterns. A candidate who recognizes and corrects their own missteps scores higher than one who never makes mistakes, because course correction demonstrates metacognition and adaptability.

---

## The Seven Deadly Anti-Patterns

### Anti-Pattern 1: Over-Reliance (The "Blind Trust" Trap)

**Definition:** Accepting and submitting AI-generated code without understanding how it works or whether it's correct.

**Why it happens:**
- Time pressure creates urgency to "just get it working"
- Code looks correct on surface (compiles, runs on sample input)
- Candidate lacks confidence to question the AI

**What interviewers observe:**
- Inability to explain code when asked
- Failure to catch obvious bugs
- Panic when asked to modify or extend the solution
- Using terminology without understanding (buzzword syndrome)

#### Case Study 1.1: The Binary Search Disaster

**Scenario:**
A candidate was asked to implement binary search in a sorted array.

**What happened:**
```java
// Candidate's prompt: "Write binary search in Java"
// AI generated this code:

public class Solution {
    public static int binarySearch(int[] arr, int target) {
        int left = 0;
        int right = arr.length;  // BUG: Mixing conventions

        while (left < right) {  // BUG: Mixing conventions
            int mid = (left + right) / 2;

            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                left = mid;  // BUG: Should be mid + 1
            } else {
                right = mid;  // BUG: Mixing conventions
            }
        }

        return -1;
    }
}
```

**Note:** There are multiple valid binary search variants (closed interval with `left <= right` and `right = arr.length - 1`, or half-open interval with `left < right` and `right = arr.length`). This code is a broken hybrid mixing conventions inconsistently, leading to infinite loops.

Candidate submitted this without testing. Interviewer tested with `[1, 3, 5, 7, 9]`, target `7`.

**Execution trace:**
```
Iteration 1: left=0, right=5, mid=2, arr[2]=5, left=2
Iteration 2: left=2, right=5, mid=3, arr[3]=7, return 3 ✅

Interviewer: "Looks good. What about target 5?"
Iteration 1: left=0, right=5, mid=2, arr[2]=5, return 2 ✅

Interviewer: "And target 1?"
Iteration 1: left=0, right=5, mid=2, arr[2]=5, right=2
Iteration 2: left=0, right=2, mid=1, arr[1]=3, right=1
Iteration 3: left=0, right=1, mid=0, arr[0]=1, return 0 ✅

Interviewer: "Now try target 4 (not in array)"
Iteration 1: left=0, right=5, mid=2, arr[2]=5, right=2
Iteration 2: left=0, right=2, mid=1, arr[1]=3, left=1
Iteration 3: left=1, right=2, mid=1, arr[1]=3, left=1
INFINITE LOOP! 🔥
```

**What went wrong:**
The candidate never mentally traced the code or tested edge cases. The bugs weren't caught because basic test cases passed by luck. When asked to explain the algorithm, candidate said: "It divides the search space in half each time"—technically correct but demonstrated no deep understanding.

**Root cause:** Over-reliance—accepting code without verification.

**Recovery strategy:**
```java
// AFTER recognizing the problem, candidate should say:

"I see the issue. Let me trace through this more carefully.
The problem is in my loop boundaries and pointer updates."

// Then implement correctly:
public static int binarySearch(int[] arr, int target) {
    if (arr == null || arr.length == 0) {
        return -1;
    }

    int left = 0;
    int right = arr.length - 1;  // ✅ Correct boundary

    while (left <= right) {  // ✅ Correct condition
        int mid = left + (right - left) / 2;  // ✅ Overflow-safe

        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;  // ✅ Exclude mid
        } else {
            right = mid - 1;  // ✅ Exclude mid
        }
    }

    return -1;
}

// "Let me verify with the failing case..."
// Traces through target=4 case, confirms termination
// "This now correctly returns -1 for missing elements."
```

**Interviewer's take:**
> "The initial mistake was concerning, but the candidate's methodical debugging process actually impressed me. They showed they could think independently and didn't just re-prompt the AI. The recovery demonstrated problem-solving skills."

#### Case Study 1.2: The Hash Function Mystery

**Scenario:**
Implement a custom hash function for a Java object.

**What happened:**
```java
public class Person {
    private String name;
    private int age;

    // AI-generated hashCode()
    @Override
    public int hashCode() {
        return name.hashCode() ^ age;  // Looks reasonable?
    }
}
```

**Interviewer:** "Walk me through this hash function."

**Candidate:** "It combines the name's hash code with the age using XOR, which distributes the values well."

**Interviewer:** "What happens if name is null?"

**Candidate:** "Um... it would... uh..." ❌

**What went wrong:**
1. NullPointerException not considered
2. XOR is poor for combining hash codes (doesn't follow best practices)
3. Candidate couldn't explain *why* XOR was chosen (because AI chose it, not them)

**Correct implementation:**
```java
public class Person {
    private String name;
    private int age;

    @Override
    public int hashCode() {
        // Use Objects.hash() which handles nulls and follows best practices
        return Objects.hash(name, age);
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;

        Person person = (Person) obj;
        return age == person.age &&
               Objects.equals(name, person.name);
    }
}

// Explanation ready:
// "Objects.hash() uses a proven algorithm (31 * hash + field) that:
//  1. Handles null values safely
//  2. Provides good distribution
//  3. Is consistent with standard practice
//  4. Matches the contract with equals()"
```

**Prevention checklist for over-reliance:**
```
BEFORE SUBMITTING AI-GENERATED CODE:
✓ Can I explain every line?
✓ Do I know why this approach was chosen?
✓ Have I tested edge cases mentally?
✓ Could I implement this from scratch if needed?
✓ Do I understand the time/space complexity?

If ANY answer is NO → Don't submit yet!
```

---

### Anti-Pattern 2: Under-Utilization (The "Martyr" Trap)

**Definition:** Not leveraging AI effectively—doing manually what AI could do faster, or using AI only for trivial tasks.

**Why it happens:**
- Overcompensation from fear of over-relying
- Not knowing what AI is good at
- Wanting to show "I can code without help"

**What interviewers observe:**
- Taking unnecessarily long on boilerplate
- Reinventing wheels AI could have provided
- Inefficient time allocation

#### Case Study 2.1: The Boilerplate Bottleneck

**Scenario:**
Implement a thread-safe LRU cache with generic types.

**What happened:**
Candidate spent 20 minutes writing boilerplate:
```java
public class LRUCache<K, V> {
    private final int capacity;
    private final Map<K, Node<K, V>> cache;
    private Node<K, V> head;
    private Node<K, V> tail;

    // Manually writing doubly-linked list node class
    private static class Node<K, V> {
        K key;
        V value;
        Node<K, V> prev;
        Node<K, V> next;

        Node(K key, V value) {
            this.key = key;
            this.value = value;
        }
    }

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.cache = new HashMap<>();

        // Initialize dummy head and tail
        head = new Node<>(null, null);
        tail = new Node<>(null, null);
        head.next = tail;
        tail.prev = head;
    }

    // 40 more lines of manual implementation...
}
```

**Time budget:**
- 20 minutes on boilerplate ❌
- 10 minutes remaining for actual LRU logic ❌
- Ran out of time before completing `put()` method ❌

**What went wrong:**
The candidate wanted to prove they could implement from scratch, but the interviewer cared about demonstrating:
1. Understanding of LRU eviction policy
2. Thread-safety approach
3. Complexity analysis
4. Testing strategy

All lost to manual boilerplate writing.

**Better approach:**
```java
// Prompt to AI: "Generate a basic LRU cache structure in Java with
// generic types, using LinkedHashMap with access-order mode"

import java.util.LinkedHashMap;
import java.util.Map;

public class LRUCache<K, V> {
    private final int capacity;
    private final Map<K, V> cache;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        // LinkedHashMap with access-order mode handles LRU automatically
        this.cache = new LinkedHashMap<>(capacity, 0.75f, true) {
            @Override
            protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
                return size() > LRUCache.this.capacity;
            }
        };
    }

    public synchronized V get(K key) {
        return cache.get(key);
    }

    public synchronized void put(K key, V value) {
        cache.put(key, value);
    }
}

// Time saved: 15 minutes
// Now spend that time on:
// 1. Explaining why LinkedHashMap works for LRU
// 2. Discussing thread-safety (synchronized vs. ConcurrentHashMap)
// 3. Writing comprehensive tests
// 4. Analyzing trade-offs
```

**Candidate explains to interviewer:**
> "I used AI to generate the boilerplate structure using `LinkedHashMap` with access-order mode, which is perfect for LRU because it maintains insertion/access order. The anonymous inner class overrides `removeEldestEntry()` to enforce capacity. This is actually a well-known pattern in Java—I wanted to focus our time on the more interesting aspects: thread-safety strategies and edge case handling."

**Interviewer's take:**
> "Smart delegation. They used AI for what it's good at—generating standard patterns—then focused discussion on architectural decisions and trade-offs. That's exactly what we want to see."

#### Case Study 2.2: The Test Case Tedium

**Scenario:**
Implement a function to validate credit card numbers (Luhn algorithm).

**What happened:**
Candidate implemented the algorithm correctly, then manually wrote test cases:
```java
@Test
public void testValidCard() {
    assertTrue(isValidCard("4532015112830366"));
}

@Test
public void testInvalidCard() {
    assertFalse(isValidCard("4532015112830367"));
}

@Test
public void testNullCard() {
    assertFalse(isValidCard(null));
}

// 8 more manually written test methods...
// Time spent: 10 minutes
```

**Better approach:**
```
Prompt: "Generate comprehensive JUnit 5 tests for this Luhn validation
function, including: valid cards from different issuers (Visa, Mastercard,
Amex), invalid check digits, null/empty strings, non-numeric input, edge
cases like single digit and maximum length"
```

AI generates:
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;
import static org.junit.jupiter.api.Assertions.*;

class CreditCardValidatorTest {

    @ParameterizedTest
    @ValueSource(strings = {
        "4532015112830366",  // Visa
        "5425233430109903",  // Mastercard
        "374245455400126",   // Amex
        "6011000991300009"   // Discover
    })
    void testValidCardNumbers(String cardNumber) {
        assertTrue(isValidCard(cardNumber),
            "Card " + cardNumber + " should be valid");
    }

    @ParameterizedTest
    @ValueSource(strings = {
        "4532015112830367",  // Invalid check digit
        "1234567812345670",  // Wrong pattern
        "1234567812345678"   // Invalid check digit
    })
    void testInvalidCardNumbers(String cardNumber) {
        assertFalse(isValidCard(cardNumber),
            "Card " + cardNumber + " should be invalid");
    }

    @Test
    void testNullInput() {
        assertFalse(isValidCard(null));
    }

    @Test
    void testEmptyString() {
        assertFalse(isValidCard(""));
    }

    @ParameterizedTest
    @ValueSource(strings = {
        "123abc456",         // Contains letters
        "1234-5678-9012",    // Contains dashes
        "1234 5678 9012"     // Contains spaces
    })
    void testNonNumericInput(String cardNumber) {
        assertFalse(isValidCard(cardNumber));
    }

    @Test
    void testSingleDigit() {
        assertFalse(isValidCard("5"));
    }

    @Test
    void testMaxLength() {
        // 19 digits (max for some cards)
        assertTrue(isValidCard("6304000000000000000")); // Valid 19-digit
    }
}

// Time spent: 2 minutes to review and validate
// Time saved: 8 minutes → used for actual implementation refinement
```

**Prevention checklist for under-utilization:**
```
ASK YOURSELF:
✓ Is this task repetitive or boilerplate? → Let AI do it
✓ Is this a standard pattern/algorithm? → Get AI baseline, then customize
✓ Am I doing something AI excels at (testing, docs)? → Delegate
✓ Would doing this manually give me time to focus on what matters? → NO

GOOD USES OF AI TIME SAVINGS:
✓ Deeper analysis of trade-offs
✓ More thorough testing
✓ Better code review
✓ Clearer communication with interviewer
```

---

### Anti-Pattern 3: Prompt Paralysis (The "Perfectionist" Trap)

**Definition:** Spending excessive time crafting the "perfect" prompt instead of iterating quickly.

**Why it happens:**
- Belief that prompt quality is paramount
- Fear of getting "bad" code
- Perfectionism

**What interviewers observe:**
- Candidate staring at screen, typing and deleting
- Long pauses with no visible progress
- Over-thinking simple tasks

#### Case Study 3.1: The Two-Minute Prompt

**Scenario:**
Implement a function to find the longest common prefix in an array of strings.

**What happened:**
Candidate spent 2 minutes crafting this prompt:

```
"I need a Java function that takes an array of strings as input and returns
the longest common prefix shared by all strings. The function should:
- Handle null input by throwing IllegalArgumentException
- Handle empty array by returning empty string
- Handle single string by returning that string
- Compare characters efficiently using vertical scanning
- Have O(S) time complexity where S is sum of all characters
- Have O(1) space complexity
- Follow Java naming conventions
- Include Javadoc comments
- Include inline comments
- Handle edge cases like different length strings
- Stop early when mismatch found
- Use StringBuilder for efficiency"
```

**Interviewer observation:**
> "Candidate has been silent for 2 minutes staring at the prompt. In a 45-minute interview, that's 4% of total time on a single prompt. They haven't even gotten code yet."

**What went wrong:**
This level of detail is unnecessary for a first pass. The prompt paralysis cost time and demonstrated indecisiveness.

**Better approach:**

**First iteration (10 seconds):**
```
"Java function to find longest common prefix in string array"
```

AI generates basic solution. Review it. Identify gaps.

**Second iteration (15 seconds):**
```
"Add null/empty checks and optimize for early termination"
```

AI refines. Done.

**Total time:** 25 seconds vs. 120 seconds = 95 seconds saved

**The iteration mindset:**
```
┌──────────────────────────────────────────────────────┐
│  PROMPT PARALYSIS (❌ Don't do this)                 │
│  1. Think for 2 min about perfect prompt            │
│  2. Write elaborate specification                    │
│  3. Get code that's 90% right                        │
│  4. Small refinements                                │
│  Total time: 3-4 minutes                             │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│  QUICK ITERATION (✅ Do this)                         │
│  1. Simple prompt (10 sec)                           │
│  2. Get code that's 70% right (5 sec)                │
│  3. Quick refinement prompt (10 sec)                 │
│  4. Get code that's 90% right (5 sec)                │
│  5. Manual polish for last 10%                       │
│  Total time: 30-45 seconds                           │
└──────────────────────────────────────────────────────┘
```

#### Case Study 3.2: The Architecture Document

**Scenario:**
Implement a simple REST API endpoint to get user by ID.

**What happened:**
Candidate spent 5 minutes writing:

```
"Create a Spring Boot REST controller with the following specifications:
- Endpoint: GET /api/users/{id}
- Path variable: id (Long type)
- Return type: ResponseEntity<UserDTO>
- Service layer: UserService with findById method
- Repository layer: UserRepository extending JpaRepository
- Exception handling: Return 404 if user not found
- Logging: Add debug logs for entry and exit
- Validation: Ensure id is positive
- DTOs: UserDTO with id, name, email fields
- Mapping: Use ModelMapper for entity to DTO conversion
- Testing: Include unit tests with MockMvc
- Documentation: Add OpenAPI/Swagger annotations
- Security: Add @PreAuthorize for admin only
..."
```

**Interviewer:** "We're 5 minutes in and there's no code yet. Let's move faster."

**What went wrong:**
Treating the prompt like a requirements document. Over-specification led to:
1. Wasted time
2. Reduced flexibility (locked into initial design)
3. Demonstrates poor time management

**Better approach:**

```
"Spring Boot REST endpoint GET /users/{id} returning user or 404"
```

Generate, review, iterate. Build incrementally.

**Recovery strategy when you catch yourself:**
```
MENTAL CHECKPOINT (every 30 seconds):
- Am I still typing the prompt? → STOP
- Have I sent the prompt yet? → DO IT NOW
- Is this iteration #1? → Keep it simple
- Is this iteration #3+? → Maybe just edit the code manually

REMEMBER: Prompt → Code → Review → Iterate
         NOT: Perfect Prompt → Perfect Code
```

---

### Anti-Pattern 4: Iteration Hell (The "Never Done" Trap)

**Definition:** Getting stuck in endless refinement loops, making changes that don't meaningfully improve the solution.

**Why it happens:**
- Seeking perfection over completion
- Not knowing when "good enough" is good enough
- Following AI suggestions blindly

**What interviewers observe:**
- Frequent small changes to working code
- Lost track of what problem they're solving
- No forward progress

#### Case Study 4.1: The Variable Name Vortex

**Scenario:**
Implement function to calculate factorial.

**What happened:**
Candidate got working code in 30 seconds, then spent 3 minutes on this:

**Iteration 1:** Working code
```java
public static int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

**Iteration 2:** "Make variable names more descriptive"
```java
public static int calculateFactorial(int number) {
    if (number <= 1) return 1;
    return number * calculateFactorial(number - 1);
}
```

**Iteration 3:** "Add more explicit base case"
```java
public static int calculateFactorial(int number) {
    if (number < 0) {
        throw new IllegalArgumentException("Negative number");
    }
    if (number == 0 || number == 1) {
        return 1;
    }
    return number * calculateFactorial(number - 1);
}
```

**Iteration 4:** "Add Javadoc"
```java
/**
 * Calculates the factorial of a given number.
 * @param number the input number
 * @return the factorial value
 */
public static int calculateFactorial(int number) {
    // ... same code
}
```

**Iteration 5:** "Use BigInteger for large numbers"
```java
public static BigInteger calculateFactorial(int number) {
    // Complete rewrite...
}
```

**Interviewer:** "You've been refining this for 4 minutes. It was correct after iteration 3. Let's move on."

**What went wrong:**
1. No stopping criteria
2. Changing things that don't matter (variable names in a 3-line function)
3. Lost sight of time budget
4. No interviewer communication about trade-offs

**The 3-Iteration Rule:**
```
┌────────────────────────────────────────────┐
│ Iteration 1: Get something working        │
│ Iteration 2: Fix bugs and major issues    │
│ Iteration 3: Polish if time permits       │
│ Iteration 4+: STOP! You're in hell!       │
└────────────────────────────────────────────┘

STOPPING CRITERIA:
✓ Tests pass
✓ Edge cases handled
✓ Time/space complexity is reasonable
✓ Code is readable
✓ You can explain it clearly

DO NOT ITERATE ON:
✗ Variable names in short functions
✗ Comment phrasing
✗ Code style (tabs vs spaces)
✗ Micro-optimizations
✗ Alternative approaches that aren't clearly better
```

#### Case Study 4.2: The Algorithm Carousel

**Scenario:**
Find if array contains duplicate values.

**What happened:**

**Iteration 1:** HashSet approach (O(n) time, O(n) space)
```java
public static boolean containsDuplicate(int[] nums) {
    Set<Integer> seen = new HashSet<>();
    for (int num : nums) {
        if (!seen.add(num)) return true;
    }
    return false;
}
```

**Candidate:** "This uses O(n) space. Let me optimize."

**Iteration 2:** Sorting approach (O(n log n) time, O(1) space)
```java
public static boolean containsDuplicate(int[] nums) {
    Arrays.sort(nums);
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] == nums[i-1]) return true;
    }
    return false;
}
```

**Candidate:** "But sorting modifies the array. Let me fix that."

**Iteration 3:** Back to HashSet with defensive copy
```java
public static boolean containsDuplicate(int[] nums) {
    int[] copy = Arrays.copyOf(nums, nums.length);
    Arrays.sort(copy);
    for (int i = 1; i < copy.length; i++) {
        if (copy[i] == copy[i-1]) return true;
    }
    return false;
}
```

**Candidate:** "Wait, now I'm using O(n) space anyway. Let me..."

**Interviewer:** "Stop. You've circled back to similar complexity. Pick one approach and move on."

**What went wrong:**
- No clear optimization goal
- Didn't discuss trade-offs with interviewer first
- Each iteration introduced new issues
- Lost 5 minutes to indecision

**Better approach:**
```java
// After iteration 1:
Candidate: "I have an O(n) time, O(n) space solution using HashSet.
For this problem size and context, that's optimal. If memory were
severely constrained, I could sort in-place for O(n log n) time and
O(1) space, but that modifies the input. Should I optimize further?"

Interviewer: "No, O(n) time is fine. Let's move to the next question."

// Time saved: 5 minutes
```

**Recovery strategy:**
```
IF YOU'VE DONE 3+ ITERATIONS:
1. Stop typing
2. State current state: "I have a working solution with [complexity]"
3. Ask: "Would you like me to optimize this or move forward?"
4. If optimize: Get specific direction on what to optimize
5. If move forward: Move forward!

RED FLAGS YOU'RE IN ITERATION HELL:
- You've lost count of iterations
- You're changing things that already worked
- You can't articulate what the next iteration improves
- You're exploring alternatives without clear criteria
- Interviewer looks impatient or bored
```

---

### Anti-Pattern 5: Context Overload (The "Kitchen Sink" Trap)

**Definition:** Providing too much irrelevant information in prompts, drowning the signal in noise.

**Why it happens:**
- Belief that more context = better results
- Not understanding what information is relevant
- Copying entire files when only a snippet is needed

**What interviewers observe:**
- Very long prompts
- Slow AI responses (processing irrelevant data)
- AI generates generic solutions that don't use the context

#### Case Study 5.1: The Codebase Dump

**Scenario:**
Add a method to existing class to validate email format.

**What happened:**
Candidate pasted entire file (400 lines) into prompt:
```
"Here's my User class:

public class User {
    private Long id;
    private String username;
    private String email;
    private String passwordHash;
    private LocalDateTime createdAt;
    private LocalDateTime lastLoginAt;
    private Set<Role> roles;
    private Address address;
    private List<Order> orders;
    private UserPreferences preferences;

    // 50+ lines of getters/setters
    // 10+ other methods
    // 200 lines of business logic
    // 100 lines of related inner classes

    // ... entire class ...
}

Add a method to validate email format."
```

**Problems:**
1. AI response took 8 seconds (vs. 2 seconds for minimal prompt)
2. AI suggested placing method in wrong location
3. AI included unnecessary refactoring suggestions
4. Candidate had to wade through 100 lines of generated output

**Better approach:**
```
"Add email validation method to this User class:

public class User {
    private String email;

    // Add method here to validate email format:
    // - Must contain @
    // - Must have domain with dot
    // Return boolean
}
"
```

**Result:**
- AI response in 2 seconds
- Focused, relevant code
- Easy to review and integrate

#### Case Study 5.2: The Architecture Essay

**Scenario:**
Implement a helper function to parse date strings.

**What happened:**
```
Prompt: "I'm building a microservices-based e-commerce platform using
Spring Boot with a React frontend. We're using PostgreSQL for persistence
and Redis for caching. The architecture follows Domain-Driven Design
principles with CQRS and Event Sourcing. Our CI/CD pipeline uses Jenkins
and deploys to Kubernetes on AWS EKS. We have 15 microservices including
user-service, order-service, inventory-service, payment-service...

[200 more words of architecture description]

...I need a utility function to parse date strings in format 'yyyy-MM-dd'
to LocalDate in Java."
```

**What went wrong:**
- 95% of context is irrelevant to parsing a date
- AI can't extract useful signal from the noise
- Wastes tokens and time
- Demonstrates poor communication skills

**Better approach:**
```
"Java function to parse date string 'yyyy-MM-dd' to LocalDate, handling
invalid formats with exception"
```

Done. Everything else is noise.

**The Minimum Sufficient Context Principle:**

```
ASK: What is the MINIMUM information needed to generate correct code?

INCLUDE:
✓ The specific task
✓ Input/output types
✓ Key constraints or requirements
✓ Relevant existing code (if needed for integration)

EXCLUDE:
✗ Architecture details (unless they affect implementation)
✗ Entire files (provide excerpts)
✗ Business domain details (unless they affect logic)
✗ Your tech stack (unless it affects approach)
✗ Your reasoning about the problem (save for interviewer)

GUIDELINE: If you can remove it without changing the answer, remove it.
```

#### Case Study 5.3: The Historical Novel

**Scenario:**
Fix a bug in sorting function.

**What happened:**
```
"I implemented this sorting function yesterday but it's not working correctly.
I tried bubble sort first but it was too slow so I switched to quicksort.
Then I realized quicksort has worst-case O(n²) so I considered merge sort
but that uses O(n) space. I talked to my friend who suggested using Java's
built-in sort but I wanted to implement from scratch for learning. Then I
tried insertion sort for small arrays but...

[300 more words of history]

Here's the code that's not working:

[code]

Fix the bug."
```

**What went wrong:**
The backstory is irrelevant. AI doesn't need to know your journey, just:
1. What the code should do
2. What it's doing wrong
3. The actual code

**Better approach:**
```
"This quicksort partition function returns wrong pivot index for
array [3, 1, 2]. Expected: 1, Got: 2.

[code]

Find and fix the bug."
```

**Prevention checklist:**
```
BEFORE HITTING SEND:
□ Can I remove the first paragraph? (Usually yes)
□ Can I remove background/history? (Yes)
□ Can I reduce code context to <50 lines? (Usually yes)
□ Is every sentence necessary for the AI to answer? (Be honest)
□ Am I explaining my reasoning? (Save for interviewer, not AI)

CONTEXT SIZE GUIDELINES:
- Simple function: 0-10 lines of context
- Method in class: 20-30 lines of context
- Integration task: 50-80 lines of context
- If you need >100 lines, reconsider the task scope
```

---

### Anti-Pattern 6: Trust Without Verification (The "Looks Good" Trap)

**Definition:** Not testing AI-generated code thoroughly, assuming correctness because it compiles or passes basic tests.

**Why it happens:**
- Time pressure to move fast
- Overconfidence in AI accuracy
- Lack of systematic testing approach

**What interviewers observe:**
- Code fails on edge cases
- Security vulnerabilities missed
- No evidence of code review
- Surprised by bugs the interviewer finds

#### Case Study 6.1: The Integer Overflow

**Scenario:**
Implement a function to sum array elements.

**What happened:**
```java
public static int sumArray(int[] arr) {
    int sum = 0;
    for (int num : arr) {
        sum += num;
    }
    return sum;
}
```

Candidate tested with `[1, 2, 3, 4, 5]` → returns `15` ✓

Submitted without further testing.

**Interviewer:** "What if the array is `[Integer.MAX_VALUE, 1]`?"

**Candidate:** "Um..."

**Execution:**
```java
sum = 0
sum += Integer.MAX_VALUE  // sum = 2147483647
sum += 1                  // sum = -2147483648 (overflow!)
return -2147483648 ❌
```

**What went wrong:**
Candidate didn't apply the Chapter 4 edge case checklist:
- ✗ Null inputs
- ✗ Empty collections
- ✗ Boundary values (min/max, 0, negative)
- ✗ Overflow conditions

**Correct implementation:**
```java
public static long sumArray(int[] arr) {
    if (arr == null) {
        throw new IllegalArgumentException("Array cannot be null");
    }

    long sum = 0;  // Use long to prevent overflow
    for (int num : arr) {
        sum += num;
    }
    return sum;
}

// Test cases:
// Normal: [1,2,3,4,5] → 15
// Empty: [] → 0
// Negative: [-1,-2,-3] → -6
// Max values: [Integer.MAX_VALUE, 1] → 2147483648 (no overflow)
// Null: null → IllegalArgumentException
```

#### Case Study 6.2: The SQL Injection (Revisited)

**Scenario:**
Database query function.

**What happened:**
```java
public List<User> searchUsers(String name) {
    String query = "SELECT * FROM users WHERE name = '" + name + "'";
    // Execute query...
}
```

Candidate tested with `name = "Alice"` → Works ✓

Submitted without security review.

**Interviewer:** "What if someone passes `'; DROP TABLE users; --` as the name?"

**Candidate:** "Oh no..."

**What went wrong:**
Skipped the Chapter 4 security checklist:
- ✗ Input validation
- ✗ SQL injection check
- ✗ Parameter sanitization

This is the CRITICAL security vulnerability from Chapter 4. Should never make it past review.

**The 30-Second Review Checklist (From Chapter 4):**
```
1. CORRECTNESS (10 sec): Logic bugs?
2. EDGE CASES (10 sec): Null, empty, boundaries?
3. EFFICIENCY (5 sec): Complexity optimal?
4. READABILITY (3 sec): Clear names, structure?
5. SECURITY (2 sec): Vulnerabilities?

TOTAL: 30 seconds that could save the interview!
```

#### Case Study 6.3: The Hidden Bug

**Scenario:**
Find first unique character in a string.

**What happened:**
```java
public static int firstUniqChar(String s) {
    Map<Character, Integer> count = new HashMap<>();

    // Count occurrences
    for (char c : s.toCharArray()) {
        count.put(c, count.getOrDefault(c, 0) + 1);
    }

    // Find first unique
    for (char c : s.toCharArray()) {
        if (count.get(c) == 1) {
            return s.indexOf(c);
        }
    }

    return -1;
}
```

Candidate tested: `"leetcode"` → returns `0` (index of 'l') ✓

Submitted.

**Interviewer:** "Test with `"loveleetcode"`"

**Execution:**
```
Input: "loveleetcode"
After counting: {l=2, o=2, v=1, e=4, t=1, c=1, d=1}
First loop finds 'v' (count=1) at position 0 in second iteration
Returns: s.indexOf('v') = 2 ✓

Actually correct! But...
```

**Interviewer:** "What's the time complexity?"

**Candidate:** "O(n) for counting, O(n) for finding, so O(n) overall."

**Interviewer:** "Look at line 11 more carefully."

**Candidate:** "Oh! `indexOf()` is O(n), and I'm calling it inside an O(n) loop. It's O(n²)!"

**What went wrong:**
Code worked but had hidden inefficiency. Candidate didn't analyze complexity carefully.

**Optimized version:**
```java
public static int firstUniqChar(String s) {
    if (s == null || s.isEmpty()) {
        return -1;
    }

    Map<Character, Integer> count = new HashMap<>();

    // Count occurrences
    for (char c : s.toCharArray()) {
        count.put(c, count.getOrDefault(c, 0) + 1);
    }

    // Find first unique - O(n) since we already have index from iteration
    for (int i = 0; i < s.length(); i++) {
        if (count.get(s.charAt(i)) == 1) {
            return i;  // Already have the index!
        }
    }

    return -1;
}

// Time: O(n) not O(n²)
// Space: O(k) where k = number of unique characters
```

**Testing protocol:**
```
NEVER SUBMIT WITHOUT:
1. Mental trace with sample input
2. Test with NULL
3. Test with EMPTY
4. Test with BOUNDARY values
5. Test with EDGE cases
6. Verify COMPLEXITY claims
7. Check SECURITY (if applicable)

TIME INVESTMENT: 30-60 seconds
RETURN: Catching bugs before interviewer does = HUGE WIN
```

---

### Anti-Pattern 7: Copy-Paste Syndrome (The "Black Box" Trap)

**Definition:** Using code without understanding how it works, inability to modify or debug it independently.

**Why it happens:**
- Treating AI as a black box oracle
- Not reading generated code carefully
- Accepting solutions beyond current skill level

**What interviewers observe:**
- Can't modify code when asked
- Can't debug when issues arise
- Can't answer "why" questions
- Code quality inconsistent with candidate's explanations

#### Case Study 7.1: The Regex Mystery

**Scenario:**
Validate phone number format: (123) 456-7890

**What happened:**
AI generated:
```java
public static boolean isValidPhone(String phone) {
    if (phone == null) return false;
    return phone.matches("\\(\\d{3}\\)\\s\\d{3}-\\d{4}");
}
```

**Candidate:** "Looks good!" ✓ Submitted.

**Interviewer:** "Explain this regex."

**Candidate:** "It, uh, matches the phone number format..."

**Interviewer:** "What does `\\d{3}` mean?"

**Candidate:** "It's... something with digits?"

**Interviewer:** "What would happen if I pass `(123) 456-7890x999`?"

**Candidate:** "It would... um..."

**Execution:**
```java
// Using find() instead of matches():
Pattern pattern = Pattern.compile("\\(\\d{3}\\)\\s\\d{3}-\\d{4}");
pattern.matcher("(123) 456-7890x999").find()  // Returns TRUE! Bug! 🔴
```

**What went wrong:**
1. Didn't understand regex syntax
2. Used `find()` which matches substrings, not `matches()` which matches entire string
3. Can't debug or modify

**Correct implementation:**
```java
public static boolean isValidPhone(String phone) {
    if (phone == null) return false;

    // \\(          - Literal open paren (escaped)
    // \\d{3}       - Exactly 3 digits
    // \\)          - Literal close paren
    // \\s          - Single whitespace
    // \\d{3}       - Exactly 3 digits
    // -            - Literal hyphen
    // \\d{4}       - Exactly 4 digits

    return phone.matches("\\(\\d{3}\\)\\s\\d{3}-\\d{4}");
}

// Now can explain each part!
```

**Interviewer:** "Why does this work without anchors `^` and `$`?"

**Candidate:** "Java's `matches()` method already requires the entire string to match—it implicitly anchors to the start and end. If I wanted to check if the pattern exists anywhere in the string, I'd use `Pattern.compile(...).matcher(phone).find()` instead. That's a common source of confusion."

#### Case Study 7.2: The Stream API Black Box

**Scenario:**
Group list of strings by their first character.

**AI generated:**
```java
public static Map<Character, List<String>> groupByFirstChar(List<String> words) {
    return words.stream()
                .filter(s -> s != null && !s.isEmpty())
                .collect(Collectors.groupingBy(s -> s.charAt(0)));
}
```

**Candidate:** ✓ Submitted without review.

**Interviewer:** "Walk me through this code."

**Candidate:** "It uses streams to group the words..."

**Interviewer:** "What does `groupingBy` do specifically?"

**Candidate:** "It groups them..."

**Interviewer:** "Can you implement this without streams?"

**Candidate:** "Uh... I guess I could..."

[Struggles for 3 minutes, can't implement it]

**What went wrong:**
Candidate used Stream API without understanding the underlying logic. When asked to implement without it, couldn't.

**What candidate should have done:**

**Step 1:** Understand the stream version
```java
// Stream version breakdown:
// 1. words.stream() - Convert list to stream
// 2. filter(...) - Remove nulls and empty strings
// 3. groupingBy(s -> s.charAt(0)) - Group by first character
//    - Creates a Map<Character, List<String>>
//    - For each string, extracts classifier (first char)
//    - Adds string to list associated with that character
```

**Step 2:** Can implement manually if needed
```java
public static Map<Character, List<String>> groupByFirstCharManual(List<String> words) {
    Map<Character, List<String>> result = new HashMap<>();

    for (String word : words) {
        // Filter: skip nulls and empty
        if (word == null || word.isEmpty()) {
            continue;
        }

        // Group by first character
        char firstChar = word.charAt(0);

        // Get or create list for this character
        result.computeIfAbsent(firstChar, k -> new ArrayList<>())
              .add(word);
    }

    return result;
}

// Now I understand WHAT groupingBy does under the hood!
```

**Interviewer:** "Much better. Now you clearly understand the operation, whether using streams or not."

#### Case Study 7.3: The Algorithm Import

**Scenario:**
Implement Dijkstra's shortest path algorithm.

**What happened:**
AI generated 150 lines of Dijkstra's implementation with priority queue, graph representation, distance tracking, etc.

Candidate copy-pasted it all.

**Interviewer:** "Explain how this algorithm works."

**Candidate:** "It finds the shortest path using a priority queue..."

**Interviewer:** "Why do we need the priority queue specifically?"

**Candidate:** "To... keep track of the vertices?"

**Interviewer:** "What's the time complexity and why?"

**Candidate:** "Um... O(n log n)? Because of sorting?"

**Interviewer:** "What happens when we relax an edge?"

**Candidate:** [blank stare]

**What went wrong:**
Used advanced algorithm without understanding. Classic black box syndrome.

**Better approach:**

**Option 1:** If you don't understand it, say so
```
"I recognize this is Dijkstra's algorithm, but I'm not confident I could
explain or debug it in detail. For this interview, would you prefer I:
a) Implement a simpler BFS solution that I can fully explain, or
b) Use this implementation and we can discuss the parts I understand?"
```

This honesty shows self-awareness and often impresses interviewers more than bluffing.

**Option 2:** Study the code before submitting
```
1. Read through the generated code
2. Add comments explaining each section
3. Trace through with a small example
4. Verify you can answer: What, Why, How, Complexity
5. Only THEN submit
```

**Option 3:** Ask AI to explain
```
Prompt: "Explain this Dijkstra implementation step-by-step, including:
1. Why we use a priority queue
2. What 'relaxing an edge' means
3. Time and space complexity with explanation
4. Trace through example: graph with 4 nodes"
```

Then read and understand the explanation BEFORE submitting the code.

**The Understanding Checklist:**
```
BEFORE USING CODE YOU DIDN'T WRITE:

□ Can I explain what each major section does?
□ Can I trace through it with a concrete example?
□ Do I know the time and space complexity?
□ Could I debug it if it had a bug?
□ Could I modify it if requirements changed?
□ Do I understand the data structures used?
□ Can I explain the algorithm's key insights?

IF ANY NO: Don't use it, simplify the problem, or study first!
```

---

## Interview-Specific Pitfalls

Beyond the seven anti-patterns, there are interview-specific behaviors that can sink your performance:

### Pitfall 1: Silent Programming

**Scenario:**
Candidate receives problem, immediately starts typing prompts to AI, says nothing for 3 minutes.

**Interviewer's perspective:**
> "I have no idea what they're thinking. Are they understanding the problem? Do they have an approach? This is not a collaborative interview—it's me watching someone use a chatbot."

**What's wrong:**
Interviews evaluate communication as much as coding. Silence breaks collaboration.

**Better approach:**
```
TALK WHILE YOU WORK:

"Let me think through this problem first..."
[Analyze problem aloud]

"I'm going to use a HashMap approach because..."
[Explain reasoning]

"Now I'll have AI generate the basic structure..."
[Show what you're prompting]

"Let me review this code for edge cases..."
[Walk through your review process]

GUIDELINE: Interviewer should understand your thought process at all times.
```

### Pitfall 2: AI Blame-Shifting

**Scenario:**
Code has a bug.

**Candidate:** "The AI made a mistake here..."

**Interviewer thinking:** 🚩 Red flag. You submitted it. It's your code.

**What's wrong:**
Taking credit for success but blaming AI for failure demonstrates lack of ownership.

**Better approach:**
```
❌ "The AI generated buggy code"
❌ "The AI didn't handle this edge case"
❌ "The AI's implementation has an issue"

✅ "I missed this edge case in my review"
✅ "My prompt wasn't specific enough about this requirement"
✅ "I should have tested this scenario—let me fix it"

PRINCIPLE: You own the code, regardless of who/what wrote it.
```

### Pitfall 3: The Debug Spiral

**Scenario:**
Code doesn't work. Candidate keeps re-prompting AI with error messages, getting new code, running, getting new errors, repeat...

**After 5 iterations:**
- Still not working
- Candidate increasingly frustrated
- No systematic debugging
- Just hoping AI will magically fix it

**What's wrong:**
Demonstrates inability to debug independently. Treating AI as a magic 8-ball.

**Better approach:**
```
WHEN CODE DOESN'T WORK:

1. STOP re-prompting after 2 attempts
2. READ the code carefully
3. TRACE through execution manually
4. IDENTIFY the bug location
5. EXPLAIN the bug to interviewer
6. FIX it yourself (or with targeted AI help)

SHOW: "I can debug systematically, not just rely on trial-and-error prompting"
```

**Example:**
```
"I've tried having AI fix this twice but the bug persists. Let me trace
through manually to find the root cause..."

[Traces through code with sample input]

"Ah, I see the issue—the loop boundary is off by one on line 12.
The condition should be `i < arr.length` not `i <= arr.length`.
This causes an ArrayIndexOutOfBoundsException on the last iteration."

[Fixes the single line]

"There we go. This demonstrates that I can identify and fix bugs
independently when AI suggestions don't immediately work."
```

### Pitfall 4: Neglecting Complexity Analysis

**Scenario:**
Candidate implements solution, runs tests, everything passes.

**Interviewer:** "What's the time complexity?"

**Candidate:** "Um... let me think..." [Long pause] "O(n)?"

**Interviewer:** "Are you sure? I see nested loops on lines 5-9."

**Candidate:** "Oh right, maybe O(n²)?"

**What's wrong:**
AI writes the code but doesn't always provide complexity analysis. Candidate must do this independently.

**Better approach:**
```
ALWAYS ANALYZE BEFORE CLAIMING DONE:

1. Identify loops: How many times does each run?
2. Identify operations inside loops: What's their complexity?
3. Multiply: Nested loop doing O(1) work? That's O(n²)
4. Identify space: What data structures, what size?
5. STATE the complexity confidently

TEMPLATE:
"This solution has time complexity O([X]) because [reason].
Space complexity is O([Y]) because [reason].
This is optimal for this problem because [explanation]."
```

### Pitfall 5: No Test-Before-Claim

**Scenario:**
Candidate gets code, glances at it, immediately says "This looks good!"

**Interviewer:** "Did you test it?"

**Candidate:** "Well, it looks correct..."

**Interviewer:** "Test it now."

[Tests, finds bug]

**What's wrong:**
Claiming correctness without verification is overconfident and sloppy.

**Better approach:**
```
NEVER SAY "DONE" WITHOUT:

1. Mental trace with sample input
2. Running actual tests (if environment allows)
3. Checking edge cases explicitly
4. Stating: "I've tested [X], [Y], [Z] cases and they all pass"

PHRASE IT:
"Let me verify this works..." [Test]
"I've tested normal cases, edge cases, and null inputs—all pass."
"Now I'm confident this is correct."
```

---

## Recovery Strategies

Everyone makes mistakes. Strong candidates recover well. Here's how:

### Recovery Strategy 1: Self-Awareness Check

**When to use:** You realize you've been doing something wrong (any anti-pattern).

**What to do:**
1. **Acknowledge it:** "I realize I've been spending too much time on variable names. Let me refocus on the core logic."
2. **Pivot:** Stop the bad behavior immediately
3. **Move forward:** Don't dwell on the mistake

**Example:**
```
[Candidate has made 5 small iterations on working code]

Candidate: "Actually, I've been over-iterating on small details. This code
works correctly and handles edge cases. Let me move on to testing instead
of continuing to polish."

Interviewer: [impressed by self-awareness]
```

### Recovery Strategy 2: Reset the Problem

**When to use:** You're stuck in iteration hell or debug spiral.

**What to do:**
1. **Pause:** "Let me step back for a moment."
2. **Restate the problem:** "The core requirement is [X]. Let me reconsider my approach."
3. **Simplify:** Start with simpler version or different approach
4. **Explain your reset:** "I was getting caught in refinement details. Let me refocus on the essential requirements."

**Example:**
```
[After 4 failed attempts to fix a bug]

Candidate: "I've been trying to patch this incrementally, but let me take a
different approach. The core issue is [explains bug]. Instead of fixing the
existing code, let me reimplement this function with a clearer algorithm."

[Rewrites from scratch in 2 minutes, works correctly]

Interviewer: "Good decision to restart rather than continuing down the
wrong path."
```

### Recovery Strategy 3: Ask for Direction

**When to use:** You're unsure if you're on the right track or spending time on the right things.

**What to do:**
1. **State your situation:** "I have a working solution with [properties]."
2. **Ask explicitly:** "Would you like me to optimize this or move to the next part?"
3. **Get clarity:** Let interviewer guide time allocation

**Example:**
```
Candidate: "I have a working O(n log n) solution using sorting. I could
optimize to O(n) with a HashMap, but that would take another 5-7 minutes.
Given our time budget, what would you prefer?"

Interviewer: "The O(n log n) solution is fine. Let's discuss trade-offs
instead and move on."

[Saves 5 minutes, demonstrates good judgment]
```

### Recovery Strategy 4: Acknowledge and Fix

**When to use:** Interviewer points out an error.

**What to do:**
1. **Acknowledge:** "You're absolutely right. Good catch."
2. **Explain the error:** "I missed the edge case where..."
3. **Fix it immediately:** [Correct the code]
4. **Verify the fix:** "Let me test this edge case now... [test] ... confirmed fixed."

**Example:**
```
Interviewer: "This fails for empty arrays."

Candidate: "You're absolutely right—I didn't add a null and empty check.
That's a critical oversight. Let me add that now."

[Adds check]

if (arr == null || arr.length == 0) {
    throw new IllegalArgumentException("Array cannot be null or empty");
}

"Now testing with empty array... [test] ... correctly throws exception.
Thank you for catching that."

[Shows they can take feedback and fix issues quickly]
```

### Recovery Strategy 5: Learn and Apply

**When to use:** You made a mistake and don't want to repeat it.

**What to do:**
1. **Internalize the lesson:** Understand why the mistake happened
2. **Apply immediately:** Use the learning on the next problem
3. **Mention it:** "Based on the earlier edge case I missed, I'm making sure to check for null and empty inputs first this time."

**Example:**
```
[First problem: Missed null check, interviewer caught it]

[Second problem: Candidate's first action]

Candidate: "Before implementing, let me identify the edge cases to handle:
- Null input
- Empty collection
- Single element
- Boundary values

I want to make sure I don't miss these like I did earlier."

[Interviewer nods approvingly—candidate learned and adapted]
```

---

## The Anti-Pattern Detection Checklist

Use this during the interview to self-monitor:

```
EVERY 2-3 MINUTES, QUICK CHECK:

□ Am I understanding the code I'm submitting? (Anti-Pattern 1)
□ Am I using AI for things it's good at? (Anti-Pattern 2)
□ Have I been working on this prompt for >30 seconds? (Anti-Pattern 3)
□ Is this my 4th+ iteration on working code? (Anti-Pattern 4)
□ Is my prompt longer than the code will be? (Anti-Pattern 5)
□ Have I tested edge cases and complexity? (Anti-Pattern 6)
□ Can I explain and modify this code? (Anti-Pattern 7)

□ Am I talking through my process? (Interview Pitfall 1)
□ Am I owning my code? (Interview Pitfall 2)
□ Am I debugging systematically? (Interview Pitfall 3)

IF ANY RED FLAG: Stop, acknowledge, pivot.
```

---

## Practical Application

### Exercise 5.1: Recognizing Anti-Patterns

**Difficulty:** 🟡 Intermediate
**Time:** 30-45 minutes
**Goal:** Practice identifying anti-patterns in realistic interview scenarios and applying recovery strategies

See [Exercise 5.1](../exercises/exercise-05-01-recognizing-antipatterns.md) for multiple scenarios where you'll identify which anti-pattern is occurring and suggest how to recover.

---

## Key Takeaways

✅ **Anti-patterns are predictable:** Learn to recognize them in yourself

✅ **Over-reliance is the #1 killer:** Always understand code before submitting

✅ **Perfect is the enemy of good:** Stop after 3 iterations on working code

✅ **Context quality > quantity:** Minimal sufficient information in prompts

✅ **Test everything:** Never claim "done" without verification

✅ **Own your code:** Whether you or AI wrote it, you're responsible

✅ **Recover gracefully:** Self-awareness and course correction are strengths

✅ **Communicate constantly:** Silence is failure in collaborative interviews

✅ **Know when to stop:** Time management is part of the evaluation

✅ **Trust but verify:** Use Chapter 4's checklist on all AI code

---

## Self-Assessment

Rate your risk for each anti-pattern (1-5, where 5 = high risk):

- [ ] **Over-Reliance:** Do I accept code without understanding?
- [ ] **Under-Utilization:** Do I avoid using AI when I should?
- [ ] **Prompt Paralysis:** Do I overthink prompts?
- [ ] **Iteration Hell:** Do I keep refining working code?
- [ ] **Context Overload:** Do I provide too much irrelevant information?
- [ ] **Trust Without Verification:** Do I skip thorough testing?
- [ ] **Copy-Paste Syndrome:** Do I use code I don't understand?

**For any 4+:** Review that section and complete Exercise 5.1

**For any 3 or below:** You're aware of the risk, stay vigilant

---

## Real Interviewer Perspectives

We asked hiring managers what anti-patterns concern them most:

**Senior Engineer at Google:**
> "The biggest red flag is when candidates can't explain code they just submitted. If you used AI, that's fine—but you better understand what it generated. I've seen candidates freeze when I ask 'walk me through this,' and that's an instant no."

**Tech Lead at Microsoft:**
> "I actually like when candidates make a mistake and catch it themselves. It shows metacognition. What I don't like is endless iteration on trivial things. If you're on your fifth refinement of variable names, you've lost the plot."

**Startup CTO:**
> "We had a candidate paste our entire 500-line file into ChatGPT. The AI took 20 seconds to respond and gave generic advice. That told me they don't understand how to use the tool effectively. Context quality matters more than quantity."

**FAANG Interviewer:**
> "Silent coding is the worst. I need to understand your thought process. If you're just typing prompts to an AI without explaining what you're doing or why, I can't evaluate you properly. This is a collaborative exercise, not a solo coding session."

---

## Additional Resources

- [Common Mistakes in Coding Interviews](https://www.interviewcake.com/article/python/coding-interview-mistakes)
- [The Art of Debugging](https://pragprog.com/titles/pbdp/the-art-of-debugging/)
- [Thinking, Fast and Slow](https://en.wikipedia.org/wiki/Thinking,_Fast_and_Slow) - Cognitive biases relevant to anti-patterns
- [The Pragmatic Programmer](https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/) - Ownership and craftsmanship
- Chapter 4 Review Checklist - Your primary defense against anti-patterns 6 & 7

---

## Next Steps

Congratulations! You've completed Part 1: Foundations. You now understand:
- What AI coding assistants are and when to use them (Chapter 1)
- How to craft effective prompts (Chapter 2)
- How to iterate productively (Chapter 3)
- How to validate code systematically (Chapter 4)
- How to avoid common pitfalls (Chapter 5)

**Before moving to Part 2:**
- ✅ Complete Exercise 5.1: Recognizing Anti-Patterns
- ✅ Review your self-assessment scores from all chapters
- ✅ Practice 2-3 LeetCode problems using all techniques learned
- ✅ Record yourself and check for anti-patterns
- ✅ Ready to move to Part 2: Interview Context Applications (Coming Soon)

---

[← Previous Chapter](chapter-04-code-validation.md) | [↑ Part 1 Overview](../PART1-FOUNDATIONS.md) | Next: Part 2 (Coming Soon)
