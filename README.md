![image](https://github.com/user-attachments/assets/bbb5af5b-c16e-4f65-ae87-141e49cf9d9b)

## 📱 [프로젝트명] iOS 앱 개발 (2024.12 – 2025.03)

### 📌 프로젝트 개요
- **프로젝트 소개**: 프로젝트 종료 후 팀원을 평가하여 객관적으로 자신을 알아가는 플렛폼
- **개발 기간**: 3개월 (iOS 개발 단독 담당)  
- **팀 구성**: iOS (김수은), 서버 (팀원 A), 디자인 (팀원 B)  
- **배포 링크**: [App Store 바로가기](https://apps.apple.com/kr/app/curve/id6740055690)  

---

### 📊 기여도 분포

| **구분**          | **비율**  | **담당자**    |
|-------------------|----------|--------------|
| iOS 개발         | 50%      | 김수은 (단독개발)  |
| 서버 개발        | 30%      | 팀원 A       |
| 디자인 및 기획   | 20%      | 팀원 B       |

---

### 📈 성과 및 결과

- 성능 최적화: 네트워크 요청 응답 속도 30% 향상
- 사용자 경험 개선: SwiftUI 최적화를 통해 앱 실행 속도 20% 향상
- 실제 프로젝트 모집 진행: 내부 테스트를 통해 10개 이상의 팀 프로젝트 성사

---

### 🔍 주요 기능

| **기능**              | **설명**                                          | **기술 스택**                  |
|-----------------------|-------------------------------------------------|---------------------------------|
| 🔍 **프로젝트 검색**   | 키워드로 프로젝트 검색, 필터 적용                 | SwiftData, SwiftUI             |
| 💬 **실시간 채팅**    | Socket.io 기반의 1:1 및 그룹 채팅 구현             | Socket.io, Combine, MVVM       |
| 📱 **푸시 알림**     | FCM 기반 알림 구현 (새 메시지, 공지사항)             | Firebase, UserNotifications    |
| 🛠️ **환경 설정**     | 사용자의 불편사항 건의 화면 구현                 | SwiftUI, WebKit |

---

### 앱 화면
#### 1. 로그인 및 회원가입
![image](https://github.com/user-attachments/assets/8225a9e8-603a-4e18-8f15-e95b1f463add)
#### 2. 프로젝트 모집 및 라운지 글 작성 & 프로필
![image](https://github.com/user-attachments/assets/cf16e594-465b-400c-9e07-295ea5948afb)
#### 3. 프로젝트 신청 / 관리 및 채팅 화면
![image](https://github.com/user-attachments/assets/f48cda18-849a-4eb1-b569-2c00ae7919ff)

---

### 🔥 기술적 도전과 해결 과정

#### ✅ 1. **실시간 채팅 구현 (Socket.io 연동)**
- **문제**: 서버와의 비동기 통신 지연으로 메시지 수신이 늦어지는 문제 발생  
- **해결**: Combine을 활용한 이벤트 스트림 처리 → 지연 시간 **40% 단축**  
- **성과**: 동시 접속자 1,000명 이상에서도 안정적으로 실시간 채팅 구현  

#### ✅ 2. **로컬 데이터 관리 (SwiftData 사용)**
- **문제**: 서버에서 받아온 대용량 데이터를 오프라인에서도 활용해야 함  
- **해결**: SwiftData를 도입해 캐싱 전략 구현 → API 호출 **60% 감소**  

---


### 💻 주요 코드 예제

#### ✅ **실시간 채팅 메시지 수신 코드 (Socket.io 연동)**

```swift
class ChatViewModel: ObservableObject {
    private var socket: SocketIOClient
    @Published var messages: [Message] = []
    
    init(socket: SocketIOClient) {
        self.socket = socket
        configureSocketEvents()
    }
    
    func configureSocketEvents() {
        socket.on("newMessage") { [weak self] data, _ in
            guard let self = self, let messageData = data.first as? [String: Any] else { return }
            if let message = Message.fromJSON(messageData) {
                DispatchQueue.main.async {
                    self.messages.append(message)
                }
            }
        }
    }
}
