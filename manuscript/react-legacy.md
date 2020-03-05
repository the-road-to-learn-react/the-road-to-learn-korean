# 리액트 레거시

리액트는 2013년 이후 많은 변화를 겪었습니다. 리액트의 여러 버전들, 리액트 애플리케이션을 작성하는 방식과 특히 리액트의 컴포넌트 모두 급격하게 변화했습니다. 하지만 많은 리액트 애플리케이션이 지난 몇 년 간 개발되었기 때문에 모든 것이 현 상황을 염두에 두고 만들어진 것은 아닙니다. 이번 장에서는 리액트 레거시를 다룹니다.

리액트에서 레거시로 여겨지는 모든 것을 다루지는 않을 것입니다. 일부 기능은 여러 번 개편되었습니다. 오래된 리액트 애플리케이션에서 이전 버전의 기능을 봤을 지도 모릅니다. 하지만 아마 지금 버전과는 차이가 있을 것입니다.

이번 장에서는 [모던 리액트 애플리케이션](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/react-modern-final)과 이것의 [레거시 버전](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/react-legacy)을 비교해볼 것입니다. 모던 리액트와 레거시 리액트의 가장 큰 차이점은 클래스 컴포넌트와 함수형 컴포넌트의 차이에서 비롯된다는 것을 알게될 것입니다.

## 리액트 클래스 컴포넌트

리액트 컴포넌트는 **클래스 컴포넌트**를 만들기 위한 **createClass 컴포넌트**에서 부터 **함수형 컴포넌트**까지 많은 변화를 겪어왔습니다. 오늘날의 리액트 애플리케이션을 보면 클래스 컴포넌트보다 모던 함수형 컴포넌트를 먼저 볼 가능성이 큽니다.

{title="Code Playground",lang="javascript"}
~~~~~~~
class InputWithLabel extends React.Component {
  render() {
    const {
      id,
      value,
      type = 'text',
      onInputChange,
      children,
    } = this.props;

    return (
      <>
        <label htmlFor={id}>{children}</label>
        &nbsp;
        <input
          id={id}
          type={type}
          value={value}
          onChange={onInputChange}
        />
      </>
    );
  }
}
~~~~~~~

일반적으로 클래스 컴포넌트는 JSX를 반환하는 **render 메서드**를 필수적으로 갖고 있는 자바스크립트 클래스입니다. 이 클래스는 리액트 컴포넌트의 모든 기능(예: state에 대한 상태 관리, 사이드 이펙트 대한 위한 생명주기 메서드)을 물려받기 위해 `React.Component`를 상속받습니다([클래스 상속](https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming))). 클래스 인스턴스(`this`)를 통해 리액트 props에 접근할 수 있습니다.

한동안 클래스 컴포넌트는 리액트 애플리케이션을 작성하기 위한 일반적인 방법이었습니다. 마침내 함수형 컴포넌트가 추가되었고 이후 두 컴포넌트는 각자의 분명한 목적을 갖고 공존했습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~
const InputWithLabel = ({
  id,
  value,
  type = 'text',
  onInputChange,
  children,
}) => (
  <>
    <label htmlFor={id}>{children}</label>
    &nbsp;
    <input
      id={id}
      type={type}
      value={value}
      onChange={onInputChange}
    />
  </>
);
~~~~~~~

만약 레거시 애플리케이션에 사이드 이펙트나 state가 없었다면 클래스 컴포넌트 대신에 함수형 컴포넌트를 사용했을 것입니다. 리액트 훅이 도입된 2018년 이전까지만 해도 리액트의 함수형 컴포넌트는 사이드 이펙트(`useEffect` 훅)나 state(`useState`/`useReducer` 훅)를 다룰 수 없었습니다. 이로 인해 이 컴포넌트는 **비 상태 함수형 컴포넌트**로 불렸고, props을 입력으로 넣어 JSX를 출력하는 일만 담당했습니다. state나 사이드 이펙트를 사용하기 위해서는 반드시 함수형 컴포넌트를 클래스 컴포넌트로 리팩토링 해야 했습니다. state나 사이드 이펙트 모두 필요하지 않을 땐 클래스 컴포넌트도 사용했지만 가벼운 함수형 컴포넌트를 더 많이 사용했습니다.

리액트 훅의 등장과 함께 함수형 컴포넌트는 클래스 컴포넌트처럼 state와 사이드 이펙트를 갖게 되었습니다. 그리고 두 컴포넌트 사이에 실질적인 차이점이 더 이상 존재하지 않게 되자 리액트 커뮤니티는 더욱 가벼운 함수형 컴포넌트를 선택했습니다.

### 읽어보기

