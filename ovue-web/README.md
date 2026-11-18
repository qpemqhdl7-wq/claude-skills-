# O.Vue WEB 홈페이지

반응형 홈페이지 제작·리뉴얼 서비스 O.Vue WEB의 원페이지 랜딩 사이트입니다.

- `index.html` — 빌드 없이 더블클릭으로 열리는 단일 파일 (CSS 내장, Motion 애니메이션 CDN)
- `design-system/o-vue-web/MASTER.md` — 색상·폰트·간격 디자인 토큰

## 교체가 필요한 플레이스홀더 (`TODO` 주석으로 표시)
- 요금제 가격 (`000,000원`), 옵션 가격
- 카카오톡 채널 링크, 전화번호, 이메일
- 푸터 사업자 정보 (대표자, 사업자등록번호)
- 포트폴리오 썸네일 (현재 가상 브랜드 예시)
- `og-image.png` (링크 공유 이미지, 1200×630)
- 상담 폼 전송 처리 (현재 화면 메시지만 표시 — 구글 시트/폼 서비스 연결 필요)

## 사용한 외부 컴포넌트
- 강점(Why) 벤토 그리드: 21st.dev [bento grid 01](https://21st.dev/@avanishverma4/components/bento-grid-01) — React/framer-motion 원본을 정적 HTML + Motion(vanilla)으로 변환, O.Vue 디자인 토큰으로 재스타일

## 폰트
- 사이트 전체 폰트: **G마켓 산스** (Light 300 / Medium 500 / Bold 700) — 업체 제공 원본 OTF를 woff2로 변환해 `fonts/`에 포함 (외부 CDN 불필요)
- 용량 최적화: 자주 쓰는 한글 2,350자 + 자모 + 영문·숫자·기호 + 페이지에 쓰인 모든 글자만 남김 (3종 합계 1.26MB → 약 430KB). 목록에 없는 드문 글자는 기기 기본 폰트로 표시됩니다.
- 문구에 드문 글자를 새로 넣었다면 `fonts/subset-chars.txt`에 그 글자를 추가하고 원본 OTF로 다시 생성하세요:
  `pyftsubset GmarketSansMedium.otf --text-file=fonts/subset-chars.txt --flavor=woff2 --layout-features='*' --output-file=fonts/GmarketSansMedium.woff2` (Light·Bold도 동일)

## 로고 (`assets/`)
업체 로고 이미지를 벡터(SVG)로 변환해 웹용으로 정리했습니다.
- `ovue-logo-gold.svg` / `ovue-logo-white.svg` — 전체 로고 (Website Creation 포함): 푸터 등 넓은 자리
- `ovue-badge-gold.svg` / `ovue-badge-white.svg` — 태그라인 없는 로고: 헤더 등 작은 자리
- `favicon.svg` — 브라우저 탭 아이콘 (네이비 바탕 + 금색 창틀 + O.)
