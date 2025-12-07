<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Adcontium — 콘텐츠 여정 (Tailwind UI)</title>
  <!-- Tailwind CDN for quick styling -->
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-slate-50 text-slate-800">
  <div class="max-w-6xl mx-auto p-6">
    <header class="mb-6">
      <h1 class="text-3xl font-semibold">Adcontium — 콘텐츠 제작 여정</h1>
      <p class="text-sm text-slate-500">포스터 → 전단지(Flyer Sheet) → 리플릿(3-Fold Leaflet) → 팸플릿 → 상세페이지 → 보도자료 → 블로그 → 영상</p>
    </header>

    <div class="grid grid-cols-12 gap-6">
      <!-- Left column: Project creation -->
      <section class="col-span-12 lg:col-span-4 space-y-4">
        <div class="bg-white p-4 rounded-lg shadow-sm">
          <h2 class="text-lg font-medium">1. 프로젝트 생성</h2>
          <label class="block text-sm mt-3">Product Name</label>
          <input id="productName" class="mt-1 w-full rounded border px-3 py-2" placeholder="예: Radiance Ampoule" />

          <label class="block text-sm mt-3">Brief (ingredients / highlight)</label>
          <textarea id="productBrief" class="mt-1 w-full rounded border px-3 py-2 h-20"></textarea>

          <label class="block text-sm mt-3">Images (max 3)</label>
          <input id="productImages" type="file" accept="image/*" multiple class="mt-1" />

          <label class="block text-sm mt-3">Target Languages (select one or more)</label>
          <select id="targetLanguages" multiple class="mt-1 w-full rounded border px-3 py-2">
            <option value="KR" selected>Korean (KR)</option>
            <option value="EN">English (EN)</option>
            <option value="CN">Chinese (CN)</option>
            <option value="JP">Japanese (JP)</option>
            <option value="FR">French (FR)</option>
            <option value="HI">Hindi (HI)</option>
            <option value="ID">Indonesian (ID)</option>
            <option value="VI">Vietnamese (VI)</option>
            <option value="TH">Thai (TH)</option>
          </select>

          <div class="flex gap-2 mt-4">
            <button id="btnCreateProject" class="bg-blue-600 text-white px-4 py-2 rounded" onclick="createProject()">Create Project</button>
            <button class="bg-gray-100 px-4 py-2 rounded" onclick="loadProjects()">Refresh</button>
          </div>
          <p id="createResult" class="text-sm text-slate-500 mt-3"></p>
        </div>

        <div class="bg-white p-4 rounded-lg shadow-sm">
          <h2 class="text-lg font-medium">2. Projects</h2>
          <div id="projectsList" class="mt-3 space-y-2 text-sm"></div>
        </div>

        <div class="bg-white p-4 rounded-lg shadow-sm">
          <h2 class="text-lg font-medium">3. Selected Project</h2>
          <label class="block text-sm">Project ID</label>
          <input id="selectedProjectId" class="mt-1 w-full rounded border px-3 py-2" placeholder="Enter or select project ID" />
          <div class="flex gap-2 mt-3">
            <button class="bg-green-600 text-white px-3 py-2 rounded" onclick="loadSelectedProject()">Load</button>
            <button class="bg-gray-100 px-3 py-2 rounded" onclick="clearSelection()">Clear</button>
          </div>

          <label class="block text-sm mt-3">Server URL (optional)</label>
          <input id="serverUrl" class="mt-1 w-full rounded border px-3 py-2" placeholder="http://localhost:4000 (leave empty for local mock)" />
        </div>
      </section>

      <!-- Right column: Workflow & Results -->
      <section class="col-span-12 lg:col-span-8 space-y-4">
        <div class="bg-white p-4 rounded-lg shadow-sm">
          <h2 class="text-lg font-medium">4. Workflow — Generate Content</h2>
          <p class="text-sm text-slate-500">Buttons call server API type keys automatically and pass selected languages. Sequential logic: previous finalized results are passed to the next step.</p>

          <div class="grid grid-cols-2 gap-3 mt-4">
            <button class="gen-btn bg-indigo-600 text-white px-3 py-2 rounded" data-type="poster">Poster</button>
            <button class="gen-btn bg-indigo-600 text-white px-3 py-2 rounded" data-type="flyer_sheet">Flyer Sheet</button>
            <button class="gen-btn bg-indigo-600 text-white px-3 py-2 rounded" data-type="three_fold_leaflet">3-Fold Leaflet</button>
            <button class="gen-btn bg-indigo-600 text-white px-3 py-2 rounded" data-type="pamphlet_long">Pamphlet (Long)</button>
            <button class="gen-btn bg-indigo-600 text-white px-3 py-2 rounded" data-type="detail_page">Detail Page</button>
            <button class="gen-btn bg-indigo-600 text-white px-3 py-2 rounded" data-type="press_release">Press Release</button>
            <button class="gen-btn bg-indigo-600 text-white px-3 py-2 rounded" data-type="blog_article">Blog Article</button>
            <button class="gen-btn bg-indigo-600 text-white px-3 py-2 rounded" data-type="video">Video Storyboard</button>
          </div>

          <div id="workflowNotice" class="text-sm text-slate-500 mt-3">Select languages and click the step button. If server is configured, the app calls <code>/api/generate</code> with type and languages. Otherwise local mock is used.</div>

          <!-- Spinner overlay -->
          <div id="spinnerOverlay" class="hidden fixed inset-0 bg-black/30 flex items-center justify-center z-50">
            <div class="bg-white p-6 rounded shadow flex items-center gap-3">
              <svg class="animate-spin h-6 w-6 text-indigo-600" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8v8H4z"></path></svg>
              <div class="text-sm">AI generating content... please wait</div>
            </div>
          </div>
        </div>

        <div class="bg-white p-4 rounded-lg shadow-sm">
          <h2 class="text-lg font-medium">5. Results</h2>
          <div id="resultsArea" class="mt-3 space-y-3"></div>
        </div>

        <div class="bg-white p-4 rounded-lg shadow-sm flex items-center justify-between">
          <div>
            <h3 class="text-md font-medium">6. Proposal</h3>
            <p class="text-sm text-slate-500">Generate final PDF proposal from finalized contents (server preferred).</p>
          </div>
          <div class="flex gap-2">
            <button class="bg-emerald-600 text-white px-4 py-2 rounded" onclick="generateProposal()">Generate Proposal (PDF)</button>
            <button class="bg-gray-100 px-4 py-2 rounded" onclick="downloadAllJson()">Export Project JSON</button>
          </div>
        </div>
      </section>
    </div>
  </div>

