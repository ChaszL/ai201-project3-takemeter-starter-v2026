# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->
This reads comments from a Reddit forum about building advice for a Minecraft build. It sorts each comment into one of four labels: general_chat, block_advice, layout_tip, and outside_resources. General chat is any comment with no real building advice, block advice explains how to modify the existing structure by replacing blocks, layout tips explain how to add to the structure or add details that require building, and outside resources direct users to outside websites and videos about building. The goal is to separate comments that actually help the builder from the noise, and to tell what kind of help each one gives.

**Community:** Minecraft building advice.

**Candidate distinctions:**

1. **Structured vs. unstructured advice.** Most comments are short sentances or paragraphs, but some comments are long, bulleted or numbered comments that give advice. 
2. **Advice to center the house vs. not.** Some comments recommend centering
the house, but most don't mention that placement. Some comments also say the uneven aspect of the house gives it more character and to leave it. 
1. **Points to YouTube vs. gives advice directly.** A lot of comments tell the
   poster to watch YouTubers, often Grian and they do this mostly while giving advice

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `general_chat`

**Definition:**
A comment thart gives no actual building advice such as praising them, jokes, questions, and etc.
**Example 1:**
> TNT and lava make a great combo

**Example 2:**
> Take away the shield and you'd think this was from 2014

### `layout_tip`

**Definition:**
A comment that gives structural advice on changing something about the builds structure or its surroundings. 
**Example 1:**
> Try to avoid having completely flat surfaces. Try extending the roof out a block or two, consider adding a frame around the corners of the building, maybe have glass pane windows with a flower box in front, etc.

**Example 2:**
> Add dimension! I know it sounds weird and wasteful but it will make a big difference- take your logs on the side, and pull them towards the front of the house by one block. Then take more logs and place them on top of one log, and go across until you meet the other log. By this time you should be able to put your face against the planks, and have your head under (but not touching!) the logs. Do that all the way around. Then build the logs up and adjust the roof so that they cover the logs top. Add windows some windows on the front of the house and your done!

### `block_advice`

**Definition:**
A comment that give advice on which blocks to use to make the build look appealing. 
**Example 1:**
> Dont use the same block for everything, for example, use another wood for roof or even stone will look great

**Example 2:**
> I only do simple with minimal detail, but what I'd do is use spruce for the roof, or cobble, and cobble for the floor, and use some lanterns to light up the exterior and give it something extra.

### `outside_resources`

**Definition:**
A comment that directs the poster to another website, youtube video, or reasource outside the original public channel.
**Example 1:**
> Watch a Grian video?

**Example 2:**
> Pay attention to the real world. Look at details of architecture. Do you want to build a castle? Google "castles" and take a look at examples. Want to build a different roof? Google "roofs". Expend time in creative mode practicing walls, roofs, foundations, floors, gardens, etc.

### Stretch: a fourth label

**The fourth label:** `outside_resources`

**What it captures that the other three miss:** Many comments in this thread don't give advice themselves. They send the poster somewhere else, such as a Grian video, Pinterest, or a Google search for real buildings. Without this label those comments would be forced into layout_tip or block_advice even though they contain no tip, or into general_chat even though they do help.


### The hardest boundary

**Which two labels:**
`block_advice` and `layout_tip`
**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->
If following the advice changes the shape, outline, or structure of the build, it's layout_tip, even if it names blocks. If the shape stays the same and only materials change, it's block_advice
---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:**
These post came from a reddit forum about minecraft building advice 
https://www.reddit.com/r/Minecraft/comments/1193yy0/tips_on_how_to_build_better/

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
I did the first 20 cold and then the rest were prelabled by claude and checked by me. 

**Counts per label:**

| Label | Count | Share |
|---|---|---|
| block_advice | 20 | 8.7% |
| layout_tip | 86 | 37.6% |
| outside_resources | 56 | 24.5% |
| general_chat | 67 | 29.3% |
| **Total** | 229 | 100% |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:*
> "It’s all about detailing. You build similar to how I start a build. General shape I’m going for and from there play with different window designs, play with stairs and half slabs to outline and border edges and such. Vary the color pallets and textures, add verticality with balconies, add more color with plant life. It’s all about layering on details to make the place feel less like a box and more like a functional building. Keep practicing you’ve got a good start!"

> *Could have been:*
> block_advice
> *I chose, because:*
> I chose layout_tips because it is mainly talking about placing blocks that will change the structure and not just changing blocks to improve the build.

**2.**
> *The post:*
>"Front porches, darker roofs (also bring them out another block so there's an overhang), and place the logs in the corners one block out too for some depth. Windows with trapdoors for shutters look nice too. And if you want to get real fancy, use some shears to get vines and leaves for some greenery. I use Pinterest a lot for inspo"

> *Could have been:*
>layout_tips
> *I chose, because:*
>I chose outside_resources because it seems like most of this information comes from pintrest and saying that was a way to direct the poster to pintrest.

**3.**
> *The post:*
> "Do not build roofs out of wood. It can burn down when struck by lightning. Use fireproof materials instead. Stone, copper, anything that doesn't burn. When you build the roof, make eaves by giving it an overhang. An overhang of one block would improve it quite a bit. Some people build these overhangs out of a different block to give the roof some contrast. If you do this, the top of the roof can be built out of the same block (use slabs)."

> *Could have been:*
> block_advice
> *I chose, because:*
> I chose layout_tips because it is mainly talking about placing blocks that will change the structure and not just changing blocks to improve the build.

---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:**
distilbert-base-uncased
**Settings:** <!-- epochs, learning rate, batch size, seed -->
3 epochs, learning rate 2e-5, batch size 16, seed 42, max length 128 tokens. Trained on CPU.
**Anything I changed from the defaults, and why:**
Nothing. I used the default settings for this first run so I'd have a baseline to compare against.
**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->
70/15/15 stratified split of 229 posts: 159 train / 35 val / 35 test.


---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I asked for a complete unlabeled csv file of all the post under a reddit threat.
- *What came back:* A complete unlabeled csv file of all post in the minecraft building advice forum
- *What I changed:* I changed some of the formatting inside the file since some comments had errors.

**Moment 2**

- *What I asked for:* To prelabel all the remaining data that did not have a cold note on it
- *What came back:* all other data was prelabeled and ready to be checked and corrected by me
- *What I changed:* I changed some labels to better fit the situation and context 

**Pre-labelling disclosure:**
I labeled 20 post. Claude prelabeled 209
<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy | 0.543 |  |  |
| Macro F1 | 0.329 |  |  |
| F1 — `block_advice` | 0.000 |  |  |
| F1 — `general_chat` | 0.000 |  |  |
| F1 — `layout_tip` | 0.706 |  |  |
| F1 — `outside_resources` | 0.609 |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->
I predict my trained model will beat the block_advice and general_chat label F1 since the baseline model got zero of those correct.

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
