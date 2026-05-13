# 🎮 Game Glitch Investigator: The Impossible Guesser

> A debugging challenge where an AI-generated Streamlit game was intentionally broken — and I had to play detective to make it playable, testable, and clean.

---

## 📌 Project Overview

This project is a **Number Guessing Game** built with Streamlit that started life broken. An AI generated the initial code, but the game had critical bugs that made it literally unwinnable: the secret number reset on every click, and the "higher/lower" hints lied to the player.

My job was to investigate the glitches, fix them, refactor the logic into a testable module, and verify the fixes with a passing test suite.

This repo is a small but complete example of the real-world debugging loop: **reproduce → diagnose → fix → refactor → test**.

---

## 🛠️ Skills Demonstrated

| Skill | How It Shows Up Here |
|---|---|
| **Debugging & Root-Cause Analysis** | Traced an "impossible to win" symptom back to a state-management bug, not a logic bug. |
| **Streamlit State Management** | Used `st.session_state` to persist values across re-runs — a Streamlit-specific pattern that trips up most beginners. |
| **Python Logic Refactoring** | Extracted game logic out of the UI layer into a pure-function module (`logic_utils.py`) so it could be unit-tested. |
| **Test-Driven Verification** | Used `pytest` to confirm fixes — all 4 tests pass after refactor. |
| **AI-Assisted Development** | Used Claude to surface the right Streamlit pattern (`st.session_state`) instead of guessing. |
| **Reading Broken Code** | Practiced the underrated skill of not trusting code just because it runs. |

---

## 🧱 Tech Stack

- **Python 3.12**
- **Streamlit** — UI framework
- **pytest** — testing
- **Git / GitHub** — version control

---

## 🐛 Bugs Found

### Bug 1 — The Disappearing Secret Number
**Symptom:** Every click of "Submit" generated a *new* secret number, so the game was unwinnable.

**Root cause:** The secret number was being assigned at the top of the script. Streamlit re-runs the entire script top-to-bottom on every interaction, so `random.randint(...)` fired again on each click and overwrote the previous value.

**Fix:** Stored the secret number inside `st.session_state` so it's initialized once per session and persists across re-runs.

```python
if "secret_number" not in st.session_state:
    st.session_state.secret_number = random.randint(1, 100)
```

### Bug 2 — The Lying Hints
**Symptom:** When the guess was too high, the game said "Try higher" (and vice versa).

**Root cause:** The comparison operators in the hint logic were flipped.

**Fix:** Corrected the comparison so the hint accurately reflects whether the guess is above or below the secret number.

---

## 🧭 Approach

I worked through this in four deliberate steps rather than just patching whatever I noticed first:

1. **Reproduce before fixing.** I played the game using the built-in "Developer Debug Info" panel to confirm the bug existed and to see exactly *how* it failed. The secret number visibly changed every click — that pointed straight at state, not logic.
2. **Diagnose with the right question.** Instead of asking "how do I fix this," I asked Claude: *"How do I keep a variable from resetting in Streamlit when I click a button?"* That surfaced `st.session_state` as the right tool.
3. **Fix the smallest thing that works.** Wrapped the secret-number assignment in a `session_state` check, then corrected the hint comparison. No over-engineering.
4. **Refactor for testability.** Moved game logic out of `app.py` and into `logic_utils.py` so the rules could be tested independently of the Streamlit UI. Ran `pytest` to verify.

---

## ⚙️ Setup

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Run the app
python -m streamlit run app.py

# 3. Run the tests
pytest
```

---

## ✅ Test Results

After the refactor, all tests pass:

```
============================= test session starts ==============================
platform linux -- Python 3.12.1, pytest-9.0.2, pluggy-1.6.0
collected 4 items

tests/test_game_logic.py::test_winning_guess              PASSED  [ 25%]
tests/test_game_logic.py::test_guess_too_high             PASSED  [ 50%]
tests/test_game_logic.py::test_guess_too_low              PASSED  [ 75%]
tests/test_game_logic.py::test_get_range_for_difficulty   PASSED  [100%]

============================== 4 passed in 0.01s ===============================
```

---

## 📸 Demo

**Before the fix — the secret number changes on every guess:**
![Broken game](image-1.png)

**After the fix — playable, accurate hints, winnable:**
![Fixed game](image-2.png)

---

## 💡 Key Takeaways

- **AI-generated code is a starting point, not a finished product.** It often runs without doing what you want.
- **Streamlit's re-run model is the source of most "weird" bugs** in beginner Streamlit apps. `st.session_state` is the answer more often than not.
- **Separating logic from UI pays for itself instantly** the moment you want to write a test.
- **Asking the right question matters more than asking many questions** — the framing "how do I stop a variable from resetting" got me to the answer faster than "why is my game broken."

---

## 📂 Repo Structure

```
.
├── app.py              # Streamlit UI
├── logic_utils.py      # Pure game logic (testable)
├── tests/
│   └── test_game_logic.py
├── requirements.txt
└── README.md
```