<script>
/*
  Adcontium Tailwind UI (client-only demo + server integration points)
  Features added per user request:
   - API standard type keys mapped on buttons
   - Selected languages auto-included in generate payload
   - Loading spinner overlay while AI generation in progress
   - Sequential: previous finalized content texts are passed to next step
   - Tailwind UI styling
*/

// Local state in localStorage (fallback when server is not provided)
const STORAGE_KEY = 'adcontium_mvp_v2';
function uid(){return 'id_' + Math.random().toString(36).slice(2,9);} 
function loadState(){const r=localStorage.getItem(STORAGE_KEY); return r?JSON.parse(r):{projects:[]};}
function saveState(s){localStorage.setItem(STORAGE_KEY, JSON.stringify(s));}

// DOM refs
const resultsArea = document.getElementById('resultsArea');
const projectsList = document.getElementById('projectsList');
const spinner = document.getElementById('spinnerOverlay');

// Wire up generate buttons
document.querySelectorAll('.gen-btn').forEach(btn=>{
  btn.addEventListener('click', async ()=>{
    const type = btn.getAttribute('data-type');
    await generateHandler(type);
  });
});

// Create project
async function createProject(){
  const name = document.getElementById('productName').value.trim();
  const brief = document.getElementById('productBrief').value.trim();
  const files = document.getElementById('productImages').files;
  const langs = Array.from(document.getElementById('targetLanguages').selectedOptions).map(o=>o.value);
  if(!name) return alert('Enter product name');
  const imgs = [];
  if(files && files.length){
    for(let i=0;i<Math.min(3,files.length);i++) imgs.push(await fileToDataUrl(files[i]));
  }
  const state = loadState();
  const project = { id: uid(), briefData:{productName:name, productBrief:brief}, productImageUrls: imgs, targetLanguages: langs, contents:[], createdAt: new Date().toISOString() };
  state.projects.push(project); saveState(state); renderProjects();
  document.getElementById('createResult').innerText = 'Created project: '+project.id;
  document.getElementById('selectedProjectId').value = project.id; loadSelectedProject();
}

function fileToDataUrl(file){return new Promise(r=>{const fr=new FileReader(); fr.onload=()=>r(fr.result); fr.readAsDataURL(file);});}

