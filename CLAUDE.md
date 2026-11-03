# vet-briefing

데일리 브리핑·보고 페이지를 모아 두는 정적 사이트. `vet-briefing.vercel.app` 으로 배포된다.

`/drafts/hub/*` 는 이 저장소에 없다. `vercel.json` 의 rewrite 가
**`growingtiger/signature-hub`** 프로젝트로 넘긴다 (2026-10-10 분리 — 이 저장소가 1.2GB라
배포가 느리고 Vercel 빌드 비용이 컸다). 허브 페이지를 고치려면 signature-hub 를 고친다.

## 골든룰: 철회된 논문은 싣지 않는다 (2026-11-03)

2026-11-03 브리핑 첫 꼭지에 **철회된 논문**이 그대로 나갔다. 인스타 카드까지 올라간 뒤
외부에서 제보를 받고 내렸다. 같은 일이 없도록 아래를 지킨다.

**1. 고르기 전에 철회 여부를 본다.**
- PubMed 기록의 Publication Type에 `Retracted Publication` 이 있거나 `Retraction in` 이 달려 있으면 철회된 논문이다.
- Crossref(`api.crossref.org/works/<DOI>`)의 `update-to` 에 `type: retraction` 이 있으면 철회된 논문이다.
- `Expression of Concern`(우려 표명)이 붙은 논문도 같이 걸러 낸다.
- 오래된 논문을 다시 꺼내 쓸 때도 그때 한 번 더 본다. 철회는 **게재 뒤에 붙는다.**

**2. 철회된 논문은 브리핑에 넣지 않는다.**
꼭 다뤄야 할 만큼 영향이 큰 논문이면 제목에 `[철회됨]` 을 달고 **철회 사유와 철회 공고 DOI**를
함께 적는다. 그 결론을 임상 근거로 제시하지 않는다.

**3. 출처 DOI는 요약한 그 논문의 DOI여야 한다.**
학술지 사이트에서 새로 받은 번호, 서신·답변·정오표·철회 공고의 DOI를 원논문 자리에 적지 않는다.
적기 전에 **DOI를 열어 제목이 같은지 한 번 확인한다.**
(2026-11-03에는 원논문 DOI가 `10.1111/jsap.13880` 인데 `10.1111/jsap.70211` 로 적혀 나갔다.)

**4. 이미 나간 뒤에 알게 되면.**
원장님께 먼저 알리고, 보고서·카드·캡션을 어떻게 할지(정정 안내를 달지, 꼭지를 빼고 다시 만들지)
지시를 받는다. 조용히 지우지 않는다.

## 골든룰: "내 카톡으로 보내줘"는 이렇게 한다

카카오톡으로 직접 보내는 기능은 없다. **"불가능하다"고 답하지 말 것.** 정해진 경로가 있다.

1. **허브 페이지를 만든다** — `growingtiger/signature-hub` 저장소에 `<이름>.html` 을 추가한다.
   기존 페이지(`salim-*.html`, `signal-*.html` 등)를 그대로 본떠, 보낼 글을 `<div class="box">` 에 넣고
   **[복사] 단추**를 붙인다.
2. **목록에 올린다** — 같은 저장소 `list-74a54603.json` 맨 위에 `{slug, label, title, at}` 한 줄을 넣는다.
3. **main 에 푸시한다** — 주소는 `https://vet-briefing.vercel.app/drafts/hub/<이름>.html`
4. **휴대폰 알림(ntfy)을 보낸다** — 알림을 보내야 "보낸 것"이다. 링크만 채팅에 적어 두고 끝내지 말 것.
   - 세션에서 `ntfy.sh` 에 닿으면 그대로 보낸다.
   - **막혀 있으면**(클라우드 세션의 네트워크 정책) 깃허브 액션으로 보낸다:
     `mcp__github__actions_run_trigger` → `run_workflow`, `growingtiger/signature-hub`,
     `notify.yml`, `ref: main`, `inputs: {title, message, slug}`.
     실행 결과를 `actions_list` 로 확인하고 `conclusion: success` 인지 본다.
   - 실제 **카톡 메시지 발송은 맥의 `~/ai-assistant/kakao_hub.py`** 가 한다 (2026-10-20 원장 결정).
     클라우드 세션은 허브 페이지 + ntfy 알림까지만 한다. 카톡 메시지까지 꼭 필요하면 `ListAgents` 로 맥 세션이
     보이는지 확인해 `SendMessage` 로 발송을 부탁하고, 닿지 않으면 ntfy 알림으로 끝낸 뒤 그 사실을 말씀드린다.
