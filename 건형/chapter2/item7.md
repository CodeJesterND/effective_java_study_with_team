## 📖 다 쓴 객체 참조를 해제하라

- 

## 💡 주요 내용 정리

- 자바처럼 가비지 컬렉터를 갖춘 언어로 넘어오면 다 쓴 객체를 알아서 회수해가니 자칫 메모리 관리에 더 이상 신경 쓰지 않아도 된다고 오해할 수 있다.

- 객체 참조를 null 처리하는 일은 예외적인 경우여야 합니다. 다 쓴 참조를 해체하는 가장 좋은 방법은 그 참조를 담은 변수를 유효 범위 밖으로 밀어내는 것입니다.

- 자기 메모리를 직접 관리하는 클래스라면 프로그래머는 항시 메모리 누수에 주의해야 합니다.

- 캐시 역시 메모리 누수를 일으키는 주범입니다.

- 메모리 누수의 세 번째 주범은 리스너(Listener) 혹은 콜백(callback)입니다.
- 콜백을 등록만 하고 명확히 해지하지 않는다면, 콜백은 계속 쌓여갈 것입니다.

## 🛠️ 실습 코드
- 예제 코드 및 결과 코드 설명

## 🤔 토론 및 질문
- 실제로 코드를 짜면서 메모리 누수 문제를 겪은적이 있었는지?
