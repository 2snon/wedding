# 이준원 · 박소윤 모바일 청첩장

GitHub Pages로 배포하는 모바일 청첩장입니다.

---

## 1. GitHub Pages로 배포하기

1. GitHub에서 새 저장소(Repository)를 만듭니다.
   - 이름은 자유롭게 (예: `wedding-invitation`)
   - Public으로 설정 (Private은 GitHub Pages 무료 플랜에서 제한될 수 있어요)
2. 이 폴더 안의 파일들(`index.html`, `images` 폴더, `README.md`)을 저장소에 업로드합니다.
   - GitHub 웹사이트에서 "Add file → Upload files"로 드래그해서 올려도 되고,
   - `git` 명령어를 쓸 줄 아시면 아래처럼 하셔도 됩니다.
     ```bash
     git init
     git add .
     git commit -m "청첩장 초기 업로드"
     git branch -M main
     git remote add origin https://github.com/{아이디}/{저장소이름}.git
     git push -u origin main
     ```
3. 저장소 페이지에서 **Settings → Pages** 로 이동합니다.
4. "Build and deployment" 항목에서
   - Source: `Deploy from a branch`
   - Branch: `main` / 폴더: `/ (root)` 선택 후 Save
5. 몇 분 기다리면 아래 형태의 주소로 청첩장이 열립니다.
   ```
   https://{아이디}.github.io/{저장소이름}/
   ```

> 카카오톡 공유 등을 위해 주소를 더 짧게 만들고 싶다면, 네이버 me2.do, bit.ly 같은
> 무료 단축 URL 서비스를 이용하시면 됩니다.

---

## 2. 나중에 사진 추가하는 방법

지금은 `index.html` 안에 사진이 들어갈 자리마다 "사진을 준비 중입니다" 같은
점선 박스(placeholder)가 표시되어 있습니다. 사진이 준비되면 아래 두 군데를 수정하면 됩니다.

### (1) 사진 파일 넣기
`images` 폴더 안에 사용할 사진 파일들을 넣어주세요.
예:
```
images/cover.jpg      (표지 사진)
images/gallery1.jpg   (갤러리 사진 1 - 가로로 넓은 사진 추천)
images/gallery2.jpg   (갤러리 사진 2)
images/gallery3.jpg   (갤러리 사진 3)
```

### (2) index.html에서 플레이스홀더를 사진으로 교체

**표지 사진** — 아래 부분을 찾아서:
```html
<div class="cover-photo">
  <div class="cover-eyebrow">Wedding Invitation</div>
  <div class="cover-photo-label">
    <svg ...>...</svg>
    사진을 준비 중입니다
  </div>
</div>
```
이렇게 바꿔주세요:
```html
<div class="cover-photo">
  <div class="cover-eyebrow">Wedding Invitation</div>
  <img src="images/cover.jpg" alt="커버 사진" style="width:100%;height:100%;object-fit:cover;">
</div>
```

**갤러리 사진** — 아래처럼 생긴 블록 3개를 찾아서 (`onclick` 속성은 사진을 눌렀을 때
스와이프로 넘겨보는 기능이니 그대로 남겨두세요):
```html
<div class="ph wide" onclick="openLightbox(0)">
  <svg ...>...</svg>
  사진을 준비 중입니다
</div>
```
이렇게 바꿔주세요:
```html
<div class="ph wide" onclick="openLightbox(0)">
  <img src="images/gallery1.jpg" alt="갤러리 사진 1">
</div>
```
나머지 두 개(`onclick="openLightbox(1)"` / `openLightbox(2)`)도 같은 방식으로
`gallery2.jpg`, `gallery3.jpg`로 바꿔주시면 됩니다. `onclick`의 숫자(0, 1, 2)는
그대로 유지해주세요.

> 사진은 가로/세로 비율을 미리 맞춰서 넣으시면 더 깔끔합니다.
> - 표지 사진: 3:4 비율 (세로로 긴 사진)
> - 갤러리 큰 사진(wide): 16:10 비율 (가로로 넓은 사진)
> - 갤러리 작은 사진 2장: 3:4 비율

