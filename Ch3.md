## next js 라우팅 시스템

- 파일시스템 기반 페이지와 리우팅을 사용한다.

**동적 라우팅규칙**

/pages/페이지이름의폴더/slug 형식으로 만들면 된다.

예시코드)
```jsx
function getServerSideProps({params}){
const {name} =params;
return {
props:{
name
}

function Greet(props){
return(

<h1> Hello, {props.name}! </h1>
)
}
```

**주의할점**

getServerSideProps나 getStaticProps 함수는 반드시 객체를 반환해야 한다. 이러한 함수가 반환한 값을 페이지에서 사용할 때는 함수가 반환한 객체의 props 속성값을 사용해야 된다는 점도 잊지 마세요.

## 컴포넌트에서 경로 매개변수 사용하기

next에서 사용하는 useRouter훅은 useParams훅과 같은 역할을 한다. 방법은 동일하다.

예시코드)
```jsx
import {useRouter}from "next/router"

function Greet(){
const {query} = useRouter();
return <h1> Hello, {props.name}! </h1>
}
```
## 클라이언트에서의 내비게이션

next js는 웹 사이트 성능을 최적화할 수 있는 많은 방법을 제공한다. 그중 하나가 바로 클라이언트에서 내비게이션을 처리하는 것이다.

그중 하나가 Link텀포넌트를 통해 서로 다른 라우트 간의 이동을 최적화할 수도 있다.

