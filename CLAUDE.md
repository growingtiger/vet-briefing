# vet-briefing

데일리 브리핑·보고 페이지를 모아 두는 정적 사이트. `vet-briefing.vercel.app` 으로 배포된다.

`/drafts/hub/*` 는 이 저장소에 없다. `vercel.json` 의 rewrite 가
**`growingtiger/signature-hub`** 프로젝트로 넘긴다 (2026-10-10 분리 — 이 저장소가 1.2GB라
배포가 느리고 Vercel 빌드 비용이 컸다). 허브 페이지를 고치려면 signature-hub 를 고친다.

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
   - 실제 **카톡 메시지 발송은 맥의 `~/ai-assistant/kakao_hub.py`** 가 한다. 클라우드 세션은 그 맥에 닿을 수 없으니
     허브 페이지 + ntfy 알림까지만 할 수 있다. 카톡 메시지 자체는 가지 않는다는 점을 원장님께 말씀드린다.
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
