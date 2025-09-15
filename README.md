# Trainline – 2nd Round Technical Interview

## Introduction
Welcome to the **Trainline 2nd Round Technical Interview – Tech Test**.

This will be a **90-minute pair programming exercise** where you’ll work with one of our engineers to solve a practical coding challenge.

Before diving in, please take a moment to read these guidelines carefully so you know what to expect and how best to prepare.

### What to Expect
- The exercise is **open-ended**: you can solve it in any way you think is appropriate.
- We expect you to approach the task as if writing production-ready code:
    - Use good programming practices
    - Apply appropriate design patterns
    - Consider testability
    - Think about API design
    - Make good use of modern frameworks/language features
- The goal is **not** to simply finish the challenge, but to demonstrate your thought process, coding style, and how you collaborate when solving problems.

### Tools & Resources
- You are free to use **Google, Stack Overflow, or any online reference**.
- You may use **any OSS packages/nuget code** you wish.
- You may use **any IDE**.
- A blank starter solution is available for you to [download/clone](https://github.com/trainlinerecruitment/starter-solution-csharp).

### Preparation
- Please **do not bring pre-written code**. We’d like to see how you think and work during the session.
- If you want to do some prep beforehand, that’s fine—but **not expected**. We know your time is valuable.

### The Interview
- Treat this like pairing with a teammate. Your interviewer will be there to help - you can ask questions, talk through ideas.
- Most importantly: **enjoy it**! We want this to be a positive experience.

Good luck! 😃

---

## Scenario

We need to implement a **room booking and availability service** as a simple RESTful API.

Room availability is obtained by calling an external service. For this exercise, the external service is simulated by fetching JSON data from a file on GitHub.

### Example Availability Response

```json
{
  "availability": {
    "monday":    "000000000011111111110011100010100011101010110100",
    "tuesday":   "000001100111100011110011111110100011101111110100",
    "wednesday": "000000000011111111110000000000000000001010000100",
    "thursday":  "000000000011100111110011100011111111101010110100",
    "friday":    "000000000011100101110010011111101110011111101100"
  }
}
```

### How to Interpret the Data
- Each string represents **48 half-hour slots** in a day (00:00 → 23:30).
- **0** = free, **1** = booked.
- Example: On Monday, the room is first booked at **05:00** and remains booked until **10:00**.
- Only weekdays are included (no Saturday/Sunday).

---

## Your Task

Build a RESTful API with the following functionality:

1. **Retrieve room availability for all days of the week**

2. **Retrieve room availability for a given day of the week**
    - Day may be specified as a string (`"monday"`…, `"friday"`) or number (`1` = Monday, …, `5` = Friday).

3. **Check if a room is free at a specific time and duration**
    - Example: “Is room X available on Tuesday at 14:30 for 90 minutes?”
    - If the day/time is invalid, return an appropriate error response.

4. **Resilience**
    - Assume the upstream service is unstable. Your solution should handle errors gracefully (timeouts, retries, fallbacks, etc.).

5. **Human-readable output**
    - You decide the exact format, but here’s an example:

```json
{
  "room": "xyz",
  "schedule": [
    {
      "day": "monday",
      "availability": {
        "00:00": false,
        "00:30": false,
        "01:00": false,
        // removed for brevity...
        "23:00": false,
        "23:30": false
      }
    },
    {
      "day": "tuesday",
      "availability": {
        "00:00": true,
        "00:30": true,
        "01:00": true,
        // removed for brevity...
        "23:00": false,
        "23:30": false
      }
    }
  ]
}
```

---

## Notes & Constraints

- Use the provided [example availability file on GitHub](https://raw.githubusercontent.com/trainlinerecruitment/room-availability/main/availability.json) to simulate availability.
- The API design should allow querying **by room name**, but for this exercise assume all rooms share the same sample availability above.
- The physical date is not relevant; availability repeats weekly at "day of week" level.
- When presenting results, always show slots in **30-minute increments**.
- Design your solution so that the upstream URI can be easily swapped to point to a real service later.

---

## Candidate Tips

Here are some suggestions to help you succeed in this session:

- **Think aloud**: Talk through your approach, trade-offs, and reasoning. This helps us understand how you problem-solve.
- **Clarify assumptions early**: If anything is unclear (e.g., input format, edge cases), ask questions. Real-world coding is about collaboration.
- **Iterate incrementally**: Start with a simple working solution, then refine it with tests, error handling, and improvements.
- **Use tests to guide you**: Writing a quick unit test can make it easier to validate your logic and show us how you approach testability.
- **Balance speed and quality**: Don’t get stuck on polishing everything - focus on showing a pragmatic, production-minded approach.
- **Show resilience thinking**: Handle failures (timeouts, bad inputs) in a way that would make sense in a real-world system.
- **Communicate with your interviewer**: Treat them like a coding buddy - pairing is about teamwork as much as code.

**Remember:** we’re more interested in **how you think** than whether you finish every requirement.  
