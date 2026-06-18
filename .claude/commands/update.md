주어진 티커의 기업 분석 리포트에 최신 분기 데이터를 추가하고 GitHub에 푸시한다.
사용법: /update {티커} (예: /update BE)

[파일명 규칙]
  ORCL  → oracle.html / transcripts/orcl-*.html
  BE    → bloom-energy.html / transcripts/be-*.html
  GEV   → ge-vernova.html / transcripts/gev-*.html
  CEG   → constellation.html / transcripts/ceg-*.html
  VST   → vistra.html
  WRT1V → wartsila.html

[원칙]
- 기존 HTML 파일의 구조·내용·메모·하이라이트를 절대 건드리지 않는다
- 새 데이터만 추가한다 (기존 데이터 수정 금지)
- 모든 수치는 공식 공시(8-K, IR Press Release) 기반. 못 찾으면 "데이터 없음"
- LOI/MOU와 구속력 있는 계약(binding) 반드시 구분
- 추측·날조 절대 금지

[작업 순서]

1. transcript-btn CSS 확인 및 추가
   company/{회사명}.html head에 아래 CSS가 없으면 추가:
   .transcript-btn{display:inline-block;margin-top:10px;font-size:12px;font-weight:700;color:#2563a8;background:#f0f5ff;border:1px solid #c7d9f8;border-radius:6px;padding:4px 12px;text-decoration:none;}
   .transcript-btn:hover{background:#e0ecff;}

2. 기존 transcript div → 전문 보기 버튼으로 교체
   기존 <div class="transcript">...</div> 를 찾아서:
   a. 전문 내용을 컨퍼런스 콜 톤 한국어로 번역
   b. company/transcripts/{티커소문자}-{분기}.html 로 저장 (아래 전문 페이지 형식 사용)
   c. HTML에서 transcript div를 버튼으로 교체:
      <a class="transcript-btn" href="transcripts/{티커소문자}-{분기}.html" target="_blank">📄 전문 보기</a>

3. 전문 페이지 형식 (transcripts/*.html):
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{회사명} {분기} 어닝콜 전문</title>
<style>
  :root{--ink:#13171e;--paper:#f9f9f7;--text:#1a1f29;--muted:#6b7280;--line:#e6e9ee;--accent:#2563a8;
    --hl-yellow:rgba(255,220,0,.45);--hl-green:rgba(72,199,116,.38);--hl-red:rgba(255,100,100,.35);
    --sans:-apple-system,BlinkMacSystemFont,"Apple SD Gothic Neo","Malgun Gothic","Noto Sans KR",sans-serif;}
  *{box-sizing:border-box}html,body{margin:0;padding:0}
  body{background:var(--paper);color:var(--text);font-family:var(--sans);line-height:1.8;}
  header{background:var(--ink);color:#fff;padding:16px 24px;display:flex;align-items:center;gap:16px;flex-wrap:wrap;position:sticky;top:0;z-index:20;}
  .back{font-size:12px;color:#9aa6b7;text-decoration:none;}
  .back:hover{color:#fff;}
  .hdr-info{flex:1;}
  .hdr-info h1{font-size:16px;font-weight:800;margin:0;}
  .hdr-info p{font-size:12px;color:#9aa6b7;margin:2px 0 0;}
  .wrap{max-width:820px;margin:0 auto;padding:32px 24px 120px;}
  .transcript{font-size:14px;line-height:1.9;color:#222;white-space:pre-wrap;word-break:break-word;}
  .memo-section{position:fixed;bottom:70px;right:24px;width:280px;background:#fff;border:1px solid var(--line);border-radius:10px;box-shadow:0 4px 20px rgba(0,0,0,.12);padding:12px;z-index:50;}
  .memo-section h4{font-size:12px;font-weight:700;margin:0 0 6px;}
  .memo-section textarea{width:100%;height:100px;border:1px solid var(--line);border-radius:6px;padding:8px;font-size:12px;font-family:var(--sans);resize:vertical;}
  .memo-status{font-size:11px;color:var(--muted);margin-top:4px;}
  .memo-status.saved{color:#1f7a4d;}
  .hl-toolbar{position:fixed;bottom:16px;left:50%;transform:translateX(-50%);z-index:100;background:var(--ink);border-radius:40px;padding:8px 14px;display:flex;align-items:center;gap:8px;box-shadow:0 4px 20px rgba(0,0,0,.35);}
  .hl-btn{width:24px;height:24px;border-radius:50%;border:2.5px solid transparent;cursor:pointer;}
  .hl-btn.active{border-color:#fff;}
  .hl-btn.yellow{background:#ffd600;} .hl-btn.green{background:#3ecf72;} .hl-btn.red{background:#ff6464;}
  .hl-erase{background:none;border:1px solid #555;color:#aaa;font-size:11px;padding:3px 9px;border-radius:20px;cursor:pointer;}
  .hl-divider{width:1px;height:18px;background:#333;}
  .hl-label{font-size:11px;color:#888;}
  .hl-yellow{background:var(--hl-yellow);border-radius:2px;}
  .hl-green{background:var(--hl-green);border-radius:2px;}
  .hl-red{background:var(--hl-red);border-radius:2px;}
</style>
</head>
<body>
<header>
  <a class="back" href="../{회사명}.html">← {회사명} 리포트</a>
  <div class="hdr-info">
    <h1>{회사명} {분기} {이벤트명}</h1>
    <p>{날짜} · {출처}</p>
  </div>
</header>
<div class="wrap">
  <div class="transcript">{전문 전체 — 절대 생략 없이}</div>
</div>
<div class="memo-section">
  <h4>📝 메모</h4>
  <textarea id="memoTA" placeholder="중요한 내용 메모..."></textarea>
  <div class="memo-status" id="memoStatus"></div>
</div>
<div class="hl-toolbar">
  <span class="hl-label">마커</span>
  <div class="hl-divider"></div>
  <button class="hl-btn yellow active" id="hlY"></button>
  <button class="hl-btn green" id="hlG"></button>
  <button class="hl-btn red" id="hlR"></button>
  <div class="hl-divider"></div>
  <button class="hl-erase" id="hlE">지우기</button>
</div>
<script>
const KEY='memo-'+location.pathname;
const ta=document.getElementById('memoTA');
const st=document.getElementById('memoStatus');
const saved=localStorage.getItem(KEY);
if(saved)ta.value=saved;
let timer;
ta.addEventListener('input',()=>{
  st.textContent='저장 중...';st.className='memo-status';
  clearTimeout(timer);
  timer=setTimeout(()=>{localStorage.setItem(KEY,ta.value);st.textContent='✓ 저장됨';st.className='memo-status saved';},600);
});
let cc='yellow',em=false;
const hb={yellow:document.getElementById('hlY'),green:document.getElementById('hlG'),red:document.getElementById('hlR')};
const he=document.getElementById('hlE');
function sc(c){em=false;cc=c;Object.keys(hb).forEach(k=>hb[k].classList.toggle('active',k===c));he.style.background='none';he.style.color='#aaa';}
hb.yellow.onclick=()=>sc('yellow');hb.green.onclick=()=>sc('green');hb.red.onclick=()=>sc('red');
he.onclick=()=>{em=!em;if(em){Object.values(hb).forEach(b=>b.classList.remove('active'));he.style.background='#333';he.style.color='#fff';}else sc(cc);};
const HLKEY='hl-'+location.pathname;
function saveHL(){const d=[];document.querySelectorAll('mark[data-hlid]').forEach(m=>d.push({id:m.dataset.hlid,color:m.dataset.color}));localStorage.setItem(HLKEY,JSON.stringify(d));}
document.body.addEventListener('mousedown',e=>{if(em)e.preventDefault();});
document.body.addEventListener('mouseup',e=>{
  if(em)return;
  const s=window.getSelection();if(!s||s.isCollapsed)return;
  const r=s.getRangeAt(0);if(!r||r.toString().trim()==='')return;
  try{const m=document.createElement('mark');m.className='hl-'+cc;m.dataset.hlid=Date.now();m.dataset.color=cc;r.surroundContents(m);s.removeAllRanges();saveHL();}catch(e){s.removeAllRanges();}
});
document.body.addEventListener('click',e=>{
  if(!em)return;
  let el=e.target;
  while(el&&el.tagName!=='BODY'){if(el.tagName==='MARK'){const p=el.parentNode;while(el.firstChild)p.insertBefore(el.firstChild,el);p.removeChild(el);p.normalize();saveHL();return;}el=el.parentElement;}
});
</script>
</body>
</html>

4. 전문 수집 규칙
   transcripts/{티커소문자}-{분기}.html 파일이 없거나 전문이 비어있으면:
   - 웹에서 해당 분기 어닝콜 전문을 찾는다 (Motley Fool, Seeking Alpha, 공식 IR 순)
   - 컨퍼런스 콜 톤에 맞게 한국어로 번역
   - 절대 중간에 끊지 않고 전체를 저장
   - 저장 후 파일 크기 확인

5. 최신 분기 재무 수치 업데이트
   - 현재 가장 최신 분기 확인 → 새 분기 실적 발표 여부 웹 검색
   - 미발표면 "아직 발표되지 않은 분기"라고 알리고 종료
   - 발표됐으면 차트 데이터 배열에 수치 추가, 재무 테이블 행 추가

6. 백로그 업데이트 (공시 있을 때만)

7. 주요 계약·딜 추가 (새로 발표된 것만)

8. 새 IR 이벤트 추가
   <!-- NEW_CALL_INSERT --> 주석 바로 아래에 삽입
   전문은 transcripts/*.html 로 저장, 버튼으로 연결
   기존 블록 건드리지 않음

9. 알파 시그널 업데이트 (새 시그널만, 기존 수정 금지)

[GitHub 푸시]
git add company/{회사명}.html company/transcripts/
git commit -m "분기 업데이트 {티커} {분기}"
git push origin main
