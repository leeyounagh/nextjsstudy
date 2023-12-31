## 테스트

- 단위 테스트

코드의 각 함수가 제대로 작동하는지 확인하기 위한 테스트 이다.

함수에 올바른 입력과 잘못된 입력을 각각 주고 그 결과가 예상과 일치하는지 본다.

작동 과정에서 예측하지 못한 오류가 발생하는지도 확인 한다.

- 엔드 투 엔드 테스트

애플리케이션에 대한 사용자 상호 작용을 흉내내서 특정 작동이 발생했을 때 적절한 응답을 하는지 확인하기 위한 테스트 이다. 웹 사이트를 웹 브라우저에서 직접 테스트하는 것과 비슷하다. 예를 들어 폼을 만들었다면 해당 폼이 제대로 작동하는지, 사용자가 입력한 값을 제대로 하는지 폼의 내용을 제출할 때 지정한 작업을 처리하는지 등을 확인하기 위해 엔드 투 엔드 테스트를 사용할 수 있다. 그 외에도 특정 css 클래스를 사용했을 때 ui를 제대로 표시 하는지 또는 특정 html 요소를 제대로 마운트 하는지 등을 테스트 할 수 있다.

- 통합 테스트

애플리케이션에서 함수나 모듈과 같이 서로 구분되어 있는 영역이 함께 잘 작동하는지 확인하기 위한 테스트 이다. 두개의 함수 조합이 원하는 결과값을 반환하는지 여부를 검사하는 것 등이 여기에 속한다. 각 함수를 개별적으로 테스트하는 단위 테스트와 달리 통합 테스트에서는 서로 연관된 함수와 모듈을 한데 묶어서 주어진 입력에 맞는 적절한 출력을 만들어내는지 검사한다.

next js를 테스트하는 것은 리액트나 express.js 애플리케이션 등을 테스트하는 것과 다르지 않다. 적절한 테스트 러너와 라이브러리를 고르고 코드가 제대로 작동하는지만 확인하면 된다.

**테스트러너란?**

테스트 러너란 코드에서 모든 테스트를 찾고 테스트 결과를 수집하여 콘솔에 표시할 수 있는 것을 총칭한다. 테스트 러너 프로세스가 실패하거나 종료시 반환값이 0이 아닌 경우 테스트는 실패한 것으로 간주 된다.
