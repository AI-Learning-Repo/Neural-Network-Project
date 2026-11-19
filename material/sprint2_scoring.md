# Sprint 2 Jigsaw Challenge: Format, Voting, and Scoring

## 1. Purpose and roles

Use the Jigsaw Challenge to **demonstrate your project group's Sprint 2 AI assistant, discuss technical decisions, and learn from other project groups**. The activity combines a working-product presentation with discussion of Qwen, tools, databases, external APIs, MCP client/server integration, Streamlit, testing, evaluation, and Scrum.

Complete both roles during the session:

- **Presenter:** Explain and demonstrate the project you represent, including what worked, what did not work, and what was learned.
- **Active listener:** Ask substantive technical questions, take notes, and identify ideas to bring back to your original project team.

A well-explained limitation or failed experiment is useful evidence of technical understanding. The Jigsaw is a learning activity as well as a competition.

## 2. How the in-class Jigsaw will work

1. **Prepare with your project team:** Prepare the Sprint 2 presentation together. Practice explaining the complete project and demonstrating the working assistant so that every member is ready to represent the group.
2. **Join your assigned mixed-group Jigsaw group:** A Jigsaw group normally contains **four representatives from four different project groups**, though some Jigsaw groups may have three. Introduce each representative by name and project group number, record the group list, and agree on the presentation order.
3. **Present and discuss:** Present your Sprint 2 work and answer technical questions. Ask at least one substantive technical question during the session.
4. **Complete the collaborative task:** Before wrapping up your Jigsaw group, agree on **one interesting technical integration, one evaluation or testing lesson, one technical limitation, and one improvement worth adopting**. Keep an individual copy of all four findings.
5. **Vote via the Zoom poll:** When the Jigsaw discussions finish, join the class Zoom session (keep your microphone muted and audio off to prevent feedback) and answer the two poll questions below. Check every selection against your recorded Jigsaw group list before submitting.
6. **Results and showcase:** The **normalized group scores** are calculated. The leading two groups may give a short presentation to the whole class if time allows.
7. **Plan the next sprint:** Rejoin your **original project group** in class. Bring your shared findings, review the Product Backlog, and discuss priorities for Sprint 3.

**During presentations and technical defenses, explain the project independently; do not use live AI assistance.**

## 3. The two voting questions

The Zoom poll may list project groups that were not represented in your assigned Jigsaw group. **Vote only for groups represented in your Jigsaw group.**

### Question 1: Best Presentation (choose exactly one)

> **Which project group delivered the best Sprint 2 presentation in your assigned Jigsaw group?**
>
> Consider the working AI assistant, technical explanation, demonstration, MCP and interface integration, testing and evaluation, Scrum process, and answers to questions.

**Choose one other group that actually presented in your assigned Jigsaw group. Exclude the project group you represent.**

### Question 2: Concept Connectors

> **Which project groups were represented by the strongest Concept Connectors in your assigned Jigsaw group?**
>
> Consider representatives whose questions or contributions connected course concepts, clarified technical choices, uncovered limitations, or helped others understand the projects.

- **Four-person Jigsaw group:** Choose **exactly two distinct other groups**.
- **Three-person Jigsaw group:** Choose **exactly one other group**.

Exclude the project group you represent and any group absent from your Jigsaw group. Each group choice identifies the representative from that group in your Jigsaw group.

## 4. Voting privacy and validity

**Individual choices will not be shared with classmates.** The poll is confidential among students. Identifiable responses can be inspected to check accidental self-votes, selections of absent groups, or other voting mistakes. **This is not technically an anonymous Zoom poll**, because access to individual responses is required for validation.

Only eligible votes are included in the official scores. Each question is checked **independently**:

- A valid Best Presentation answer is counted even if the student's Concept Connector answer is incomplete.
- A blank, incomplete, duplicate, self-vote, or out-of-group Concept Connector answer is excluded **in full**. A single valid-looking selection from an incomplete two-selection answer is not counted.
- Missing or invalid answers are flagged during validation and excluded from the score calculation.

## 5. How the competition score is calculated

The numbers **7** and **3** are *weights in the Jigsaw competition*, not course grade points. Best Presentation contributes up to **7 points** and Concept Connectors up to **3 points**.

For each project group $g$:

- $P_g$ = number of **valid Best Presentation votes** received by that group's representatives.
- $C_g$ = number of **valid Concept Connector selections** received by those representatives.
- $E_{P,g}$ = number of **actual valid Best Presentation voting opportunities** for the group.
- $E_{C,g}$ = number of **actual valid Concept Connector voting opportunities** for the group.

The normalized rates are:

$$
\text{Presentation Rate}_g=\frac{P_g}{E_{P,g}}
$$

$$
\text{Connector Rate}_g=\frac{C_g}{E_{C,g}}
$$

The final Sprint 2 Jigsaw competition score is:

$$
\boxed{\text{Score}_g=7\left(\frac{P_g}{E_{P,g}}\right)+3\left(\frac{C_g}{E_{C,g}}\right)}
$$

The score ranges from **0 to 10** when there are valid voting opportunities in both categories. A higher score means the group's representatives received a larger share of the valid votes available to them, with more weight placed on the presentation.

### Worked example

Assume the representatives are assigned to four-person Jigsaw groups. Some students submit incomplete Concept Connector answers, so the two opportunity totals differ.

| Measure | Group A | Group B |
|---|---:|---:|
| Participating representatives | 4 | 3 |
| Valid presentation votes $P_g$ | 8 | 7 |
| Presentation opportunities $E_{P,g}$ | 12 | 9 |
| Valid connector selections $C_g$ | 6 | 5 |
| Connector opportunities $E_{C,g}$ | 9 | 7 |
| Presentation rate | $8/12=66.67\%$ | $7/9=77.78\%$ |
| Connector rate | $6/9=66.67\%$ | $5/7=71.43\%$ |
| **Final score** | **$6.667/10$** | **$7.587/10$** |

For Group A:

$$
\begin{aligned}
\text{Score}_A
&=7\times\frac{8}{12}+3\times\frac{6}{9} \\
&=4.667+2.000 \\
&\approx 6.667
\end{aligned}
$$

For Group B:

$$
\begin{aligned}
\text{Score}_B
&=7\times\frac{7}{9}+3\times\frac{5}{7} \\
&\approx 5.444+2.143 \\
&\approx 7.587
\end{aligned}
$$

Group A received more raw presentation votes and connector selections, but **Group B has the higher normalized score** because its representatives received a higher proportion of their valid voting opportunities. The official ranking uses normalized scores, not raw totals.

*Note:* Each valid Concept Connector ballot contributes one selection to each group chosen; it also creates **one voting opportunity for each eligible other group** in that Jigsaw group. The two selections in a four-person Jigsaw group are **not** counted as two opportunities for every group. Calculations are performed using unrounded values, and only reported scores are rounded.

## 6. Winning group and badges

The group with the highest **validated normalized score** wins the Sprint 2 Jigsaw competition. If two groups have exactly the same score, the group with the higher **Presentation Rate** ranks first. If both measures are equal, the tie will be resolved on review rather than applying an arbitrary rule.

The leading two groups may give a short presentation to the whole class, time permitting.

The winning group will receive recognition badges. **Jigsaw competition scores do not directly become course grade points.** Badge recognition contributes to a separately defined, capped course bonus later; that scheme will be explained separately.