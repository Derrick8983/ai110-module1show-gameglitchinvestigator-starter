# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
When I first ran the game it looked simple but I could already tell something was off. The hints were giving wrong directions and the new game button was not fully working.

- List at least two concrete bugs you noticed at the start
  (for example: "the secret number kept changing" or "the hints were backwards").

---The "Go Higher" and "Go Lower" hints were swapped — when your guess was too high it told you to go higher, which sent you in the wrong direction.
The new game button did not reset the score, status, or history, so old game data carried over into the new game.

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)? Claude
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).

    Claude correctly identified that the hint messages were swapped and fixed them so "Too High" maps to "Go LOWER!" and "Too Low" maps to "Go HIGHER!". I verified it by running the game and confirming the hints pointed me in the right direction toward the secret number.

- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).
At first Claude suggested the secret number was stable, but it was actually regenerating whenever the difficulty changed because the session state check did not account for difficulty. I caught this because switching difficulty mid-game would give a new secret without starting a new game.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
I had a clear idea of how the game should behave, so once it matched that expectation I knew the fix worked.  also ran pytest to confirm the logic functions passed all three test cases.

- Describe at least one test you ran (manual or using pytest)
  and what it showed you about your code.
  Running pytest on test_game_logic.py showed that check_guess returned the correct outcome strings for a win, a too-high guess, and a too-low guess. Before logic_utils.py was implemented those tests failed with a NotImplementedError, which confirmed the stubs were the problem.

- Did AI help you design or understand any tests? How?
Yes, Claude helped me understand that the test file expected check_guess to return just the outcome string like "Win", not a tuple, so logic_utils had to be implemented differently from the version in app.py.

---

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.
Streamlit reruns the entire script from top to bottom every time the user interacts with the page. Without session state, the random.randint line ran on every rerun and picked a new number each time.

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
Think of Streamlit like a whiteboard that gets completely erased and redrawn every time you click anything. Session state is like a sticky note you put on the board before it gets erased — whatever you write on the sticky note survives the reset.

- What change did you make that finally gave the game a stable secret number?
We stored the secret in st.session_state and only regenerated it when the difficulty actually changed by comparing the current difficulty to the one saved in secret_difficulty. That way the secret stays fixed for the whole game unless you switch difficulty or start a new game.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
I really liked the #FIX ME comment approach — it let me mark broken spots in the code and come back to them without losing track. I want to keep using comments like that as a lightweight way to track what still needs work.
  - This could be a testing habit, a prompting strategy, or a way you used Git.

- What is one thing you would do differently next time you work with AI on a coding task? I want to try to understand each fix before accepting it instead of just running the code and seeing if it works. If I can explain why a change fixes the bug then I actually learned something.

- In one or two sentences, describe how this project changed the way you think about AI generated code.
This project showed me that AI-generated code can have real, sneaky bugs — the game looked like it worked but the hints were backwards and the score system had glitches. Reviewing AI code critically instead of trusting it automatically is a skill I need to keep building.


I litterly answered all teh questions and claude changed most of my answer.. so yea