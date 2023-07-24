## 메타데이터

- 메타 데이터를 제공하지 않으면 웹 사이트가 꼭 필요한 정보를 제공하지 않는다고 생각해서 검색 엔진이 해당 페이지의 점수를 낮게 매길것 이다.
- 메타 태그를 이용해서 브라우저가 사용자 경험을 최적화할 수 있는데 당연히 메타데이터가 없다면 사용자 경험 또한 좋아질 수 없다.

### next js에서의 메타데이터

- next js는 내장 head 컴포넌트를 제공하여 이러한 데이터를 쉽게 다룰 수 있게 도와준다. 이 컴포넌트는 어떤 컴포넌트 에서든 html페이지의 head 내부 데이터를 변경할 수 있도록 해준다. 즉 동적으로 메타데이터, 링크,스크립트 등의 정보를 변경하고, 추가하고, 삭제할 수 있다.

예시)

```jsx
import Head from "next/head";

function IndexPage() {
  return (
    <>
      <Head>
        <title> welcome to my Next.js website</title>
      </Head>
    </>
  );
}
```

## 공통 메타 태그 그룹

같은 태그를 다루는 페이지나 컴포넌트를 여러개 만드는 경우가 있다. 이 경우에는 공통 head 메타 태그를 다루는 하나 또는 여러 개의 컴포넌트를 만드는 것이 좋다.

예시 코드)

```jsx
import Head from "next/head";

function PostMeta(props) {
  return (
    <Head>
      <title>{props.title}</title>
      <meta name="description" content={props.subtitle} />
      <meta property="og:title" contnent={props.title} />
      <meta property="og:description" contnent={props.subtitle} />
      <meta name="twitter:card" content="summary" />
    </Head>
  );
}
```

위와 같이 컴포넌트를 만들고, 아래와 같이 불러와서 쓰면 된다.

```jsx
import posts from "../../data/posts";
export function getServerSideprops({params}){
 const {slug} =params;
const post =posts.find((p) => p.slug ===slug);
return {
props:{{
post
}
}

function Post({post}){
  return(
  <div>
 <PostHead {...post}/>
</div>
)
}
}
```

## 코드 구성과 데이터 불러오기

### 1. 디렉터리 구조 구성

코드를 쉽게 유지 보수하고 확장하려면 프로젝트의 디렉터리 구조를 간결하고 분명하게 구성하고 유지하는 것이 무엇보다 중요하다.

**next js 기본 폴더 구조**

```jsx
- node_modules/
- package.json
- pages/
- public/
- styles/
```

public과 node_modules/디렉터리를 제외한 다른 디렉터리를 모드 src/ 디렉터리 아래로 옮길 수 있다. 다만 프로젝트 내에 pages/와 src/pages 디렉터리가 둘 다 있는 경우 next.js가 src/pages/ 디렉터리를 무시한다는 점을 기억해야된다. 이유는 최상위에 있는 pages/ 디렉터리의 우선순위가 더 높기 때문이다.

### 컴포넌트 구성

- 먼저 컴포넌트들을 세 가지로 분류하고 각 컴포넌트와 관련된 스타일 및 테스트 파일을 같은곳에 두어야 한다. 이를 위해 다음과 같이 components/ 디렉터리를 만들고 그안에 추가 디렉터리를 만든다.
- 코드를 더 효율적으로 구성하기 위해 **아토믹 디자인 원칙**에 따라 각 컴포넌트를 서로 다른 수준의 디렉터리에 둔다. 이는 많이 사용되는 방식이며 이 외에도 다양한 방법으로 코드를 구성할 수 있다. 여기서는 컴포넌트를 다음과 같이 네 가지 종류로 나눈다.

1. **atoms**

코드에서 사용되는 가장 기본적인 컴포넌트들이다. button,input,p와 같은 표준 html 요소를 감싸는 용도로 사용되거나, 애니메이션 또는 컬러 팔레트 등과 같은 용도로 사용되는 컴포넌트를 이곳에 저장한다.

1. **molecules**

atoms에 속한 컴포넌트 여러 개를 조합하여 좀 더 복잡한 구조를 만드는 컴포넌트들이다. 유틸리티 기능들은 많이 사용되지 않는다. 예를 들어 input과 label 컴포넌트를 가져와서 새로운 컴포넌트를 만들면 이 컴포넌트는 molecules에 속한다.

1. **organisms**

molecules와 atoms를 섞어서 더 복잡한 구조의 컴포넌트를 만든다. 예를 들면 회원 가입 양식이나 푸터, 캐러셀 등이 이에 속한다.

1. **templates**

일종의 페이지 스켈레톤으로 어디에 organisms, atioms, molecules를 배치할지 결정해서 사용자가 접근할 수 있는 페이지를 만든다.

### 유틸리티 구성

컴포넌트를 만들지 않는 코드 파일도 있다. 이를 유틸리티 스크립트라고 한다. 공통함수와 같은것들이 utilities 폴더 내부에 정의되고 사용된다.

### 정적 자원 구성

next js에서는 정적 파일을 쉽게 제공할 수 있다. 제공할 파일을 public/디렉터리 아래에 두면 나머지는 프레임워크가 알아서 해준다.

정적 자원을 구성하기 전에 next.js 애플리케이션에서 어떤 정적 파일을 제공해야 할지 파악할 필요가 있다. 일반적인 웹 사이트에서는 다음과 같은 정적 자원을 사용한다.

- 이미지
- 컴파일한 자바스크립트 파일
- 컴파일한 css 파일
- 아이콘(favicon 및 웹 앱 아이콘)
- mainfest.json, robot.txt 등의 정적 파일

icons/ 디렉터리는 주로 웹 앱 매니페스트 아이콘을 제공할 용도로 사용 된다. 웹 앱 매니페스트는 json 파일로, 앱의 이름이나 모바일 기기에 앱을 설치할 때 표시할 아이콘과 같이 프로그레시브 웹 앱에 관한 유용한 정보를 가지고 있다.

### lib 파일 구성

lib 파일은 서드파티 라이브러리를 감싸는 스크립트를 지친하는 말이다. 유틸리티 스크립트는 범용이기 때문에 컴포넌트나 라이브러리에서 가져다 쓸 수 있지만 lib 파일은 특정 라이브러리에 특화 된것 이다.