// Load projects list
function renderProjects(){ const s=loadState(); projectsList.innerHTML=''; if(!s.projects.length) projectsList.innerHTML='<div class="text-sm text-slate-500">No projects</div>';
  s.projects.forEach(p=>{
    const el = document.createElement('div'); el.className='p-2 border rounded flex items-center justify-between';
    el.innerHTML = `<div><div class="font-medium">${p.briefData.productName}</div><div class="text-xs text-slate-500">ID: ${p.id}</div></div>`;
    const btn = document.createElement('button'); btn.className='px-2 py-1 bg-slate-100 rounded text-sm'; btn.innerText='Select'; btn.onclick=()=>{ document.getElementById('selectedProjectId').value=p.id; loadSelectedProject(); };
    el.appendChild(btn); projectsList.appendChild(el);
  }); }
renderProjects();

function loadSelectedProject(){ const pid = document.getElementById('selectedProjectId').value.trim(); if(!pid) return alert('Project ID required'); const state = loadState(); const p = state.projects.find(x=>x.id===pid); if(!p) return alert('Project not found');
  // render header
  document.getElementById('projTitle')?.remove?.();
  // show details in results area header
  resultsArea.innerHTML = '';
  const head = document.createElement('div'); head.className='mb-2'; head.innerHTML = `<div class="text-lg font-semibold">${p.briefData.productName}</div><div class="text-sm text-slate-500">${p.briefData.productBrief||''}</div>`;
  resultsArea.appendChild(head);
  // render existing contents
  (p.contents||[]).slice().reverse().forEach(c=> renderContentCard(p.id,c));
}

function renderContentCard(projectId, content){
  const card = document.createElement('div'); card.className='p-3 border rounded bg-white shadow-sm';
  const title = document.createElement('div'); title.className='flex justify-between items-start'; title.innerHTML = `<div class="font-medium">${content.type}</div><div class="text-xs text-slate-500">${new Date(content.createdAt).toLocaleString()}</div>`;
  card.appendChild(title);
  const pre = document.createElement('pre'); pre.className='mt-2 p-2 bg-slate-50 rounded text-sm'; pre.textContent = content.output?.text || JSON.stringify(content.output || {}, null, 2);
  card.appendChild(pre);
  const btnRow = document.createElement('div'); btnRow.className='flex gap-2 mt-3';
  const fin = document.createElement('button'); fin.className='px-3 py-1 bg-emerald-600 text-white rounded text-sm'; fin.innerText = content.status==='Finalized'?'Unfinalize':'Finalize'; fin.onclick = ()=> toggleFinalize(projectId, content.id); btnRow.appendChild(fin);
  const del = document.createElement('button'); del.className='px-3 py-1 bg-red-100 rounded text-sm'; del.innerText='Delete'; del.onclick=()=> deleteContent(projectId, content.id); btnRow.appendChild(del);
  card.appendChild(btnRow);
  resultsArea.appendChild(card);
}

function toggleFinalize(pid,cid){ const s = loadState(); const p = s.projects.find(x=>x.id===pid); const c = p.contents.find(x=>x.id===cid); if(!c) return; if(c.status==='Finalized'){ c.status='draft'; delete c.finalizedAt; } else { c.status='Finalized'; c.finalizedAt = new Date().toISOString(); } saveState(s); loadSelectedProject(); }
function deleteContent(pid,cid){ const s=loadState(); const p=s.projects.find(x=>x.id===pid); p.contents = p.contents.filter(x=>x.id!==cid); saveState(s); loadSelectedProject(); }

// Generate handler: builds payload, calls server or falls back to mock
async function generateHandler(typeKey){
  const pid = document.getElementById('selectedProjectId').value.trim(); if(!pid) return alert('Select project first');
  const server = document.getElementById('serverUrl').value.trim();
  const langs = Array.from(document.getElementById('targetLanguages').selectedOptions).map(o=>o.value);
  // gather previous finalized content ids/texts to pass
  const s = loadState(); const proj = s.projects.find(x=>x.id===pid);
  const finalized = (proj.contents||[]).filter(c=>c.status==='Finalized');
  const payload = { projectId: pid, type: typeKey, targetLanguages: langs, previousContents: finalized.map(c=>({ type:c.type, id:c.id, text:c.output?.text })) };

  // show spinner
  spinner.classList.remove('hidden');
  try{
    if(server){
      // call server API /api/generate (expects type key)
      const resp = await fetch(server.replace(/\/$/,'') + '/generate', { method:'POST', headers:{ 'Content-Type':'application/json' }, body: JSON.stringify(payload) });
      if(!resp.ok) throw new Error('Server generate failed: ' + resp.status);
      const j = await resp.json();
      // store result into project
      const content = { id: j.id || uid(), type: typeKey, status:'draft', output: j.output || j, createdAt: new Date().toISOString() };
      proj.contents.push(content); saveState(s);
      renderContentCard(pid, content);
    } else {
      // local mock generation that uses previous finalized texts
      const generated = localGenerateMock(proj, typeKey);
      proj.contents.push(generated); saveState(s); renderContentCard(pid, generated);
    }
  }catch(err){
    console.error(err); alert('Generation error: ' + (err.message||err));
  }finally{
    spinner.classList.add('hidden');
  }
}

