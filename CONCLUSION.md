# Lab 05 — Conclusion

<!--
Read this before you write.

This file is graded by your instructor, by hand, and it is worth 6 of the 15 points.

- Type it yourself. Do not use a chatbot, an AI assistant, an AI autocomplete or a "rewrite my
  text" tool for any part of it, not even for grammar. English that is plain and a little
  clumsy but yours is better than polished text that is not.
- Use your own numbers. Every number you quote must be one your own notebook printed. Copy
  the number exactly as it appeared. A number your notebook did not print is marked as wrong.
- Answer in your own words, about 80-200 words per question. A short answer that uses your
  numbers beats a long answer that could have been written by anyone.
- Delete these comment lines if you like. Keep the three headings and the last line.
-->

## 1. Which layer changed your results the most?

Give the numbers before -> after for that layer (correct / wrong / no answer, out of 20) and say
whether it changed what the agent *can do* or only what it *admits*. If no layer changed the
number of correct answers, say so and explain what the layers did change instead.

Layer 1 changed my results the most. The bare agent got 16 correct, 4 wrong and 4 with no answer out of 24. With layer 1 it got 18 correct, 6 wrong and 0 with no answer. It was the only layer that raised the number of correct answers, so it changed what the agent can do. Before it, the loop stopped at the first tool error, so all four alias questions had no answer. With layer 1 the model read the error and tried again, q23 and q24 became correct, but q21 and q22 became wrong. Layer 2 changed nothing and used the same 62 model calls. Layer 3 kept 18 correct but turned two wrong answers into no answer, so it only changed what the agent admits.

## 2. One failure the full harness did not fix

Pick one question that the full harness (layer 3) still gets wrong or refuses. Give its id
(q01 ... q20). Open its trace in the notebook: at which step did it go wrong, and what did
the model do instead? Name one thing you would add to the harness, and what it would cost
(extra model calls, tokens, time, or a new way to fail).

I picked q18, which asks by how many percentage points CPI in Jambyl fell between 2022 and 2025. With layer 3 it has no answer. The first step was right, get_series returned 16.1 for 2022 and 10.6 for 2025. The second step went wrong. The model asked calculate for 16.1 minus 10.6, divided by 100, and got 0.055, but a change in percentage points is just the difference of the two values, with no division. Then it wrote 0.55, which no tool returned, so grounding rejected it. It repeated the same mistake when asked to fix it and ended as unverified. Grounding caught it only by luck, because 0.055 would have passed. I would add a check that a pp answer is just one value minus the other. It costs one more check and sometimes one more model call, and it could wrongly reject a correct answer written another way.

## 3. Workflow or agent for the monthly ranking?

Dala Insight wants to publish, every month, "which region is highest and which is lowest on one
indicator in one year". Would you ship the fixed workflow or the agent? Use the accuracy, the
number of model calls, the tokens and the time you measured in Part 1. Then say in one sentence
what kind of question would make you choose the other one.

I would ship the fixed workflow. On the 15 fixed questions the workflow got 13 correct, 0 wrong and 2 with no answer, using 15 model calls, 3852 tokens and 17.9 seconds. The bare agent got 9 correct, 2 wrong and 4 with no answer, using 26 model calls, 19835 tokens and 65.8 seconds. The agent's two wrong answers were the rank questions q08 and q11, exactly the monthly ranking kind, and it got them wrong even with layer 3. The workflow never gave a wrong answer. It only gave no answer on two alias names, and a report with a gap is better than one with the wrong region. I would choose the agent for questions that need several steps, like a change across years, where no fixed template exists.

---

I wrote this file myself, without any AI tool.
