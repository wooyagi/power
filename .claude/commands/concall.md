주어진 티커의 IR 이벤트 내용을 수집하고 전문 페이지를 만들어 GitHub에 푸시한다.
사용법:
  /concall BE           ← 자동 수집
  /concall BE Q2 2026   ← 특정 분기만
  (파일 첨부 시)        ← 첨부된 파일을 전문으로 사용

[파일명 규칙]
  ORCL  → oracle.html / transcripts/orcl-*.html
  BE    → bloom-energy.html / transcripts/be-*.html
  GEV   → ge-vernova.html / transcripts/gev-*.html
  CEG   → constellation.html / transcripts/ceg-*.html

[수집 대상 — 최근 4개 분기 기간 내]
- Earnings Call (어닝콜)
- Investor Day / Capital Markets Day
- Financial Analyst Meeting / Analyst Day
- Industry Conference (JPMorgan, Goldman 등 주요 IB 주최)
- NDR 중 공개된 발언
- 기타 공개 IR 이벤트

[수집 원칙]
- 공식 자료(웹캐스트, 슬라이드, 트랜스크립트) 최우선
- 공식 전문 없어도 언론 보도·애널리스트 리포트에서 발언 사실 확인되면 수집
- 비공개 자료는 건너뜀
- 각 이벤트별 출처·날짜 표기
- 못 구한 이벤트는 "자료 미확보" 명시

[전문 처리 규칙 — 핵심]
- 전문은 절대 생략하거나 중간에 끊지 않는다
- 길이에 관계없이 전체를 저장한다
- 컨퍼런스 콜 톤에 맞게 한국어로 번역
- company/transcripts/{티커소문자}-{분기}.html 로 저장 (txt 아님)
- 전문 페이지는 하이라이트 + 메모 기능 포함 (아래 형식 참조)

