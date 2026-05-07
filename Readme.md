# [Analysis] Trabill
다인원 여행 경비 지출 관리 앱
![image](https://github.com/user-attachments/assets/d613cc5c-2c76-4154-93d8-1b78570d1342)

---

# Introduction 
본 문서는 Conceptualiztion 단계에서 정의된 내용을 기반으로 시스템의 요구사항과 목표를 구체화하는 Analysis 단계의 문서이다.

## Summary
최근 여행 증가로 인하여 교통비, 숙박비, 식비 등 다양한 비용을 여러 사람이 나누어 결제하는 상황이 증가하고 있다.
그러나 각 지출마다 참여 인원이 다르고 결제 방식이 다양해지면서 비용 정산 과정이 복잡해지는 문제가 발생한다. 또한 지출 내역이
누적될 수록 수기로 기록하는 과정에서 시간 소요가 많이 되며 일부 지출 내역이 누락되거나 계산 오류가 발생할 가능성이 존재한다.
이에 본 시스템은 여행 중 발생하는 지출 내역을 효육적으로 기록하고 관리하며 각자의 사용자마다 자동 정산 기능을 통해 사용자 간의
투명하고 정확한 비용 분담을 지원하는 앱이다. 이를 통해 사용자는 다인원의 여행일 경우에도 여행 경비 관리를 편리하게 사용할 수 
있을 것으로 기대한다.

## Business Goals
- 사용자가 여행 중 발생하는 지출 내역을 쉽게 기록할 수 있어야한다.
- 그룹 단위로 경비를 공유하고 참여자 간 비용 분담을 명확하게 확인할 수 있어야한다.
- 지출 내역을 실시간으로 공유하여 모든 참여자가 동일한 정보를 확인할 수 있어야한다.
- 자동 정산 기능을 통해 복잡한 계산 없이 정확한 정산 결과를 제공해야한다.
- 주요 이벤트 발생 시 알림 기능을 통해 사용자에게 정보를 전달할 수 있어야한다.

## Technical Goals
- 사용자 입력을 기반으로 지출 데이터를 안정적으로 저장하고 관리할 수 있어야한다.
- 참여 인원 및 지출 정보를 바탕으로 정확한 정산 알고리즘을 수행할 수 있어야한다.
- 그룹 단위 데이터 관리가 가능하도록 데이터 구조를 설계해야한다.
- 직관적인 UI/UX를 통해 사용자가 쉽게 기능을 사용할 수 있도록 해야한다.
- 시스템은 실시간 데이터 처리 및 빠른 응답 속도를 제공하여 사용자 경험을 저해하지 않아야한다.

---

# Use case analysis
## Use case Diagram
Conceptualization에서 System context diagram과 Use case list를 참조하여 Use 
case diagram을 다음과 같이 작성하였다. 모델링 도구는 Star UML을 사용하였으며 
Actor와 Use case들과의 관계를 나타내었다.

<img width="871" height="676" alt="UseCaseDiagram1" src="https://github.com/user-attachments/assets/e9e8d10d-9b4e-4ef1-8165-f071ff5c0de2" />

사용자(User)를 중심으로 회원가입, 로그인, 그룹 생성 및 참여, 지출 관리, 정산 결과 조회 기능 등을 정의하였다. 지출 등록, 수정, 삭제 및 정산 요청과 같은 주요 이벤트 발생 시 알림을 제공하기 위해 Notification 기능을 포함하였다. 해당 알림 기능은 공통적으로 수행되는 기능으로 <<include>> 관계로 표현하였다. Concpetualization에서 정산 계산 use case가 있었지만 시스템 내부로 간주하여 use case로 분리하지 않았다. 또한 알림 기능은 외부 시스템인 Notification Service와의 연동을 통해 알림을 받도록 구성하였다.

## Use case Description
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

# Use case #3 : Create Group

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

# Use case #4 : Join Group

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

# Use case #5 : Add Expense

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
| 1 | 사용자가 그룹 상세 화면에서 정산 비용 추가하기 버튼을 누른다. |
| 2 | 시스템은 지출 등록 화면으로 이동한다. |
| 3 | 시스템은 지출 금액, 지출 내용, 카테고리, 결제자, 참여 멤버 입력 항목을 제공한다. |
| 4 | 사용자는 지출 금액을 입력한다. |
| 5 | 사용자는 지출 내용을 입력한다. |
| 6 | 사용자는 카테고리를 선택한다. |
| 7 | 사용자는 해당 지출을 결제한 사람을 선택한다. |
| 8 | 사용자는 해당 지출에 참여한 멤버를 선택한다. |
| 9 | 사용자는 등록 버튼을 선택한다. |
| 10 | 시스템은 입력된 지출 정보의 유효성을 검사한다. |
| 11 | 시스템은 지출 정보를 DB에 저장한다. |
| 12 | 시스템은 그룹 멤버들에게 지출 등록 알림을 전송한다. |
| 13 | 시스템은 지출 목록 화면으로 이동하고 새로 등록된 지출 내역을 표시한다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 1 | 1a. 금액이 입력되지 않은 경우 <br><br> ...1a.1. 시스템은 “금액을 입력해주세요.” 메시지를 출력한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 3초 |
| Frequency | 지출 발생 시마다 |
| Concurrency | 제한 없음 |
| Due Date | 제한 없음 |

---

# Use case #6 : Edit Expenses

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 등록한 지출 내역을 수정하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 수정 가능한 지출 내역이 존재해야 한다. |
| Trigger | 사용자가 수정 버튼을 클릭할 때 |
| Success Post Condition | 지출 정보가 수정된다. |
| Failed Post Condition | 수정에 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 지출 상세 화면에 접근한다. |
| 1 | 사용자가 수정 버튼을 선택한다. |
| 2 | 시스템은 기존 정보를 표시한다. |
| 3 | 사용자가 수정 내용을 입력한다. |
| 4 | 시스템은 수정 내용을 저장한다. |
| 5 | 그룹 사용자에게 수정 알림을 전송한다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 1 | 1a. 수정 권한이 없는 경우 <br><br> ...1a.1. 시스템은 “수정 권한이 없습니다.” 메시지를 출력한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 3초 |
| Frequency | 필요 시 |
| Concurrency | 제한 없음 |
| Due Date | 제한 없음 |

---

# Use case #7 : Delete Expenses

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 지출 내역을 삭제하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 삭제 가능한 지출 내역이 존재해야 한다. |
| Trigger | 사용자가 삭제 버튼을 클릭할 때 |
| Success Post Condition | 지출 내역이 삭제된다. |
| Failed Post Condition | 삭제에 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 지출 상세 화면에 접근한다. |
| 1 | 사용자가 삭제 버튼을 클릭한다. |
| 2 | 시스템은 삭제 여부를 재확인한다. |
| 3 | 사용자가 삭제를 승인한다. |
| 4 | 시스템은 지출 정보를 삭제한다. |
| 5 | 그룹 사용자에게 삭제 알림을 전송한다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 1 | 1a. 사용자가 삭제를 취소한 경우 <br><br> ...1a.1. 시스템은 삭제를 수행하지 않는다. |
| RELATED INFORMATION |  |
| Performance | ≤ 3초 |
| Frequency | 필요 시 |
| Concurrency | 제한 없음 |
| Due Date | 제한 없음 |

---

# Use case #8 : View Expenses

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 그룹의 지출 내역을 조회하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 그룹에 지출 내역이 존재해야 한다. |
| Trigger | 사용자가 지출 조회 화면에 접근할 때 |
| Success Post Condition | 지출 내역이 화면에 표시된다. |
| Failed Post Condition | 조회에 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 지출 조회 화면에 접근한다. |
| 1 | 시스템은 저장된 지출 데이터를 조회한다. |
| 2 | 시스템은 지출 목록을 화면에 출력한다. |
| 3 | 사용자는 지출 상세 내역을 확인한다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 1 | 1a. 저장된 지출 내역이 없는 경우 <br><br> ...1a.1. 시스템은 “등록된 지출 내역이 없습니다.” 메시지를 출력한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 3초 |
| Frequency | 자주 |
| Concurrency | 제한 없음 |
| Due Date | 제한 없음 |

---

# Use case #9 : Request Settlement

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 여행 경비 정산을 요청하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 그룹 내 지출 데이터가 존재해야 한다. |
| Trigger | 사용자가 정산 요청 버튼을 클릭할 때 |
| Success Post Condition | 정산 결과가 생성된다. |
| Failed Post Condition | 정산 요청에 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 정산 요청 화면에 접근한다. |
| 1 | 사용자가 정산 요청 버튼을 클릭한다. |
| 2 | 시스템은 그룹의 지출 데이터를 조회한다. |
| 3 | 시스템은 사용자별 정산 금액을 계산한다. |
| 4 | 시스템은 정산 결과를 저장한다. |
| 5 | 그룹 사용자에게 정산 결과 알림을 전송한다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 1 | 1a. 지출 데이터가 존재하지 않는 경우 <br><br> ...1a.1. 시스템은 “정산할 지출 내역이 없습니다.” 메시지를 출력한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 5초 |
| Frequency | 여행 종료 시 |
| Concurrency | 제한 없음 |
| Due Date | 제한 없음 |

---

# Use case #10 : View Settlement Results

| GENERAL CHARACTERISTICS |  |
|---|---|
| Summary | 사용자가 정산 결과를 조회하는 기능 |
| Scope | Trabill |
| Level | User level |
| Author | 이예린 |
| Status | Analysis |
| Primary Actor | 사용자 |
| Preconditions | 정산 결과가 생성되어 있어야 한다. |
| Trigger | 사용자가 정산 결과 조회 화면에 접근할 때 |
| Success Post Condition | 정산 결과가 화면에 표시된다. |
| Failed Post Condition | 조회에 실패한다. |
| MAIN SUCCESS SCENARIO |  |
| Step | Action |
| s | 사용자가 정산 결과 화면에 접근한다. |
| 1 | 시스템은 저장된 정산 데이터를 조회한다. |
| 2 | 시스템은 사용자별 송금 금액 및 대상 정보를 출력한다. |
| 3 | 사용자는 정산 결과를 확인한다. |
| EXTENSION SCENARIOS |  |
| Step | Branching Action |
| 1 | 1a. 정산 결과가 존재하지 않는 경우 <br><br> ...1a.1. 시스템은 “정산 결과가 존재하지 않습니다.” 메시지를 출력한다. |
| RELATED INFORMATION |  |
| Performance | ≤ 3초 |
| Frequency | 필요 시 |
| Concurrency | 제한 없음 |
| Due Date | 제한 없음 |


