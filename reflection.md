# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
Simple enough to play, with an option to enable a hint. Also had a very simple interface.
- List at least two concrete bugs you noticed at the start  
  (for example: "the secret number kept changing" or "the hints were backwards").
  a. On even attempts, 'secret' becomes a string.
  b. After round ends, user is unable to start a new game
  c. Enter button on keyboard does not submit
  d. Easy difficulty is hardest(ranges wrong)

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
  I used Claude Code as my AI assistant throughout this project.

- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
  Claude identified the hard difficulty range (1–50) was actually easier than Normal (1–100), and suggested changing it to 1–200. I verified this by reading the `get_range_for_difficulty` function and confirming the ranges now scale correctly: Easy (1–20), Normal (1–100) and Hard (1–200).

- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).
  The AI initially did not catch that the New Game button was missing resets for `status`, `score`, and `history` — it only reset `attempts` and `secret`. I discovered this was wrong by testing the game: after winning, clicking New Game still showed the "You already won" screen. I verified the fix by playing through a full game, clicking New Game, and confirming the game started correctly.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
  I tested each fix by running the game manually in Streamlit and running `pytest tests/ -v` to check the logic functions. A bug was only considered fixed when the game behaved correctly end-to-end.

- Describe at least one test you ran (manual or using pytest) and what it showed you about your code.
  I ran `pytest tests/ -v` after refactoring the logic into `logic_utils.py`. All 3 tests passed — `test_winning_guess`, `test_guess_too_high`, and `test_guess_too_low` — which confirmed that `check_guess` correctly returns `"Win"`, `"Too High"`, and `"Too Low"` for the right inputs.

- Did AI help you design or understand any tests? How?
  The AI explained that the tests were already written in `test_game_logic.py` and were importing from `logic_utils`, which is why the refactor was necessary. It also helped me understand that `pytest` couldn't find the module until a `conftest.py` was added to the project root.

---

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.
In streamlit,everytime you interact with the page, the entire script reruns from top to bottom. A new number would be generated each time on eveery re run.
- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
If you had a website, and you clicked any button on the page, imagine if the whole page gets rebuilt from scratch, like you refreshed the website. Thats what streamlit does on every interaction. Si if we had a variable storing a random secret number, it gets recreated everytime the page is rebuilt. session_state is like a notepad that survives the rebuild.whatever you write on it stays across reruns. so we can jut store our variable in session_state.
- What change did you make that finally gave the game a stable secret number?

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
  i learnt the need for seperating logic from ui code to make functions independently testable.
- What is one thing you would do differently next time you work with AI on a coding task?
Assume AI's fixes will have bugs and look for them before running code and then finding bugs.
- In one or two sentences, describe how this project changed the way you think about AI generated code.
It cannot be inherently depended on, but it is very helpful and time saving.