**사진을 클릭하면** 화면 전체에 크게 보이면서, 손가락으로 좌우로 밀어(스와이프)
사진 3장을 순서대로 넘겨볼 수 있습니다. 파일명만 `images/gallery1.jpg`,
`gallery2.jpg`, `gallery3.jpg`로 맞춰주면 별도 코드 수정 없이 바로 작동해요.
(다른 파일명을 쓰고 싶다면 `index.html` 안의 `galleryImages` 배열 값을 바꿔주세요.)

---

## 3. 오시는 길 지도

보내주신 약도 이미지를 `images/map.jpg`로 저장해서 "오시는 길" 섹션에 바로
연결해뒀습니다. 나중에 지도를 다른 이미지로 바꾸고 싶으시면 `images/map.jpg`
파일을 새 이미지로 덮어쓰기만 하면 됩니다 (파일명은 그대로 유지).

---

## 4. 방명록 설정 방법 (Firebase)

방명록은 방문자들이 남긴 글이 서로에게 공유되어 보여야 하기 때문에, 무료
백엔드 서비스인 **Firebase(Firestore)** 를 연결해두었습니다. 아래 순서대로
한 번만 설정해주시면 됩니다. (전부 무료 범위 안에서 사용 가능합니다.)

1. [Firebase 콘솔](https://console.firebase.google.com/)에 구글 계정으로 로그인 후
   **프로젝트 추가**를 눌러 새 프로젝트를 만듭니다. (Google Analytics는 꺼두셔도 됩니다.)
2. 왼쪽 메뉴 **빌드 → Firestore Database** 로 이동해 **데이터베이스 만들기**를 클릭합니다.
   - 위치는 `asia-northeast3(서울)` 선택을 추천드립니다.
   - 보안 규칙은 우선 **테스트 모드**로 시작합니다.
3. 왼쪽 메뉴 상단의 **⚙️(프로젝트 설정)** 클릭 → 아래로 스크롤 → "내 앱"에서
   **웹 아이콘(</>)** 을 눌러 웹 앱을 등록합니다. (앱 닉네임은 자유롭게)
4. 등록하면 아래와 같은 `firebaseConfig` 값이 나옵니다. 이 값을 통째로 복사해서
   `index.html` 안의 `const firebaseConfig = { ... }` 부분을 찾아 그대로 교체해주세요.
   ```js
   const firebaseConfig = {
     apiKey: "...",
     authDomain: "...",
     projectId: "...",
     storageBucket: "...",
     messagingSenderId: "...",
     appId: "..."
   };
   ```
5. Firestore Database → **규칙(Rules)** 탭에서 아래 규칙으로 바꾸고 **게시**를 눌러주세요.
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /guestbook/{entryId} {
         allow read, create: if true;
         allow update, delete: if true;
       }
     }
   }
   ```
6. 저장 후 GitHub Pages에 다시 업로드(재배포)하면 방명록이 정상적으로 작동합니다.

> 방명록 글마다 4자리 비밀번호를 입력하게 되어 있고, 삭제 시 같은 비밀번호를
> 입력해야 지워집니다. 다만 완전한 보안 방식은 아니므로 민감한 내용은 남기지
> 않도록 안내해주시는 게 좋아요.
>
> Firebase 설정을 아직 하지 않은 상태에서는 방명록 영역에 "Firebase 설정이
> 필요합니다"라는 안내만 표시되고, 청첩장의 나머지 기능은 정상적으로 작동합니다.

---

## 5. 바꿔야 할 정보 체크리스트

파일을 열어서 아래 항목들이 실제 정보와 맞는지 확인해주세요.

- [ ] 예식 날짜/시간 (`countdown` 스크립트의 `target` 날짜도 함께 수정 필요)
- [ ] 예식장 이름 및 주소, 지도 좌표(`openNaverMap` 함수의 위도/경도)
- [ ] 주차장 안내 정보
- [ ] 신랑·신부 연락처 (전화번호)
- [ ] 계좌번호
- [ ] 양가 부모님 성함
- [ ] 방명록용 Firebase 설정 (위 "4. 방명록 설정 방법" 참고)

---

## 6. 로컬에서 미리보기

인터넷에 올리기 전에 내 컴퓨터에서 먼저 확인하고 싶다면, `index.html` 파일을
더블클릭해서 웹 브라우저로 열면 됩니다. (인터넷 연결 없이도 대부분 정상적으로 보입니다.)
