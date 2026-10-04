# 보드게임 참조표 제작 표준 가이드 (AutoManual Standards)

본 문서는 `AutoManual` 프로젝트의 모든 보드게임 참조표가 시각적·기능적 통일성을 유지하고, 일관된 사용자 경험(UX)을 제공하기 위한 **필수 참조표 개발 표준 규격**입니다.

> ⚠️ **핵심 규칙:**  
> **앞으로 제작되는 모든 신규 참조표는 `RockHard1977.html` 및 `Recall.html`과 동일하게 `📝 하우스 룰 & 메모` 탭을 반드시 기본 탑재해야 합니다.**

---

## 1. 표준 페이지 아키텍처 (Page Architecture)

모든 참조표는 단일 HTML 파일로 완결되며, 다음의 5단 레이아웃을 엄격히 준수합니다:

```
[1. 상단 고정 내비게이션 & 실시간 검색창 (.top-nav-bar)]
           ↓
[2. 표준 탭 메뉴 바 (.tab-bar)]
  - Tab 1: 📖 규칙 및 참조표 (기본)
  - Tab 2: (선택) ⚠️ 공식 에라타 & FAQ / 🎲 특수 시뮬레이터·도구
  - Tab N: 📝 하우스 룰 & 메모 (★ 필수 탑재!)
           ↓
[3. 메인 카드 컨테이너 (.card)]
  - 히어로 헤더 (.game-hero, 메타 뱃지 필)
  - 빠른 목차 바로가기 (.shortcut-nav 칩 링크)
  - 탭 컨텐츠 1: 규칙 및 참조표 본문 (#tab-manual)
  - 탭 컨텐츠 N: 하우스 룰 & 메모 관리 (#tab-notes)
           ↓
[4. 표준 스크립트 모듈]
  - 탭 제어 (switchTab)
  - 탭 연동 실시간 검색 & 키보드 단축키
  - LocalStorage 기반 하우스 룰 & 메모 CRUD + JSON 백업/복원
  - URL Query(?q=) 및 Hash(#sec-) 자동 탭 활성화
           ↓
[5. A4 인쇄 스타일 (@media print)]
```

---

## 2. 필수 컴포넌트 규격 상세

### 2.1 상단 고정 내비게이션 (`.top-nav-bar`)
- **최대 폭:** `max-width: 860px`, `sticky; top: 10px; z-index: 1000;`
- **배경:** 반투명 글래스모피즘 `backdrop-filter: blur(10px);`
- **내부 요소:**
  - `btn-home`: `index.html` 허브 목록 링크 (`← 참조표 목록`)
  - `btn-aid`: (있을 경우) 인쇄용 에이드 링크 (`📄 인쇄용 에이드 vX.X`)
  - 게임 뱃지: 게임 고유 테마 색상 뱃지 (예: `<span class="badge-recall">⏳ Recall</span>`)
  - 검색 박스 (`.search-box`):
    - `<span class="search-icon">🔍</span>`
    - `<input type="text" id="searchInput" placeholder="...">`
    - `<span id="searchCount" class="search-count"></span>`
    - `<button type="button" id="prevMatch" class="search-nav-btn">▲</button>`
    - `<button type="button" id="nextMatch" class="search-nav-btn">▼</button>`
    - `<button type="button" id="clearSearch" class="search-clear-btn">✕</button>`

### 2.2 표준 탭 메뉴 바 (`.tab-bar`)
- 탭 버튼은 `.tab-btn` 클래스를 사용하며, 활성 상태 시 `.active` 클래스 적용.
- **모든 참조표 필수 구성:**
  ```html
  <div class="tab-bar">
    <button type="button" class="tab-btn active" data-tab="tab-manual">
      <span>📖 규칙 및 참조표</span>
    </button>
    <!-- 선택: 에라타, FAQ, 주사위 계산기 등 -->
    <button type="button" class="tab-btn" data-tab="tab-notes">
      <span>📝 하우스 룰 & 메모</span>
      <span id="notesBadge" class="tab-count">0</span>
    </button>
  </div>
  ```

