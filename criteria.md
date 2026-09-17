# Acceptance criteria — The Unofficial Guide

Five criteria that say what "working" means for this system, written in unit 1
**before** any results existed.

An acceptance criterion names a target: a number, a count, a rate, or something
a person could plainly observe. *"Retrieval works"* is an opinion. *"For at
least 4 of my 5 test questions, the top results include a chunk containing the
answer"* is a criterion.

Under each one, write a sentence or two on **why that target** and not a
stricter or looser one. A reason that says something about your corpus or your
pipeline earns credit; *"80% seemed reasonable"* does not.

> Missing your own targets next unit costs you nothing. Setting a target so
> easy you can't miss it does.

---

## 1. Retrieved chunks contain the answer

For at least 4 of my 5 test questions, the retrieved chunks include one that
contains the answer.

**Why this target:**

My five test questions retrieved chunks with best distances between 0.253 and 0.593, and for each one the top chunk was a document that directly answers it (e.g. `admin_housing_lottery.txt` for the lottery question, `money_textbooks.txt` for textbook costs). I'm setting the target at 4 of 5 rather than 5 of 5 because "food options" sits noticeably closer to the cutoff (0.593) than the other four — a post I haven't tested yet could plausibly land past the gate without actually answering the question, and I'd rather budget for one miss than claim a target I can't back with margin.

---

## 2. Every answer names a source

Every answer the system produces names at least one source document.

**Why this target:**

This is 5 of 5, not 4 of 5, because naming a source isn't something retrieval can get partially right — it's a formatting rule in the system prompt (`GROUNDING_INSTRUCTION` in `generate.py`) that applies to every answer regardless of whether the retrieved chunks were any good. Even my refused near-miss answer (Sample Answer in the README) named the documents it retrieved. The only way this breaks is the model ignoring its own system instructions, which I haven't seen happen across any question I've run.

---

## 3. The relevance gate stops out-of-corpus questions

When I ask a question my documents clearly don't cover, the relevance gate
stops it and the system returns "I don't have enough information about that" —
in at least 4 of 5 tries.

<!-- The five questions are the ones in `OUT_OF_SCOPE` at the bottom of
     `questions.py`, and `run_eval.py` puts them through the gate and writes
     what happened into your run log. Swap them for your own if you'd rather —
     just keep five of them, or the "4 of 5" above has nothing to be 4 of. -->

**Why this target:**

My five real questions topped out at 0.593, and the five `OUT_OF_SCOPE` questions came back between 0.825 and 0.934 — a clean gap of over 0.2, no overlap between the groups. I set my cutoff at 0.7, comfortably inside that gap. I'm still targeting 4 of 5 rather than 5 of 5 because this gap is measured on only five out-of-scope questions; a future question that happens to share vocabulary with my corpus (mentioning "students" or "courses" without actually being about campus life, the way my own broken test questions did before I fixed them) could land closer to the middle than any of these five did.

---

## 4. Something about your chunks

<!-- YOU WRITE THIS ONE.

     How would you know if your chunks were the right size? Name something
     countable or observable.

     Examples of the right shape — don't copy these, they should come from
     what you actually saw in Milestone 3:
       - "At least 4 of 5 sampled chunks read as a complete thought, with no
          sentence cut in half at either end."
       - "No chunk is shorter than 200 characters, since anything below that
          in my corpus turned out to be a heading with no content under it." -->
For at least 4 of 5 sampled chunks, the chunk reads as a complete thought — no sentence is cut off at either end, and every sentence in it is about the same topic.


**Why this target:**

I read the 5 chunks `python app.py chunks` printed after switching to paragraph-grouped chunking, and all 5 came back as complete thoughts — no cut sentences, one topic each (e.g. `admin_add_drop_deadline.txt` stayed one self-contained explanation of the deadline). I'm setting the target at 4 of 5 rather than 5 of 5 because that's a sample of 5 out of 88 chunks, and I haven't ruled out a document elsewhere in the corpus that mixes two topics in one paragraph, which my paragraph-boundary splitter wouldn't catch.



---

## 5. Your choice

<!-- YOU WRITE THIS ONE TOO.

     Pick something you actually care about getting right. It could be about
     speed, about refusals, about a particular kind of question your corpus
     handles badly, about source attribution being correct rather than merely
     present — anything, as long as it names a number or an observable
     outcome. -->

For at least 4 of my 5 test questions, the source the answer names is a document that actually contains the answer — not just any source.


**Why this target:**

Criterion 2 only checks that an answer names *a* source, and that's easy to satisfy without the source actually being right. My corpus has several near-duplicate documents that make a wrong-but-plausible citation realistic rather than hypothetical — `admin_add_drop_deadline.txt` and `admin_withdrawal_deadline.txt` cover two different but similarly-worded deadlines, and each course has separate `_exams`/`_workload` files alongside its main file. I set the target at 4 of 5, matching criterion 1, since correct citation depends on the same retrieval step succeeding.



---

<!-- ─────────────────────────────────────────────────────────────────────────
     UNIT 2 — read this before you change anything above.

     If a criterion turns out to be BROKEN rather than merely unmet, you can
     revise it, and that earns credit. But never delete or edit the original
     line. Add the revision underneath it, like this:

         ## 1. Retrieved chunks contain the answer

         For at least 4 of my 5 test questions, the retrieved chunks include
         one that contains the answer.

         **Why this target:** ...

         > **Revised in unit 2:** For at least 4 of 5 questions, the top three
         > results contain the answer.
         >
         > **Why revised:** I couldn't judge "the chunks include one that
         > contains the answer" the same way twice — I scored two questions
         > differently on Monday than on Wednesday. The new version is
         > something I can actually check.

     That's a revision because the criterion couldn't be MEASURED.

     Lowering a target because you missed it is not a revision, and it costs
     you the point:

         ✗ "I said 4 of 5 but got 2 of 5, so 2 of 5 is more realistic."

     A number you missed stays where it is, gets diagnosed, and gets a fix
     attempted. That's where the points are.

     The whole reason the originals stay visible is so someone can see what you
     said before you knew the answer.
     ───────────────────────────────────────────────────────────────────────── -->