[전문 페이지 형식 — transcripts/*.html]
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{회사명} {분기} 어닝콜 전문</title>
<style>
  :root{--ink:#13171e;--paper:#f9f9f7;--text:#1a1f29;--muted:#6b7280;--line:#e6e9ee;--accent:#2563a8;
    --hl-yellow:rgba(255,220,0,.45);--hl-green:rgba(72,199,116,.38);--hl-red:rgba(255,100,100,.35);
    --sans:-apple-system,BlinkMacSystemFont,"Apple SD Gothic Neo","Malgun Gothic","Noto Sans KR",sans-serif;
    --mono:ui-monospace,"SF Mono",Menlo,monospace;}
  *{box-sizing:border-box}html,body{margin:0;padding:0}
  body{background:var(--paper);color:var(--text);font-family:var(--sans);line-height:1.8;-webkit-font-smoothing:antialiased;}
  header{background:var(--ink);color:#fff;padding:16px 24px;display:flex;align-items:center;gap:16px;flex-wrap:wrap;position:sticky;top:0;z-index:20;}
  .back{font-size:12px;color:#9aa6b7;text-decoration:none;font-family:var(--mono);}
  .back:hover{color:#fff;}
  .hdr-info{flex:1;}
  .hdr-info h1{font-size:16px;font-weight:800;margin:0;}
  .hdr-info p{font-size:12px;color:#9aa6b7;margin:2px 0 0;font-family:var(--mono);}
  .wrap{max-width:820px;margin:0 auto;padding:32px 24px 120px;}
  .transcript{font-size:14px;line-height:1.9;color:#222;white-space:pre-wrap;word-break:break-word;}
  .memo-section{position:fixed;bottom:70px;right:24px;width:280px;background:#fff;border:1px solid var(--line);border-radius:10px;box-shadow:0 4px 20px rgba(0,0,0,.12);padding:12px;z-index:50;}
  .memo-section h4{font-size:12px;font-weight:700;margin:0 0 6px;color:var(--ink);}
  .memo-section textarea{width:100%;height:100px;border:1px solid var(--line);border-radius:6px;padding:8px;font-size:12px;font-family:var(--sans);resize:vertical;line-height:1.5;}
  .memo-section textarea:focus{outline:2px solid var(--accent);border-color:transparent;}
  .memo-status{font-size:11px;color:var(--muted);margin-top:4px;font-family:var(--mono);}
  .memo-status.saved{color:#1f7a4d;}
  .hl-toolbar{position:fixed;bottom:16px;left:50%;transform:translateX(-50%);z-index:100;background:var(--ink);border-radius:40px;padding:8px 14px;display:flex;align-items:center;gap:8px;box-shadow:0 4px 20px rgba(0,0,0,.35);}
  .hl-btn{width:24px;height:24px;border-radius:50%;border:2.5px solid transparent;cursor:pointer;}
  .hl-btn:hover{transform:scale(1.15);} .hl-btn.active{border-color:#fff;}
  .hl-btn.yellow{background:#ffd600;} .hl-btn.green{background:#3ecf72;} .hl-btn.red{background:#ff6464;}
  .hl-erase{background:none;border:1px solid #555;color:#aaa;font-size:11px;padding:3px 9px;border-radius:20px;cursor:pointer;font-family:var(--sans);}
  .hl-erase:hover{border-color:#aaa;color:#fff;}
  .hl-divider{width:1px;height:18px;background:#333;}
  .hl-label{font-size:11px;color:#888;font-family:var(--mono);}
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
  <div class="transcript" id="transcript">
{전문 전체 내용 — 절대 생략 없이 전체 삽입}
  </div>
</div>
<div class="memo-section">
  <h4>📝 메모</h4>
  <textarea id="memoTA" placeholder="이 전문에서 중요한 내용 메모..."></textarea>
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
const KEY = 'memo-' + location.pathname;
const ta = document.getElementById('memoTA');
const st = document.getElementById('memoStatus');
const saved = localStorage.getItem(KEY);
if(saved) ta.value = saved;
let timer;
ta.addEventListener('input', () => {
  st.textContent = '저장 중...'; st.className = 'memo-status';
  clearTimeout(timer);
  timer = setTimeout(() => {
    localStorage.setItem(KEY, ta.value);
    st.textContent = '✓ 저장됨'; st.className = 'memo-status saved';
  }, 600);
});
let cc='yellow', em=false;
const hb={yellow:document.getElementById('hlY'),green:document.getElementById('hlG'),red:document.getElementById('hlR')};
const he=document.getElementById('hlE');
function sc(c){em=false;cc=c;Object.keys(hb).forEach(k=>hb[k].classList.toggle('active',k===c));he.style.background='none';he.style.color='#aaa';}
hb.yellow.onclick=()=>sc('yellow');hb.green.onclick=()=>sc('green');hb.red.onclick=()=>sc('red');
he.onclick=()=>{em=!em;if(em){Object.values(hb).forEach(b=>b.classList.remove('active'));he.style.background='#333';he.style.color='#fff';}else sc(cc);};
const HLKEY = 'hl-' + location.pathname;
function saveHL(){const d=[];document.querySelectorAll('mark[data-hlid]').forEach(m=>d.push({id:m.dataset.hlid,color:m.dataset.color,text:m.textContent}));localStorage.setItem(HLKEY,JSON.stringify(d));}
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

[메인 HTML 업데이트]
company/{회사명}.html 의 <!-- NEW_CALL_INSERT --> 주석 바로 아래에 삽입.
기존 블록·메모·하이라이트는 절대 건드리지 않는다.

transcript-btn CSS가 없으면 head에 추가:
.transcript-btn{display:inline-block;margin-top:10px;font-size:12px;font-weight:700;color:#2563a8;background:#f0f5ff;border:1px solid #c7d9f8;border-radius:6px;padding:4px 12px;text-decoration:none;}
.transcript-btn:hover{background:#e0ecff;}

삽입할 블록:

어닝콜:
<div class="call-block">
  <div class="call-head" onclick="toggleCall(this)">
    <span class="qtag" style="background:#2563a8">Q? 20??</span>
    <span class="ctitle">날짜 · 한줄 요약</span>
    <span class="chev">▶</span>
  </div>
  <div class="call-body">
    <div class="call-kv">
      <div><span class="k">매출</span><span class="v">$?M (+?% YoY)</span></div>
      <div><span class="k">비GAAP GPM</span><span class="v">?%</span></div>
      <div><span class="k">비GAAP OPM</span><span class="v">?%</span></div>
      <div><span class="k">비GAAP EPS</span><span class="v">$?</span></div>
    </div>
    <ul class="call-pts"><li>핵심 포인트</li></ul>
    <div class="src">출처: 소스명 · 날짜 · <a href="URL" target="_blank">링크</a></div>
    <a class="transcript-btn" href="transcripts/{티커소문자}-{분기}.html" target="_blank">📄 전문 보기</a>
  </div>
</div>

기타 IR 이벤트:
<div class="call-block">
  <div class="call-head" onclick="toggleCall(this)">
    <span class="qtag" style="background:#7c3aed">Investor Day</span>
    <span class="ctitle">날짜 · 이벤트명 · 한줄 요약</span>
    <span class="chev">▶</span>
  </div>
  <div class="call-body">
    <div class="call-kv">
      <div><span class="k">이벤트</span><span class="v">이벤트명</span></div>
      <div><span class="k">주최</span><span class="v">주최사명</span></div>
      <div><span class="k">형식</span><span class="v">공개 웹캐스트 / 슬라이드 공개 / 언론 보도 등</span></div>
    </div>
    <ul class="call-pts"><li>핵심 발언·내용 요약</li></ul>
    <div class="src">출처: 소스명 · 날짜 · <a href="URL" target="_blank">링크</a></div>
    <a class="transcript-btn" href="transcripts/{티커소문자}-{이벤트}.html" target="_blank">📄 전문 보기</a>
  </div>
</div>

[GitHub 푸시]
git add company/transcripts/ company/{회사명}.html
git commit -m "IR 이벤트 업데이트 {티커} {기간}"
git push origin main
