내가 구독하는 Substack 글을 수집해 "리서치 데스크 톤" 한국어로 번역하고,
substack/ 섹션에 전문 페이지를 만들어 GitHub에 푸시한다.

사용법:
  /substack                  ← 구독 목록(substack/subscriptions.txt) 전체에서 최신 무료 글 수집
  /substack {slug}           ← 특정 퍼블리케이션 최신 글만 (예: /substack doomberg)
  /substack {글 URL}         ← 특정 글 하나만 번역 (무료 공개 글)
  /substack add {피드URL}     ← 구독 목록에 피드 추가
  (본문 붙여넣기)            ← 유료·구독자 전용 등 자동 수집 불가한 글을 직접 처리

──────────────────────────────────────────────
[중요 — 계정 접근 원칙]
- 이 기능은 Substack 계정에 직접 로그인하지 않는다(자격증명 저장 금지).
- "내가 구독하는 사람 글"은 substack/subscriptions.txt 에 적어둔 퍼블리케이션의
  공개 RSS 피드(https://{slug}.substack.com/feed 또는 커스텀도메인/feed)로 가져온다.
- 무료 공개 글은 피드에 전문이 실린다. 유료·구독자 전용 글은 피드에 미리보기만
  실리므로, 그 경우 본문을 직접 붙여넣으면 동일하게 번역·게시한다.
- 어떤 경우에도 원문에 없는 내용을 지어내지 않는다(수치·인용·주장 날조 금지).

[네트워크 요구사항]
- 이 명령은 substack.com(및 각 퍼블리케이션 커스텀 도메인)으로의 아웃바운드 HTTPS가
  허용된 세션에서 실행해야 한다. 아웃바운드가 차단된 환경(egress 정책 차단)에서는
  자동 수집이 되지 않으므로, 그때는 "본문 붙여넣기" 방식으로 처리한다.
  - 피드/글 수집은 WebFetch 우선, 실패 시 curl(브라우저 User-Agent) 재시도.
  - 둘 다 403/CONNECT 차단이면 자동 수집 불가 → 사용자에게 원문 URL·본문 요청.

──────────────────────────────────────────────
[구독 목록 파일 — substack/subscriptions.txt]
- 한 줄에 하나. "표시명 | 피드URL" 또는 "피드URL"만.
- '#'로 시작하는 줄은 주석.
- /substack (인자 없음) 실행 시 이 목록의 각 피드에서 "아직 번역 안 한 최신 글"을 수집.
- /substack add {피드URL} 은 이 파일에 한 줄 추가 후 커밋.

[slug → 피드 URL 규칙]
- 표준: https://{slug}.substack.com/feed
- 커스텀 도메인(예: Doomberg=newsletter.doomberg.com): https://{도메인}/feed
- 개별 글 URL이 주어지면 그 URL을 직접 WebFetch 한다.

──────────────────────────────────────────────
[수집·선별 절차]
1. 대상 피드 목록 결정
   - 인자 없음  → subscriptions.txt 전체
   - slug 지정  → 해당 피드 1개
   - URL 지정   → 그 글 1개(피드 파싱 생략)
2. 각 피드에서 최근 항목(item) 파싱: title, link, pubDate, creator(author),
   content:encoded(본문). 최신순.
3. 중복 제거: substack/ 폴더에 이미 같은 글의 페이지가 있으면(파일명 규칙으로 판정) 건너뜀.
4. 기본은 피드당 "가장 최근 1개"만 처리. 사용자가 "N개"를 요청하면 그만큼.
5. 본문이 미리보기로 잘려 있으면(유료) → 해당 글은 "유료·본문 미확보"로 표시하고
   사용자에게 원문 붙여넣기를 요청. 임의 보완·요약 대체 금지.

[번역 규칙 — "리서치 데스크 톤"]
- 이 사이트(기저발전 투자 리서치 데스크)의 톤으로 재구성해 한국어 번역한다:
  간결하고 분석적이며, 투자·산업 함의 중심. 군더더기·인사말·홍보성 표현은 정돈.
- 단, 사실관계는 원문에 충실: 수치·고유명사·인용·인과 주장은 원문 그대로 옮긴다.
  (톤을 다듬는 것이지 내용을 바꾸거나 지어내는 것이 아니다.)
- 문단 구조는 유지하되 한국어 가독성에 맞게 자연스럽게. 원문 소제목은 소제목으로 옮긴다.
- 애매하거나 원문에 없는 부분은 추정하지 말고 [원문 불명확] 등으로 표시.
- 페이지 맨 위 "데스크 요약"에 투자·산업 관점 핵심 3~5개를 불릿으로 정리(원문 근거 범위 내).

──────────────────────────────────────────────
[파일 저장 규칙]
- 전문 페이지: substack/{slug}-{YYYY-MM-DD}-{제목약어}.html
  예) substack/doomberg-2026-07-15-strait-of-hormuz.html
  - {slug}: 퍼블리케이션 식별자(도메인 앞부분 또는 표시명 소문자·하이픈)
  - {YYYY-MM-DD}: 원문 발행일
  - {제목약어}: 영문 제목을 소문자·하이픈으로 3~5단어 축약