* [자바스크립트 클래스](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes).
* [클래스 컴포넌트를 함수형 컴포넌트로 리팩토링 하는 방법](https://www.robinwieruch.de/react-hooks-migration).
* [클래스 컴포넌트에 대해 자세히 알아보기](https://reactjs.org/docs/react-component.html).
* [리액트의 다른 레거시 & 모던 컴포넌트 타입](https://www.robinwieruch.de/react-component-types).

### 실습하기
* 일반적이진 않지만 더 효율적인 또 다른 [클래스 컴포넌트 문법](https://github.com/the-road-to-learn-react/react-alternative-class-component-syntax)에 대해 알아봅시다.

## 리액트 클래스 컴포넌트: State

리액트 훅이 등장하기 전에 클래스 컴포넌트는 상태를 가질 수 있다는 이유로 함수형 컴포넌트보다 더 뛰어났습니다. 클래스 생성자와 함께 컴포넌트의 초기 상태를 할당할 수 있습니다. 또한 컴포넌트의 인스턴스(`this`)는 최신 state(`this.state`)와 컴포넌트의 state 업데이트 메서드(`this.setState`)에 접근할 수 있습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~
class App extends React.Component {
  constructor(props) {
    super(props);

# leanpub-start-insert
    this.state = {
      searchTerm: 'React',
    };
# leanpub-end-insert
  }

  render() {
# leanpub-start-insert
    const { searchTerm } = this.state;
# leanpub-end-insert

    return (
      <div>
        <h1>My Hacker Stories</h1>

        <SearchForm
          searchTerm={searchTerm}
# leanpub-start-insert
          onSearchInput={() => this.setState({
            searchTerm: event.target.value
          })}
# leanpub-end-insert
        />
      </div>
    );
  }
}
~~~~~~~

만약 state 객체 안에 하나 이상의 프로퍼티가 있다면 `setState` 메서드는 얕은 업데이트만 수행합니다. Only the properties passed to `setState` are overwritten, and all other properties in the state object stay intact. Since state management is important for frontend applications, there was no way around class components without hooks for function components.

리액트 클래스 컴포넌트에는 컴포넌트의 state를 관리하기 위한 두 개의 오래된(deprecated) API(`this.state` 와 `this.setState`)가 있습니다. 함수형 컴포넌트에서 리액트의 useState와 useReducer 훅이 이 일을 담당합니다. 클래스 컴포넌트는 일반적인 state API를 무조건 사용해야 하는 반면에 함수형 컴포넌트에서 관련된 state 항목(state 객체와 업데이트 메서드)은 하나의 state 훅으로 쌓여집니다. 이것이 바로 리액트 훅을 도입한 주된 이유 중 하나였습니다. 이제 클래스 컴포넌트로부터 벗어납시다.

### 실습하기

* 상태를 갖고 있는 클래스 컴포넌트(예: Input)를 작성합니다.

## 명령형 리액트

함수형 컴포넌트에서 useRef 훅은 주로 명령형 프로그래밍에 사용됩니다. 리액트의 역사 속에서 *ref*와 ref의 사용법은 여러 번 바뀌었습니다.

* String Refs (deprecated)
* Callback Refs
* createRef Refs (클래스 컴포넌트에만 사용)
* useRef Hook Refs (함수형 컴포넌트에만 사용)

최근에 리액트 팀은 함수형 컴포넌트의 useRef 훅과 동등한 가장 최신 버전의 ref인 **리액트의 createRef**를 도입했습니다.

{title="Code Playground",lang="javascript"}
~~~~~~~
class InputWithLabel extends React.Component {
  constructor(props) {
    super(props);

# leanpub-start-insert
    this.inputRef = React.createRef();
# leanpub-end-insert
  }

  componentDidMount() {
    if (this.props.isFocused) {
# leanpub-start-insert
      this.inputRef.current.focus();
# leanpub-end-insert
    }
  }

  render() {
    ...

    return (
      <>
        ...
        <input
# leanpub-start-insert
          ref={this.inputRef}
# leanpub-end-insert
          id={id}
          type={type}
          value={value}
          onChange={onInputChange}
        />
      </>
    );
  }
}
~~~~~~~

헬퍼 함수들과 함께 ref는 클래스의 생성자에서 만들어지며, JSX에서는 `ref` 속성에 할당되고, 이 예제에서는 생애주기 메서드에 사용되었습니다. ref는 버튼을 클릭했을 때 입력 필드에 포커스를 주는 등 다른 상황에서도 사용될 수 있습니다.

클래스 컴포넌트에서 createRef가 사용되는 곳이 바로 함수형 컴포넌트에서 useRef 훅이 사용되는 곳입니다. 리액트가 함수형 컴포넌트로 변화하고 있기에
ref를 관리하고 명령형 프로그래밍 원리를 적용하기 위해서만 useRef 훅을 사용하는 일이 일반적입니다.

### 읽어보기

* [리액트의 다양한 ref 기술들](https://reactjs.org/docs/refs-and-the-dom.html).