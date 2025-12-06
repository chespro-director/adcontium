# adcontium
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

      <!-- Create Project -->
      <div class="card">
        <h2>2. 새 프로젝트 생성</h2>
        <label>제품명</label>
        <input id="productName" placeholder="제품명 입력" />
        <label>주요 성분/특징 (간단히)</label>
        <textarea id="productBrief" placeholder="예: 히알루론산, 콜라겐 등"></textarea>
        <label>이미지 업로드 (최대 3)</label>
        <input id="productImages" type="file" accept="image/*" multiple />
        <label>타겟 시장 선택</label>
        <select id="targetMarkets" multiple>
          <option value="US">US</option>
          <option value="CN">CN</option>
          <option value="JP">JP</option>
          <option value="KR" selected>KR</option>
        </select>
        <button onclick="createProject()">프로젝트 만들기</button>
        <div id="createResult" class="small"></div>
      </div>

      <!-- Projects list -->
      <div class="card">
        <h2>3. Projects</h2>
        <div id="projectsList"></div>
        <div class="small" style="margin-top:8px">로컬 브라우저 저장(localStorage)에 보관됩니다.</div>
      </div>
    </div>

    <div class="col">
      <!-- Project detail & content generation -->
      <div class="card">
        <h2>4. 선택된 프로젝트</h2>
        <label>Project ID</label>
        <input id="selectedProjectId" placeholder="프로젝트 ID 선택 또는 입력" />
        <button onclick="loadSelectedProject()">불러오기</button>

        <div id="projectDetailArea" style="margin-top:12px; display:none;">
          <h3 id="projTitle"></h3>
          <div id="projBrief" class="small"></div>
          <div id="projImages" style="margin-top:8px"></div>

          <h4 style="margin-top:12px">콘텐츠 생성 단계 (권장 순서대로 진행)</h4>
          <div class="btn-row">
            <button onclick="generateContent('poster')">포스터 생성</button>
            <button onclick="generateContent('leaflet')">전단지 생성</button>
            <button onclick="generateContent('pamphlet')">리플릿 생성</button>
            <button onclick="generateContent('pamphlet_long')">팸플릿(심층)</button>
            <button onclick="generateContent('detailPage')">상세페이지 생성</button>
            <button onclick="generateContent('video')">동영상 스토리보드 생성</button>
          </div>

          <div id="contentResults" style="margin-top:12px"></div>

          <div style="margin-top:12px">
            <button class="secondary" onclick="exportProject()">프로젝트 JSON 내보내기</button>
            <button onclick="generateProposal()">제품 제안서(PDF) 생성</button>
          </div>
        </div>
      </div>

      <!-- Tips & guide -->
      <div class="card">
        <h2>가이드</h2>
        <p class="small">
          • 권장 순서: <span class="chip">포스터 → 전단지 → 리플릿 → 팸플릿 → 상세페이지 → 동영상</span><br/>
          • 각 단계의 '확정(Finalize)'만 제안서에 포함됩니다. (기술백서 워크플로우 반영)<br/>
          • 실제 Gemini API가 준비되면 서버 endpoint를 연결해 실전 사용 가능합니다. (config: <code>SERVER</code>)
        </p>
        <div style="margin-top:8px">
          <label>서버 주소 (선택)</label>
          <input id="serverUrl" placeholder="예: http://localhost:4000" />
          <div class="small">빈칸이면 브라우저 내 모킹(logic)으로 동작합니다.</div>
        </div>
      </div>
    </div>
  </div>

<script>
/*
  Adcontium MVP HTML Demo
  - localStorage used for persistence (projects, company)
  - content generation will attempt server call if SERVER provided; otherwise generate locally using templates
  - PDF generation uses jsPDF
*/

const STORAGE_KEY = 'adcontium_mvp_v1';
const COMPANY_KEY = 'adcontium_company_v1';

// Helper - UUID
function uid() {
  return 'p_' + Math.random().toString(36).slice(2,10);
}

// Load saved projects
function loadState() {
  const raw = localStorage.getItem(STORAGE_KEY);
  return raw ? JSON.parse(raw) : { projects: [] };
}
function saveState(state) {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
}

