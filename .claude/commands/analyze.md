주어진 티커의 기업 분석 리포트를 신규 생성하고 GitHub에 푸시한다.
사용법: /analyze {티커} (예: /analyze ORCL)

[파일명 규칙 — 반드시 준수]
티커가 아닌 회사명 소문자 하이픈 형식으로 저장한다:
  ORCL   → oracle.html
  BE     → bloom-energy.html
  GEV    → ge-vernova.html
  CEG    → constellation.html
  VST    → vistra.html
  NRG    → nrg-energy.html
  SMR    → nuscale.html
  OKLO   → oklo.html
  WRT1V  → wartsila.html
  위 목록에 없는 티커는 회사 공식명을 소문자·하이픈 형식으로 변환

[사전 작업]
1. company/ 및 company/transcripts/ 폴더가 없으면 자동 생성한다
2. company/{회사명}.html 이 이미 존재하면 덮어쓰기 전에 확인 메시지를 출력한다

[데이터 수집 원칙]
- 모든 수치는 IR 자료·공식 공시(10-Q/10-K, 8-K, IR deck) 등 1차 자료 기반
- stockanalysis.com, macrotrends.net 교차 확인
- 수치마다 출처 명기. 못 찾으면 "데이터 없음" 표기, 절대 추정·날조 금지
- 가장 최근 발표 분기 기준 직전 12개 분기 대상
- LOI/MOU와 구속력 있는 계약(binding) 반드시 구분
- 사업부별 "강점/경쟁력"은 근거와 함께 서술, 단정 금지

[수집 항목]
1. 기업 개요 (사업부별 상세, 경쟁 강점·해자, 근거 포함)
2. 헤드라인 재무 (12개 분기, GAAP + 비GAAP)
3. 밸류에이션 (시총, PER, PBR, EV/EBITDA, 배당)
4. 백로그 (공시 있을 때만, Book-to-Bill 추정 명시)
5. 주요 계약·딜 (계약/LOI 구분, 출처·링크)
6. 핵심 포인트 & 알파 시그널 (근거·추론 명기, 단정 금지)
7. IR 이벤트 (최근 4개 분기, 어닝콜 + Investor Day + Analyst Meeting 등)

[IR 이벤트 수집 원칙]
- 공식 자료(웹캐스트, 슬라이드, 트랜스크립트) 최우선
- 공식 전문 없어도 언론 보도·애널리스트 리포트에서 발언 사실 확인되면 수집
- 비공개 자료는 건너뜀
- 각 이벤트별 출처·날짜 표기, 못 구한 이벤트는 "자료 미확보" 명시

[전문 처리 규칙 — 핵심]
각 어닝콜·IR 이벤트의 전문을:
1. 웹에서 찾아서 컨퍼런스 콜 톤에 맞게 한국어로 번역
2. 절대 중간에 끊지 않고 전체를 저장한다
3. company/transcripts/{티커소문자}-{분기}.html 파일로 저장
   예) transcripts/be-q1-2026.html
       transcripts/be-investor-day-2025.html
4. 전문 페이지는 아래 형식으로 만든다 (하이라이트+메모 포함)
5. 메인 HTML의 해당 블록에는 "전문 보기" 버튼을 넣고 target="_blank"로 새 탭에서 열기

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

[메인 HTML IR 이벤트 블록 형식]
<!-- NEW_CALL_INSERT --> 주석 바로 아래에 삽입:

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