- 목록 페이지: substack/index.html 의 <!-- NEW_POST_INSERT --> 주석 바로 아래에 카드 삽입.
  기존 카드·메모·하이라이트는 절대 건드리지 않는다(최신 글이 위로 쌓임).

[전문 페이지 형식 — substack/*.html]
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{번역 제목} — {표시명} · Substack 번역</title>
<style>
  :root{--ink:#13171e;--paper:#f9f9f7;--text:#1a1f29;--muted:#6b7280;--line:#e6e9ee;--accent:#ff6719;
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
  .art-title{font-size:24px;font-weight:800;line-height:1.35;letter-spacing:-.3px;margin:0 0 8px;color:var(--ink);}
  .art-meta{font-size:12px;color:var(--muted);font-family:var(--mono);margin:0 0 22px;}
  .desk-box{background:#fff;border:1px solid var(--line);border-left:4px solid var(--accent);border-radius:10px;padding:16px 18px;margin:0 0 26px;}
  .desk-box h3{font-size:12px;font-weight:800;color:var(--accent);margin:0 0 8px;letter-spacing:.3px;font-family:var(--mono);text-transform:uppercase;}
  .desk-box ul{margin:0;padding-left:18px;}
  .desk-box li{font-size:14px;line-height:1.7;margin-bottom:5px;color:#222;}
  .article{font-size:15px;line-height:1.9;color:#222;word-break:break-word;}
  .article p{margin:0 0 16px;}
  .article h2{font-size:18px;font-weight:800;margin:30px 0 12px;color:var(--ink);letter-spacing:-.2px;}
  .article h3{font-size:15px;font-weight:800;margin:24px 0 10px;color:var(--ink);}
  .article blockquote{margin:0 0 16px;padding:6px 16px;border-left:3px solid var(--line);color:#555;font-style:italic;}
  .article a{color:#c2540f;}
  .src{margin-top:34px;padding-top:16px;border-top:1px solid var(--line);font-size:12px;color:var(--muted);font-family:var(--mono);line-height:1.7;}
  .src a{color:#c2540f;}
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
  @media(max-width:560px){.memo-section{display:none;}}
</style>
</head>
<body>
<header>
  <a class="back" href="index.html">← Substack 번역</a>
  <div class="hdr-info">
    <h1>{표시명}</h1>
    <p>{원저자} · 원문 {YYYY-MM-DD} · Substack 번역</p>
  </div>
</header>
<div class="wrap">
  <h1 class="art-title">{번역 제목}</h1>
  <p class="art-meta">{원저자} · {표시명} · 원문 발행 {YYYY-MM-DD}</p>

  <div class="desk-box">
    <h3>데스크 요약</h3>
    <ul>
      <li>투자·산업 관점 핵심 포인트 1</li>
      <li>핵심 포인트 2</li>
      <li>핵심 포인트 3</li>
    </ul>
  </div>

  <div class="article" id="article">
{리서치 데스크 톤으로 번역한 본문 전체 — <p>·<h2>·<h3>·<blockquote>로 구조화, 절대 생략·요약하지 않음}
  </div>

  <div class="src">
    원제: {원문 제목}<br>
    원저자: {원저자} · 퍼블리케이션: {표시명}<br>
    원문: <a href="{원문 URL}" target="_blank">{원문 URL}</a><br>
    수집: Substack RSS · 번역: 리서치 데스크 톤 (사실관계는 원문 기준)
  </div>
</div>
<div class="memo-section">
  <h4>📝 메모</h4>
  <textarea id="memoTA" placeholder="이 글에서 중요한 내용 메모..."></textarea>
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

──────────────────────────────────────────────
[목록 페이지 카드 형식 — substack/index.html]
<!-- NEW_POST_INSERT --> 주석 바로 아래에 삽입:

<a class="post-card" href="{slug}-{YYYY-MM-DD}-{제목약어}.html" data-pub="{slug}" data-name="{검색용 소문자 키워드}">
  <div class="post-card-head">
    <span class="post-pub">{표시명}</span>
    <span class="post-date">{YYYY-MM-DD}</span>
  </div>
  <div class="post-title">{번역 제목}</div>
  <p class="post-excerpt">{데스크 요약 첫 줄 또는 한 문장 요약}</p>
  <div class="post-foot"><span class="post-author">✍ {원저자}</span><span class="post-arrow">→</span></div>
</a>

──────────────────────────────────────────────
[GitHub 푸시]
git add substack/ index.html
git commit -m "Substack 번역 {표시명} {YYYY-MM-DD}"
git push origin main

- 여러 글을 한 번에 처리하면 커밋 메시지는 "Substack 번역 {건수}건 {날짜}".
- 자동 수집이 전혀 안 된 경우(전부 유료·차단)에는 무엇이 왜 안 됐는지 사용자에게 보고하고,
  원문 URL·본문을 받으면 이어서 처리한다.
