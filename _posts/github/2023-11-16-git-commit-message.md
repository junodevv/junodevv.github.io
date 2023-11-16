---
title: "[Github] Angular 커밋 메시지 컨벤션"
category: GitHub
tags: git github commit-message
---

우테코를 진행하며 알게된 Angular 커밋 메시지 컨벤션 요약

---

# Angular 커밋 메시지 컨벤션 요약

**커밋 메시지 구조:**

```
<타입>(<범위>): <메시지>
```

1. `<타입>`: 커밋의 종류, 일반적으로 다음 중 하나
    - `feat`: 새로운 기능 추가
    - `fix`: 버그 수정
    - `docs`: 문서 변경
    - `style`: 코드 스타일 변경 (공백, 포맷팅 등)
    - `refactor`: 코드 리팩토링
    - `test`: 테스트 코드 추가 또는 수정
    - `chore`: 빌드 프로세스, 도구, 라이브러리 추가 또는 수정
    - `perf`: 성능 개선 관련 커밋
2. `<범위>` (옵션): 커밋의 변경 범위를 나타냄. 필요에 따라 생략할 수 있음.
3. `<메시지>`: 변경사항에 대한 간결한 설명을 제공. 커밋 메시지는 명령조를 사용하여 작성하며, 첫 글자는 대문자로 시작하고 마침표를 사용하지 않는다. 예: "Add new feature", "Fix issue with login".

**예시 커밋 메시지:**

- `feat(user-auth): Add new user registration feature`
- `fix(bug-123): Fix issue with login authentication`
- `docs(readme): Update project documentation`
- `style(css): Format the styles in main.css`
- `refactor(codebase): Refactor the core data processing logic`
- `test(api): Add unit tests for API endpoints`
- `chore(deployment): Configure deployment process`
- `perf(optimization): Improve database query performance`

이러한 커밋 메시지 컨벤션은 코드베이스의 변경 내용을 명확하게 문서화하고 이해하기 쉽게 만들어주며, 협업 및 유지 보수를 용이하게 한다. 코드 저장소와 프로젝트에서 이러한 컨벤션을 따르는 것이 좋다.

## reference
[Angular 커밋 메시지 컨벤션](https://gist.github.com/stephenparish/9941e89d80e2bc58a153)
[ChatGPT](https://chat.openai.com/) 