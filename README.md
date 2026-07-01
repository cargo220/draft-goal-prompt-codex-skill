# draft-goal-prompt Codex Skill

Lee가 Codex `/goal`에 붙여넣을 목표 프롬프트를 작성할 때 쓰는 개인 Codex skill입니다.

이 스킬은 장기 방향인 `Mission`과 이번 세션에서 완료할 `Current Goal`을 분리합니다. 또한 완료 조건, 중단 경계, 다음 goal 후보, handoff 규칙을 함께 정리해 Codex가 긴 작업을 더 안정적으로 이어가게 합니다.

## Install

GitHub에서 설치할 때는 repo 안의 skill folder를 지정합니다.

```bash
python3 /home/lee/.codex/skills/.system/skill-installer/scripts/install-skill-from-github.py \
  --repo <owner>/<repo> \
  --path draft-goal-prompt
```

설치 후 새 Codex 세션을 시작하거나 skills를 reload하면 `$draft-goal-prompt`로 사용할 수 있습니다.

## Skill Path

```text
draft-goal-prompt/SKILL.md
```

## Notes

- 최종 `/goal` 본문은 한국어 기본입니다.
- `Mission`이 남아 있으면 다음 `Current Goal` 후보를 제안합니다.
- 단순 진행 확인만 필요한 경우에는 5분 무응답 후 안전한 범위에서 계속 진행하도록 목표 문구를 작성합니다.
- 침묵은 삭제, 이동, 설치, 다운로드, hardware 실행, secret/private data 처리, scope 확장 승인으로 해석하지 않습니다.