// Company
function saveCompany() {
  const name = document.getElementById('companyName').value.trim();
  const addr = document.getElementById('companyAddr').value.trim();
  const email = document.getElementById('companyEmail').value.trim();
  const c = { name, addr, email };
  localStorage.setItem(COMPANY_KEY, JSON.stringify(c));
  renderCompany();
}
function renderCompany() {
  const raw = localStorage.getItem(COMPANY_KEY);
  const el = document.getElementById('companyInfo');
  if (!raw) { el.innerText = '회사 정보가 저장되어 있지 않습니다.'; return; }
  const c = JSON.parse(raw);
  el.innerText = `회사: ${c.name || '-'} / 이메일: ${c.email || '-'} / 주소: ${c.addr || '-'}`;
}
renderCompany();

// Projects list rendering
function renderProjects() {
  const state = loadState();
  const box = document.getElementById('projectsList');
  box.innerHTML = '';
  if (!state.projects.length) { box.innerHTML = '<div class="small">프로젝트가 없습니다.</div>'; return; }
  state.projects.forEach(p => {
    const div = document.createElement('div');
    div.className = 'project-item';
    const left = document.createElement('div');
    left.style.flex = '1';
    left.innerHTML = `<strong>${p.briefData.productName || '(무명)'}</strong><div class="small">${p.targetMarkets.join(', ')}</div><div class="small">ID: ${p.id}</div>`;
    const right = document.createElement('div');
    if (p.productImageUrls && p.productImageUrls.length) {
      const img = document.createElement('img');
      img.src = p.productImageUrls[0];
      img.className = 'thumb';
      right.appendChild(img);
    }
    const btn = document.createElement('button');
    btn.textContent = '선택';
    btn.onclick = () => { document.getElementById('selectedProjectId').value = p.id; loadSelectedProject(); };
    right.appendChild(btn);
    div.appendChild(left);
    div.appendChild(right);
    box.appendChild(div);
  });
}
renderProjects();

// Create project (images -> base64 store)
async function createProject() {
  const productName = document.getElementById('productName').value.trim();
  const productBrief = document.getElementById('productBrief').value.trim();
  const files = document.getElementById('productImages').files;
  const marketsSelect = document.getElementById('targetMarkets');
  const markets = Array.from(marketsSelect.selectedOptions).map(o => o.value) || ['KR'];

  if (!productName) return alert('제품명을 입력하세요.');

  // read up to 3 images as data URL
  const urls = [];
  if (files && files.length) {
    for (let i=0;i<Math.min(3,files.length);i++){
      urls.push(await fileToDataUrl(files[i]));
    }
  }

  const state = loadState();
  const project = {
    id: uid(),
    briefData: { productName, productBrief },
    productImageUrls: urls,
    targetMarkets: markets,
    createdAt: new Date().toISOString(),
    contents: []  // will hold generated content objects {id,type,status, output, createdAt, finalizedAt}
  };
  state.projects.push(project);
  saveState(state);
  renderProjects();
  document.getElementById('createResult').innerText = '프로젝트 생성 완료: ' + project.id;
  document.getElementById('selectedProjectId').value = project.id;
  loadSelectedProject();
}

// helper
function fileToDataUrl(file){ return new Promise(resolve=>{
  const r = new FileReader();
  r.onload = ()=> resolve(r.result);
  r.readAsDataURL(file);
}); }

// Load selected project detail
function loadSelectedProject(){
  const pid = document.getElementById('selectedProjectId').value.trim();
  if (!pid) return alert('프로젝트 ID를 입력하세요.');
  const state = loadState();
  const p = state.projects.find(x=>x.id === pid);
  if (!p) return alert('프로젝트를 찾을 수 없습니다.');
  document.getElementById('projectDetailArea').style.display = 'block';
  document.getElementById('projTitle').innerText = p.briefData.productName || '(무명 제품)';
  document.getElementById('projBrief').innerText = p.briefData.productBrief || '(브리프 없음)';
  const imgs = document.getElementById('projImages');
  imgs.innerHTML = '';
  (p.productImageUrls || []).forEach(src=>{
    const im = document.createElement('img');
    im.src = src; im.className='thumb'; im.style.marginRight='6px';
    imgs.appendChild(im);
  });
  renderContents(p);
}

