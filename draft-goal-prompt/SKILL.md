---
name: draft-goal-prompt
description: "Draft concise, ready-to-paste Codex /goal prompts for Lee as a goal prompt harness with Mission, Current Goal, Goal Review, and full next-goal handoff framing. Use when Lee asks for a goal prompt, next-session prompt, objective prompt, Mission/Current Goal framing, or phrases such as \"goal 프롬프트 만들어줘\", \"다음 세션 goal 써줘\", \"목표 프롬프트 작성해줘\", or when Lee wants a vague task turned into an audit-friendly Codex goal."
---

# draft-goal-prompt

## Purpose

Turn Lee's rough next-session intent into a ready-to-paste Codex `/goal` prompt.

This is a goal prompt harness, not a command to run an endless loop. Separate:

- `Mission`: the durable long-term direction or reason for a large project. This is optional and is not a done condition.
- `Current Goal`: the concrete target state for the next Codex session or thread.

When a `Mission` remains unfinished after the `Current Goal`, require a next-goal handoff: the current session should propose concrete next `Current Goal` candidates, but must not execute them unless Lee explicitly approves.

Optimize for a goal another Codex session can inspect, pursue, verify, pause, resume, and hand off without rediscovering the task from scratch.

## Workflow

1. Decide whether Lee needs a short single-goal prompt or a long-project framing. Use `Mission` only when a durable direction changes how the current goal should be scoped.
2. Extract or infer the target workspace, important files, current state, desired outcome, safety boundaries, and verification evidence.
3. Ask at most 1-3 concise clarifying questions only when missing information would change the outcome, validation, or safety boundary.
4. Prefer conservative assumptions when missing details are low-risk. State those assumptions inside the goal prompt.
5. Never fabricate paths, tool names, repo state, or previous decisions. Use a TODO placeholder or ask Lee when exact context is needed.
6. Draft a compact goal contract using the section set below. Write the ready-to-paste `/goal` text in Korean by default for Lee, while keeping stable section labels, file paths, commands, code identifiers, and official terms in English when clearer.
7. For nontrivial or high-impact goals, include an inline `Goal Review` block before the ready-to-paste `/goal` so Lee can inspect the goal's intent, exclusions, completion test, approval boundary, and Mission fit before using it.
8. For long-running `Mission` prompts, require the executing Codex session to output `Next Goal candidates`, `Recommended Next Goal`, and a complete `Ready-to-paste next /goal` for the recommendation. Do not make the next `/goal` a lower-quality compact summary.
9. If Lee wants unattended progress, include a 5-minute continuation rule that applies only to safe in-scope work and never treats silence as approval for high-risk actions.
10. Keep the final `/goal` practical to paste, but do not reduce the goal contract quality just to save length. Put bulky background in an inline `Detailed Handoff` block, or ask Lee before creating/updating a handoff file.
11. Do not create goal draft files by default. Do not execute the drafted goal, install skills, edit AGENTS files, or change automation unless Lee explicitly asks for that as the current task.

## Lee Defaults

- Use Korean for Lee-facing explanations and for the final ready-to-paste `/goal` prompt body. Keep section labels, code, commands, file names, local paths, and official terms in English when clearer unless Lee explicitly asks to translate them too.
- Prefer current local evidence over memory or assumptions. Tell the next Codex session which local files to inspect first.
- Keep work small, concrete, and verifiable. Break broad goals into a first useful slice when needed.
- Include stop boundaries for secrets, destructive actions, broad rewrites, network installs, runtime-heavy stacks, or workspace changes outside the named scope.
- For `llm-wiki` work, route through `/home/lee/llm-wiki/AGENTS.md`, `README.md`, `00-index.md`, and relevant task guides.
- For `eclipse-v3` work, tell Codex to read `/home/lee/eclipse-v3/AGENTS.md` before changing files.
- For robotics or Physical AI work, default to small hands-on steps with concrete artifacts and verification. Keep Isaac Sim/Lab and RoboLab fenced off unless Lee explicitly changes that boundary.
- Avoid storing secrets, credentials, private raw data, or unreviewed external text in generated goals, logs, memories, skills, or wiki notes.

## Goal Prompt Contract

Use this compact section set for the final ready-to-paste prompt. Omit `Mission` and `Handoff` for short tasks when they would add noise.

