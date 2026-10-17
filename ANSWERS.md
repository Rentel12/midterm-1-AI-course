# Midterm 1: answers

GitHub username: rentel12
Session code: kettle-62
Fingerprint: d46e83ca

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used GitHub Copilot in VS Code to help automate the identification and correction of coding errors and to make the development process more efficient. I also used ChatGPT to understand the assignment requirements, clarify the purpose of each task, and interpret error messages that appeared in the terminal during setup and troubleshooting, including problems related to Git.

I wrote the report and my answers myself. However, I used AI assistance to clarify technical concepts, check my understanding of the task, and improve the wording, clarity, and overall presentation of my written responses. AI tools were used as support for learning, debugging, and editing rather than as a replacement for my own understanding.

I reviewed the final answers and am responsible for the submitted work. I understand that I may be asked to explain my calculations, reasoning, technical decisions, and conclusions during the oral check.

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.400 | 0.450 | 0.500 | 0.442 |
| B | 0.367 | 0.567 | 0.850 | 0.517 |


### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.450 | 0.331 | 0.575 |
| A, k=5 | 0.500 | 0.377 | 0.623 |
| B, k=3 | 0.567 | 0.441 | 0.684 |
| B, k=5 | 0.850 | 0.739 | 0.919 |


### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 11919.82 |
| A, k=5 | 16007.28 |
| B, k=3 | 7259.78 |
| B, k=5 | 8240.55 |


### 1d. Decision (at most 150 words)

Chosen candidate: B, k=5

B, k=5 is my choice: recall is 51/60 = 0.850 (95% Wilson interval 0.739–0.919), versus 34/60 = 0.567 (0.441–0.684) for B, k=3. These intervals do not overlap, supporting higher recall for B, k=5 in this sample. The A, k=3, A, k=5 and B, k=3 intervals overlap one another, so I do not claim those candidates differ. B, k=5 costs $8,240.55/year, below the $10,100 budget. Its recall is EN 18/20 = 0.90, RU 17/20 = 0.85 and KK 16/20 = 0.80, so KK is served worst. It costs about $980.77 more than B, k=3 but finds 17 more correct chunks in the test. Both A candidates exceed budget.

### 1e. Next measurement (at most 60 words)

Run a larger, blinded held-out retrieval evaluation with at least 100 questions per language, comparing B, k=5 with B, k=3 on the same questions. This could show whether the KK gap persists and whether B, k=5's recall gain justifies its additional annual cost.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.889 | RU degraded and KK degraded (tie) | 0.833 | 0.664 | 0.927 | 30497.58 |
| L | 0.822 | KK degraded | 0.633 | 0.455 | 0.781 | 27000.00 |

Personal-data pages per year sent abroad under H: 775625

### 2b. Memo (at most 300 words)

H extracts all four required fields correctly on 160/180 pages (0.889); L does so on 148/180 (0.822). H's weakest cells tie at RU degraded and KK degraded: 25/30 = 0.833, with a 95% Wilson interval of 0.664–0.927. L is weakest on KK degraded: 19/30 = 0.633 (0.455–0.781). These intervals are wide and overlap, so the samples do not establish a clear difference for these weakest groups. The pooled accuracies hide the degraded-Kazakh weakness. H costs $30,497.58/year: each image is 45 × 63 = 2,835 input tokens and returns about 416 output tokens. L needs ceil(8,500/4,000) = 3 servers, or $27,000/year.

H sends scans to a data centre abroad. The specific destination, vendor/subprocessor access and storage, and retention of scans, logs and backups are not given and must be verified. L's inference server is in Kazakhstan, but the bank must still identify who can access it and its logs/backups, and set and verify retention and deletion. Since ИИН is covered by Kazakhstan's personal-data law, I would not send personal-data pages abroad based only on a vendor's statement. Under H, 8,500 × 365 × 25% = 775,625 such pages leave the country annually.

I recommend L: it costs less and keeps inference local, although its observed accuracy is 6.7 percentage points lower. Restrict access by role, encrypt stored data and connections, keep audit logs, define a short retention/deletion schedule, and test deletion from backups. Before considering H, independently verify its data location, processors, retention, and controls. Residual risks include L's lower accuracy—especially on degraded KK—and internal access or retention failures.

## Part 3: Explain your own numbers

### Q1

Number: 1.43
Why (at most 40 words): KK/EN ratios are 333/315=1.06 (characters), 645/315=2.05 (bytes), and 179/61=2.93 (tokens); 2.93/2.05=1.43. Encoding explains part, but the extra premium is tokenizer-related.

### Q2

Number: 395 e5 tokens
Why (at most 40 words): Chunk length is ~657.5 e5 tokens; the sentence starts at 394.5 and, under 20 tokens long, ends before 414.5, within the first 512. It can match/retrieve the chunk; ~145.5 tail tokens are truncated, usually silently (no error).

### Q3

Number: 76.9%
Why (at most 40 words): At T=1, Monday and Friday sum to 0.9254; top-p keeps these two, making Monday 0.7112/0.9254 = 76.9%. Top-p removes and renormalizes tail tokens; lowering temperature reshapes probabilities without removing them.

### Q4

The penalized score is correct answers minus the penalty: Y = 68 − 8 = 60 and X = 70 − 30 = 40. Therefore, Y − X = 20. The public leaderboard ignores wrong answers, so it can rank systems differently.

### Q5

Number: 16.25
Why (at most 40 words): The model has 325 tokens per parameter versus Chinchilla's 20, so it is over-trained by this compute-optimal ratio. Chinchilla argued large models such as GPT-3 were under-trained and needed more tokens.