5. **막히면 조용히 끝내지 않는다** — 무엇이 왜 막혔는지 분명히 말한다.

글에는 이모지를 쓰지 않는다.

## ntfy 알림 안내 페이지

`drafts/hub/ntfy-setup.html` 은 원장님이 ntfy 앱을 구독하는 안내다.
여기에 알림 주제(topic) 이름이 그대로 적혀 있는데 **이 저장소는 공개 저장소다.**
주제를 아는 사람은 원장님 휴대폰으로 알림을 보내거나 알림을 구독해 볼 수 있다.
주제를 바꾸려면 ntfy 앱 재구독 + signature-hub 의 시크릿 `NTFY_TOPIC` 갱신 + 이 안내 페이지 수정을 함께 한다.

## 골든룰: 새 앱은 이태호 통합 포털에도 등록한다

새 PWA·웹앱·홈페이지·정기 결과물 페이지를 만들어 배포하면, **어느 저장소에서 만들었든**
`growingtiger/leetaeho-portal` 의 `public/apps.json` 의 `apps` 에 한 줄을 넣고 main 에 푸시한다
(2026-10-10 원장 지시, leetaeho-portal/CLAUDE.md 와 같은 규칙).

항목: `id`(고유), `group`(me·owners·hospital·public·outputs·family·console), `repo`(~/ 아래 폴더 이름, 없으면 ""),
`name`, `url`, `desc`(한 줄), `kw`(검색 별칭).

**직원용 시그니처 통합 포털**(`signature-amc-referral/public/portal.html`)에는 원장님이 말씀하실 때만 넣는다.
한쪽에 넣었다고 다른 쪽에 넣지 않는다.

## 골든룰: 공용 저장소는 Supabase로 (2026-10-22 원장 결정)

여러 기기·여러 직원이 함께 보는 자료를 서버에 둘 때는 **Supabase Storage**를 쓴다.
공통 부품 **`api/_store.js`를 그대로 복사**해 쓰고(원본: signature-amc-intake), 앱마다 경로 앞머리만 다르게 잡는다.
한 Supabase 프로젝트(서울)·한 버킷(`pwa-store`)을 여러 앱이 나눠 쓴다.

- 환경변수는 `SUPABASE_URL`, `SUPABASE_SECRET_KEY` 두 개뿐이다(값은 다른 앱과 같다. 원장님이 직접 넣는다).
- **환경변수가 없으면 공용 기능만 꺼진 채 나머지는 그대로 돌아가게 만든다.** 이 폴백을 없애지 말 것.
- 기록 하나에 파일 하나로 둔다(동시에 저장해도 서로를 덮지 않는다). 목록 화면이 필요하면 요약만 모은
  작은 `index.json`을 따로 둔다. 어긋났을 때 파일을 보고 다시 만드는 길(rebuild)을 함께 만든다.
- 경로에 무작위 문자열을 섞는다. **그 문자열을 바꾸면 기존 자료를 못 찾는다.**
- 큰 파일(PDF 등)은 Vercel 함수 본문 한도(4.5MB)를 넘으므로, 서버는 **서명 주소만 만들어 주고
  브라우저가 Supabase에 직접 올린다.** 비밀 키는 브라우저로 내려보내지 않는다.
- **개인정보가 든 파일(환자 차트 등)은 공용 저장소에 보관하지 않는다.** 꼭 필요하면 먼저 여쭌다.

기존 앱들이 쓰는 **Vercel Blob 방식(전체 상태를 한 덩어리 JSON으로 주고받는 sync)은 그대로 둔다.**
이미 돌아가는 것을 옮기지 않는다. 다만 **기록이 계속 쌓이거나 파일이 함께 붙는 새 기능**에는 쓰지 않는다.