```text
Mission:
<optional; durable long-term direction, not the completion condition>

Current Goal:
<one concrete target state for this Codex session/thread>

First context to inspect:
- <absolute local paths, AGENTS files, task guides, issue docs, logs, notes, or source files>

Scope:
- In scope: <allowed target area>
- Out of scope: <explicit exclusions>

Constraints:
- <allowed actions and important limits: edits, commands, browsing, tests, docs updates, approvals>

Done when:
- <observable artifact, behavior, or file state>
- <verification evidence or command/readback result>
- <what must be true before the goal can be marked complete>

Stop if:
- <mechanically detectable conditions requiring Lee confirmation>

Handoff:
<optional; how a later Codex session should resume if interrupted, and for long Missions how Lee should choose/approve the next Current Goal>
```

## Output Shape

For nontrivial or high-impact requests, return a review block before the final prompt:

```text
Goal Review:
- What this goal will do: <short Korean summary>
- What this goal will not do: <clear exclusions>
- Completion check: <how Lee/Codex can tell it is done>
- Main stop/approval boundary: <the most important approval gate>
- Mission fit: <why this Current Goal moves the Mission forward, or "N/A" for short tasks>
```

Then provide the final ready-to-paste `/goal`.

For long-running `Mission` work, make next-goal expansion part of the goal's expected deliverables:

- Include `Next Goal candidates` with 1-3 bounded candidates.
- Mark one `Recommended Next Goal`.
- Expand the recommendation into a complete, high-quality `Ready-to-paste next /goal`.
- Do not create files by default.
- Do not lower the next goal contract quality to reduce length.
- If bulky context makes the answer too long, use an inline `Detailed Handoff` block or ask Lee before creating/updating a file.

## Section Guidance

- `Mission`: use for large efforts such as Robot Wiki, TARS, or long learning programs. It supplies direction but must not be the finish line.
- `Current Goal`: phrase as a target state, not just activity. Prefer "produce/update/verify X" over "work on X".
- `First context to inspect`: list exact local paths when known. If a path is unknown, write `TODO: Lee to provide path` instead of inventing it.
- `Scope`: name both the allowed area and clear exclusions so Codex does not expand into adjacent cleanup.
- `Constraints`: combine allowed actions and safety rules. Include approval gates for installs, destructive commands, large downloads, hardware actuation, or runtime-heavy stacks.
- `Done when`: make each item evidence-mappable. Nontrivial goals should include at least one artifact, one verification/readback step, and one final reporting requirement.
- `Stop if`: use concrete triggers such as missing target file, failing prerequisite, unexpected dirty state in a target file, secret exposure, or a required approval boundary.
- `Handoff`: include the first file/status artifact to inspect and the rule for continuing from the first unmet `Done when` item.

## Unattended Continuation

Codex cannot directly set a blocked goal back to a resume state. The goal prompt can only reduce unnecessary blocking by defining when Codex should keep going.

Use this rule only when Lee asks for unattended progress or when a long-running goal clearly benefits from it:

- If the only blocker is a simple "continue?" confirmation and the next step is already inside `Scope` and `Constraints`, wait 5 minutes for Lee. If there is no reply, treat that as permission to continue with the safest in-scope next step.
- Do not mark the goal `blocked` just because progress confirmation would be convenient. Continue from the first unmet `Done when` item when safe.
- Never treat silence as approval for destructive actions, file deletion/move, broad rewrites, installs, network downloads, paid APIs, hardware actuation, secret/private data handling, scope expansion, or AGENTS/profile/skill/harness changes.
- If the goal is already marked `blocked`, a later session still needs Lee's resume/continue instruction or an explicit new `/goal`; the prompt cannot change that state by itself.

## Long-Mission Continuity

When the prompt includes `Mission`, make continuation explicit without turning the goal into an endless task.

- Add a `Done when` item: "If the Mission remains unfinished, include `Next Goal candidates` with 1-3 concrete candidates, `Recommended Next Goal`, and a complete `Ready-to-paste next /goal` for the recommendation."
- Add a `Handoff` rule: "A later session should use only the Lee-approved candidate as the next Current Goal."
- Keep next-goal candidates bounded and verifiable. Each candidate should be small enough to become one future `/goal`.
- Do not compact the recommended next `/goal` so much that it loses Objective, context, scope, constraints, done condition, stop boundary, verification, or handoff quality.
- Prefer inline `Detailed Handoff` for bulky context. Ask Lee before creating a separate handoff file.
- Do not let Codex automatically continue into the recommended next goal unless Lee explicitly asks for that behavior.

