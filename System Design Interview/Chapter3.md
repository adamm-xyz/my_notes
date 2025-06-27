
# System Design Interview - Chapter 3

## Framework for interviews

### The interview is scary
- Interviews simulate real life problem solving
- Complex systems are deceptively simple (ex: Google search)
- Problems are open-ended, ambiguous and theres no perfect answer

### What does interviewer look for?
- Person's ability to collaborate
- Asking good questions
- Red flags: over-engineering, narrow mindedness, stubbornness

## 4-step Process for Effective Interview

### Step 1 - Understand problem and scope
- Slow down
- Think deeply and ask questions, clarify requirements
- Write down the assumptions

Example questions:
- What specific features are we building?
- How many users?
- What are anticipated scale ups (in 3 months, 6 months, 1 year)?
- What is the stack and existing services?

### Step 2 - Propose high-level design and get buy-in
- Work together to get a basic blueprint
- Draw key components (clients, APIs, servers, message queue,etc)
- Do "back-of-envelope" calculations
- Go through a few use cases

### Step 3 - Design deep dive
- Work with interviewer to find and prioritize certain components
- Sometimes interviewers want high-level focus or resource estimates (bottlenecks)
- Time management is essential, don't get carried away on certain details

### Step 4 - Wrap up
- Interviewer will either ask follow-up questions or give you the floor
- Find bottlenecks and potential improvements
- Give the interviewer a recap of design
- Talk about error cases (network loss,etc)
- Mention operation issues (metrics, logs, etc)
- Talk about how to scale up (1M->10M users)

## Conclusion

### Dos and Donts
**Dos**
- Ask for clarification, understand requirements
- Communicate, think out loud
- Suggest multiple approaches, bounce ideas
- If interviewer likes blueprint, dive into most critical components
**Don'ts**
- Don't be unprepared for typical questions
- Don't start detailed on one component
- Don't think you're done once you give design

### Time allocation
For a 45 minute interview, roughly:
- Step 1 - 3-10 minutes
- Step 2 - 10-15 minutes
- Step 3 - 10-25 minutes
- Step 4 - 3-5 minutes
