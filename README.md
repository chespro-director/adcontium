<html lang="ko">
<head>
  <meta charset="utf-8" />
  <title>Adcontium MVP - HTML Demo</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <style>
    body { font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Noto Sans KR", Arial; max-width: 1100px; margin: 20px auto; padding: 12px; color:#111; }
    h1,h2,h3 { margin: 6px 0 12px 0; }
    .grid { display:flex; gap:12px; align-items:flex-start; }
    .col { flex:1; min-width: 280px; }
    .card { border:1px solid #e3e3e3; border-radius:8px; padding:12px; margin-bottom:12px; background:#fff; box-shadow: 0 1px 3px rgba(0,0,0,0.03); }
    label { display:block; font-size:13px; margin-top:8px; color:#444; }
    input[type=text], textarea, select { width:100%; padding:8px; margin-top:6px; border:1px solid #ddd; border-radius:6px; font-size:14px; box-sizing:border-box; }
    textarea { min-height:80px; }
    button { padding:10px 14px; margin-top:8px; border-radius:6px; border:0; background:#0066ff; color:white; cursor:pointer; }
    button.secondary { background:#666; }
    .small { font-size:13px; color:#666; }
    .project-item { display:flex; justify-content:space-between; gap:8px; padding:8px 6px; border-bottom:1px solid #f0f0f0; }
    .thumb { width:88px; height:88px; object-fit:cover; border-radius:6px; border:1px solid #eee; }
    .btn-row { display:flex; gap:8px; flex-wrap:wrap; margin-top:8px; }
    pre { background:#f7f7f9; padding:8px; border-radius:6px; max-height:220px; overflow:auto; }
    .chip { display:inline-block; padding:6px 8px; border-radius:999px; background:#f0f8ff; color:#036; margin-right:6px; font-size:13px; }
    .result { border:1px dashed #ccc; padding:8px; border-radius:6px; margin-top:6px; background:#fff; }
    .finalized { border-left:4px solid #2e7d32; padding-left:8px; background:#f7fff7; }
  </style>
  <!-- jsPDF for client-side PDF generation -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
</head>
<body>
  <h1>Adcontium — MVP (HTML Demo)</h1>
  <p class="small">첨부된 기술백서·콘텐츠 제작 여정 가이드를 반영한 브라우저 기반 MVP 데모입니다. (로컬서버 연동 가능)</p>

  <div class="grid">
    <div class="col">
      <!-- Company / Onboarding -->
      <div class="card">
        <h2>1. 회사 정보 & 온보딩</h2>
        <label>회사명</label>
        <input id="companyName" placeholder="예: ACME Inc." />
        <label>주소</label>
        <input id="companyAddr" placeholder="회사 주소" />
        <label>담당자 이메일</label>
        <input id="companyEmail" placeholder="name@company.com" />
        <button onclick="saveCompany()">정보 저장</button>
        <div id="companyInfo" class="small" style="margin-top:8px"></div>
      </div>

