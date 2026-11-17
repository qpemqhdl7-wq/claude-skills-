# claude-skills

홈페이지 제작용 Claude Code 스킬 모음입니다. 플러그인 이름은 **design-kit**입니다.

## 설치 (한 번만)

**내 컴퓨터의 Claude Code:** 아래 명령을 Claude Code 안에 입력하세요.

```
/plugin install design-kit --marketplace qpemqhdl7-wq/claude-skills-
```

예전 버전에서 위 명령이 안 되면 두 줄로 나눠 입력하세요.

```
/plugin marketplace add qpemqhdl7-wq/claude-skills-
/plugin install design-kit@my-skills
```

**클라우드 세션(claude.ai/code)에서 다른 프로젝트에 쓰기:** 그 프로젝트 저장소의 `.claude/settings.json`에 아래 내용을 넣고 커밋하세요.

```json
{
  "extraKnownMarketplaces": {
    "my-skills": {
      "source": { "source": "github", "repo": "qpemqhdl7-wq/claude-skills-" }
    }
  },
  "enabledPlugins": {
    "design-kit@my-skills": true
  }
}
```

## 21st.dev 연결 (짱구맨이 자동으로 사용)

내 컴퓨터 터미널에서 한 번만 실행하세요. `21st_sk_...` 자리에 21st.dev에서 받은 키를 넣습니다. 키는 이 저장소에 절대 넣지 마세요.

```
claude mcp add --scope user --transport http 21st https://21st.dev/api/mcp --header "x-api-key: 21st_sk_..."
```

연결이 안 되어 있으면 짱구맨이 알려 주고 21st 없이 계속 만듭니다.

## 사용법

```
/짱구맨 강남 필라테스 스튜디오 홈페이지, 체험 수업 예약이 목표
/jjg 강남 필라테스 스튜디오 홈페이지   ← 같은 기능의 짧은 별칭
```

또는 그냥 "카페 홈페이지 만들어줘"라고 말해도 자동으로 불러옵니다.

`/짱구맨`이 하는 일:

1. 업종·타깃·목표 행동을 정리합니다. 빠진 것만 최대 3개 질문합니다.
2. ui-ux-pro-max 데이터로 스타일·색상·폰트를 정하고 `design-system/` 폴더에 저장합니다.
3. 21st.dev에서 섹션별 컴포넌트(히어로, 가격표, 후기 등)와 로고를 찾아 디자인에 맞게 고쳐 씁니다.
4. 페이지를 만듭니다. 프로젝트가 없으면 `index.html` 한 파일(Tailwind)로 만듭니다.
5. Motion으로 등장·호버·스크롤 애니메이션을 넣습니다 (React면 `npm install motion` 자동 실행).
6. 모바일·데스크톱 스크린샷을 찍어 깨진 곳을 고칩니다.
7. 결과와 바꿔야 할 임시 문구를 알려 줍니다.

## 들어 있는 스킬

| 스킬 | 용도 |
|---|---|
| `짱구맨` (별칭 `jjg`) | 홈페이지 원스톱 제작 (이 저장소 자체 제작) |
| `ui-ux-pro-max` | 스타일 80종, 팔레트 192종, 폰트 조합 74종, UX 가이드라인 검색 |
| `design`, `design-system`, `brand`, `ui-styling`, `banner-design`, `slides` | 로고·토큰·브랜드·배너·슬라이드 |

`짱구맨` 외의 스킬은 [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) v2.15.0에서 가져왔습니다 (MIT, `LICENSE-ui-ux-pro-max`).