## Audit-Friendliness Check

Before returning the prompt, check:

- The final prompt distinguishes long-term `Mission` from `Current Goal` when the task is part of a larger project.
- `Current Goal` has one bounded target state.
- `Done when` can be mapped to evidence, checklist items, files, commands, screenshots, or readback.
- Nontrivial or high-impact prompts include a `Goal Review` block before the ready-to-paste `/goal`.
- Long-running `Mission` prompts include `Next Goal candidates`, `Recommended Next Goal`, and a complete `Ready-to-paste next /goal` requirement instead of leaving Lee to invent the next step from scratch.
- If unattended progress is requested, the 5-minute rule is present and explicitly excludes high-risk approvals.
- `Stop if` is mechanically detectable; it is not just "if confused".
- Local files use absolute paths when known.
- The ready-to-paste `/goal` text is Korean-facing by default; English remains only for stable labels, paths, commands, identifiers, or terms that are clearer untranslated.
- The prompt avoids vague verbs such as "improve", "clean up", "continue", "everything", or "all" unless paired with a bounded list and verification.
- The final `/goal` is practical to paste without sacrificing contract quality. Do not create files by default; use inline `Detailed Handoff` for bulky background or ask Lee before writing a file.

If the task is still unclear after this check, generate a short interview prompt instead of pretending the `/goal` is ready.

## Examples

### Robot Wiki Mission + Current Goal

```text
Mission:
Lee의 Robot Wiki를 ROS2, robotics-core, Physical AI, TARS 프로젝트 적용 지식이 재사용 가능한 하나의 큰 위키가 되도록 확장한다. 이 Mission은 방향이며 완료 조건이 아니다.

Current Goal:
Robot Wiki에서 다음으로 진행할 하나의 작은 확장 목표를 정하고, 그 목표를 실행 가능한 계획으로 정리한다. 이번 세션에서는 파일 이동이나 대량 작성까지 하지 말고, 첫 번째로 만들거나 갱신할 노트 묶음과 검증 방법을 확정한다.

First context to inspect:
- `/home/lee/llm-wiki/AGENTS.md`
- `/home/lee/llm-wiki/README.md`
- `/home/lee/llm-wiki/00-index.md`
- `/home/lee/llm-wiki/wiki/robot-wiki/_index.md`
- `/home/lee/llm-wiki/wiki/robot-wiki/ros2/_index.md`
- `/home/lee/llm-wiki/wiki/robot-wiki/physical-ai/_index.md`

Scope:
- In scope: Robot Wiki의 다음 한 조각을 고르는 계획, 후보 노트 경로, 링크 전략, 검증 방법.
- Out of scope: 실제 대량 작성, 폴더 이동, 기존 노트 삭제, 스킬 설치.

Constraints:
- Korean-facing summary를 기본으로 쓴다.
- 현재 로컬 파일을 먼저 읽고, 없는 경로를 추측하지 않는다.
- Isaac Sim/Lab 또는 RoboLab 실행은 Lee가 명시적으로 허용하기 전까지 제외한다.
- 단순히 계속 진행해도 되는지 확인하는 상황에서 Lee가 5분 동안 응답하지 않으면, `Scope`와 `Constraints` 안의 가장 보수적인 다음 단계를 계속한다. 침묵을 파일 삭제/이동, 설치, 다운로드, hardware 실행, scope 확장 승인으로 해석하지 않는다.

Done when:
- 다음 확장 목표 하나가 선택되어 있고 선택 이유가 현재 Robot Wiki 구조와 연결되어 있다.
- 만들거나 갱신할 노트 후보 경로와 각 노트의 역할이 적혀 있다.
- 링크/검증 방법이 구체적으로 적혀 있다.
- Mission이 아직 남아 있으므로 `Next Goal candidates` 1-3개, `Recommended Next Goal` 1개, 추천 후보에 대한 `Goal Review`, 완성된 `Ready-to-paste next /goal`이 포함되어 있다.

Stop if:
- Robot Wiki index나 라우팅 파일을 읽을 수 없다.
- 필요한 경로가 실제로 존재하지 않는데 추정으로 진행해야 한다.
- 계획을 넘어 파일 이동, 삭제, 설치가 필요해진다.

Handoff:
다음 세션은 이 답변의 `Ready-to-paste next /goal`을 검토하되, Lee가 승인한 경우에만 새 `Current Goal`로 실행한다. 중단된 경우에는 위 `First context to inspect` 파일을 먼저 읽고 첫 번째 미완료 `Done when` 항목부터 이어간다.
```