transcript-btn CSS (head에 추가):
.transcript-btn{display:inline-block;margin-top:10px;font-size:12px;font-weight:700;color:#2563a8;background:#f0f5ff;border:1px solid #c7d9f8;border-radius:6px;padding:4px 12px;text-decoration:none;}
.transcript-btn:hover{background:#e0ecff;}

[파일 저장 및 GitHub 푸시]
1. company/transcripts/ 폴더 생성 (없으면)
2. 각 전문을 company/transcripts/{티커소문자}-{분기}.html 로 저장
3. company/{회사명}.html 저장
4. company/index.html 에서 해당 기업 카드 pending 제거, href 추가
5. git add . && git commit -m "기업 분석 추가 {티커}" && git push origin main

[비상장사 처리 규칙]
아래 회사들은 비상장(공시 데이터 없음)으로, 재무 섹션을 생략하고
뉴스·계약 동향 중심 페이지를 생성한다:
  TerraPower, Kairos Power (KairosPower), X-energy

비상장사 페이지 구성 (재무 차트·테이블 없음):
1. 회사 개요
   - 기술 방식 (로 설계, 냉각 방식, 출력 규모)
   - 전략적 포지션 (밸류체인 내 위치, 경쟁 강점)
   - 주요 투자자·파트너 (근거 포함)
2. 주요 계약·파트너십
   - 계약(binding) / LOI·MOU 구분 태그 필수
   - 출처·날짜·링크 포함
   - 수치는 원문 그대로 (예: 690MW, 2032년 목표)
3. 뉴스·동향 (날짜별 누적, 최근순)
   - 규제·인허가 진행 현황
   - 자금 조달·투자 유치
   - 기술 개발·마일스톤
   - 파트너십·MOU 동향
4. 핵심 포인트 & 투자 논지 함의
   - 상용화 타임라인 현실적 평가 (단정 금지, 근거 포함)
   - BTM 기저발전 논지에서의 역할
   - 리스크 (규제·기술·자금)
5. IR 이벤트 (공개된 컨퍼런스 발표, 언론 인터뷰 등)
   - 전문 페이지 별도 생성 (하이라이트·메모 포함)

네비게이션 탭: 회사개요 / 계약·파트너십 / 뉴스·동향 / 핵심·논지 / IR이벤트
"재무 데이터 없음 — 비상장사" 안내 배너 상단에 표시

[추가 파일명 규칙]
  SMR         → nuscale.html
  CCJ         → cameco.html
  FCEL        → fuelcell-energy.html
  LEU         → centrus-energy.html
  TerraPower  → terrapower.html
  KairosPower → kairos-power.html
  X-energy    → x-energy.html

[한국 상장사 처리 규칙]
회사명이 한국 기업이거나 KRX 티커(6자리 숫자)로 입력된 경우:
- 재무 공시: DART(dart.fss.or.kr) 기반으로 수집
- 통화: 원(₩), 억원 단위로 표기
- 분기: 1Q/2Q/3Q/4Q (한국 회계기준 K-IFRS 또는 K-GAAP)
- 컨콜: 국문 어닝콜 + 영문 IR 자료 병행 수집
- 밸류에이션: PER, PBR, EV/EBITDA (한국 시장 기준)
- 배당수익률 포함
- 수치 출처: DART 사업보고서·분기보고서 우선, 
  KIND(kind.krx.co.kr), 네이버금융, 각사 IR 자료 교차 확인

[회사명으로 분석 가능한 목록 — 이름으로 입력 시 자동 매핑]
상장사:
  HD현대중공업     → KRX 329180 → hd-hyundai-heavy.html
  HD한국조선해양   → KRX 009540 → hd-ksoe.html
  두산에너빌리티   → KRX 034020 → doosan-enerbility.html
  STX엔진         → KRX 077970 → stx-engine.html
  한화에어로스페이스 → KRX 012450 → hanwha-aerospace.html
  삼성물산        → KRX 028260 → samsung-ct.html
  GS에너지        → KRX 078930 → gs-energy.html

비상장 한국 기업:
  HiMSEN          → himensen.html  (HD현대중공업 엔진 브랜드, 별도 페이지)

[분석 실행 방법]
/analyze HD현대중공업
/analyze 두산에너빌리티
/analyze STX엔진
  MSFT → microsoft.html
  META → meta.html
  GOOGL → google.html
  AMZN → amazon.html
