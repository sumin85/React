# Project_1

### 📅서비스 설명

친구 간 상호 동의한 연결을 통해, 별도의 소통 없이도 일정이 자동 공유되고 조율되는 스마트 일정 공유 플랫폼입니다. 개인의 캘린더에 일정을 등록하면, 해당 일정은 친구 목록과의 관계 설정(서로 친구)이 되어 있는 경우에 한해 자동으로 공유되며, 그룹 단위 일정 조율도 손쉽게 처리할 수 있습니다.

### 💻와이어프레임
1. 온보딩/로그인화면
	- 소셜 로그인 버튼
	- 이메일/비밀번호 입력 및 회원가입 버튼
2. 메인 홈
	- 상단: 오늘의 일정 / 주간 일정 요약
	- 중단: 친구 일정 요약
	- 하단: 그룹 일정 알림
3. 일정 등록
	- 제목, 설명, 시작/종료 시간, 공개여부, 반복 옵션 선택
4. 친구 목록
	- 친구 검색, 요청 리스트, 수락/거절 버튼
5. 그룹 목록
	- 참여 그룹 리스트
    - 그룹 생성 / 초대 기능
6. 그룹 상세
	- 멤버 리스트, 권한 보기
    - 그룹 일정 리스트 및 일정 제안
7. 그룹 일정 조율
	- 후보 시간 선택
    - 멤버 투표
    - 자동 추천 시간 표시
8. 알림 센터
	- 친구 요청, 그룹 초대, 일정 업데이트 등의 알림 목록 
***

### 📂ERD (DB관계도)

**User** <br>
 ├── id (PK) <br>
 ├── name <br>
 ├── email <br>
 ├── password <br>
 ├── profile_image_url <br>
 └── created_at <br>

**Friend** <br>
 ├── id (PK) <br>
 ├── user_id (FK → User.id) <br>
 ├── friend_id (FK → User.id) <br>
 ├── status (pending, accepted, rejected) <br>
 └── created_at <br>

**Schedule** <br>
 ├── id (PK) <br>
 ├── user_id (FK → User.id) <br>
 ├── title <br>
 ├── description <br>
 ├── start_time <br>
 ├── end_time <br>
 ├── is_private <br>
 └── created_at <br>

**Group** <br>
 ├── id (PK) <br>
 ├── name <br>
 ├── owner_id (FK → User.id) <br>
 └── created_at <br>

**GroupMember** <br>
 ├── id (PK) <br>
 ├── group_id (FK → Group.id) <br>
 ├── user_id (FK → User.id) <br>
 ├── role (admin/member) <br>
 └── joined_at <br>

**GroupSchedule** <br>
 ├── id (PK) <br>
 ├── group_id (FK → Group.id) <br>
 ├── title <br>
 ├── description <br>
 ├── preferred_time_slots (JSON) <br>
 ├── selected_time <br>
 ├── created_by (FK → User.id) <br>
 ├── status (pending, confirmed) <br>
 └── created_at <br>

**Notification** <br>
 ├── id (PK) <br>
 ├── user_id (FK → User.id) <br>
 ├── type (friend_request, schedule_update, etc) <br>
 ├── message <br>
 ├── is_read <br>
 └── created_at <br>

### API 명세서
| Method | Endpoint                  | 설명           | 인증 | 비고               |
|--------|---------------------------|----------------|------|--------------------|
| POST   | `/auth/signup`            | 회원가입       | ❌   | 이메일/비번         |
| POST   | `/auth/login`             | 로그인         | ❌   | JWT 발급           |
| GET    | `/schedules`              | 내 일정 조회    | ✅   | 쿼리로 주간 필터    |
| POST   | `/schedules`              | 일정 추가       | ✅   |                    |
| PATCH  | `/schedules/:id`          | 일정 수정       | ✅   |                    |
| DELETE | `/schedules/:id`          | 일정 삭제       | ✅   |                    |
| POST   | `/friends`                | 친구 요청       | ✅   |                    |
| GET    | `/friends`                | 친구 목록 조회  | ✅   |                    |
| POST   | `/groups`                 | 그룹 생성       | ✅   |                    |
| GET    | `/groups`                 | 그룹 목록 조회  | ✅   |                    |
| GET    | `/groups/:id`             | 그룹 상세       | ✅   |                    |
| POST   | `/groups/:id/schedule`    | 그룹 일정 제안  | ✅   |                    |
| GET    | `/notifications`          | 알림 목록 조회  | ✅   |                    |

### 온보딩/사용자 흐름 다이어그램
```
[앱 시작]
     ↓
[온보딩 화면]
     ↓
 ┌────────────┐
 │            │
↓로그인     회원가입↓
 │            │
 └──────┬─────┘
        ↓
    [홈 화면]
  ┌────┼────┬────┐
  ↓    ↓    ↓    ↓
[내 일정] [친구 목록] [그룹 목록]
   ↓         ↓            ↓
[일정 관리] [친구 추가/일정 조회] [그룹 생성/조율]
```


### MVP 릴리즈 기준 우선순위
🔹 1순위 (필수 기능)
 - [ ] 회원가입 / 로그인 (소셜 포함)

 - [ ] 내 일정 등록 / 수정 / 삭제

 - [ ] 친구 추가 및 일정 공유

 - [ ] 기본 그룹 생성 및 일정 조율 기능

 - [ ]  실시간 알림 (Firebase)

🔹 2순위 (중요 기능)
  - [ ] 그룹 내 일정 후보 등록 및 투표

  - [ ] 일정 자동 추천 로직

  - [ ] 친구 요청 관리 UI

🔹 3순위 (부가 기능)
  - [ ] 일정 반복 설정

  - [ ] 다크모드 / UX 개선

  - [ ] 푸시 알림 고도화

  - [ ] 프라이버시 설정 옵션