// Local mock generator uses previous finalized texts to create coherent output
function localGenerateMock(project, type){
  const name = project.briefData.productName || 'Product';
  const brief = project.briefData.productBrief || '';
  const prevTexts = (project.contents||[]).filter(c=>c.status==='Finalized').map(c=>c.output?.text).join('\n');
  const now = new Date().toISOString();
  const base = (t)=> ({ id: uid(), type: t, status:'draft', createdAt: now, output:{ text: `=== Mocked ${t} for ${name} ===\nBased on: ${prevTexts ? prevTexts.substring(0,200) + '...' : 'no previous finalized content'}\n\nContent:\n` } });
  if(type==='poster'){
    const c = base('poster'); c.output.text += `Headlines:\n- ${name} — Instant Glow\n- ${name} for Radiant Skin\n- Try ${name} Today\n\nVisual: close-up product shot.`; return c; }
  if(type==='flyer_sheet'){
    const c = base('flyer_sheet'); c.output.text += `Tagline: ${name} - Feel the difference\nPromo: Launch 20% off\nCTA: Buy Now`; return c; }
  if(type==='three_fold_leaflet'){
    const c = base('three_fold_leaflet'); c.output.text += `Fold1: Cover\nFold2: Features\nFold3: Usage & FAQ`; return c; }
  if(type==='pamphlet_long'){
    const c = base('pamphlet_long'); c.output.text += `Brand story, R&D, ingredients deep-dive.`; return c; }
  if(type==='detail_page'){
    const c = base('detail_page'); c.output.text += `Hero: ${name}\nUSP: ${brief}\nFAQ: ...`; return c; }
  if(type==='press_release'){
    const c = base('press_release'); c.output.text += `Headline: ${name} Launch Announcement\nLead: ${brief}\nQuotes: CEO says...`; return c; }
  if(type==='blog_article'){
    const c = base('blog_article'); c.output.text += `Title: Why ${name} is the next must-have\nBody: Story-based content...`; return c; }
  if(type==='video'){
    const c = base('video'); c.output.text += `30s storyboard: Shot1 Hook, Shot2 Product, Shot3 CTA`; return c; }
  return base(type);
}

// Utility: download project JSON
function downloadAllJson(){ const pid = document.getElementById('selectedProjectId').value.trim(); if(!pid) return alert('Select project'); const s = loadState(); const p = s.projects.find(x=>x.id===pid); const blob = new Blob([JSON.stringify(p,null,2)],{type:'application/json'}); const url = URL.createObjectURL(blob); const a = document.createElement('a'); a.href = url; a.download = `project_${pid}.json`; a.click(); }

function generateProposal(){ const server = document.getElementById('serverUrl').value.trim(); const pid = document.getElementById('selectedProjectId').value.trim(); if(!pid) return alert('Select project'); if(server){ window.open(server.replace(/\/$/,'') + '/proposal?projectId=' + encodeURIComponent(pid), '_blank'); } else { alert('No server configured — use server to generate PDF proposal or export JSON.'); } }

function clearSelection(){ document.getElementById('selectedProjectId').value=''; resultsArea.innerHTML=''; }

// initial seed demo
(function seed(){ const s=loadState(); if(!s.projects.length){ const demo = { id:'demo1', briefData:{productName:'Radiance Collagen Ampoule', productBrief:'Hyaluronic + Collagen for instant hydration'}, productImageUrls:[], targetLanguages:['KR','EN'], contents:[], createdAt:new Date().toISOString() }; s.projects.push(demo); saveState(s); } renderProjects(); })();

</script>
</body>
</html>
