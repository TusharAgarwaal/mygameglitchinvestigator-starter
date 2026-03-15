# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the secret number kept changing" or "the hints were backwards").

---In one case, it told me to go lower when I entered 1 (lowest number). In another case, it asked to go higher than 100 (highest number).
For this, I think the higher/lower logic is causing the issue.
--- In other case, it showed as 8 attempts allowed but stopped me at 7. The attempt range might be counting wrong.
--- In the third case observed, the secret number was set based on the hardcoded range 1-100 instead of the range of the difficulty mode selected. So the secret number was out of range for the range of the mode selected.

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

--- I used Claude and Copilot on this project
--- From the bugs that were discovered, AI sugeested to change the hardcoded range which is shown in display while playing the game. The sugesstion seemed correct as the logic fixed not only the range in the display but also the secret number based on the difficulty selected.
--- The range displayed and the secret number changed everytime the difficulty mode is changed, fitting the range expected, which is logically correct.

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

--- The secret number was within the range fixed for each difficulty mode and the range displayed also was fixed. No error shown in the terminal as well.

--- While running the pytest, the following was shown:
============================= test session starts ==============================
platform linux -- Python 3.12.1, pytest-9.0.2, pluggy-1.6.0 -- /home/codespace/.python/current/bin/python
cachedir: .pytest_cache
rootdir: /workspaces/mygameglitchinvestigator-starter
plugins: anyio-4.11.0
collected 4 items                                                              

tests/test_game_logic.py::test_winning_guess PASSED                      [ 25%]
tests/test_game_logic.py::test_guess_too_high PASSED                     [ 50%]
tests/test_game_logic.py::test_guess_too_low PASSED                      [ 75%]
tests/test_game_logic.py::test_get_range_for_difficulty PASSED           [100%]

============================== 4 passed in 0.01s ===============================

--- Initially the pytest failed and that's where the AI assisted. It suggested the fix that was made was causing the issue and what next steps to take to remove the pytest error.

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.
- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
- What change did you make that finally gave the game a stable secret number?

--- The secret number is a random number generated but originally it was harcoded to be between 1 & 100 instead of the range of the mode selected.
--- I will explain my friend that this acts like an app and sometimes you need to close and rerun in case the app gets buggy like in the phone.
--- I changed the hardcoded range of the secret number to the range of the mode selected, which makes it fall within the expected range and betetr to guess it.

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

--- I will use AI as an assistant which will make things work faster and easier. Like making same changes to several places and in different files.
--- I will not allow AI to make direct changes without reviewing it as AI could be wrong or it misunderstood from the prompt I gave.
--- AI definitely makes coding faster and easier to debug. The generated code will be correct but not logically as the way expected, hence supervision is required.