- nextjs는 기본적으로 현재 화면에 표시되는 페이지의 모든 link에 대해 연결된 부분 또는 페이지를 미리 읽어온다. 미리 불러오는 기능은 link컴포넌트에 preload={false}라는 속성을 전달해서 비활성화 할수 있다.
- 동적 경로 매개 변수를 통해 페이지를 더 쉽게 연결할 수 있다. 예를 들어 / blog/[date[/[slug].js라는 페이지를 연결한다고 가정해보자 href 속성은 next js가 어떤 페이지를 렌더링해야 하는지 알려준다.

복잡한 url을 사용한다면 href 속성에 객체를 전달할 수도 있습니다.
```jsx
<Link ref={{pathname:"/blog/[date]/[slug]"
query:{
date:"2020-01-01",
slug:"happy-new-year",
foo:"bar"
}
}}>
read post
</Link>
```
위와 같은 링크를 클릭할시 next js는 http://루트경로/blog/2020-01-01/happy-new-year/foo:bar라는 주소로 연결될것 이다.

## router.push 메서드

link컴포넌트 대신 useRouter 훅을 사용해서 다른 페이지로 이동할수 있다.

사용방법은 리액트에서의 useNavigation과 같은 방법으로 사용하면 된다.

또한 **link태그와 마찬가지로 객체를 전달해서 더 복잡한 url로 이동할 수도 있다.**
```jsx
router.push({
pathname:"/blog/[date]/[slug]",
query:{
date:"2020-01-01",
slug:"happy-new-year",
foo:"bar"
}
})
```
## 정적 자원 제공

정적 자원은 이미지,폰트, 아이콘,컴파일한 css또는 js 파일과 같이 동적으로 변하지 않는 모든 종류의 파일을 의미 한다. 이런 정적 자원은 /public 디렉터리 안에 저장하는 방식으로 클라이언트에 쉽게 제공된다.

- 정적 자원을 관리하고 제공하는 것은 생각보다 쉽지만 특정 유형의 파일은 웹 사이트의 성능과 seo 점수에 큰영향을 미친다. 바로 이미지 파일이 제일 영향이 크다.
- 일반적으로 최적화되지 않은 이미지를 제공하는 것은 사용자 경험에 나쁜 영향을 준다. 이미지를 불러오는 데 시간이 오래 걸리고, 이미지를 불러온 후에도 이미지 주변의 레이아웃이 변경되는듯 ux 관점에서도 좋지 않다. 이를 누적 레이아웃 이동(cls)라고 한다.

### cls 현상

이미지는 비동기로 데이터를 불러오기때문에 처음 빌드 될때 텍스트가 먼저 불러와진다. 이미지가 전부 로드 되었을때 텍스트 사이에 로드되지 않았던 이미지가 로드되기 때문에, 이미지를 불러오고 나면 문자영역이 아래로 밀려나게 된다. 따라서 사용자가 문자 영역을 읽고 있었다면 영역이 이동하면서 어느 부분을 읽고 있었는지 놓칠 수도 있다.

이러한 cls현상을 next js에서는 IMage컴포넌트를 사용해서 해결할수 있다.

## 자동 이미지 최적화

next js에서는 html의 모든 img 태그에 복잡한 srcset 속성값을 지정해서 화면 크기별로 이미지를 조정 한다. 이러한 이미지 최적화 기능을 사용하면 이미지를 webp와 같은 최신 이미지 포맷으로 제공 할수 있다. webp를 지원하지 않는 브라우저의 경우 png나 jpeg와 같은 예전 이미지 포맷도 제공한다. 필요한 경우 이미지 크기를 조절해서 속도가 느려지는 상황도 피할수 있다.

### 장점

클라이언트가 이미지를 요구할때만 최적화 할 수 있다.

화면 크기별로 이미지를 조절하고 싶다면 srcset 속성값을 사용해서 최적화 할 수 있다.이를 위해서는 정적 자원을 제공할 때 몇 가지 작업을 추가해야 하는데 next.js에서는 쉽게 처리 할 수 있다. next.config.js 파일에서 설정을 추가하고 image컴포넌트를 사용 하는것이다. 방법은 아래와 같다.
```jsx
module.exports={
images:{
domains:["images.unsplash.com"]
}
```
위와 같이 세팅한후, Image컴포넌트를 사용하면 속성값에 따라서 사진이 늘어난 형태로 표시 된다.

## Image 태그의 layout속성

1. fixed: html img 태그와 동일하다. 이미지 크기를 지정하면 더크거나 작은화면에서도 이미지 크기를 조절하지 않고 지정한대로 유지된다.
2. responsive: fixed와 반대 방식으로 작동한다. 화면 크기를 조절하면 그에 따라 이미지를 최적화해서 제공한다.
3. intrinsic: fixed와 responsive를 절반씩 수용한다. 크기가 작은 화면에서는 이미지 크기를 조절하지만 이미지보다 큰 화면에서는 이미지 크기를 조절하지 않는다.
4. fill: 부모 요소의 가로와 세로 크기에 따라 이미지를 늘린다. layout에 fill을 지정한 경우 width와 height 속성값을 함께 지정할 수 없다. width/height 속성을 지정하는것중 하나만 가능하다.

이미지 크기를 화면 크기에 따라 조절하고 싶다면 아래와 같이 수정하면 된다.
```jsx
<div style={{position:relative}>
<Image src="" layout="fill" objectfill="cover"/>
</div>
```
layout속성을 fill로 지정했기 때문에 부모요소의 div 크기에 영향을 받아서 기준이 되는 부모요소에 position relative를 지정해줘야 제대로 렌더링 된다.

## 외부 서비스를 통한 자동 이미지 최적화

next js에서는 서버에서 자동으로 이미지 최적화 작업을 처리한다. 이는 컴퓨팅 자원이 충분하지 않은 작은 서버에서 실행된다면 이미지 최적화로 인해 성능에 영향을 미칠 수 있다. 이는 next.config파일 내에 loader 속성을 지정 하여 외부 서비스를 통해 자동 이미지 최적화 작업을 처리한다.

## 외부 서비스를 통한 자동 이미지 최적화

next js에서는 서버에서 자동으로 이미지 최적화 작업을 처리한다. 이는 컴퓨팅 자원이 충분하지 않은 작은 서버에서 실행된다면 이미지 최적화로 인해 성능에 영향을 미칠 수 있다. 이는 next.config파일 내에 loader 속성을 지정 하여 외부 서비스를 통해 자동 이미지 최적화 작업을 처리한다.
