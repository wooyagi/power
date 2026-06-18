# /concall 명령어

최근 4개 분기 earnings call transcript를 수집한다.
- 공식 자료를 최대한 사용해야하며, 전문 제공을 하지 않는다면 웹 검색 필요
- 4분기 다 못 구하면, 구한 분기만 처리하고 누락 분기를 명시
- 각 분기별로 어떤 소스에서 가져왔는지 출처 표기
- 분기별 포인트는 논리적이고 직관적으로 요약이 필요
- 가장 밑에 전문을 첨부해줘. 읽어보기 쉽게 컨퍼런스 콜 톤에 맞춰 번역할 것

[파일 업데이트]
컨콜 내용을 수집한 뒤 아래 형식의 HTML 블록을 작성하고,
company/{티커소문자}.html 파일의
"<!-- NEW_CALL_INSERT" 주석 바로 아래에 삽입한다.

<div class="call-block">
  <div class="call-head" onclick="toggleCall(this)">
    <span class="qtag">Q? 20??</span>
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
    <ul class="call-pts">
      <li>핵심 포인트 1</li>
    </ul>
    <div class="src">출처: 소스명 · 날짜 · <a href="URL" target="_blank">링크</a></div>
    <div class="transcript">
[전문 번역]
    </div>
  </div>
</div>

삽입 후 커밋 메시지 "컨콜 업데이트 {티커} {분기}"로 main에 푸시한다.
기존 블록들은 그대로 아래로 밀린다.