### 2.3 히어로 헤더 & 빠른 목차
- **히어로 헤더 (`.game-hero`)**:
  - `game-hero-title`: 게임명 (한글 + 영문)
  - `game-hero-subtitle`: 한 줄 테마/스토리 소개
  - `meta-pills`: 인원, 플레이 시간, 핵심 장르/메커니즘, 디자이너, 룰북 버전 뱃지
- **빠른 목차 바로가기 (`.shortcut-nav`)**:
  - 본문의 주요 `id="sec-..."`로 이동하는 칩 링크들을 모아 상단에 제공

### 2.4 하우스 룰 & 메모 탭 (`#tab-notes`) ★필수
- **폼 입력창:**
  - 제목 입력: `<input type="text" id="noteTitle" class="form-input">`
  - 카테고리 선택: `<select id="noteCategory" class="form-select">`
    - `🏠 하우스 룰`
    - `⚠️ 에라타 메모`
    - `⚙️ 세팅 팁`
    - `💡 전략/참고`
  - 내용 입력: `<textarea id="noteContent" class="form-textarea">`
  - 컨트롤 버튼:
    - `💾 메모 저장하기` (`#btnSaveNote`)
    - `JSON 백업` (`#btnExportNotes`)
    - `JSON 복원` (`#btnImportNotes`) + 숨김 파일 인풋 (`#importFileInput`)
- **저장소 키 규칙:**
  - 게임 ID별로 격리된 로컬스토리지 키를 사용합니다:
    - 록 하드: `rockhard1977_user_notes`
    - 리콜: `recall_user_notes`
    - 신규 게임: `<game_id>_user_notes`
- **목록 표시 (`#notesContainer`):**
  - 저장된 메모가 없을 경우 점선 박스의 빈 상태 안내문 노출
  - 개별 메모 카드: 제목, 삭제 버튼, 카테고리 필 뱃지, 작성 일시(`YYYY-MM-DD HH:MM`), 본문

---

## 3. 표준 JavaScript 동작 요건

1. **탭 자동 전환 기능이 결합된 검색 (`goToMatch`):**
   - 검색 결과 점프 시 해당 노드가 속한 부모 탭(`.tab-content`)이 숨겨져(`display: none`) 있다면 자동으로 `switchTab(parentTab.id)`를 호출하여 탭을 열어준 뒤 스크롤해야 합니다.
2. **키보드 단축키 지원:**
   - `/`: 검색창 즉시 포커스 (인풋/텍스트에어리어 작성 중이 아닐 때)
   - `Enter`: 다음 검색 결과로 이동
   - `Shift + Enter`: 이전 검색 결과로 이동
   - `Escape`: 검색어 지우기 및 포커스 해제
3. **URL 해시(#) 및 파라미터(?q=) 연동:**
   - URL에 `#sec-...` 해시가 포함되어 유입되면 해당 섹션이 포함된 탭을 자동 활성화하고 스무스 스크롤로 포커스합니다.
   - URL에 `?q=키워드` 파라미터가 포함되어 유입되면 즉시 검색을 수행합니다.

---

## 4. 메인 허브(`index.html` & `search-data.js`) 연동 체크리스트

새로운 참조표를 추가할 때는 다음 단계를 따릅니다:

1. `search-data.js`의 `REFERENCE_DATABASE` 배열에 새 객체 등록:
   ```javascript
   {
     id: "게임ID",
     title: "게임 타이틀 (한글/영문)",
     subtitle: "서브타이틀",
     version: "v1.0",
     file: "게임HTML파일명.html",
     aidFile: "인쇄용에이드파일명.html", // 있을 경우
     tags: ["태그1", "태그2", "태그3"],
     description: "게임 설명 요약",
     sections: [
       {
         id: "sec-xxx",
         title: "섹션 제목",
         category: "카테고리",
         keywords: ["키워드1", "키워드2"],
         content: "검색용 텍스트 내용 요약..."
       }
     ]
   }
   ```
2. `index.html` 상단의 추천 빠른 검색어 태그(`.quick-tags`)에 대표 키워드 추가.
