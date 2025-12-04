# my1rm

## 기능 명세서

### 1RM 계산로직 (Epley 공식)

- w는 중량(kg), r은 반복 횟수
- 예외 처리 로직(input 밑에 빨간글씨로 안내)
- - w <= 0: 입력 불가
- - 1<= r <= 15 : 1회 반복은 그 자체가 1RM이므로 공식 미적용, 입력값 그대로 1RM 처리, 15회 이상은 정확도가 떨어짐

### 카드 부여

- 3대 운동 종목에 대해 모두 계산했을 경우, 3대 총합을 계산, 등급을 부여, 3대 총합과 등급 이미지를 사용해 카드 생성
- 등급
- - ~ 200kg: 아기 리프터
- - ~ 300kg: 멸치
- - ~ 400kg: 슬림탄탄
- - ~ 500kg: 마초맨
- - 500kg ~ : 케미컬

### 결과 공유 (Serverless Logic)

- 서버가 없으므로 데이터를 URL Query String에 압축하여 전달합니다.
- 생성 URL 예시: https://my1rm.com/?s=120&b=80&d=140&n=James
- - s: Squat 1RM
- - b: Bench 1RM
- - d: Deadlift 1RM
- - n: User Name

### 동작 원리

- 사용자가 '공유하기' 클릭
- 위 형식의 URL 생성 후 클립보드 복사.
- 타인이 URL 접속
- JS가 URL 파라미터를 파싱
- readonly 모드로 결과 카드 렌더링.

---

## 파일 구조

```
my1rm/
├── index.html # 메인 진입점 (Hub 역할)
├── assets/
│ ├── images/ # 등급별 이미지, 아이콘
│ └── favicon.ico
├── css/
│ ├── variables.css # 색상, 폰트, 사이즈 변수
│ ├── reset.css # 브라우저 기본 스타일 초기화
│ ├── style.css # 공통 레이아웃
│ └── components/ # 컴포넌트별 스타일
│ ├── carousel.css
│ ├── inputs.css
│ └── cards.css
└── js/
├── app.js # 메인 컨트롤러 (진입점)
├── core/
│ ├── router.js # URL 파싱 및 뷰 전환 관리
│ ├── store.js # LocalStorage 관리자
│ └── utils.js # 1RM 공식, 숫자 포맷팅 등 유틸 함수
└── modules/ # 확장 가능한 모듈 폴더
└── weight/ # [현재 구현] 3대 운동 모듈
├── calculator.js
└── ui.js # 3D 캐러셀 및 DOM 조작
```

---

## 데이터 스키마

- localStorage에는 JSON 문자열로 데이터를 저장합니다.
- 키 이름은 충돌 방지를 위해 네임스페이스를 사용합니다.
- Key: my1rm_user_data

```
{
"profile": {
"theme": "dark", // 'dark' | 'light'
"lang": "ko" // 'ko' | 'en'
},
"records": {
"squat": {
"weight": 100,
"reps": 5,
"1rm": 116,
"updatedAt": "2023-10-27T10:00:00Z"
},
"bench": {
"weight": 80,
"reps": 3,
"1rm": 88,
"updatedAt": "2023-10-27T10:05:00Z"
},
"deadlift": {
"weight": null, // 아직 입력 안함
"reps": null,
"1rm": 0,
"updatedAt": null
}
}
}
```

## 단계별 개발 로드맵

### 1일차, 기획 & 프로젝트 세팅

- [x] 기획서 작성
- [x] 피그마 작업
- [x] 파비콘 및 컬러 팔레트 확정
- [ ] Git Repo 생성 및 GitHub Pages 설정 (https://github.com/kudongku/my1rm.git)
- [ ] index.html, variable.css 기본 뼈대 작성
- [ ] Hello World 페이지 호스팅

### 2일차, Core 로직 구현

- [ ] utils.js: Epley 공식 구현
- [ ] store.js: LocalStorage 읽기/쓰기 구현
- [ ] Console에서 로직 테스트
- [ ] 기능 테스트 완료 (JS Only)

### 3일차, UI 기본 구조

- [ ] HTML 마크업 (시멘틱 태그 활용)
- [ ] Flexbox/Grid로 기본 레이아웃 잡기
- [ ] 3D Carousel CSS 기본 구현 (transform-style: preserve-3d)
- [ ] 레이아웃 잡힌 정적 페이지

### 4일차, 인터랙션 구현

- [ ] JS로 Carousel 터치/스와이프 이벤트 연결
- [ ] 입력 폼과 1RM 계산 로직 연결 (실시간 반영)
- [ ] 동작하는 계산기 (디자인 미흡)

### 5일차, 핵심 기능 완성

- [ ] 3대 중량 합산 로직 및 등급 카드 UI 구현
- [ ] URL 파라미터 파싱 및 공유 링크 생성 기능
- [ ] 공유 가능한 MVP

### 6일차, 디자인 폴리싱

- [ ] Neon Glow 효과, 애니메이션(부드러운 전환) 추가
- [ ] 모바일 반응형 디테일 수정
- [ ] 도메인 구입 및 연결 (CNAME)
- [ ] 디자인 완성본

### 7일차, 최종 점검 & 런칭

- [ ] 예외 케이스 테스트 (음수 입력, 모바일 키패드 이슈 등)
- [ ] 지인들에게 링크 공유 후 피드백 반영
- [ ] Product Hunt / 커뮤니티 홍보
- [ ] 서비스 런칭
