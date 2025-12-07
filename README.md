<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Adcontium MVP - Web UI</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; max-width: 800px; margin: auto; }
    input, button, select, textarea { padding: 8px; margin: 6px 0; width: 100%; }
    .card { border: 1px solid #ccc; padding: 15px; margin-top: 15px; border-radius: 6px; }
    .row { display: flex; gap: 10px; }
    .row button { flex: 1; }
  </style>
</head>
<body>
  <h1>Adcontium MVP Web Interface</h1>
  <p>단일 파일 서버(server.js)와 연동되는 웹페이지입니다.</p>

  <!-- PROJECT CREATION -->
  <div class="card">
    <h2>1. 프로젝트 생성</h2>
    <label>Product Name</label>
    <input id="productName" placeholder="예: Collagen Ampoule" />

    <label>Product Images (max 3)</label>
    <input id="productImages" type="file" accept="image/*" multiple />

    <label>Target Markets (Multiple)</label>
    <select id="targetMarkets" multiple>
      <option value="KR" selected>Korea (KR)</option>
      <option value="EN">English (EN)</option>
      <option value="CN">Chinese (CN)</option>
      <option value="JP">Japanese (JP)</option>
      <option value="FR">French (FR)</option>
      <option value="HI">Hindi (HI)</option>
      <option value="ID">Indonesian (ID)</option>
      <option value="VI">Vietnamese (VI)</option>
      <option value="TH">Thai (TH)</option>
    </select>

    <button onclick="createProject()">프로젝트 생성</button>
  </div>

  <!-- PROJECT ID INPUT -->
  <div class="card">
    <h2>2. Project ID 입력</h2>
    <input id="projectId" placeholder="생성된 Project ID 입력" />
  </div>

  <!-- CONTENT GENERATION -->
  <div class="card">
    <h2>3. 콘텐츠 생성</h2>
    <div class="row">
      <button onclick="gen('poster')">포스터 생성</button>
      <button onclick="gen('leaflet')">전단지 생성</button>
    </div>
    <div class="row">
      <button onclick="gen('pamphlet')">팸플릿 생성</button>
      <button onclick="gen('detailPage')">상세페이지 생성</button>
    </div>
    <div class="row">
      <button onclick="gen('pressRelease')">보도자료 생성</button>
      <button onclick="gen('blog')">블로그 작성</button>
    </div>
    <div class="row">
      <button onclick="gen('video')">동영상 스토리보드 생성</button>
    </div>
  </div>

  <!-- RESULTS -->
  <div class="card">
    <h2>4. 생성 결과</h2>
    <div id="results"></div>
  </div>

  <!-- PROPOSAL PDF -->
  <div class="card">
    <h2>5. PDF 제안서 다운로드</h2>
    <button onclick="downloadPDF()">PDF 생성</button>
  </div>

<script>
// ------------------ CONFIG ------------------
const SERVER = "http://localhost:4000"; // server.js 실행 주소

// ------------------ 1. PROJECT CREATION ------------------
async function createProject() {
  const name = document.getElementById("productName").value;
  const files = document.getElementById("productImages").files;

  if (!name) return alert("Product Name을 입력하세요.");

  const urls = [];
  for (let i = 0; i < files.length && i < 3; i++) {
    const f = files[i];
    const fd = new FormData();
    fd.append("file", f);

    // Storage 직접 업로드가 없으므로 Base64로 서버 전달
    const base64 = await fileToBase64(f);
    urls.push(base64);
  }

  const res = await fetch(`${SERVER}/createProject`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ productName: name, images: urls })
  });

  const j = await res.json();
  alert("프로젝트 생성됨: " + j.projectId);
  document.getElementById("projectId").value = j.projectId;
}

function fileToBase64(file) {
  return new Promise((resolve) => {
    const reader = new FileReader();
    reader.onload = () => resolve(reader.result);
    reader.readAsDataURL(file);
  });
}

// ------------------ 2. CONTENT GENERATION ------------------
async function gen(type) {
  const pid = document.getElementById("projectId").value;
  if (!pid) return alert("프로젝트 ID를 입력하세요.");

  const res = await fetch(`${SERVER}/generate`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ projectId: pid, type })
  });
  const j = await res.json();

  const div = document.createElement("div");
  div.className = "card";
  div.innerHTML = `
    <h3>${type} 생성 결과</h3>
    <pre>${j.output?.text}</pre>
    <button onclick="finalize('${j.id}')">확정하기</button>
  `;

  document.getElementById("results").prepend(div);
}

// ------------------ 3. FINALIZE ------------------
async function finalize(id) {
  await fetch(`${SERVER}/finalize`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ contentId: id })
  });
  alert("콘텐츠 확정 완료!");
}

// ------------------ 4. PDF DOWNLOAD ------------------
async function downloadPDF() {
  const pid = document.getElementById("projectId").value;
  if (!pid) return alert("프로젝트 ID 입력 필요");

  const res = await fetch(`${SERVER}/proposal`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ projectId: pid })
  });

  const blob = await res.blob();
  const url = URL.createObjectURL(blob);
  const a = document.createElement("a");
  a.href = url;
  a.download = `proposal_${pid}.pdf`;
  a.click();
}
</script>
</body>
</html>