### Eclipse V3 Short Code Goal

```text
Current Goal:
Lee가 지정한 `eclipse-v3` 제어 코드의 가독성을 높이되 런타임 동작은 유지한다.

First context to inspect:
- `/home/lee/eclipse-v3/AGENTS.md`
- TODO: Lee가 지정한 대상 파일 경로

Scope:
- In scope: 지정 파일의 최소 가독성 수정과 repo guide가 요구하는 가까운 문서 업데이트.
- Out of scope: 알고리즘 변경, 토픽명/설정값/public interface 변경, 새 dependency 추가, 광범위한 포맷팅.

Constraints:
- 동작 변경이 필요해 보이면 먼저 Lee에게 묻는다.
- Korean comments는 의미가 무거운 변수나 분기에만 추가한다.
- 변경 전후 diff를 확인해 의도치 않은 동작 변경을 찾는다.

Done when:
- 지정 파일만 또는 repo guide가 요구한 직접 관련 문서만 변경되어 있다.
- edited Python files에 대해 `python3 -m py_compile` 또는 repo가 제공하는 focused validation이 통과한다.
- 최종 답변에 변경 파일, 검증 명령, 동작 보존 관점의 diff 요약이 포함되어 있다.

Stop if:
- 지정 대상 파일이 없거나 Lee의 요청 범위를 확정할 수 없다.
- 동작 변경 없이는 목표를 달성할 수 없다.
- target file에 예상치 못한 충돌성 변경이 있어 사용자의 판단이 필요하다.
```

### Robotics Practice Goal

```text
Mission:
Lee가 ROS2와 Physical AI를 실제 로봇 프로젝트에 적용할 수 있도록, 작은 실습과 재사용 가능한 위키 기록을 계속 축적한다.

Current Goal:
하나의 작은 ROS2 또는 Physical AI 학습 질문을 정하고, 로컬에서 안전하게 확인 가능한 최소 실습을 수행한 뒤 practice card로 저장한다.

First context to inspect:
- `/home/lee/AGENTS.md`
- `/home/lee/llm-wiki/AGENTS.md`
- `/home/lee/llm-wiki/_system/agents/tasks/robotics-expert.md`
- `/home/lee/llm-wiki/_system/agents/tasks/robot-lab-workflow.md`
- TODO: Lee가 지정한 project, dataset, or package path

Scope:
- In scope: 한 가지 질문, 한 가지 작은 실행/확인, 한 개 practice card.
- Out of scope: 큰 simulator setup, 대용량 모델 다운로드, GPU-heavy runtime, hardware actuation, Isaac Sim/Lab, RoboLab.

Constraints:
- raw/source evidence와 synthesis를 분리한다.
- 공식 문서나 현재 정보가 필요한 경우에는 출처를 확인하고 링크를 남긴다.
- 비밀 정보나 private raw log를 저장하지 않는다.
- 단순 진행 확인만 필요한 경우 Lee가 5분 동안 응답하지 않으면 안전한 로컬 조사나 기록 정리처럼 `Scope` 안의 보수적인 다음 단계를 계속한다. 침묵을 대용량 다운로드, hardware 실행, Isaac/RoboLab 실행, private data 저장 승인으로 해석하지 않는다.

Done when:
- practice card에 `목적 -> 실행 명령/코드 -> 결과 -> 실패/경고 -> Lee가 실수했을 때 대응법 -> 다음 실습`이 채워져 있다.
- 실행 결과 또는 실패 로그가 구체적으로 기록되어 있다.
- 생성/수정한 note path와 검증 결과가 최종 답변에 포함되어 있다.
- Mission이 아직 남아 있으므로 다음 작은 실습 `Next Goal candidates` 1-3개, `Recommended Next Goal` 1개, 추천 후보에 대한 `Goal Review`, 완성된 `Ready-to-paste next /goal`이 포함되어 있다.

Stop if:
- Isaac/RoboLab 실행, hardware actuation, large download, secret/private log 저장이 필요해진다.
- 실습 환경이 없어 결과를 검증할 수 없고 추정으로만 써야 한다.

Handoff:
중단되면 practice card path와 마지막 command output을 먼저 읽고, 비어 있는 practice card section부터 이어간다. 완료된 뒤에는 Lee가 승인한 `Ready-to-paste next /goal`만 새 `Current Goal`로 실행한다.
```
