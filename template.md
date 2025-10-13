# Daily LeetCode Practice Template

Use this template for each day's practice notes. Copy a filled template into `daily.md` (or paste under the day's heading) after your session.

---

Date: YYYY/MM/DD
Day: DAYn
Duration: (e.g. 1h30m)
Problems attempted: #
Problems solved: #
Focus / Skill tags: (e.g. DP, Greedy, Two Pointers, Bit Manipulation)
Mood / Notes: (optional quick remark)

---

## Quick Summary
- Main goal for today: 
- Top takeaways: (1-2 bullets)
- Confidence: (Low/Medium/High)

---

## Problems

Repeat the `Problem` block below for each problem you worked on (1..N). If you did only one problem, fill only Problem 1.

### Problem 1 — [Problem Title](LEETCODE_LINK) — #ID
- Status: Solved / Attempted / Stuck
- Difficulty: Easy / Medium / Hard
- Time spent: (e.g. 45m)
- Language: (C++/Python/Java/...)

#### Approach (short)
Write a 2-4 sentence summary of the approach you used. Mention the main trick or insight.

#### Key idea / observations
- Bullet the critical idea(s) that made the solution work.
- If you used a pattern (sliding window, binary search on answer, etc.) mention it here.

#### Complexity
- Time: O(...)
- Space: O(...)

#### Code
Put your final code here (fenced code block). If you used more than one language, include both and mark the main one.

```cpp
// Example (C++)

```

#### Mistakes / Pitfalls / Debug notes
- List mistakes you made or tricky test-cases that caught you.
- Note why a wrong approach failed if you tried one.

#### Related problems / follow-ups
- Links or ids to related LeetCode problems to practice later.

---

### Problem 2 — [Problem Title](LEETCODE_LINK) — #ID
- (Repeat fields as above)

---

## Retrospective
- What went well:
- What to improve (next time):
- Concrete next-day goal (e.g. "Daily: 1 hard + 2 medium; revisit sliding-window")

## Files / Snippets added
- If you saved code to a file, list path(s) here (e.g. `./solutions/2025-10-13/problem_1488.cpp`).

## Links / References
- Official problem: (link)
- Editorial / blog: (link)

---

## Minimal example (1 problem day)

Date: 2025/10/13
Day: DAY25
Duration: 45m
Problems attempted: 1
Problems solved: 1
Focus / Skill tags: Greedy, Binary Search

Quick Summary
- Main goal: Practice a medium greedy problem
- Top takeaways: Learned to use set.lower_bound for scheduling
- Confidence: Medium

Problem 1 — Avoid Flood (leetcode-1488)
- Status: Solved
- Difficulty: Medium
- Time spent: 45m
- Language: C++

Approach (short)
- Use a map for last full day per lake and a set of dry days; greedily assign dry days using lower_bound.

Key idea
- Maintain available dry days in a set and always pick the earliest dry day after the lake's last fill.

Complexity
- Time: O(n log n)
- Space: O(n)

Code
```cpp
// paste your final C++ solution here
```

Mistakes
- Forgot to erase used dry day from the set on first attempt.

Related
- leetcode-2300 (binary search on sorted pairs) to practice binary search + vector operations.

Retrospective
- What went well: Greedy selection logic is solid.
- What to improve: Write tests for corner cases (no dry days).

---

## Multi-problem example (3 problems)

Date: 2025/10/13
Day: DAY25
Duration: 3h
Problems attempted: 4
Problems solved: 3
Focus / Skill tags: DP, Two Pointers, Hash Table

Problem 1 — ...
(Use Problem blocks above for each problem)

---

## How to use this template
1. Copy the template into a new section under today's `## DAYn: YYYY/MM/DD` in `daily.md` or keep a personal folder of daily notes and paste the filled version into `daily.md` later.
2. For multiple problems, duplicate the `Problem` block and number them sequentially.
3. Keep the 'Retrospective' short; it helps track long-term improvement.
4. Use `Files / Snippets added` to record which local source files you created so you can link them from `daily.md`.

---

Good luck — keep it short and consistent; the goal is discoverable improvements over time.