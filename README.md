# 모드기어 베이스 앰프 모금 페이지 배포 안내

## 1. 계좌 정보 입력
`index.html` 하단 `<script>` 안의 `CONFIG`만 수정합니다.

```js
BANK: "카카오뱅크",
ACCOUNT: "3333-00-0000000",
HOLDER: "홍길동",
GOAL: 150000,     // 목표 금액(원). 0으로 바꾸면 진행률 막대를 숨깁니다.
FIREBASE_URL: ""  // 2단계에서 채움
```

## 2. 총액을 친구들과 공유하기 (Firebase, 무료)
GitHub Pages는 서버가 없는 정적 호스팅이므로, 이 단계를 건너뛰면 금액이 각자의 기기에만 저장됩니다.

1. https://console.firebase.google.com 에서 프로젝트 생성
2. 빌드 → Realtime Database → 데이터베이스 만들기 (위치: asia-southeast1 권장)
3. 규칙(Rules) 탭에 아래 내용 입력 후 게시

```json
{
  "rules": {
    "donations": {
      ".read": true,
      "$id": {
        ".write": "!data.exists()",
        ".validate": "newData.hasChildren(['amount','at']) && (newData.child('amount').val() == 5000 || newData.child('amount').val() == 10000 || newData.child('amount').val() == 20000 || newData.child('amount').val() == 30000)"
      }
    }
  }
}
```
   - 새 기록 추가만 허용하고, 기존 기록 수정·삭제와 정해진 4개 금액(5천·1만·2만·3만원) 외의 값은 막습니다.
4. 데이터 탭 상단의 URL(`https://...firebasedatabase.app`)을 복사해 `FIREBASE_URL`에 붙여넣기 (끝의 `/` 제외)

## 3. GitHub Pages 배포
1. GitHub에서 새 저장소 생성 (Public)
2. `index.html` 업로드 (Add file → Upload files → Commit)
3. Settings → Pages → Source: `Deploy from a branch`, Branch: `main` / `(root)` → Save
4. 1~2분 후 `https://<아이디>.github.io/<저장소이름>/` 주소를 친구들에게 공유

## 은행 앱 바로 열기
- 송금 확인 창의 은행 버튼은 각 은행 앱의 URL 스킴으로 앱만 실행합니다(계좌번호는 이미 복사된 상태라 붙여넣기만 하면 됨).
- 확인 출처: 토스·카카오뱅크·KB국민은 토스페이먼츠 공식 문서, 그 외는 공개 정리 자료(확실하지 않음 — 앱 업데이트로 바뀔 수 있음).
- 앱이 설치되어 있지 않으면 열리지 않으며, 1.8초 뒤 안내 메시지가 뜹니다.
- Claude 아티팩트 미리보기 안에서는 보안 제한으로 앱이 열리지 않을 수 있습니다. GitHub Pages 주소에서 사용하세요.

## 참고
- '예' 버튼은 실제 입금을 확인하지 않는 자기 신고 방식입니다. 실제 입금 내역과 가끔 대조해 주세요.
- 잘못 눌린 기록은 Firebase 콘솔 데이터 탭에서 직접 삭제할 수 있습니다.
- 공개 저장소이므로 계좌번호가 코드에 노출됩니다. 친구들에게만 링크를 공유하세요.
