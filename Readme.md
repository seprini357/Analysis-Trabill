# [Analysis] Trabill
다인원 여행 경비 지출 관리 앱
![image](https://github.com/user-attachments/assets/d613cc5c-2c76-4154-93d8-1b78570d1342)

---

# 1. Introduction 
본 문서는 Conceptualiztion 단계에서 정의된 내용을 기반으로 시스템의 요구사항과 목표를 구체화하는 Analysis 단계의 문서이다.

## 1.1 Summary
최근 여행 증가로 인하여 교통비, 숙박비, 식비 등 다양한 비용을 여러 사람이 나누어 결제하는 상황이 증가하고 있다.
그러나 각 지출마다 참여 인원이 다르고 결제 방식이 다양해지면서 비용 정산 과정이 복잡해지는 문제가 발생한다. 또한 지출 내역이
누적될 수록 수기로 기록하는 과정에서 시간 소요가 많이 되며 일부 지출 내역이 누락되거나 계산 오류가 발생할 가능성이 존재한다.
이에 본 시스템은 여행 중 발생하는 지출 내역을 효육적으로 기록하고 관리하며 각자의 사용자마다 자동 정산 기능을 통해 사용자 간의
투명하고 정확한 비용 분담을 지원하는 앱이다. 이를 통해 사용자는 다인원의 여행일 경우에도 여행 경비 관리를 편리하게 사용할 수 
있을 것으로 기대한다.

## 1.2 Business Goals
- 사용자가 여행 중 발생하는 지출 내역을 쉽게 기록할 수 있어야한다.
- 그룹 단위로 경비를 공유하고 참여자 간 비용 분담을 명확하게 확인할 수 있어야한다.
- 지출 내역을 실시간으로 공유하여 모든 참여자가 동일한 정보를 확인할 수 있어야한다.
- 자동 정산 기능을 통해 복잡한 계산 없이 정확한 정산 결과를 제공해야한다.
- 주요 이벤트 발생 시 알림 기능을 통해 사용자에게 정보를 전달할 수 있어야한다.

## 1.3 Technical Goals
- 사용자 입력을 기반으로 지출 데이터를 안정적으로 저장하고 관리할 수 있어야한다.
- 참여 인원 및 지출 정보를 바탕으로 정확한 정산 알고리즘을 수행할 수 있어야한다.
- 그룹 단위 데이터 관리가 가능하도록 데이터 구조를 설계해야한다.
- 직관적인 UI/UX를 통해 사용자가 쉽게 기능을 사용할 수 있도록 해야한다.
- 시스템은 실시간 데이터 처리 및 빠른 응답 속도를 제공하여 사용자 경험을 저해하지 않아야한다.

---

# 2. Use case analysis
## 2.1 Use case Diagram
Conceptualization에서 System context diagram과 Use case list를 참조하여 Use 
case diagram을 다음과 같이 작성하였다. 모델링 도구는 Star UML을 사용하였으며 
Actor와 Use case들과의 관계를 나타내었다.

<img width="871" height="676" alt="UseCaseDiagram1" src="https://github.com/user-attachments/assets/e9e8d10d-9b4e-4ef1-8165-f071ff5c0de2" />

사용자(User)를 중심으로 회원가입, 로그인, 그룹 생성 및 참여, 지출 관리, 정산 결과 조회 기능 등을 정의하였다. 지출 등록, 수정, 삭제 및 정산 요청과 같은 주요 이벤트 발생 시 알림을 제공하기 위해 Notification 기능을 포함하였다. 해당 알림 기능은 공통적으로 수행되는 기능으로 <<include>> 관계로 표현하였다. Concpetualization에서 정산 계산 use case가 있었지만 시스템 내부로 간주하여 use case로 분리하지 않았다. 또한 알림 기능은 외부 시스템인 Notification Service와의 연동을 통해 알림을 받도록 구성하였다.

## 2.2 Use case Description
### Use case #1: Sign up

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 Trabill 서비스를 사용하기 위해 회원 계정을 생성하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Last Update |  |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 시스템이 실행되어야한다. |
| Trigger | 사용자가 로그인 화면에서 회원가입 버튼을 눌렀을 때 |
| Success Post Condition | 새로운 회원 계정이 생성되어 로그인할 수 있다. |
| Failed Post Condition | 회원가입이 실패하여 계정이 생성되지 않는다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 회원가입을 한다. |
| 1 | 사용자가 이메일, 닉네임, 비밀번호, 계좌번호등의 회원 정보를 입력한다. |
| 2 | 사용자가 회원가입 버튼을 누른다. |
| 3 | 시스템은 입력값의 유효성을 검사한다. |
| 4 | 시스템은 회원 정보를 DB에 저장한다. |
| 5 | 회원가입이 완료되면 성공 메세지를 띄우고 로그인 화면으로 이동하여 use case가 끝난다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 1 | 1a. 입력값이 비어있는 경우 <br><br> ...1a.1. 시스템은 “모든 정보를 입력해주세요.” 메시지를 출력한다. <br> ...1a.2. 사용자는 정보를 다시 입력한다. <br> ...1a.3. 이메일 형식이 올바르지 않은 경우 <br><br> ...1a.4. 시스템은 “올바른 이메일 형식을 입력해주세요.” 메시지를 출력한다. ...1a.5. 이미 가입된 이메일인 경우 <br><br> ...1a.6. 시스템은 “이미 존재하는 계정입니다.” 메시지를 출력한다.|
| RELATED INFORMATION |  |
| Performance | ≤ 5초 |
| Frequency | 신규 사용자 가입 시 |
| Concurrency |  |
| Due Date |  |

### Use case #2 : Login

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 Trabill을 사용하기 위해 회원 인증을 받기 위한 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Last Update |  |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 사용자가 회원가입을 완료한 상태여야 한다. |
| Trigger | 사용자가 로그인을 하기 위해 이메일과 비밀번호를 입력한 후 회원 인증을 받으려고 할 때 |
| Success Post Condition | 사용자가 인증되어 메인 화면으로 이동한다. |
| Failed Post Condition | 로그인에 실패하면 사용 허가를 받지 못하여 서비스를 사용할 수 없다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 앱 시스템에 로그인한다. |
| 1 | 사용자가 이메일과 비밀번호를 입력하고 로그인 버튼을 클릭한다. |
| 2 | 시스템 DB에 등록된 회원인지 확인하고 등록된 회원이라면 로그인에 성공한다. |
| 3 | 로그인이 성공하면 시스템은 메인화면으로 이동하여 use case가 끝난다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 1 | 1a. 이메일 또는 비밀번호가 입력되지 않은 경우 <br><br> ...1a.1. 시스템은 “이메일과 비밀번호를 입력해주세요.” 메시지를 출력한다. <br> ...1a.2. 사용자는 로그인 화면으로 돌아간다. |
| 2 | 2a. 등록되지 않은 이메일인 경우 <br><br> ...2a.1. 시스템은 “존재하지 않는 계정입니다.” 메시지를 출력한다. <br> ...2a.2. 사용자는 로그인 화면으로 돌아간다. |
| 3 | 3a. 비밀번호가 일치하지 않는 경우 <br><br> ...3a.1. 시스템은 “비밀번호가 올바르지 않습니다.” 메시지를 출력한다. <br> ...3a.2. 사용자는 비밀번호를 다시 입력한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 5초 |
| Frequency |  |
| Concurrency | 제한 없음|
| Due Date |  |

### Use case #3 : Create Group

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 여행 정산을 위한 그룹을 생성하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 로그인 상태로 '다같이 여행해요'를 선택했을 경우 |
| Trigger | 사용자가 그룹 생성 버튼을 눌렀을 때 |
| Success Post Condition | 새로운 여행 그룹이 생성된다. |
| Failed Post Condition | 그룹 생성이 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 새로운 여행 그룹을 생성한다. |
| 1 | 사용자가 그룹 생성 버튼을 누른다. |
| 2 | 새로운 여행 그룹을 생성할 수 있는 화면으로 전환된다. |
| 3 | 사용자가 그룹 이름을 설정할 수 있는 입력칸에 이름을 입력한다. |
| 4 | 사용자가 멤버 초대하기 버튼을 누른다. |
| 5 | 멤버 초대하기 버튼을 누르면 친구 초대 팝업창이 뜬다. |
| 6 | 시스템에 있는 다른 사용자의 이메일을 눌러 친구를 초대한다. |
| 7 | 다른 사용자가 수용 시 멤버 리스트에 해당 사용자의 이름과 계좌번호가 추가된다. |
| 8 | 하단에 그룹 만들기 버튼을 누른다. |
| 9 | 등록 절차가 끝나면 DB에 정보가 등록된다. |
| 10 | 그룹 만들기에 성공 시에 메세지로 성공했음을 알린다. |
| 11 | 그룹 리스트가 있는 화면으로 전환되며 해당 use case는 끝이 난다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 3 | 3a. 그룹 이름이 비어있는 경우 <br><br> ...3a.1. 시스템은 “그룹 이름을 입력해주세요.” 메시지를 출력한다. |
| 5 | 5a. 팝업창이 뜨지 않는 경우 <br><br> ...5a.1. 시스템은 “에러가 발생하였습니다.” 메시지를 출력한다. |
| 6 | 6a. 시스템에 등록되지 않는 사용자의 이메일를 입력할 경우 <br><br> ...6a.1. 시스템은 “등록되지 않은 회원입니다.” 메시지를 출력한다. 6b. 잘못된 이메일 양식을 입력할 경우 6b.1 시스템은 "잘못된 양식입니다" 메시지를 출력한다.  6c. 이메일을 입력하지 않을 경우 6c.1 시스템은 "이메일을 입력해주세요" 메시지를 출력한다.|
| RELATED INFORMATION |  |
| Performance | ≤ 5초 |
| Frequency |  |
| Concurrency |  |
| Due Date |  |

### Use case #4 : Join Group

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 초대 코드를 통해 그룹에 참여하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 참여 가능한 그룹 코드가 존재해야 한다. |
| Trigger | 사용자가 그룹 참여 초대를 받았을 때 |
| Success Post Condition | 사용자가 그룹에 참여한다. |
| Failed Post Condition | 그룹 참여에 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 초대받은 여행 그룹에 참여한다. |
| 1 | 시스템은 초대받은 사용자의 이메일로 그룹 초대 정보를 제공한다. |
| 2 | 사용자는 그룹 참여 화면에 접근한다. |
| 3 | 시스템은 초대 코드 입력창을 제공한다. |
| 4 | 사용자는 전달받은 초대 코드를 입력한다. |
| 5 | 사용자는 참여하기 버튼을 누른다. |
| 6 | 시스템은 입력된 초대 코드가 존재하는지 확인한다. |
| 7 | 시스템은 초대 코드가 해당 사용자에게 발급된 코드인지 확인한다. |
| 8 | 시스템은 사용자를 해당 그룹의 멤버로 등록한다. |
| 9 | 시스템은 그룹 참여 완료 메시지를 출력한다. |
| 10 | 시스템은 사용자를 그룹 리스트 화면으로 이동시키고 참여한 그룹을 표시한다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 4 | 4a. 초대 코드를 입력하지 않은 경우 <br><br> ...4a.1. 시스템은 “초대 코드를 입력해주세요.” 메시지를 출력한다. <br> ...4a.2. 사용자는 초대 코드를 다시 입력한다. |
| 6 | 6a. 존재하지 않는 초대 코드인 경우 <br><br> ...6a.1. 시스템은 “유효하지 않은 코드입니다.” 메시지를 출력한다. <br> ...6a.2. 사용자는 그룹 리스트 화면으로 돌아간다. |
| 7 | 7a. 해당 사용자에게 발급된 초대 코드가 아닌 경우 <br><br> ...7a.1. 시스템은 “참여 권한이 없는 초대 코드입니다.” 메시지를 출력한다. <br> ...7a.2. 사용자는 그룹 참여에 실패한다. |
| 8 | 8a. 이미 참여 중인 그룹인 경우 <br><br> ...8a.1. 시스템은 “이미 참여한 그룹입니다.” 메시지를 출력한다. <br> ...8a.2. 시스템은 해당 그룹 화면으로 이동한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 5초 |
| Frequency | |
| Concurrency | |
| Due Date | |

### Use case #5 : Add Expense

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 여행 경비 지출 내역을 등록하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 그룹에 참여한 상태여야 한다. |
| Trigger | 사용자가 정산 비용 추가하기 버튼을 클릭할 때 |
| Success Post Condition | 지출 내역이 저장된다. |
| Failed Post Condition | 지출 등록에 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 참여 중인 여행 그룹에 지출 내역을 등록한다. |
| 1 | 사용자가 그룹 가계부 화면에서 정산 비용 추가하기 버튼을 누른다. |
| 2 | 시스템은 비용 추가하기 화면으로 이동한다. |
| 3 | 시스템은 결제 금액, 지출 내용 메모, 증빙자료, 참여 멤버 입력 항목 등을 제공한다. |
| 4 | 사용자는 지출 금액을 입력한다. |
| 5 | 사용자는 지출 내용을 메모한다. |
| 6 | 사용자는 증빙 자료 추가 버튼을 눌러 카메라나 사진첩을 통해 영수증을 올린다. |
| 7 | 사용자는 해당 지출을 결제한 사람을 선택한다. |
| 8 | 사용자는 추가하기 버튼을 누른다. |
| 9 | 시스템은 입력된 지출 정보의 유효성을 검사한다. |
| 10 | 시스템은 정산 정보를 DB에 저장한다. |
| 11 | 시스템은 그룹 멤버들에게 지출 등록 알림을 전송한다. |
| 12 | 시스템은 그룹 가계부 화면으로 이동하고 새로 등록된 지출 내역을 표시한다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 4 | 4a. 정산 금액이 입력되지 않은 경우 <br><br> ...4a.1. 시스템은 “금액을 입력해주세요.” 메시지를 출력한다. <br>...4a.2. 사용자는 금액을 다시 입력한다. <br><br>4b. 금액이 숫자가 아니거나 0 이하인 경우 <br>...4b.1. 시스템은 “올바른 금액을 입력해주세요.” 메시지를 출력한다.|
| 5 | 5a. 정산 내용이 입력되지 않은 경우 <br><br> ...5a.1. 시스템은 “정산 내용을 입력해주세요.” 메시지를 출력한다. |
| 6 | 6a. 사용자가 사진 업로드를 취소한 경우 <br><br> ...6a.1. 시스템은 기존 정산 등록 화면을 유지한다. <br> ...6a.2. 사용자는 영수증 없이 정산 등록을 계속 진행할 수 있다. |
| 6 | 6b. 지원하지 않는 파일 형식의 이미지를 업로드한 경우 <br><br> ...6b.1. 시스템은 “지원하지 않는 파일 형식입니다.” 메시지를 출력한다. <br> ...6b.2. 사용자는 다른 이미지를 다시 선택한다. |
| 6 | 6c. 이미지 업로드 용량이 제한을 초과한 경우 <br><br> ...6c.1. 시스템은 “업로드 가능한 파일 크기를 초과했습니다.” 메시지를 출력한다. <br> ...6c.2. 사용자는 다른 이미지를 선택한다. |
| 6 | 6d. 카메라 또는 사진첩 접근 권한이 허용되지 않은 경우 <br><br> ...6d.1. 시스템은 “카메라 및 사진 접근 권한이 필요합니다.” 메시지를 출력한다. <br> ...6d.2. 사용자는 권한 설정 화면으로 이동하거나 권한 요청을 다시 시도한다. |
| 6 | 6e. 이미지 업로드 중 네트워크 오류가 발생한 경우 <br><br> ...6e.1. 시스템은 “이미지 업로드 중 오류가 발생했습니다.” 메시지를 출력한다. <br> ...6e.2. 사용자는 업로드를 다시 시도한다. |
| 10 | 10a. DB 저장 중 오류가 발생한 경우 <br><br> ...10a.1. 시스템은 “정산 등록 중 오류가 발생했습니다.” 메시지를 출력한다. <br> ...10a.2. 시스템은 정산 비용 추가하기 화면을 유지한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 10초 |
| Frequency | |
| Concurrency | |
| Due Date | |

### Use case #6 : Edit Expenses

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 등록한 정산 내역을 수정하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 수정 가능한 정산 내역이 존재해야 한다. |
| Trigger | 사용자가 수정 버튼을 클릭할 때 |
| Success Post Condition | 정산 정보가 수정된다. |
| Failed Post Condition | 정산 정보 수정에 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 등록한 정산 내역을 수정한다. |
| 1 | 사용자가 수정 버튼을 누른다. |
| 2 | 사용자는 수정할 정산 항목을 선택한다. |
| 3 | 시스템은 기존 지출 정보가 입력된 수정 화면을 제공한다. |
| 4 | 사용자는 금액, 내용, 결제자 정보를 수정한다. |
| 5 | 사용자는 저장 버튼을 누른다. |
| 6 | 시스템은 수정된 정보의 유효성을 검사한다. |
| 7 | 시스템은 수정된 지출 정보를 DB에 반영한다. |
| 8 | 시스템은 그룹 멤버들에게 지출 수정 알림을 전송한다. |
| 9 | 시스템은 수정된 지출 상세 화면을 다시 표시한다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 1 | 1a. 수정 권한이 없는 경우 <br><br> ...1a.1. 시스템은 “수정 권한이 없습니다.” 메시지를 출력한다. |
| 4 | 4a. 필수 입력값이 비어있는 경우 <br><br> ...4a.1. 시스템은 “필수 정보를 모두 입력해주세요.” 메시지를 출력한다. |
| 4 | 4b. 금액 형식이 올바르지 않은 경우 <br><br> ...4b.1. 시스템은 “올바른 금액을 입력해주세요.” 메시지를 출력한다. |
| 7 | 7a. DB 수정 중 오류가 발생한 경우 <br><br> ...7a.1. 시스템은 “정산 내용 수정 중 오류가 발생했습니다.” 메시지를 출력한다. <br> ...7a.2. 시스템은 수정 화면을 유지한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 5초 |
| Frequency | |
| Concurrency | |
| Due Date | |

### Use case #7 : Delete Expenses

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 정산 내역을 삭제하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 삭제 가능한 정산 내역이 존재해야 한다. |
| Trigger | 사용자가 삭제 버튼을 클릭할 때 |
| Success Post Condition | 지출 내역이 삭제된다. |
| Failed Post Condition | 삭제에 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 정산 내역을 삭제한다. |
| 1 | 사용자가 삭제 버튼을 클릭한다. |
| 2 | 사용자는 삭제할 정산 항목을 선택한다. |
| 3 | 시스템은 삭제 여부를 재확인한다. |
| 4 | 사용자가 삭제를 승인한다. |
| 5 | 시스템은 정산 정보를 삭제한다. |
| 6 | 시스템은 해당 정산 내역을 DB에서 삭제한다. |
| 7 | 그룹 사용자에게 삭제 알림을 전송한다. |
| 8 | 시스템은 삭제된 정산 내역을 목록에서 제거하여 표시한다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 1 | 1a. 사용자가 삭제를 취소한 경우 <br><br> ...1a.1. 시스템은 삭제를 수행하지 않는다. |
| 4 | 4a. 사용자가 삭제를 취소한 경우 <br><br> ...4a.1. 시스템은 삭제를 수행하지 않는다. |
| 6 | 6a. DB 삭제 중 오류가 발생한 경우 <br><br> ...6a.1. 시스템은 “정산 정보 삭제 중 오류가 발생했습니다.” 메시지를 출력한다. <br> ...7a.2. 시스템은 그룹 가계부 화면을 유지한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 3초 |
| Frequency | |
| Concurrency | |
| Due Date | |

### Use case #8 : View Expenses

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 그룹의 정산 내역을 확인하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 그룹에 정산 내역이 존재해야 한다. |
| Trigger | 사용자가 그룹 가계부 화면에 접속할 때 |
| Success Post Condition | 지출 내역이 화면에 표시된다. |
| Failed Post Condition | 조회에 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 그룹의 정산 내역을 확인한다. |
| 1 | 시스템은 DB에서 해당 그룹에 저장된 정산 내역을 조회한다. |
| 2 | 시스템은 정산 목록을 화면에 출력한다. |
| 3 | 사용자는 정산 상세 내역을 확인한다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 1 | 1a. 저장된 정산 내역이 없는 경우 <br><br> ...1a.1. 시스템은 “등록된 정산 내역이 없습니다.” 메시지를 출력한다. |
| 2 | 2a. 정산 데이터를 불러오지 못한 경우 <br><br> ...2a.1. 시스템은 “정산 내역을 불러오지 못했습니다.” 메시지를 출력한다. <br> ...2a.2. 사용자는 새로고침을 시도한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 3초 |
| Frequency | |
| Concurrency | |
| Due Date | |

### Use case #9 : Request Settlement

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 정산을 요청하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 그룹 내 정산 가능한 데이터가 존재해야 한다. |
| Trigger | 사용자가 정산하기 버튼을 눌렀을 때 |
| Success Post Condition | 정산 결과가 생성된다. |
| Failed Post Condition | 정산 요청에 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 여행 그룹의 지출 내역을 기준으로 정산을 요청한다. |
| 1 | 사용자가 그룹 가계부 화면에서 정산하기 버튼을 클릭한다. |
| 2 | 시스템은 그룹의 지출 데이터를 DB에서 조회한다. |
| 3 | 시스템은 각 지출 내역의 결제자와 참여 멤버 정보를 확인한다. |
| 4 | 시스템은 사용자별 총 결제 금액을 계산한다. |
| 5 | 시스템은 사용자별 실제 부담해야 할 금액을 계산한다. |
| 6 | 시스템은 사용자 간 송금해야 할 금액과 대상을 계산한다. |
| 7 | 시스템은 계산된 정산 결과를 DB에 저장한다. |
| 8 | 시스템은 정산 결과 생성 완료 메시지를 출력한다. |
| 9 | 시스템은 그룹 멤버들에게 정산 요청 및 결과 알림을 전송한다. |
| 10 | 시스템은 사용자를 정산 결과 화면으로 이동시킨다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 2 | 2a. 그룹 내 정산 데이터가 존재하지 않는 경우 <br><br> ...2a.1. 시스템은 “정산할 내역이 없습니다.” 메시지를 출력한다. <br> ...2a.2. 시스템은 정산 요청을 중단한다. |
| 3 | 3a. 정산 내역에 참여 멤버 정보가 누락된 경우 <br><br> ...3a.1. 시스템은 “멤버가 누락되었습ㄴ니다.” 메시지를 출력한다. <br> ...3a.2. 사용자는 해당 지출 내역을 수정한다. |
| 6 | 6a. 정산 금액 계산 중 오류가 발생한 경우 <br><br> ...6a.1. 시스템은 “정산 계산 중 오류가 발생했습니다.” 메시지를 출력한다. <br> ...6a.2. 시스템은 정산 요청을 중단한다. |
| 7 | 7a. 정산 결과 저장 중 오류가 발생한 경우 <br><br> ...7a.1. 시스템은 “정산 결과 저장에 실패했습니다.” 메시지를 출력한다. <br> ...7a.2. 사용자는 다시 정산 요청을 시도한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 10초 |
| Frequency | |
| Concurrency | |
| Due Date | |

### Use case #10 : View Settlement Results

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 정산 결과를 조회하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 올바른 정산 결과가 생성되어 있어야 한다. |
| Trigger | 사용자가 정산 결과 조회 화면에 접근할 때 |
| Success Post Condition | 정산 결과가 화면에 표시된다. |
| Failed Post Condition | 조회에 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 정산 확인하기 화면에서 보낼 금액과 받을 금액을 확인한다. |
| 1 | 사용자가 그룹 가계부 화면에서 정산하기 버튼을 선택한다. |
| 2 | 시스템은 해당 그룹의 정산 결과 데이터를 조회한다. |
| 3 | 시스템은 사용자가 보내야 하는 금액 리스트를 보낼 금액 영역에 표시한다. |
| 4 | 시스템은 사용자가 받아야 하는 금액 리스트를 받을 금액 영역에 표시한다. |
| 5 | 각 리스트에는 상대방의 이름, 계좌번호, 비용이 함께 표시된다. |
| 6 | 사용자는 보낼 금액 리스트에서 송금이 완료된 내역은 보냄 버튼을 누른다. |
| 7 | 시스템은 해당 항목의 송금 완료 상태를 저장한다. |
| 8 | 사용자는 받을 금액 리스트에서 입금이 확인된 항목의 받음 버튼을 누른다. |
| 9 | 시스템은 해당 항목의 입금 확인 상태를 저장한다. |
| 10 | 시스템은 해당 비용과 관련된 사용자들의 보냄/받음 상태를 확인한다. |
| 11 | 관련 사용자들이 모두 보냄 또는 받음 버튼을 누른 경우 시스템은 해당 정산 항목을 완료 상태로 변경한다. |
| 12 | 시스템은 해당 항목의 상태를 완료로 표시한다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 2 | 2a. 정산 결과가 존재하지 않는 경우 <br><br> ...2a.1. 시스템은 “정산 결과가 존재하지 않습니다.” 메시지를 출력한다. |
| 3 | 3a. 사용자가 보낼 금액이 없는 경우 <br><br> ...3a.1. 시스템은 보낼 금액 영역에 “보낼 금액이 없습니다.” 메시지를 출력한다. |
| 4 | 4a. 사용자가 받을 금액이 없는 경우 <br><br> ...4a.1. 시스템은 받을 금액 영역에 “받을 금액이 없습니다.” 메시지를 출력한다. |
| 6 | 6a. 사용자가 보냄 버튼을 잘못 누른 경우 <br><br> ...6a.1. 시스템은 “송금 완료로 표시하시겠습니까?” 확인 메시지를 출력한다. <br> ...6a.2. 사용자가 취소하면 상태를 변경하지 않는다. |
| 8 | 8a. 사용자가 받음 버튼을 잘못 누른 경우 <br><br> ...8a.1. 시스템은 “입금 완료로 표시하시겠습니까?” 확인 메시지를 출력한다. <br> ...8a.2. 사용자가 취소하면 상태를 변경하지 않는다. |
| 10 | 10a. 관련 사용자 중 일부가 아직 보냄 또는 받음 버튼을 누르지 않은 경우 <br><br> ...10a.1. 시스템은 해당 정산 항목을 완료로 변경하지 않는다. <br> ...10a.2. 시스템은 기존 보냄 또는 받음 상태를 유지한다. |
| 11 | 11a. 정산 상태 저장 중 오류가 발생한 경우 <br><br> ...11a.1. 시스템은 “정산 상태 저장 중 오류가 발생했습니다.” 메시지를 출력한다. <br> ...11a.2. 사용자는 다시 보냄 또는 받음 버튼을 선택한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 5초 |
| Frequency | 필요 시 |
| Concurrency | 제한 없음 |
| Due Date | 제한 없음 |

---

# 3. Domain analysis
## 3.1 Classes
### 3.1.1 User
시스템을 사용하는 사용자를 나타내는 클래스이다. 사용자는 회원가입 및 로그인을 실행할 수 있으며, 그룹 생성 및 참여, 지출 등록, 정산 결과 확인 등의 기능을 수행한다. 사용자 정보로는 닉네임, 이메일, 계좌번호 등을 입력할 수 있다. 하나의 사용자는 여러 그룹에 참여할 수 있고 여러 정산 내역을 알 수 있다.
### 3.1.2 Account
사용자의 인증 및 로그인 정보를 관리하는 클래스이다. 이메일과 비밀번호 등의 계정 정보를 저장하며 사용자 인증 및 로그인 기능에 사용된다. 하나의 Account는 하나의 User와 연결된다.
### 3.1.3 Group
여행 경비를 함께 관리하는 사용자들의 모임을 나타내는 클래스이다. 그룹명과 그룹의 멤버들의 데이터를 가지며 그룹 단위로 지출 내역과 정산 정보를 관리한다. 하나의 그룹에는 여러 명의 사용자가 참여할 수 있다.
### 3.1.4 GroupMember
특정 사용자가 특정 그룹에 참여한 상태를 나타내는 클래스이다. User와 Group 사이의 M:N 관계를 관리하기 위해 사용된다. 그룹 이름과 그룹 멤버 초대 상태 등의 정보를 저장할 수 있다.
### 3.1.5 Expense
여행 중 발생한 지출 내역을 나타내는 클래스이다. 지출 금액, 지출 내용, 결제자, 생성 날짜 등의 정보를 저장한다. 또한 영수증과 같은 증빙 자료를 포함할 수 있다. 하나의 지출은 특정 그룹에 속하며 여러 참여자와 연결될 수 있다.
### 3.1.6 ExpenseParticipant
특정 지출에 참여한 사용자를 나타내는 클래스이다. Expense와 User 사이의 관계를 관리하며, 각 사용자의 비용 분담 정보를 저장한다. 하나의 지출에 여러 사용자가 참여할 수 있으며, 사용자별 분담 금액 또는 정산 상태를 저장할 수 있다.
### 3.1.7 ReceiptImage
지출에 대한 증빙 자료를 관리하는 클래스이다. 사용자가 업로드한 영수증 이미지 또는 파일 정보를 저장한다. 파일 경로, 업로드 날짜, 파일 형식 등의 정보를 관리하며 하나의 ReceiptImage는 하나의 Expense와 연결된다.
### 3.1.8 Settlement
그룹 내 전체 지출 내역을 기반으로 계산된 정산 결과를 나타내는 클래스이다. 사용자별 총 지출 금액, 송금해야 하는 금액, 수령해야 하는 금액 등의 정보를 저장한다. 하나의 Settlement는 특정 Group와 연결된다.
### 3.1.9 SettlementDetail
정산 결과의 세부 송금 정보를 나타내는 클래스이다. 특정 사용자가 누구에게 얼마를 송금해야 하는지에 대한 정보를 저장하며, 송금 완료 여부와 입금 확인 여부를 관리한다. 또한 관련 사용자들이 모두 정산 완료 처리를 했을 경우 상태를 완료로 변경한다.
### 3.1.10 Notification
시스템 내 주요 이벤트 발생 시 사용자에게 전달되는 알림 정보를 나타내는 클래스이다. 그룹 초대, 지출 등록 및 수정, 지출 삭제, 정산 완료 등의 이벤트 발생 시 생성된다. 알림 내용, 생성 날짜 등의 정보를 저장한다.
### 3.1.11 NotificationService
시스템에서 발생하는 주요 이벤트를 기반으로 알림 데이터를 생성하고 사용자에게 전달하는 역할을 수행하는 클래스이다. 그룹 멤버 추가, 지출 추가 및 수정, 지출 삭제, 정산 완료 등의 이벤트 발생 시 Notification 객체를 생성한다. 생성된 알림은 애플리케이션 내부 알림 목록과 연동되어 사용자에게 표시된다.

---

# 4. User Interface prototype
## 4.1 Login Screen
![Login Screen](https://github.com/user-attachments/assets/952366cc-8b2e-466e-9e57-4c4aacdca858)

사용자가 앱을 누르면 로그인 화면이 보인다. 이미 회원가입을 한 사용자라면 이메일, 비밀번호 입력란에 가입한 정보를 넣어 로그인 버튼을 눌러 시스템에 접속한다. 회원가입이 필요한 사용자라면 로그인 버튼 밑에 회원가입 버튼을 누른다.

## 4.2 Sign up Screen
![SingUp Screen](https://github.com/user-attachments/assets/c4cea94d-a1f9-4ab2-94af-25347521311c)

회원가입 화면에서 사용자의 이메일, 닉네임, 비밀번호, 비밀번호 확인 그리고 계좌번호를 입력란을 만들어 회원 가입에 필요한 사용자 정보를 입력할 수 있도록 한다. 회원가입 버튼을 누르면 회원가입이 완료되고 다시 로그인 화면으로 돌아가 로그인할 수 있도록 한다. 

## 4.3 Main Screen
![Main Screen](https://github.com/user-attachments/assets/f6fbff42-651a-4e11-8e49-aa44410ff700)

홈 화면으로 하단에 언더바를 배치하여 항상 홈과 알림 화면 들어갈 수 있도록 한다. ‘다같이 여행해요’과 ‘나혼자 여행해요’ 버튼을 만들어 다인원 여행과 혼자 여행하는 두가지 경우를 고려하나 본 문서에서는 다인원 여행만 고려한다.

## 4.4 Group List Screen
![Group List Screen](https://github.com/user-attachments/assets/2d547c26-d47b-40e3-8c0d-4c84c8854f43)

Main Screen에서 ‘다같이 여행해요’ 버튼을 누르면 나오는 화면으로 가입한 그룹이 정렬되어 나온다. 그룹 리스트 글자 옆에 더하기 아이콘의 버튼을 누르면 여행 정산 그룹을 만들 수 있는 화면으로 넘어갈 수 있다.

## 4.5 Create Group Screen
<p align="center">
  <img src="https://github.com/user-attachments/assets/b3cbcf21-4aee-4c4e-b534-42815678a930" width="45%" />
  <img src="https://github.com/user-attachments/assets/8183d14e-5fef-4552-81cd-759b6c983f4b" width="45%" />
</p>

Group List Screen에서 더하기 아이콘 버튼을 누르면 나오는 화면으로 여행 정산 그룹을 만들 수 있는 화면이다. 여행 정산 그룹의 이름을 입력하고 멤버를 초대하려면 멤버 초대하기 버튼을 누른다. 멤버 초대하기 버튼을 누르면 초대할 사용자의 이메일을 입력할 수 있는 팝업창이 뜨는데 해당 시스템에 가입된 멤버라면 해당 사용자에게 코드번호를 부여하고 그룹에 초대된다. 초대된 멤버들은 이름과 계좌번호를 함께 나타낸다. 마지막으로 그룹 만들기 버튼을 누르면 해당 그룹이 만들어진다.

## 4.6 Group Expense List Screen
![image](https://github.com/user-attachments/assets/59dba781-98af-4746-8ba5-17f412feba98)

각 그룹의 가계부를 볼 수 있는 화면이다. 상단에는 그룹 의 정산 내용을 그래프 형식으로 사용자가 한눈에 파악하기 쉽도록 구성한다. 그룹 리스트 부분은 각자 지불한 비용과 이름이 정렬되어 있다. 설정 버튼을 누르면 작성한 정산 내용을 삭제하거나 비용을 수정할 수 있도록 한다.  하단에는 멤버 관리/초대하기, 정산 비용 추가하기, 정산하기 버튼을 배치하여 각각의 페이지로 이동하도록 한다.

## 4.7 Add Expense Screen
![image](https://github.com/user-attachments/assets/727e8cc2-7d3b-429f-a6f5-6d7985dc0395)

Group Expense List Screen에서 정산 비용 추가하기 버튼을 누르면 나오는 화면이다. 결제 금액, 메모를 입력할 수 있는 칸과 증빙 자료를 넣을 수 있어 카메라나 갤러리에 연동할 수 있는 버튼을 배치한다. 증빙 자료는 카메라로 영수증을 찍거나 사진첩에서 캡쳐한 전자 영수증을 업로드할 수 있도록 한다. 첨부한 증빙 자료는 미리보기 형태로 표시하고 사용자는 업로드한 이미지를 삭제하거나 다시 선택할 수 있다. 참여자에는 해당 그룹에 속한 참여자를 모두 나열하고 chip 버튼으로 해당 참여자를 선택할 수 있도록 한다. 해당 추가하기 버튼을 누르면 서버를 통해 해당 내용을 DB에 저장한다.

## 4.8 Edit Group Member Screen
![image](https://github.com/user-attachments/assets/06efddfa-541a-42a9-9a36-0f8348ffe07f)

Group Expense List Screen에서 멤버 관리/초대하기 버튼을 누르면 나오는 화면이다. 수정 버튼을 누르면 멤버를 삭제할 수 있으며 초대하기 버튼을 누르면 팝업창이 뜨는데 해당 시스템에 가입된 멤버라면 시스템 DB에서 이메일을 조회하여 해당 그룹에 초대한다. 초대된 멤버들은 이름과 계좌번호를 UI에 나타낸다.

## 4.9 Settlement Result Screen
![image](https://github.com/user-attachments/assets/b14bf620-b6d6-4b81-b324-95044f6427de)

Group Expense List Screen에서 정산하기 버튼을 누르면 나오는 화면이다. 해당 사용자가 보낼 금액과 받을 금액의 리스트를 나열한다. 각 리스트에 이름, 계좌번호, 비용이 뜨고 chip 버튼으로 송금과 입금이 완료되었으면 보냄, 받음 버튼을 누른다. 보냄과 받음 버튼을 해당 비용과 관련된 사용자들이 모두 누를 시에 완료라고 뜨도록 한다. 

## 4.10 Notification List Screen
![image](https://github.com/user-attachments/assets/53da6299-b8c8-4051-a69e-33539cfc7da1)

그룹 멤버 추가, 그룹 정산 비용 추가 또는 그룹 정산이 완료되었을 때 알림이 뜨도록 한다.

---

# 5. Glossary
| Term | Description |
|---|---|
| DB | 데이터베이스, 전자적으로 저장된 구조화된 정보의 집합 |
| M:N 관계 | 두 개의 테이블이 서로의 행에 대해 여러개로 연관되어 있는 상태 |
| Chip 버튼 | 사용자가 입력하거나 선택한 항목을 간결한 형태로 표시하는 UI 요소 |
| UI | 사용자 인터페이스로 사용자가 디지털 기기나 시스템과 상호작용하는 시각적, 물리적 요소 |

---

# 6. References
