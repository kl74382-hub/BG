# 💡 AI 토큰 소모 없이 직접 카드/표 탑재 및 파일 전달하는 초간단 매뉴얼

앞으로 AI 토큰 사용량을 아끼면서 빠르게 작업하실 수 있도록 **(1) AI에게 파일 전달할 때 토큰 절약법**과 **(2) 이미 번역된 표를 내가 직접 HTML 참조표에 복붙하여 탑재하는 법**을 정리했습니다.

---

## 1. AI에게 다량의 이미지/문서를 맡길 때 (토큰 & 시간 절약법)

1. **채팅창에 이미지를 직접 드래그해서 올리지 마세요:**
   * 채팅창은 한 번에 5장 제한이 있고, 여러 번 나누어 올리면 대화 턴이 길어져 사용량이 늘어납니다.
2. **`g:\AutoManual` 하위 폴더(예: `cards`, `TEST` 등)에 파일을 넣고 경로만 알려주세요:**
   * 예: *"cards 폴더에 넣었어, 23Knives.html에 바로 탑재해줘"* 한 줄만 입력하시면 한 번에 읽고 바로 파일까지 수정해 드립니다.

---

## 2. 내가 직접 HTML 참조표에 새 탭과 표를 탑재하는 3단계 (AI 사용량 0)

이미 마크다운(`.md`)이나 엑셀로 정리된 표를 `23Knives.html`, `Recall.html` 등에 직접 추가하고 싶을 때는 아래 **3곳**만 복사+붙여넣기 하시면 됩니다.

### Step 1. 상단 탭 버튼(`.tab-bar`)에 버튼 1개 추가
HTML 파일에서 `<div class="tab-bar">`를 검색(`Ctrl + F`)한 뒤, 원하는 탭 버튼 코드를 한 줄 추가합니다:

```html
<!-- data-tab="탭아이디" 만 고유하게 지정하면 스크립트가 자동으로 탭 전환을 연결합니다! -->
<button type="button" class="tab-btn" data-tab="tab-my-cards">
  <span>🃏 추가 카드 목록</span>
  <span class="tab-count">33</span>
</button>
```

### Step 2. 본문 하단에 탭 컨텐츠 박스(`.tab-content`)와 표 붙여넣기
HTML 파일에서 `<!-- TAB 3: 하우스 룰 & 메모 관리 -->` 바로 위에 아래 템플릿을 붙여넣습니다:

```html
<!-- 위에서 지정한 data-tab 이름과 id를 똑같이 맞춥니다 -->
<div id="tab-my-cards" class="tab-content" style="display:none;">
  <section id="sec-my-cards">
    <h2 class="section-header" style="margin-top:0;">🃏 카드 도감 제목</h2>

    <div style="overflow-x:auto;">
      <table class="rule-table">
        <thead>
          <tr>
            <th style="width:50px; text-align:center;">번호</th>
            <th style="width:180px;">카드 이름</th>
            <th>효과 설명</th>
          </tr>
        </thead>
        <tbody>
          <!-- 아래 <tr>...</tr> 블록을 카드 개수만큼 복사해서 내용을 채웁니다 -->
          <tr>
            <td style="text-align:center; font-weight:800;">1</td>
            <td><strong>카드 영문명</strong><br>카드 한글명</td>
            <td>카드 효과 내용</td>
          </tr>
        </tbody>
      </table>
    </div>
  </section>
</div>
```

> **💡 팁 (마크다운 표를 HTML `<table>`로 1초 만에 바꾸기):**
> 마크다운 표(`| 번호 | 이름 | ... |`)가 있을 때 무료 웹 변환기([TableConvert](https://tableconvert.com/markdown-to-html))에 붙여넣으면 `<table>...</table>` 코드로 즉시 변환됩니다. 변환된 코드의 `<table>` 태그를 `<table class="rule-table">`로만 바꾸어 붙여넣으면 디자인이 100% 자동 적용됩니다!

### Step 3. (선택) 인쇄 시 숨김 처리 & 메인 허브(`search-data.js`) 검색 연동
* **인쇄 스타일:** HTML 상단 `@media print` 안의 `#tab-tools, #tab-notes` 옆에 `, #tab-my-cards`를 추가하면 인쇄할 때 해당 탭이 깔끔하게 숨겨집니다.
* **통합 검색 연동:** `search-data.js`에서 해당 게임의 `sections: [ ... ]` 배열 맨 끝에 `{ id: "sec-my-cards", title: "섹션 제목", category: "카드 도감", keywords: ["키워드1", "키워드2"], content: "요약 설명" }` 블록 하나만 추가하면 메인 페이지(`index.html`)에서도 즉시 검색됩니다.

---

## 3. GitHub Pages 실배포(업로드) 직접 하는 법 (AI 사용량 0)

로컬(`g:\AutoManual`)에서 파일을 수정하거나 추가한 뒤 실제 웹페이지(`https://kl74382-hub.github.io/BG/`)에 반영하려면 GitHub에 업로드(Push)해야 합니다.

### 방법 A. 터미널(PowerShell)에서 한 줄로 배포하기
`g:\AutoManual` 폴더에서 터미널을 열고 아래 명령어를 붙여넣고 엔터를 누르면 즉시 실배포됩니다:

```powershell
git add *.html *.md *.js; git commit -m "docs: 매뉴얼 업데이트"; git push origin main
```
*(※ `cards/`나 `TEST/` 같은 원본 이미지·PDF 작업 폴더는 빼고 웹페이지에 필요한 `.html`, `.md`, `.js` 파일만 깔끔하게 올라갑니다.)*

### 방법 B. GitHub 웹페이지에서 직접 파일 업로드하기
1. [GitHub 저장소 (kl74382-hub/BG)](https://github.com/kl74382-hub/BG) 접속
2. 우측 상단 **`Add file` → `Upload files`** 클릭
3. 수정한 `.html` 또는 `.js` 파일을 마우스로 끌어다 놓고 하단 초록색 **`Commit changes`** 버튼 클릭
4. 약 1~2분 뒤 GitHub Pages 실배포 주소에 자동 반영됩니다.

