# ai-figma

Claude Code(VS Code)에서 Figma를 직접 조작하기 위한 로컬 MCP 작업 공간.

## 작업 규칙

- 커밋/푸시는 사용자가 Git Bash에서 직접 실행한다. Claude는 파일 내용만 수정한다.
- 변경 내용 요약 및 세부사항 정리는 Claude가 이 README에 작성/갱신한다.
- Claude는 README를 수정한 뒤 변경 내용을 요약해서 보여주고 확인을 받는다. 커밋/푸시는 그 확인 이후 사용자가 별도로 진행한다.
- Figma 접근 권한은 AI 전용으로 지정된 파일에만 한정한다.

## 변경 로그

### 2026-07-13
- TalkToFigma MCP 워크스페이스 구성 (`.mcp.json`, Bun 설치)
- 웹소켓 릴레이 외부 네트워크 노출 차단 — localhost 전용 바인딩으로 변경 (`patches/socket-localhost-binding.patch`)

## 구성

- `.mcp.json` — Claude Code에 등록된 MCP 서버 설정 (`TalkToFigma`)
- `cursor-talk-to-figma-mcp/` — [grab/cursor-talk-to-figma-mcp](https://github.com/grab/cursor-talk-to-figma-mcp) 클론. 웹소켓 릴레이 + Figma 데스크톱 플러그인으로 구성되며, Figma 공식 REST API/MCP 서버를 거치지 않고 Plugin API를 직접 호출한다. 요금제별 호출 제한과 무관하게 동작.
- `claude-talk-to-figma-mcp/` — [arinspunk/claude-talk-to-figma-mcp](https://github.com/arinspunk/claude-talk-to-figma-mcp) 클론, 참고용 대체 구현.
- `patches/` — 로컬 보안 수정 diff.

두 서브폴더는 각자 자체 `.git`을 가진 클론이라 이 레포에는 내용이 아니라 "이 시점의 커밋" 포인터만 기록된다 (embedded repo).

## 워크플로

1. VS Code에서 이 폴더를 열면 `.mcp.json`이 로드되어 `TalkToFigma` MCP 서버가 뜬다.
2. `cursor-talk-to-figma-mcp` 안에서 웹소켓 릴레이 실행: `bun run src/socket.ts` (재부팅/터미널 종료 시 같이 죽으므로 매번 다시 켜야 함).
3. Figma 데스크톱 앱에서 작업 파일을 열고 플러그인을 실행 → 채널 접속.
4. Claude Code 쪽에서 `join_channel`로 같은 채널에 접속.
5. 이후 MCP 도구로 작업.
