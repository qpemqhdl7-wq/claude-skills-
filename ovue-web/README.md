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
- 히어로 휴대폰 프레임 SVG: 21st.dev [Hero Section with Device Mockups](https://21st.dev/@solaceui/components/hero-section-6)에서 가져옴
- 진행 과정: 21st.dev [How It Works Timeline](https://21st.dev/@olewandowski1/components/how-it-works-2) — 아이콘 + 세로 연결선 타임라인을 정적 HTML로 변환. 왼쪽 고정 제목, 스크롤에 맞춰 그려지는 선, 3D 등장 효과 추가

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

## 첫 화면 (밤의 풀빌라 컨셉)
- 3초 인트로: 로고 모양 브라우저 창 테두리가 금색으로 그려짐(0~0.6초) → 밤 수영장 물빛이 차오름(~1.6초) → 풀빌라 예시 홈페이지가 조립됨(~2.4초) → 창이 3D로 기울며 휴대폰·예약 알림이 펼쳐지고 문구 등장(~3초)
- 물빛은 WebGL 셰이더(직접 작성)로 실시간 렌더링, 마우스를 따라 반사광이 움직임. 화면 밖이면 멈춤
- 같은 방문 중 재방문 시 인트로 0.3배 속도, ‘건너뛰기’ 버튼 제공. ‘동작 줄이기’ 설정이면 인트로 없이 완성 화면 표시
- 히어로 속 사이트 ‘LUMI’는 가상 브랜드 예시 (실제 풀빌라 포트폴리오가 생기면 사진으로 교체)
- 히어로 아래 띠 대신 ‘공간의 온도까지 / 화면에 담습니다’ 외곽선 문구가 스크롤에 따라 빛으로 채워짐
- 톤: 혼합 다크 — 헤더·히어로·메시지·강점·오픈 후 지원·상담·푸터는 밤 그라데이션, 포트폴리오·요금·진행 과정·FAQ는 밝게
