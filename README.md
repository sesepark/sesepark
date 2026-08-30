## 박세환 · Sehwan Park

서울대학교 자유전공학부 25학번 · 전기정보공학부 전공 · 휴머노이드 로봇 팀 [SHAPE](https://github.com/snu-shape)

로봇 원격조작으로 사람의 시연을 모으고, 그 데이터로 정책을 학습시키는 파이프라인을 만들어 왔습니다.
로봇팔 텔레오퍼레이션과 데이터 수집 워크스페이스, 휴머노이드 오퍼레이터 스테이션,
그리고 6개월째 혼자 운영 중인 웹서비스가 지금까지 만든 것들입니다.
어느 쪽이든 실제로 돌아가는 상태까지 직접 만들고 유지하는 걸 좋아합니다.

---

### 로보틱스 · Physical AI

**[xarm7-pi0-teleop](https://github.com/sesepark/xarm7-pi0-teleop)** — xArm7 텔레오퍼레이션 · pi0 데이터 수집 워크스페이스
GELLO 리더암과 스마트폰(WebXR)으로 xArm7을 조작하고, 시연을 LeRobot 데이터셋으로 기록해
pi0(OpenPI) 정책을 학습·재생합니다. 리더암 엔코더의 초기 offset 오차가 FK 비선형성을 타고
endpoint 오차로 새는 문제를, 절대 관절값 대신 팔로워 기준 관절에 리더 delta를 더한 "가상 관절"로
해결했습니다. 제어 루프(30Hz)가 네트워크 왕복 없이 돌도록 xArm7 순기구학을 직접 구현했습니다.
→ 텔레옵 구현은 [lerobot_robot_ufactory 기여분](https://github.com/sesepark/lerobot_robot_ufactory/tree/xarm7-gello-webxr) (upstream 대비 14파일 / 1,811줄)

**[ai-worker-humanoid-challenge](https://github.com/sesepark/ai-worker-humanoid-challenge)** — 휴머노이드 챌린지 Mission A (팀 프로젝트)
ROBOTIS AI Worker 기반 팀 프로젝트에서 **텔레오퍼레이션 오퍼레이터 스테이션**을 담당했습니다
(전체 88커밋 중 68커밋). 영상 패널을 RViz 밖으로 분리해 X11에서 죽던 문제를 없애고, 손목
RealSense의 USB 대역 포화를 잡고, ZED 뎁스 어시스트 오버레이와 주행 패널·cmd_vel mux를
구성했습니다.

---

### 시스템 · 서비스 개발

**[cookierunhub-case-study](https://github.com/sesepark/cookierunhub-case-study)** — 게임 정보 서비스 (단독 개발·운영)
6개월간 혼자 설계·개발·운영 중인 3개국어 웹서비스의 엔지니어링 기록입니다.
554커밋 / 약 17.8만 줄 / DB 마이그레이션 148개. 관리형 플랫폼이 아니라 자택 리눅스 서버에서
돌리기 때문에 **blue-green 무중단 배포와 10초 주기 health 기반 자동 장애 복구를 직접
구현**했습니다. 사용자 게시글까지 번역해야 해서 로케일 파일이 아니라 DB 스키마와 번역 워커로
파이프라인을 구성했습니다. (코드와 수집 데이터는 비공개, 구조와 판단 근거만 정리)

**SHAPE 팀 웹사이트** — 팀 공식 홈페이지를 단독 개발했습니다 (90커밋, 비공개 저장소).

**온디바이스 개인 에이전트** — Apple Silicon에서 전부 로컬로 도는 Swift 메뉴바 앱.
Qwen3-ASR 음성 전사와 화자 구분, 문서 인식, 인박스 요약을 기기 밖으로 데이터를 내보내지 않고
처리합니다. 보안 경계를 관례가 아니라 코드로 강제한 점(읽기 전용 접근, 메시지 본문을 지시로
해석하지 않음, 쓰기 대상 페이지 재검증)을 특히 신경 썼습니다. (정리 후 공개 예정)

**[shamoa-snu-programs](https://github.com/sesepark/shamoa-snu-programs)** — 서울대 해외 프로그램 탐색·추천 앱 (수업 최종 프로젝트)

---

### 오픈소스 기여

**[headroom](https://github.com/headroomlabs-ai/headroom)** — AI 에이전트용 컨텍스트 압축 레이어 (라이브러리 · 프록시 · MCP)
[PR #3331](https://github.com/headroomlabs-ai/headroom/pull/3331) — 메인테이너 승인, 머지 대기
터미널을 닫으면 SIGTERM이 아니라 **SIGHUP**이 오는데, `claude` 경로만 이 신호를 처리하고
나머지 도구(codex, aider, cursor 등)가 공유하는 두 경로는 처리하지 않았습니다. 그래서 래퍼가
`finally: cleanup()`을 실행하지 못한 채 죽고, 프록시가 PID 1로 재부모화되어 포트를 붙든 채
남았습니다. 코드를 읽다 찾은 게 아니라 **8일 19시간째 살아 있던 유출 프록시**에서 출발했고,
소스 문자열 매칭이 아니라 실제 자식 프로세스를 SIGHUP으로 종료시켜 누수를 재현하는 테스트를
함께 넣었습니다.

---

### 주로 쓰는 것

로봇 · ROS 2 / MoveIt / RealSense · ZED / LeRobot · OpenPI(pi0) / xArm SDK
학습 · PyTorch / MLX / LoRA 파인튜닝
백엔드 · Python · FastAPI · SQLAlchemy · PostgreSQL · Alembic
프론트 · TypeScript · Next.js · React
인프라 · Docker · Caddy · systemd · GitHub Actions · 자체 리눅스 서버 운영
그 외 · Swift · Kotlin

---

📮 park7132872@snu.ac.kr