// Render content cards
function renderContents(project){
  const area = document.getElementById('contentResults');
  area.innerHTML = '';
  project.contents = project.contents || [];
  if (!project.contents.length) { area.innerHTML = '<div class="small">생성된 콘텐츠가 없습니다.</div>'; return; }
  project.contents.slice().reverse().forEach(c=>{
    const d = document.createElement('div');
    d.className = 'result ' + (c.status === 'Finalized' ? 'finalized' : '');
    d.innerHTML = `<strong>${c.type}</strong> <span class="small">(${new Date(c.createdAt).toLocaleString()})</span>`;
    const pre = document.createElement('pre');
    pre.textContent = c.output?.text || JSON.stringify(c.output || {}, null, 2);
    d.appendChild(pre);
    const btnRow = document.createElement('div');
    btnRow.style.marginTop='6px';
    const finBtn = document.createElement('button');
    finBtn.textContent = c.status === 'Finalized' ? '확정 취소' : '최종 확정';
    finBtn.onclick = ()=> toggleFinalize(project.id, c.id);
    btnRow.appendChild(finBtn);
    const del = document.createElement('button');
    del.textContent = '삭제';
    del.style.marginLeft='6px';
    del.onclick = ()=> deleteContent(project.id, c.id);
    btnRow.appendChild(del);
    d.appendChild(btnRow);
    area.appendChild(d);
  });
}

// Toggle finalize
function toggleFinalize(pid, cid){
  const state = loadState();
  const p = state.projects.find(x=>x.id === pid);
  if (!p) return;
  const c = p.contents.find(x=>x.id === cid);
  if (!c) return;
  if (c.status === 'Finalized') {
    c.status = 'draft';
    delete c.finalizedAt;
  } else {
    c.status = 'Finalized';
    c.finalizedAt = new Date().toISOString();
  }
  saveState(state);
  loadSelectedProject();
}

// Delete content
function deleteContent(pid, cid){
  const state = loadState();
  const p = state.projects.find(x=>x.id === pid);
  if (!p) return;
  p.contents = p.contents.filter(x=>x.id !== cid);
  saveState(state);
  loadSelectedProject();
}

// Generate content: tries server, otherwise fallback to local generator
async function generateContent(type){
  const pid = document.getElementById('selectedProjectId').value.trim();
  if (!pid) return alert('Project ID 필요');
  const server = document.getElementById('serverUrl').value.trim();
  // If server provided, attempt server call
  if (server) {
    try {
      const r = await fetch(server.replace(/\/$/, '') + '/generate', {
        method: 'POST', headers:{ 'Content-Type':'application/json' },
        body: JSON.stringify({ projectId: pid, type, targetLanguage: (getPrimaryMarket(pid) || 'KR') })
      });
      if (!r.ok) { const text = await r.text(); throw new Error(text || r.status); }
      const j = await r.json();
      // If server returns id/output, store in project as content
      storeContentLocal(pid, { id: j.id || uid(), type, status:'draft', output: j.output || j });
      loadSelectedProject();
      return;
    } catch (e) {
      console.warn('Server generate failed, falling back to local mock:', e);
      // fallthrough to local
    }
  }
  // Local generation (mock) using templates that reflect the tech whitepaper + guide
  const project = loadState().projects.find(x=>x.id === pid);
  if (!project) return alert('프로젝트를 찾을 수 없습니다.');
  const generated = localGenerate(project, type);
  storeContentLocal(pid, generated);
  loadSelectedProject();
}

// store content in project localStorage
function storeContentLocal(pid, generated) {
  const state = loadState();
  const p = state.projects.find(x=>x.id === pid);
  if (!p) return;
  p.contents = p.contents || [];
  p.contents.push({
    id: generated.id || uid(),
    type: generated.type,
    status: generated.status || 'draft',
    output: generated.output,
    createdAt: new Date().toISOString()
  });
  saveState(state);
}

// pick primary market
function getPrimaryMarket(pid) {
  const s = loadState();
  const p = s.projects.find(x=>x.id===pid);
  return p?.targetMarkets?.[0] || 'KR';
}

