# Sprint 1: Online Jigsaw Challenge: Format, Voting, and Scoring


All presentations, technical discussions, voting, and planning take place online in Zoom. Join the main Zoom room and move to the assigned breakout rooms when directed. No on-campus attendance is required.

Keep presentation files, the demonstration application, and relevant test results ready for screen sharing. If screen sharing or a connection fails, notify the instructor and describe the available evidence and limitation. Rejoin Zoom after a disconnection and request help returning to the assigned room.

## 1. Purpose

Use the Jigsaw Challenge to **demonstrate the project group's Sprint 1 AI assistant or agent, discuss technical decisions, and learn from other project groups**. The activity combines a working-product presentation with discussion of Qwen, tools, databases, external APIs, evaluation, and Scrum.

Complete both roles during the session:

- **Presenter:** Explain and demonstrate the project being represented, including what worked, what did not work, and what was learned.
- **Active listener:** Ask substantive technical questions, take notes, and identify ideas to bring back to the original project team.

A well-explained limitation or failed experiment is useful evidence of technical understanding. The Jigsaw is a learning activity as well as a competition.

## 2. How the online Jigsaw will work

1. **Prepare with the project team:** Prepare the Sprint 1 presentation together. Practise explaining the complete project and demonstrating the working assistant so that every member is ready to represent the group.
2. **Join the assigned mixed-group room:** The room normally contains **four representatives from four different project groups**. Introduce each representative by name and project group number, record the group list, and agree on presentation order.
3. **Present and discuss:** Present the Sprint 1 work and answer technical questions. Ask at least one substantive technical question during the session.
4. **Complete the collaborative task:** Before leaving the room, agree on **one interesting tool integration, one evaluation lesson, one technical limitation, and one improvement worth adopting**. Keep an individual copy of all four findings.
5. **Return and vote:** When the Jigsaw breakout rooms close, return to the main Zoom room and answer the two poll questions below. Check every selection against the recorded room list.
6. **Results and showcase:** The instructor checks eligible votes, calculates **normalized group scores**, and identifies the winning project group. The leading one or two groups may present to the whole class in the main Zoom room if time allows.
7. **Plan Sprint 2:** Join the **original project group's breakout room**. Bring the shared findings, review the Product Backlog, and agree on the Sprint 2 Goal and plan.

**During presentations and technical defenses, explain the project independently; do not use live AI assistance.**

## 3. The two Zoom voting questions

The same **seven project groups** will appear as choices in both questions, even though only some are represented in the assigned Jigsaw room.

### Question 1: Best Presentation (choose exactly one)

> **Which project group delivered the best Sprint 1 presentation in the assigned Jigsaw breakout room?**
>
> Consider the working AI assistant, technical explanation, demonstration, evaluation, Scrum process, and answers to questions.

**Choose one other group that actually presented in the assigned room. Exclude the project group being represented in this session.**

### Question 2: Concept Connectors (choose exactly two)

> **Which TWO project groups were represented by the strongest Concept Connectors in the assigned Jigsaw breakout room?**
>
> Consider representatives whose questions or contributions connected course concepts, clarified technical choices, uncovered limitations, or helped others understand the projects.

**Choose two distinct other groups represented in the assigned room. Exclude the project group being represented in this session and groups absent from that room.**

The poll answer choices will be **Group 1** through **Group 7**. One representative per project group is assigned to each mixed-group room; therefore, selecting a group identifies that room's representative.

## 4. Voting privacy and validity

**Individual choices will not be shared with classmates.** The poll is confidential among students. The instructor can inspect identifiable responses if necessary to check accidental self-votes, selections of absent groups, or other voting mistakes. **This is not technically an anonymous Zoom poll**, because the instructor needs access to individual responses for validation.

Only eligible votes will be included in the official scores. Invalid or ambiguous selections may be excluded. The instructor will share **aggregate group results**, not individual ballots.

An ineligible vote can change the competition result. Before submitting, compare each selection with the assigned room's group list: one eligible group for Question 1 and two distinct eligible groups for Question 2. Inform the instructor promptly if a mistake occurs or too few groups are eligible.

## 5. How the competition score is calculated

The numbers **5** and **3** are *weights in the Jigsaw competition*, not course grade points.

For each project group $g$:

- $P_g$ = number of **valid Best Presentation votes** received.
- $C_g$ = number of **valid Concept Connector votes** received.
- $D_g$ = number of **eligible voting opportunities** for the group's representatives across all their Jigsaw rooms.

In a room with four representatives, each representative can receive votes from the **other three students**. If a group has $R_g$ representatives and all their rooms contain four students:

$$
D_g = 3R_g
$$

If room sizes differ, the instructor will use the actual room allocations:

$$
D_g = \sum_{r\,\text{where group }g\text{ is represented}}(n_r - 1)
$$

where $n_r$ is the number of participating students in room $r$. This ensures that a group is not penalized merely because it has fewer representatives or is assigned to a smaller room.

The **normalized rates** are:

$$
\text{Presentation Rate}_g = \frac{P_g}{D_g}
$$

$$
\text{Connector Rate}_g = \frac{C_g}{D_g}
$$

The **final Jigsaw competition score** is:

$$
\boxed{\text{Score}_g = 5\left(\frac{P_g}{D_g}\right) + 3\left(\frac{C_g}{D_g}\right)}
$$

The score ranges from **0 to 8**. A higher score means the group received a larger share of its possible eligible votes, taking group size and room assignments into account.

### Worked example

Assume all relevant rooms have four participating students.

| Measure | Group A | Group B |
|---|---:|---:|
| Participating representatives | 4 | 3 |
| Eligible voting opportunities $D_g$ | $4\times3=12$ | $3\times3=9$ |
| Valid presentation votes $P_g$ | 8 | 7 |
| Valid connector votes $C_g$ | 6 | 6 |
| Presentation rate | $8/12=0.667$ | $7/9=0.778$ |
| Connector rate | $6/12=0.500$ | $6/9=0.667$ |
| **Final score** | **$5(8/12)+3(6/12)=4.83$** | **$5(7/9)+3(6/9)=5.89$** |

Group A received more raw presentation votes, but **Group B has the higher normalized score** because its representatives had fewer opportunities to receive votes. The official ranking uses normalized scores, not raw vote totals.

*Note:* The two Concept Connector selections are counted as two separate votes. Each student may select a particular group **at most once** in that question. If a room is missing participants, the instructor will check the eligible opportunities using the actual attendance and room roster.

## 6. Winning group and badges

The group with the highest **validated normalized score** wins the Sprint 1 Jigsaw competition. If time allows, one or two leading groups may give a short presentation to the whole class.

The winning group may receive recognition badges. **Jigsaw competition scores do not directly become course grade points.** Badge recognition may contribute to a separately defined, capped course bonus later; the instructor will explain that scheme separately.

## 7. After the competition: begin Sprint 2 planning

After the Jigsaw, join the **original project team's Zoom breakout room** for Sprint 2 planning. Discuss which ideas or improvements to take forward. Update the Product Backlog, identify what should remain working from Sprint 1, and plan the Sprint 2 increment: **MCP client/server integration, Streamlit GUI, and function unit tests**, alongside the existing product functionality and assistant evaluation.

The Jigsaw findings provide concrete input for the team's next sprint.


## Session guides

- [Breakout-room tasks and record sheet](./sprint1_br_instructions_student.md)
- [Voting checklist](./sprint1_voting_guide_student.md)
- [Sprint 2 planning requirements](./sprint2-planning_student.md)
