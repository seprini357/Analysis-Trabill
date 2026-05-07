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
| 5 | 회원가입이 완료되면 성공 메세지를 띄우고 로그인 화면으로 이동한다. |
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
| 3 | 로그인이 성공하면 시스템은 메인화면으로 이동한다. |
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