// Local generator: produce text mimicking System Instructions from whitepaper
function localGenerate(project, type) {
  const brief = project.briefData || {};
  const name = brief.productName || '제품';
  const keyfeatures = brief.productBrief || '특징 없음';
  const firstImageAlt = project.productImageUrls && project.productImageUrls[0] ? '제품 이미지' : '';
  const language = getPrimaryMarket(project.id) === 'KR' ? '한국어' : '영어';

  if (type === 'poster') {
    const headlines = [
      `${name} — 한 번으로 느껴지는 생기`,
      `${name}로 빛나는 하루, 즉시 광채`,
      `피부 본연의 힘을 키우는 ${name}`
    ];
    const visual = `메인 이미지(${firstImageAlt})를 중심으로 클로즈업 샷 + 깨끗한 배경, 강조된 제품명`;
    return {
      id: uid(),
      type: 'poster',
      output: { text: `헤드라인(3):\n1) ${headlines[0]}\n2) ${headlines[1]}\n3) ${headlines[2]}\n\n비주얼 컨셉:\n${visual}` }
    };
  }

  if (type === 'leaflet') {
    // try to use last poster headline if exists (mimic sequential workflow)
    const posterHeadline = findLatestFinalizedText(project, 'poster')?.split('\n')[0] || `${name} — 특별 제안`;
    const cta = `지금 구매하고 20% 할인 적용 | 방문 또는 온라인 주문`;
    return {
      id: uid(),
      type: 'leaflet',
      output: { text: `포스터 핵심: ${posterHeadline}\n\n전단지 메시지:\n${name}의 특징: ${keyfeatures}\nCTA: ${cta}` }
    };
  }

  if (type === 'pamphlet') {
    return {
      id: uid(),
      type: 'pamphlet',
      output: { text:
`리플릿 (3단 구성)
1면: 표지 - ${name}
2면: 주요 기능 및 효과 - ${keyfeatures}
3면: 사용법 / 권장량 / FAQ` }
    };
  }

  if (type === 'pamphlet_long') {
    return {
      id: uid(),
      type: 'pamphlet_long',
      output: { text:
`팸플릿 (심층)
브랜드 스토리:
${name}은 연구진의 10년 연구로 탄생...
성분 심층 분석:
- 핵심 성분: ${keyfeatures}
R&D 및 신뢰 포인트: 임상 데이터/품질관리 등` }
    };
  }

  if (type === 'detailPage') {
    // collect prior finalized outputs
    const collected = gatherAllFinalizedTexts(project);
    return {
      id: uid(),
      type: 'detailPage',
      output: { text:
`상세페이지(스크롤형)
제품명: ${name}
요약(USP): ${collected.usp || '핵심 가치 요약'}
상세 설명: ${keyfeatures}
Q&A: 자주 묻는 질문(예)
후기: 고객 사용 후기 예시
정책: 환불/교환 안내` }
    };
  }

  if (type === 'video') {
    const usp = findMostSuccessfulUSP(project) || name;
    return {
      id: uid(),
      type: 'video',
      output: { text:
`30초 스토리보드 (5컷)
1. 인트로 - 2초: 제품 로고 + 1줄 헤드라인 (${usp})
2. 제품 클로즈업 - 6초: 핵심 성분 강조
3. 사용 장면 - 8초: 전후 효과 연출
4. 고객 추천 - 8초: 실제 사용후기 텍스트
5. CTA - 6초: 웹사이트/구매유도 + 프로모션` }
    };
  }

  // default fallback
  return { id: uid(), type, output: { text: `${type} 텍스트(모킹) - ${name}` } };
}

// Helper: find latest finalized text of a type
function findLatestFinalizedText(project, type) {
  const arr = (project.contents || []).filter(c=>c.type===type && c.status==='Finalized');
  if (!arr.length) return null;
  return arr[arr.length-1].output?.text || null;
}
function gatherAllFinalizedTexts(project) {
  const obj = {};
  (project.contents || []).filter(c=>c.status==='Finalized').forEach(c=>{
    obj[c.type] = c.output?.text || '';
  });
  // try set usp
  obj.usp = obj.poster?.split('\n')[0] || obj.detailPage?.split('\n')[0] || '';
  return obj;
}
function findMostSuccessfulUSP(project) {
  const detail = findLatestFinalizedText(project, 'detailPage');
  if (detail) {
    return detail.split('\n').slice(0,1).join('').slice(0,80);
  }
  return project.briefData?.productName;
}

