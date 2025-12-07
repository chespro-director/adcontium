<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <title>Adcontium MVP — README (HTML)</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <style>
    :root{
      --bg:#f7fafc; --card:#ffffff; --muted:#6b7280; --accent:#0ea5a4; --code:#111827;
      --maxw:900px;
    }
    html,body{height:100%}
    body{
      margin:0;
      font-family:Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Noto Sans KR", "Helvetica Neue", Arial;
      background:linear-gradient(180deg, #fbfdfe, var(--bg));
      color:#0f172a;
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      padding:28px;
      display:flex;
      justify-content:center;
    }
    .container{width:100%; max-width:var(--maxw); background:var(--card); border-radius:12px; box-shadow:0 8px 30px rgba(2,6,23,0.08); padding:28px;}
    header{border-bottom:1px solid #eef2f7; padding-bottom:18px; margin-bottom:18px;}
    h1{margin:0; font-size:1.6rem}
    p.lead{margin:6px 0 0; color:var(--muted)}
    section{margin-top:20px}
    h2{margin:0 0 8px; font-size:1.05rem}
    .muted{color:var(--muted); font-size:0.95rem}
    ul{margin:8px 0 8px 20px}
    pre{background:#0f172a; color:#e6eef6; padding:14px; border-radius:8px; overflow:auto; font-family:ui-monospace, SFMono-Regular, Menlo, Monaco, "Roboto Mono", "Courier New", monospace; font-size:13px}
    code{background:#eef2f7; padding:2px 6px; border-radius:6px; font-family:ui-monospace, SFMono-Regular, Menlo, Monaco, "Roboto Mono", "Courier New", monospace; font-size:13px}
    table{width:100%; border-collapse:collapse; margin-top:8px}
    th,td{border:1px solid #edf2f7; padding:8px; text-align:left}
    .kbd{display:inline-block; padding:2px 8px; background:#111827; color:#fff; border-radius:6px; font-size:12px}
    .actions{display:flex; gap:8px; margin-top:12px}
    .btn{display:inline-block; padding:8px 12px; border-radius:8px; background:var(--accent); color:white; text-decoration:none}
    .note{background:#fffbeb; border-left:4px solid #f59e0b; padding:10px; border-radius:6px; margin-top:8px}
    footer{margin-top:20px; border-top:1px solid #eef2f7; padding-top:12px; color:var(--muted); font-size:0.9rem}
    .columns{display:grid; grid-template-columns:1fr 1fr; gap:12px}
    @media(max-width:720px){ .columns{grid-template-columns:1fr} pre{font-size:12px} }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>Adcontium MVP — AI 기반 콘텐츠 제작 여정 플랫폼</h1>
      <p class="lead">포스터 → 전단지 → 리플릿 → 팸플릿 → 상세페이지 → 보도자료 → 블로그 → 영상까지 생성되는 풀 콘텐츠 파이프라인 (Next.js + Tailwind + Gemini + Firebase + PDFKit)</p>
    </header>

    <section>
      <h2>요약</h2>
      <p class="muted">본 프로젝트는 Adcontium의 AI 기반 콘텐츠 제작 워크플로우를 실험하고, 다국어·순차적 콘텐츠 생성 및 PDF 제안서 출력을 지원하는 MVP입니다. 또한 서버측 API와 연동하여 실제 Gemini(또는 유사 LLM) 호출을 수행하도록 설계되어 있습니다.</p>
    </section>

    <section>
      <h2>주요 기능</h2>
      <ul>
        <li><strong>프로젝트 생성</strong> (제품명 / 브리프 / 이미지 업로드 / 다국어 타깃)</li>
        <li><strong>콘텐츠 생성</strong> (AI 기반 자동 생성 — 포스터, 전단지, 리플릿, 팸플릿, 상세페이지, 보도자료, 블로그, 영상)</li>
        <li><strong>다국어</strong> — 한국어, 영어, 중국어, 일본어, 프랑스어, 힌디어, 인도네시아어, 베트남어, 태국어</li>
        <li><strong>콘텐츠 순차 흐름</strong> — 이전 단계의 확정 콘텐츠를 다음 단계에 전달</li>
        <li><strong>콘텐츠 확정(Finalize)</strong> — 제안서에 포함되는 항목 지정</li>
        <li><strong>PDF 제안서(PDFKit)</strong> — 확정된 콘텐츠만 PDF로 생성</li>
        <li><strong>Tailwind UI</strong> 기반의 반응형 프론트엔드 및 Next.js API</li>
      </ul>
    </section>

    <section>
      <h2>프로젝트 구조</h2>
      <pre><code>adcontium-nextjs/
├─ src/
│  ├─ pages/
│  │  ├─ index.tsx               # 한국어 Tailwind UI 페이지
│  │  ├─ api/
│  │  │  ├─ generate.ts         # AI 콘텐츠 생성 API
│  │  │  ├─ finalize.ts         # 콘텐츠 확정 API
│  │  │  └─ proposal_pdfkit.ts  # PDFKit 제안서 생성 API
│  ├─ components/
│  │  ├─ ProjectForm.tsx        # 프로젝트 생성 UI
│  │  └─ WorkflowPanel.tsx      # 콘텐츠 생성 흐름 UI
│  ├─ lib/
│  │  ├─ admin.ts               # Firebase Admin 초기화
│  │  └─ geminiServer.ts        # Gemini API 통신 모듈
└─ README.md</code></pre>
    </section>

    <section>
      <h2>설치 및 실행</h2>
      <ol>
        <li>패키지 설치
          <pre><code>npm install</code></pre>
        </li>
        <li>개발 서버 실행
          <pre><code>npm run dev</code></pre>
        </li>
        <li>브라우저 열기: <code>http://localhost:3000</code></li>
      </ol>
    </section>

    <section>
      <h2>환경변수 (.env.local)</h2>
      <pre><code>NEXT_PUBLIC_FIREBASE_API_KEY=...
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=...
NEXT_PUBLIC_FIREBASE_PROJECT_ID=...
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=...

FIREBASE_CLIENT_EMAIL=...
FIREBASE_PRIVATE_KEY=-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n
GEMINI_API_KEY=...
GEMINI_ENDPOINT=https://your-gemini-endpoint/v1/generate</code></pre>
      <p class="note">중요: <code>FIREBASE_PRIVATE_KEY</code>는 줄바꿈을 <code>\n</code>으로 이스케이프 하여 저장합니다.</p>
    </section>

    <section>
      <h2>API: 요청 예시 (Gemini)</h2>
      <p class="muted">서버가 Gemini(또는 유사 LLM)를 호출할 때 권장하는 요청/응답 형식 예시입니다.</p>
      <div class="columns">
        <div>
          <h3 class="muted">요청 (JSON)</h3>
          <pre><code>{
  "model": "gemini-2.5-flash",
  "system_instruction": "Generate poster content in Korean",
  "input": {
    "briefData": {...},
    "imageUrls": [...],
    "previousContents": [...],
    "targetLanguages": ["KR"]
  }
}</code></pre>
        </div>
        <div>
          <h3 class="muted">응답 (JSON)</h3>
          <pre><code>{
  "output": {
    "text": "생성된 콘텐츠 텍스트..."
  },
  "raw": { ... }
}</code></pre>
        </div>
      </div>
    </section>

    <section>
      <h2>콘텐츠 타입 — API용 표준 키</h2>
      <table>
        <thead>
          <tr><th>Human label</th><th>API type key</th></tr>
        </thead>
        <tbody>
          <tr><td>포스터</td><td><code>poster</code></td></tr>
          <tr><td>전단지 (Flyer Sheet)</td><td><code>flyer_sheet</code></td></tr>
          <tr><td>리플릿 (3-Fold Leaflet)</td><td><code>three_fold_leaflet</code></td></tr>
          <tr><td>팸플릿 (Long Pamphlet)</td><td><code>pamphlet_long</code></td></tr>
          <tr><td>상세페이지 (Detail Page)</td><td><code>detail_page</code></td></tr>
          <tr><td>보도자료 (Press Release)</td><td><code>press_release</code></td></tr>
          <tr><td>블로그 (Blog Article)</td><td><code>blog_article</code></td></tr>
          <tr><td>영상 (Video Storyboard)</td><td><code>video</code></td></tr>
        </tbody>
      </table>
    </section>

    <section>
      <h2>PDF 제안서 구조 (요약)</h2>
      <ul>
        <li>제품명 / 브리프</li>
        <li>제품 이미지</li>
        <li>확정된 콘텐츠 섹션(포스터, 전단지, 리플릿, 팸플릿, 상세페이지, 보도자료, 블로그, 영상)</li>
      </ul>
    </section>

    <section>
      <h2>기술 스택</h2>
      <ul>
        <li>UI: <strong>Next.js + TailwindCSS</strong></li>
        <li>AI: <strong>Google Gemini</strong> (또는 호환 가능한 LLM)</li>
        <li>DB: Firebase Firestore</li>
        <li>파일: Firebase Storage</li>
        <li>PDF: PDFKit</li>
        <li>권장 배포: Vercel</li>
      </ul>
    </section>

    <section>
      <h2>향후 확장 아이디어</h2>
      <ul>
        <li>블록체인 기반 레퍼럴 리워드 시스템 통합 (토큰 기반 보상 자동화)</li>
        <li>Web3 기반 크리에이터 수익 공유</li>
        <li>Admin 대시보드 및 분석 기능</li>
        <li>다국어 자동 번역 품질 개선 및 지역별 템플릿</li>
      </ul>
    </section>

    <section>
      <h2>기여 방법</h2>
      <p class="muted">Pull request 환영합니다. 개발 가이드는 프로젝트 루트의 CONTRIBUTING.md(예정)에 따릅니다.</p>
    </section>

    <section>
      <h2>문의</h2>
      <p class="muted">Adcontium 개발팀<br/>이메일: <a href="mailto:support@adcontium.ai">support@adcontium.ai</a> (예시)</p>
    </section>

    <footer>
      <div class="muted">README HTML — 자동 생성: Adcontium MVP</div>
    </footer>
  </div>
</body>
</html>
