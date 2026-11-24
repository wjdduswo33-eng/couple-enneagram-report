# 커플 에니어그램 심층 분석 — 랜딩 프로토타입

카카오톡 대화 내보내기(.txt/.zip) 하나로 관계 지표를 계산하는 **단일 HTML 프로토타입**입니다.
인스타그램 광고 유입 → 무료 맛보기(Teaser) → 16쪽 리포트 베타 신청까지의 흐름을 실제로 눌러볼 수 있습니다.

## 무엇이 동작하나

- **카톡 파서**: 모바일(iOS/Android) `2026년 11월 3일 오후 9:12, 지민 : …`, PC `--------------- 2026년 11월 3일 월요일 ---------------` + `[지민] [오후 9:12] …`, 24시간 표기 `2026. 11. 3. 21:12, 지민 : …` 세 형식과 여러 줄 메시지, 시스템 메시지(입장/퇴장/삭제) 처리.
- **.zip 해제**: 브라우저 내장 `DecompressionStream`(Safari 16.4+/Chrome 103+)으로 오프라인에서도 동작. 미지원 브라우저는 JSZip CDN 폴백.
- **무료 맛보기**: 메시지 수·기간, 대화 시작권(2시간 공백 기준), 평균/중간 답장 속도, 연타(5개+) 비율, 말버릇 TOP3(빈출/한 마디), 감정·애정·불안 키워드 밸런스(‰), 휴리스틱 에니어그램 유형 티저.
- **잠금 리포트**: 6개 챕터 목차, 블러 미리보기, 19,900원 → 9,900원 앵커링, 베타 신청 모달.
- **개인정보**: 분석은 전부 브라우저 안에서 수행. 서버 전송·로그 저장 없음. 원문은 계산 후 메모리에서 폐기.

## 베타 신청 리드를 실제로 받으려면

`index.html` 안의 `CONFIG.BETA_ENDPOINT` 한 곳만 바꾸면 됩니다.

```js
const CONFIG = {
  BETA_ENDPOINT: "https://script.google.com/macros/s/XXXX/exec"
};
```

Google Apps Script 예시(Google Sheet에 적재):

```js
function doPost(e) {
  const sheet = SpreadsheetApp.openById('SHEET_ID').getSheetByName('beta');
  const d = JSON.parse(e.postData.contents);
  sheet.appendRow([new Date(), d.email, d.instagram, d.at, d.source]);
  return ContentService.createTextOutput('ok');
}
```

비워 두면 데모 모드로, 신청 내역은 방문자 브라우저의 `localStorage('ce_beta_signups')`에만 남습니다.
결제는 연결되어 있지 않습니다. 지금은 수요 확인용 신청만 받습니다.

## 주의

- 샘플 커플 **지민 × 현우**와 그 수치는 모두 가상입니다. 실제 인물의 대화 데이터는 포함되어 있지 않습니다.
- 본 리포트는 대화 습관 지표 기반 참고 분석이며 심리·의학적 진단이 아닙니다.