// Export project as JSON
function exportProject() {
  const pid = document.getElementById('selectedProjectId').value.trim();
  if (!pid) return alert('프로젝트 ID 필요');
  const state = loadState();
  const p = state.projects.find(x=>x.id===pid);
  if (!p) return alert('프로젝트를 찾을 수 없습니다.');
  const blob = new Blob([JSON.stringify(p, null, 2)], {type:'application/json'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a'); a.href = url; a.download = `project_${pid}.json`; a.click();
}

// Generate PDF proposal: try server, else client-side via jsPDF
async function generateProposal() {
  const pid = document.getElementById('selectedProjectId').value.trim();
  if (!pid) return alert('프로젝트 ID 필요');
  const server = document.getElementById('serverUrl').value.trim();
  if (server) {
    try {
      const r = await fetch(server.replace(/\/$/, '') + '/proposal', {
        method:'POST', headers:{ 'Content-Type':'application/json' }, body: JSON.stringify({ projectId: pid })
      });
      if (!r.ok) { const t = await r.text(); throw new Error(t || r.status); }
      const blob = await r.blob();
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a'); a.href = url; a.download = `proposal_${pid}.pdf`; a.click();
      return;
    } catch (e) {
      console.warn('Server proposal failed, fallback to local PDF generation', e);
    }
  }

  // client-side PDF using jsPDF
  const state = loadState();
  const p = state.projects.find(x=>x.id===pid);
  if (!p) return alert('프로젝트가 없습니다.');

  const companyRaw = localStorage.getItem(COMPANY_KEY);
  const company = companyRaw ? JSON.parse(companyRaw) : {};

  // Use jsPDF (UMD loaded as window.jspdf)
  const { jsPDF } = window.jspdf;
  const doc = new jsPDF({ unit:'pt', format:'a4' });
  const margin = 40;
  let y = margin;
  doc.setFontSize(20); doc.text('Adcontium Product Proposal', margin, y); y += 28;
  doc.setFontSize(12);
  doc.text(`Company: ${company.name || '-'}`, margin, y); y += 16;
  doc.text(`Contact: ${company.email || '-'}`, margin, y); y += 16;
  doc.text(`Project: ${p.briefData.productName || '-'}`, margin, y); y += 20;

  doc.setLineWidth(0.5); doc.line(margin, y, 595-margin, y); y += 10;

  doc.setFontSize(14); doc.text('Project Brief', margin, y); y += 18;
  doc.setFontSize(11);
  const briefStr = p.briefData.productBrief || '-';
  y = addTextAuto(doc, briefStr, margin, y, 550, 12);

  // list finalized contents
  doc.setFontSize(14); doc.text('Finalized Contents', margin, y+12); y += 26;
  const finalized = (p.contents || []).filter(c=>c.status==='Finalized');
  if (!finalized.length) {
    doc.setFontSize(11); doc.text('No finalized contents yet.', margin, y); y += 18;
  } else {
    for (let c of finalized) {
      doc.setFontSize(12); doc.text(`- ${c.type}`, margin, y); y += 16;
      doc.setFontSize(10);
      y = addTextAuto(doc, (c.output?.text || JSON.stringify(c.output || {})), margin+8, y, 520, 10);
      y += 6;
      if (y > 740) { doc.addPage(); y = margin; }
    }
  }

  // finalize and save
  doc.save(`proposal_${p.id}.pdf`);
}

// helper to add text with wrapping (jsPDF)
function addTextAuto(doc, text, x, y, maxWidth, fontSize=11) {
  doc.setFontSize(fontSize);
  const lines = doc.splitTextToSize(text, maxWidth);
  for (let line of lines) {
    doc.text(line, x, y);
    y += fontSize*1.3;
  }
  return y;
}

// load initial data sample for demo convenience
(function seedDemo(){
  const state = loadState();
  if (!state.projects || !state.projects.length) {
    const demo = {
      id: 'demo1',
      briefData: { productName: 'Radiance Collagen Ampoule', productBrief: '히알루론산 + 콜라겐으로 즉시 수분과 탄력 부여' },
      productImageUrls: [],
      targetMarkets: ['KR'],
      createdAt: new Date().toISOString(),
      contents: []
    };
    state.projects = [demo];
    saveState(state);
  }
  renderProjects();
})();

</script>

</body>
</html>