# Room Availability Service - Technical Test

## Introduction

Welcome to the Trainline 2nd Round Interview - Tech Test. 

This will be a 90 minute pair programming style exercise in which you will attempt to solve the scenario below, but, <u>before you do here are a few guidelines to help guide you and ensure you can do your best on the day, so please read them carefully</u> 🙂

1. The test is designed to be open ended, by which we mean you are free to tackle it in any way you see fit.
1. We ask that you tackle this test as you would any code you were writing for a production system - Example: best practices vs hacking.
1. The purpose of this exercise is not to get to the end, but to show us the process you use to get there. We are trying to assess not only how you code but also the thought process you follow to get to the solution.
1. The sort of things we want to see are traits such as, but not limited to: 
    - Good programming practices
    - Good use of patterns
    - Thinking about testability of code
    - Good api design
    - Use of modern framework/language features
1. This **is not a closed book** test, you are free to Google search or Stack Overflow any answers you need.
1. You may use any OSS packages/nuget code you wish
1. We would prefer you not to prepare any code ahead of time, because we want to pair with you and see how you think/solve problems, but we appreciate that this can be stressful so if you wish to spend time outside the interview to prep then please do so; but please remember we don't expect it because your time is valuable.
1. You are free to use any IDE you wish, and if you would like a blank solution to bootstrap your work, [you can find one here to download/clone](https://github.com/trainlinerecruitment/starter-solution-csharp)
1. Have fun, we want you to enjoy this experience, your interviewers will be on hand to help you, use them like a coding buddy.

Good luck! 😃

---

## Scenario

We have been tasked with implementing a room booking/availability service as a simple RESTful API. A rooms availability is obtained by calling an external service, which for the purposes of this exercise will be emulated by making HTTP requests to a file accessible on GitHub.

An example `room availability response` consists of a json object containing daily availability attributes; each consisting of 48 characters (`1` or `0`).

 Example:
 ```json
 {
   "availability": {
       "monday": "000000000011111111110011100010100011101010110100",
       "tuesday": "000001100111100011110011111110100011101111110100",
       "wednesday": "000000000011111111110000000000000000001010000100",
       "thursday": "000000000011100111110011100011111111101010110100",
       "friday": "000000000011100101110010011111101110011111101100",
     }
 }
 ```

 This file structure shows:
 - The first character for a days schedule indicates `00:00` or "midnight"
 - Each character represents a 30 min time slot of the day
 - The last character for a days schedule indicates `23:30`
 - If a character is `0` the room is free for that 30 min window
 - If a character is `1` the room is booked for that 30 min window
 - So for the example room above `Monday` the room is first booked at `05:00` until `10:00`
 - There is no entry for Saturday or Sunday as these are not valid days to use meeting rooms.

---

## Your Task

 Create an API that allows users to query the upstream booking service. 
 - Your API needs to implement the following API actions:
   - Provide a mechanism to retrieve the availability for a room for a given `day of the week`. 
   - The `day of the week` can be specified in numerical format (`1 == Monday, 2 == Tuesday` etc.) or in string format (`Monday, Tuesday` etc.)
   - Provide a mechanism to retrieve the availability for a room for all days of the week.
   - Provide a mechanism to check if a room is free on a specific `day of the week` and `time of day` for a specified `duration in minutes`.
   - If a request is made for an invalid day or time then an appropriate response should be returned.
   - We have identified that the real availability service has stability issues, when making external calls we need to ensure we have resilience implemented.
   - When returning your response we require a human readable format. This format is up to you, but an example is shown below:
  ```json
  {
      "room": "xyz",
      "schedule": [
          {
            "day": "monday",
            "availability": {
                "00:00": false,
                "00:30": false,
                "01:00": false
                ...
            }
          },
          {
              "day": "tuesday",
              "availability": {
                "00:00": true,
                "00:30": true,
                "01:00": true
                ...
            }
          }
      ]
  }
  ```

## Notes

 For the purpose of this exercise:
 - You can [utilize the example room availability file located here](https://raw.githubusercontent.com/trainlinerecruitment/room-availability/main/availability.json) to simulate all room availabilities; hosted on GitHub.
 - For the purpose of this test we are using the dummy file above to simulate room availability, but eventually we must be able to easily swap the uri to a real endpoint, keep this in mind when designing your solution.
 - Your api design needs to enable querying availability per `room name` but for this exercise assume all rooms will use/have the same availability.
 - The physical date is not important, you can assume every week the schedule is the same for all rooms, only the `day of the week` is required for requests.
 - When showing the room availability in responses, it should be rendered in 30 min increments.
