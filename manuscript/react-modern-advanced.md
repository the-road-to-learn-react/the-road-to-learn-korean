# 현실 속 리액트 (심화)

지금까지 이 책을 통해 리액트 기초, 리액트 최신 버전 이전의 기능들, 애플리케이션 유지보수를 위한 기술 등을 배웠습니다. 이제 현실 속에서 리액트 애플리케이션을 개발할 차례입니다. 각 섹션에는 과제가 있습니다. 처음에는 _힌트 보기_ 부분을 건너뛰고 과제를 수행해보세요. 처음엔 꽤 힘들 수 있습니다. 도움이 필요할 때 _힌트 보기_ 부분을 참고하거나 각 섹션의 안내문을 따라하세요.

## 정렬

**과제:** 여러 아이템을 담은 목록을 다룰 때는 사용자가 효율적으로 데이터를 탐색할 수 있도록 작업 해야 하는 경우가 많습니다. 지금까지 모든 아이템을 각각의 속성들과 함께 나열했습니다. 사용자가 데이터를 더 쉽게 탐색하려면 목록을 제목, 저자, 댓글, 점수로 오름차순 및 내림차순 정렬할 수 있어야 합니다. 오름차순이나 내림차순 한 방향으로만 정렬을 구현해도 괜찮습니다. 반대 반향 정렬 기능은 다음 섹션에서 구현하게 될 것입니다.

**힌트 보기**

- App이나 List 컴퍼넌트에 정렬을 위한 상태를 새로 추가합니다.
- 각 속성(예시: `title`, `author`, `points`, `num_comments`)별로 HTML 버튼을 만들고, 이 버튼을 누르면 각 속성으로 새로 추가한 상태값이 설정되도록 합니다.
- 배열 형태의 `목록`을 선택한 속성으로 정렬하는 데 적절한 함수를 실행하기 위해 새로 만든 정렬 상태값을 활용합니다.
- [Lodash](https://lodash.com/)와 같은 유틸리티 라이브러리에 속한 `sortBy`와 같은 함수를 활용하길 추천합니다.

데이터 목록을 하나의 테이블로 생각해봅시다. 하나의 열은 목록의 한 아이템을 나타내고, 해당 열의 각 행은 해당 아이템의 속성을 나타냅니다. 헤더의 이름 등을 통해 사용자에게 각 행에 대한 정보를 제공할 수 있습니다.

{title="src/App.js",lang="javascript"}

```
# leanpub-start-insert
const List = ({ list, onRemoveItem }) => (
  <div>
    <div style={{ display: 'flex' }}>
      <span style={{ width: '40%' }}>Title</span>
      <span style={{ width: '30%' }}>Author</span>
      <span style={{ width: '10%' }}>Comments</span>
      <span style={{ width: '10%' }}>Points</span>
      <span style={{ width: '10%' }}>Actions</span>
    </div>=

    {list.map(item => (
# leanpub-end-insert
      <Item
        key={item.objectID}
        item={item}
        onRemoveItem={onRemoveItem}
      />
# leanpub-start-insert
    ))}
  </div>
);
# leanpub-end-insert
```

가장 기본적인 레이아웃을 만들기 위해 인라인 스타일을 추가합니다. 모든 열의 레이아웃을 헤더와 동일하게 맞추기 위해 각 Item 컴퍼넌트에도 헤더와 동일한 레이아웃을 적용합니다.

{title="src/App.js",lang="javascript"}

```
const Item = ({ item, onRemoveItem }) => (
# leanpub-start-insert
  <div style={{ display: 'flex' }}>
    <span style={{ width: '40%' }}>
# leanpub-end-insert
      <a href={item.url}>{item.title}</a>
    </span>
# leanpub-start-insert
    <span style={{ width: '30%' }}>{item.author}</span>
    <span style={{ width: '10%' }}>{item.num_comments}</span>
    <span style={{ width: '10%' }}>{item.points}</span>
    <span style={{ width: '10%' }}>
# leanpub-end-insert
      <button type="button" onClick={() => onRemoveItem(item)}>
        Dismiss
      </button>
    </span>
  </div>
);
```

다음 코드부터는 스타일 속성들을 표시 하지 않습니다. 스타일 속성은 공간을 많이 차지하고, 중요한 구현 로직을 보기 어렵게 만듭니다. (그래서 CSS 파일로 적절하게 분리했습니다.) 이는 가독성을 위한 위한 것이니 여러분의 코드는 그대로 두길 바랍니다.

정렬에 필요한 새로운 상태를 관리하는 주체는 List 컴퍼넌트입니다. 상태 관리를 App 컴퍼넌트에서 할 수도 있지만 List 컴퍼넌트에서 해도 충분하니 List 컴퍼넌트에 상태를 추가합니다. 추가한 정렬 상태는 `'NONE'`이라는 초기 값을 갖고, 목록은 API로부터 아이템을 받아온 순서 그대로 나타납니다. 그 다음으로, 정렬에 필요한 key를 인자로 받아 정렬 상태를 설정하는 핸들러 함수를 추가합니다.

{title="src/App.js",lang="javascript"}

```
# leanpub-start-insert
const List = ({ list, onRemoveItem }) => {
  const [sort, setSort] = React.useState('NONE');

  const handleSort = sortKey => {
    setSort(sortKey);
  };

  return (
# leanpub-end-insert
    ...
# leanpub-start-insert
  );
};
# leanpub-end-insert
```

List 컴퍼넌트의 헤더들을 버튼으로 만들어 각 행/속성으로 정렬 상태값을 설정할 수 있도록 합니다. 핸들러 함수를 인라인으로 추가해 정렬하려는 속성(`sortKey`)을 정렬 상태값에 추가합니다. "Title" 행의 버튼이 클릭되었을 때 `'TITLE'`이 새로운 정렬 상태의 값으로 설정됩니다.

{title="src/App.js",lang="javascript"}

```
const List = ({ list, onRemoveItem }) => {
  ...

  return (
    <div>
      <div>
        <span>
# leanpub-start-insert
          <button type="button" onClick={() => handleSort('TITLE')}>
            Title
          </button>
# leanpub-end-insert
        </span>
        <span>
# leanpub-start-insert
          <button type="button" onClick={() => handleSort('AUTHOR')}>
            Author
          </button>
# leanpub-end-insert
        </span>
        <span>
# leanpub-start-insert
          <button type="button" onClick={() => handleSort('COMMENT')}>
            Comments
          </button>
# leanpub-end-insert
        </span>
        <span>
# leanpub-start-insert
          <button type="button" onClick={() => handleSort('POINT')}>
            Points
          </button>
# leanpub-end-insert
        </span>
        <span>Actions</span>
      </div>

      {list.map(item => ... )}
    </div>
  );
};
```

새로운 기능을 위한 상태 관리를 구현했습니다. 하지만 아직 버튼을 클릭했을 때 어떤 액션이 이루어지지 않습니다. 아직 실제로 정렬이 동작하도록 `list`에 적용하지 않았기 때문입니다.

자바스크립트로 배열을 정렬할 때 생각보다 처리해줘야 하는 작업이 많습니다. 배열을 특정 속성으로 정렬하고자 하는데 각 요소로 string, boolean, number와 같은 기본 데이터 타입이 들어가 있어 엣지 케이스에 부딪힐 수 있기 때문입니다. 이를 방지하기 위해 [Lodash](https://lodash.com/)라는 라이브러리의 다양한 자바스크립트 유틸리티 함수들을 활용합니다 (예시: `sortBy`). 우선, 커맨드 라인에 명령어를 입력해 설치를 시작합니다.

{title="Command Line",lang="text"}

```
npm install lodash
```

둘째로, 파일 상단에 정렬을 위한 유틸리티 함수를 불러옵니다.

{title="src/App.js",lang="javascript"}

```
import React from 'react';
import axios from 'axios';
# leanpub-start-insert
import { sortBy } from 'lodash';
# leanpub-end-insert

...
```

셋째로, 자바스크립트 객체(dictionary라고 불리기도 합니다.)를 만들어 필요한 모든 `sortKey`들을 속성으로, 각 속성에 맞는 정렬 함수를 값으로 추가합니다. 속성으로 설정된 `sortKey`를 기준으로 `list`를 정렬하는 데 필요한 함수를 실행합니다. `sortKey`가 `'NONE'`이면 인자로 들어온 목록을 정렬하지 않은 채 그대로 반환하고 `'POINT'`일 땐 `points` 속성으로 아이템이 정렬된 목록을 반환합니다.

{title="src/App.js",lang="javascript"}

```
# leanpub-start-insert
const SORTS = {
  NONE: list => list,
  TITLE: list => sortBy(list, 'title'),
  AUTHOR: list => sortBy(list, 'author'),
  COMMENT: list => sortBy(list, 'num_comments').reverse(),
  POINT: list => sortBy(list, 'points').reverse(),
};
# leanpub-end-insert

const List = ({ list, onRemoveItem }) => {
  ...
};
```

`정렬 상태값` (`sortKey`)으로 우리는 필요한 정렬 기준을 만들고 얼마든지 필요한 형태로 목록을 정렬할 수 있습니다. Item 컴퍼넌트를 만들기 전에 목록을 정렬해주기만 하면 됩니다.

{title="src/App.js",lang="javascript"}

```
const List = ({ list, onRemoveItem }) => {
  const [sort, setSort] = React.useState('NONE');

  const handleSort = sortKey => {
    setSort(sortKey);
  };

# leanpub-start-insert
  const sortFunction = SORTS[sort];
  const sortedList = sortFunction(list);
# leanpub-end-insert

  return (
    <div>
      ...

# leanpub-start-insert
      {sortedList.map(item => (
# leanpub-end-insert
        <Item
          key={item.objectID}
          item={item}
          onRemoveItem={onRemoveItem}
        />
      ))}
    </div>
  );
};
```

이제 끝났습니다. 먼저 우리는 상태의 값인 `sortKey`로 dictionary 자료구조에서 필요한 정렬 함수를 찾았습니다. 그리고 이 정렬 함수로 목록을 먼저 정렬한 뒤에 render 함수 안에서 각각의 Item 컴퍼넌트를 만들었습니다. 한 번 복습해보겠습니다. 최초의 정렬 상태값은 `'NONE'`이었고 이는 정렬을 적용하지 않는다는 뜻입니다.

둘째로 사용자가 선택할 수 있는 HTML 버튼들을 만들었습니다. 그리고 버튼을 눌렀을 때 정렬 상태값이 바뀌도록 했습니다. 마지막으로 정렬 상태값을 활용해 실제 목록을 정렬했습니다.

### 연습문제:

- [마지막 섹션의 소스 코드](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Sort)를 확인합니다.
  - [마지막 섹션에서 변경된 코드](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/react-modern-final...hs/Sort?expand=1)를 확인합니다.
- [Lodash](https://lodash.com/) 자료를 더 읽어봅니다.
- `points`와 `num_comments`와 같이 숫자를 값으로 가진 속성에는 왜 역방향 정렬을 정렬했을까요?
- 사용자가 현재 선택된 정렬 기준이 무엇인지 잘 알 수 있도록 여러분만의 스타일링 기술을 사용해봅니다. 현재 선택된 정렬 기준 버튼에만 다른 색깔을 적용하는 것과 같이 단순한 방법이어도 좋습니다.

## 역방향 정렬

**과제:** 잘 동작하는 정렬을 구현했습니다. 하지만 아직 한 방향으로만 정렬됩니다. 버튼을 두 번 클릭하면 역방향으로 정렬되도록 구현하세요. 기본(오름차순) 정렬과 역방향(내림차순) 정렬을 껐다 키는 기능입니다.

**힌트 보기:**

- `sortKey` 상태와 함께, 기본 정렬 상태인지 역방향 정렬 상태인지는 또 다른 상태(예: `isReverse`)로 관리해보세요.
- 이전 정렬 상태를 기준으로 `handleSort` 핸들러에 새로운 상태를 추가합니다.
- SORTS 객체에서 반환한 정렬 함수에, 새로 추가한 `isReverse` 상태에 따라 자바스크립트 배열의 내장 함수인 `reverse()`를 적용해 배열을 정렬합니다.

기존에 만든 정렬은 숫자들을 높은 수부터 작은 수로 역방향 정렬해주는 것과 마찬가지로 문자열도 정렬해줍니다. 이제 정렬이 기본 방향인지 역방향인지 파악할 수 있는 상태를 추가해 좀 더 복잡한 작업을 해봅시다.

{title="src/App.js",lang="javascript"}

```
const List = ({ list, onRemoveItem }) => {
# leanpub-start-insert
  const [sort, setSort] = React.useState({
    sortKey: 'NONE',
    isReverse: false,
  });
# leanpub-end-insert

  ...
};
```

다음으로, 인자로 들어온 sortKey가 기본 방향 정렬인지 역방향 정렬인지 확인 로직을 핸들러 함수에 추가합니다. 만약 인자로 들어온 sortKey가 현재 sortKey 상태값과 같고 현재 역방랼 정렬이 되지 않은 상태라면 isReverse 상태값을 true로 합니다.

{title="src/App.js",lang="javascript"}

```
const List = ({ list, onRemoveItem }) => {
  const [sort, setSort] = React.useState({
    sortKey: 'NONE',
    isReverse: false,
  });

  const handleSort = sortKey => {
# leanpub-start-insert
    const isReverse = sort.sortKey === sortKey && !sort.isReverse;

    setSort({ sortKey: sortKey, isReverse: isReverse });
# leanpub-end-insert
  };

# leanpub-start-insert
  const sortFunction = SORTS[sort.sortKey];
# leanpub-end-insert
  const sortedList = sortFunction(list);

  return (
    ...
  );
};
```

마지막으로, dictionary에서 찾은 정렬 함수를 실행할 때 `isReverse` 상태값에 맞게 자바스크립트에 내장된 reverse 함수를 추가하도록 합니다.

{title="src/App.js",lang="javascript"}

```
const List = ({ list, onRemoveItem }) => {
  const [sort, setSort] = React.useState({
    sortKey: 'NONE',
    isReverse: false,
  });

  const handleSort = sortKey => {
    const isReverse = sort.sortKey === sortKey && !sort.isReverse;

# leanpub-start-insert
    setSort({ sortKey, isReverse });
# leanpub-end-insert
  };

  const sortFunction = SORTS[sort.sortKey];

# leanpub-start-insert
  const sortedList = sort.isReverse
    ? sortFunction(list).reverse()
    : sortFunction(list);
# leanpub-end-insert

  return (
    ...
  );
};
```

이제 역방향 정렬이 동작하도록 만들었습니다. 상태값 업데이트용 함수에 초기 인자로 넣어줄 객체는 만들 때는 **단축 객체 초기자 표기법**을 사용합니다.

{title="src/App.js",lang="javascript"}

```
const firstName = 'Robin';

const user = {
  firstName: firstName,
};

console.log(user);
// { firstName: "Robin" }
```

**단축 객체 초기자 표기법**을 사용하면 객체의 속성 이름과 해당 속성의 값에 할당하려는 변수의 이름이 같을 때, 속성 이름만 써주면 됩니다.

{title="src/App.js",lang="javascript"}

```
const firstName = 'Robin';

const user = {
  firstName,
};

console.log(user);
// { firstName: "Robin" }
```

이에 대한 내용이 더 궁금하다면 [JavaScript Object Initializers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)를 참고하세요.

### 연습문제:

- [마지막 섹션의 소스 코드](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Reverse-Sort))를 확인합니다.
  - [마지막 섹션에서 변경된 코드](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Sort...hs/Reverse-Sort?expand=1)를 확인합니다.
- App 컴퍼넌트가 아니라 List 컴퍼넌트 안에서 정렬 상태값을 관리할 때의 단점이 무엇일지 생각해보세요. 잘 모르겠다면 "Title'로 배열을 정렬한 뒤 다른 책을 검색해보세요. App 컴퍼넌트에서 정렬 상태값을 관리한다면 어떤 게 달라질까요?
- 사용자가 현재 선택된 정렬 기준과 정렬 방향이 무엇인지 잘 알 수 있도록 여러분만의 스타일링 기술을 사용해봅니다. 각 정렬 버튼 옆에 [위, 아래 화살표 SVG](https://www.flaticon.com/packs/arrow-set-2)를 추가하는 것도 하나의 방법입니다.

## Remember Last Searches

**Task:** Remember the last five search terms to hit the API, and provide a button to move quickly between searches. When the buttons are clicked, stories for the search term are fetched again.

**Optional Hints:**

- Don't use a new state for this feature. Instead, reuse the `url` state and `setUrl` state updater function to fetch stories from the API. Adapt them to multiple `urls` as state, and to set multiple `urls` with `setUrls`. The last URL from `urls` can be used to fetch the data, and the last five URLs from `urls` can be used to display the buttons.

First, we will refactor all `url` to `urls` state and all `setUrl` to `setUrls` state updater functions. Instead of initializing the state with a `url` as a string, make it an array with the initial `url` as its only entry:

{title="src/App.js",lang="javascript"}

```
const App = () => {
  ...

# leanpub-start-insert
  const [urls, setUrls] = React.useState([
# leanpub-end-insert
    `${API_ENDPOINT}${searchTerm}`,
# leanpub-start-insert
  ]);
# leanpub-end-insert

  ...
};
```

Second, instead of using the current `url` state for data fetching, use the last `url` entry from the `urls` array. If another `url` is added to the list of `urls`, it is used to fetch data instead:

{title="src/App.js",lang="javascript"}

```
const App = () => {

  ...

  const handleFetchStories = React.useCallback(async () => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    try {
# leanpub-start-insert
      const lastUrl = urls[urls.length - 1];
      const result = await axios.get(lastUrl);
# leanpub-end-insert

      dispatchStories({
        type: 'STORIES_FETCH_SUCCESS',
        payload: result.data.hits,
      });
    } catch {
      dispatchStories({ type: 'STORIES_FETCH_FAILURE' });
    }
# leanpub-start-insert
  }, [urls]);
# leanpub-end-insert

  ...
};
```

And third, instead of storing `url` string as state with the state updater function, concat the new `url` with the previous `urls` in an array for the new state:

{title="src/App.js",lang="javascript"}

```
const App = () => {
  ...

  const handleSearchSubmit = event => {
# leanpub-start-insert
    const url = `${API_ENDPOINT}${searchTerm}`;
    setUrls(urls.concat(url));
# leanpub-end-insert

    event.preventDefault();
  };

  ...
};
```

With each search, another URL is stored in our state of `urls`. Next, render a button for each of the last five URLs. We'll include a new universal handler for these buttons, and each passes a specific `url` with a more specific inline handler:

{title="src/App.js",lang="javascript"}

```
# leanpub-start-insert
const getLastSearches = urls => urls.slice(-5);
# leanpub-end-insert

...

const App = () => {
  ...

# leanpub-start-insert
  const handleLastSearch = url => {
    // do something
  };
# leanpub-end-insert

# leanpub-start-insert
  const lastSearches = getLastSearches(urls);
# leanpub-end-insert

  return (
    <div>
      <h1>My Hacker Stories</h1>

      <SearchForm ... />

# leanpub-start-insert
      {lastSearches.map(url => (
        <button
          key={url}
          type="button"
          onClick={() => handleLastSearch(url)}
        >
          {url}
        </button>
      ))}
# leanpub-end-insert

      ...
    </div>
  );
};
```

Next, instead of showing the whole URL of the last search in the button as button text, show only the search term by replacing the API's endpoint with an empty string:

{title="src/App.js",lang="javascript"}

```
# leanpub-start-insert
const extractSearchTerm = url => url.replace(API_ENDPOINT, '');
# leanpub-end-insert

# leanpub-start-insert
const getLastSearches = urls =>
  urls.slice(-5).map(url => extractSearchTerm(url));
# leanpub-end-insert

...

const App = () => {
  ...

  const lastSearches = getLastSearches(urls);

  return (
    <div>
      ...

      {lastSearches.map(searchTerm => (
        <button
# leanpub-start-insert
          key={searchTerm}
# leanpub-end-insert
          type="button"
# leanpub-start-insert
          onClick={() => handleLastSearch(searchTerm)}
# leanpub-end-insert
        >
# leanpub-start-insert
          {searchTerm}
# leanpub-end-insert
        </button>
      ))}

      ...
    </div>
  );
};
```

The `getLastSearches` function now returns search terms instead of URLs. The actual `searchTerm`is passed to the inline handler instead of the `url`. By mapping over the list of `urls` in `getLastSearches`, we can extract the search term for each `url` within the array's map method. Making it more concise, it can also look like this:

{title="src/App.js",lang="javascript"}

```
const getLastSearches = urls =>
# leanpub-start-insert
  urls.slice(-5).map(extractSearchTerm);
# leanpub-end-insert
```

Now we'll provide functionality for the new handler used by every button, since clicking one of these buttons should trigger another search. Since we use the `urls` state for fetching data, and since we know the last URL is always used for data fetching, concat a new `url` to the list of `urls` to trigger another search request:

{title="src/App.js",lang="javascript"}

```
const App = () => {
  ...

# leanpub-start-insert
  const handleLastSearch = searchTerm => {
    const url = `${API_ENDPOINT}${searchTerm}`;
    setUrls(urls.concat(url));
  };
# leanpub-end-insert

  ...
};
```

If you compare this new handler's implementation logic to the `handleSearchSubmit`, you may see some common functionality. Extract this common functionality to a new handler and a new extracted utility function:

{title="src/App.js",lang="javascript"}

```
# leanpub-start-insert
const getUrl = searchTerm => `${API_ENDPOINT}${searchTerm}`;
# leanpub-end-insert

...

const App = () => {
  ...

  const handleSearchSubmit = event => {
# leanpub-start-insert
    handleSearch(searchTerm);
# leanpub-end-insert

    event.preventDefault();
  };

  const handleLastSearch = searchTerm => {
# leanpub-start-insert
    handleSearch(searchTerm);
# leanpub-end-insert
  };

# leanpub-start-insert
  const handleSearch = searchTerm => {
    const url = getUrl(searchTerm);
    setUrls(urls.concat(url));
  };
# leanpub-end-insert

  ...
};
```

The new utility function can be used somewhere else in the App component. If you extract functionality that can be used by two parties, always check to see if it can be used by a third party.

{title="src/App.js",lang="javascript"}

```
const App = () => {
  ...

  // important: still wraps the returned value in []
# leanpub-start-insert
  const [urls, setUrls] = React.useState([getUrl(searchTerm)]);
# leanpub-end-insert

  ...
};
```

The functionality should work, but it complains or breaks if the same search term is used more than once, because `searchTerm` is used for each button element as `key` attribute. Make the key more specific by concatenating it with the `index` of the mapped array.

{title="src/App.js",lang="javascript"}

```
const App = () => {
  ...

  return (
    <div>
      ...

# leanpub-start-insert
      {lastSearches.map((searchTerm, index) => (
# leanpub-end-insert
        <button
# leanpub-start-insert
          key={searchTerm + index}
# leanpub-end-insert
          type="button"
          onClick={() => handleLastSearch(searchTerm)}
        >
          {searchTerm}
        </button>
      ))}

      ...
    </div>
  );
};
```

It's not the perfect solution, because the `index` isn't a stable key (especially when adding items to the list; however, it doesn't break in this scenario. The feature works now, but you can add further UX improvements by following the tasks below.

**More Tasks:**

- (1) Do not show the current search as a button, only the five preceding searches. Hint: Adapt the `getLastSearches` function.
- (2) Don't show duplicated searches. Searching twice for "React" shouldn't create two different buttons. Hint: Adapt the `getLastSearches` function.
- (3) Set the SearchForm component's input field value with the last search term if one of the buttons is clicked.

The source of the five rendered buttons is the `getLastSearches` function. There, we take the array of `urls` and return the last five entries from it. Now we'll change this utility function to return the last six entries instead of five, removing the last one. Afterward, only the five _previous_ searches are displayed as buttons.

{title="src/App.js",lang="javascript"}

```
const getLastSearches = urls =>
  urls
# leanpub-start-insert
    .slice(-6)
    .slice(0, -1)
# leanpub-end-insert
    .map(extractSearchTerm);
```

If the same search is executed twice or more times in a row, duplicate buttons appear, which is likely not your desired behavior. It would be acceptable to group identical searches into one button if they followed each other. We will solve this problem in the utility function as well. Before separating the array into the five previous searches, group the identical searches:

{title="src/App.js",lang="javascript"}

```
const getLastSearches = urls =>
  urls
# leanpub-start-insert
    .reduce((result, url, index) => {
      const searchTerm = extractSearchTerm(url);

      if (index === 0) {
        return result.concat(searchTerm);
      }

      const previousSearchTerm = result[result.length - 1];

      if (searchTerm === previousSearchTerm) {
        return result;
      } else {
        return result.concat(searchTerm);
      }
    }, [])
# leanpub-end-insert
    .slice(-6)
    .slice(0, -1);
```

The reduce function starts with an empty array as its `result`. The first iteration concats the `searchTerm` we extracted from the first `url` into the `result`. Every extracted `searchTerm` is compared to the one before it. If the previous search term is different from the current, concat the `searchTerm` to the result. If the search terms are identical, return the result without adding anything.

Lastly, the SearchForm's input field should be set with the new `searchTerm` if one of the last search buttons is clicked. We can solve this using the state updater function for the specific value used in the SearchForm component.

{title="src/App.js",lang="javascript"}

```
const App = () => {
  ...

  const handleLastSearch = searchTerm => {
# leanpub-start-insert
    setSearchTerm(searchTerm);
# leanpub-end-insert

    handleSearch(searchTerm);
  };

  ...
};
```

Last, extract the feature's new rendered content from this section as a standalone component, to keep the App component lightweight:

{title="src/App.js",lang="javascript"}

```
const App = () => {
  ...

  const lastSearches = getLastSearches(urls);

  return (
    <div>
      ...

# leanpub-start-insert
      <LastSearches
        lastSearches={lastSearches}
        onLastSearch={handleLastSearch}
      />
# leanpub-end-insert

      ...
    </div>
  );
};

# leanpub-start-insert
const LastSearches = ({ lastSearches, onLastSearch }) => (
  <>
    {lastSearches.map((searchTerm, index) => (
      <button
        key={searchTerm + index}
        type="button"
        onClick={() => onLastSearch(searchTerm)}
      >
        {searchTerm}
      </button>
    ))}
  </>
);
# leanpub-end-insert
```

This feature wasn't an easy one. Lots of fundamental React but also JavaScript knowledge was needed to accomplish it. If you had no problems implementing it yourself or to follow the instructions, you are very well set. If you had one or the other issue, don't worry too much about it. Maybe you even figured out another way to solve this task and it may have turned out simpler than the one I showed here.

### Exercises:

- Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Remember-Last-Searches).
  - Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Reverse-Sort...hs/Remember-Last-Searches?expand=1).

## Paginated Fetch

Searching for popular stories via Hacker News API is only one step towards a fully-functional search engine, and there are many ways to fine-tune the search. Take a closer look at the data structure and observe how [the Hacker News API](https://hn.algolia.com/api) returns more than a list of `hits`.

Specifically, it returns a paginated list. The page property, which is `0` in the first response, can be used to fetch more paginated lists as results. You only need to pass the next page with the same search term to the API.

The following shows how to implement a paginated fetch with the Hacker News data structure. If you are used to **pagination** from other applications, you may have a row of buttons from 1-10 in your mind -- where the currently selected page is highlighted 1-[3]-10 and where clicking one of the buttons leads to fetching and displaying this subset of data.

In contrast, we will implement the feature as **infinite pagination**. Instead of rendering a single paginated list on a button click, we will render _all paginated lists as one list_ with _one_ button to fetch the next page. Every additional _paginated list_ is concatenated at the end of the _one list_.

**Task:** Rather than fetching only the first page of a list, extend the functionality for fetching succeeding pages. Implement this as an infinite pagination on button click.

**Optional Hints:**

- Extend the `API_ENDPOINT` with the parameters needed for the paginated fetch.
- Store the `page` from the `result` as state after fetching the data.
- Fetch the first page (`0`) of data with every search.
- Fetch the succeeding page ( `page + 1`) for every additional request triggered with a new HTML button.

First, extend the API constant so it can deal with paginated data later. We will turn this one constant:

{title="src/App.js",lang="javascript"}

```
const API_ENDPOINT = 'https://hn.algolia.com/api/v1/search?query=';

const getUrl = searchTerm => `${API_ENDPOINT}${searchTerm}`;
```

Into a composable API constant with its parameters:

{title="src/App.js",lang="javascript"}

```
# leanpub-start-insert
const API_BASE = 'https://hn.algolia.com/api/v1';
const API_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
# leanpub-end-insert

// careful: notice the ? in between
# leanpub-start-insert
const getUrl = searchTerm =>
  `${API_BASE}${API_SEARCH}?${PARAM_SEARCH}${searchTerm}`;
# leanpub-end-insert
```

Fortunately, we don't need to adjust the API endpoint, because we extracted a common `getUrl` function for it. However, there is one spot where we must address this logic for the future:

{title="src/App.js",lang="javascript"}

```
const extractSearchTerm = url => url.replace(API_ENDPOINT, '');
```

In the next steps, it won't be sufficient to replace the base of our API endpoint, which is no longer in our code. With more parameters for the API endpoint, the URL becomes more complex. It will change from X to Y:

{title="src/App.js",lang="javascript"}

```
// X
https://hn.algolia.com/api/v1/search?query=react

// Y
https://hn.algolia.com/api/v1/search?query=react&page=0
```

It's better to extract the search term by extracting everything between `?` and `&`. Also consider that the `query` parameter is directly after the `?` and all other parameters like `page` follow it.

{title="src/App.js",lang="javascript"}

```
# leanpub-start-insert
const extractSearchTerm = url =>
  url
    .substring(url.lastIndexOf('?') + 1, url.lastIndexOf('&'));
# leanpub-end-insert
```

The key ( `query=`) also needs to be replaced, leaving only the value (`searchTerm`):

{title="src/App.js",lang="javascript"}

```
const extractSearchTerm = url =>
  url
    .substring(url.lastIndexOf('?') + 1, url.lastIndexOf('&'));
# leanpub-start-insert
    .replace(PARAM_SEARCH, '');
# leanpub-end-insert
```

Essentially, we'll trim the string until we leave only the search term:

{title="src/App.js",lang="javascript"}

```
// url
https://hn.algolia.com/api/v1/search?query=react&page=0

// url after  substring
query=react

// url after replace
react
```

The returned result from the Hacker News API delivers us the `page` data:

{title="src/App.js",lang="javascript"}

```
const App = () => {
  ...

  const handleFetchStories = React.useCallback(async () => {
    dispatchStories({ type: 'STORIES_FETCH_INIT' });

    try {
      const lastUrl = urls[urls.length - 1];
      const result = await axios.get(lastUrl);

      dispatchStories({
        type: 'STORIES_FETCH_SUCCESS',
# leanpub-start-insert
        payload: {
          list: result.data.hits,
          page: result.data.page,
        },
# leanpub-end-insert
      });
    } catch {
      dispatchStories({ type: 'STORIES_FETCH_FAILURE' });
    }
  }, [urls]);

  ...
};
```

We need to store this data to make paginated fetches later:

{title="src/App.js",lang="javascript"}

```
const storiesReducer = (state, action) => {
  switch (action.type) {
    case 'STORIES_FETCH_INIT':
      ...
    case 'STORIES_FETCH_SUCCESS':
      return {
        ...state,
        isLoading: false,
        isError: false,
# leanpub-start-insert
        data: action.payload.list,
        page: action.payload.page,
# leanpub-end-insert
      };
    case 'STORIES_FETCH_FAILURE':
      ...
    case 'REMOVE_STORY':
      ...
    default:
      throw new Error();
  }
};

const App = () => {
  ...

  const [stories, dispatchStories] = React.useReducer(
    storiesReducer,
# leanpub-start-insert
    { data: [], page: 0, isLoading: false, isError: false }
# leanpub-end-insert
  );

  ...
};
```

Extend the API endpoint with the new `page` parameter. This change was covered by our premature optimizations earlier, when we extracted the search term from the URL.

{title="src/App.js",lang="javascript"}

```
const API_BASE = 'https://hn.algolia.com/api/v1';
const API_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
# leanpub-start-insert
const PARAM_PAGE = 'page=';
# leanpub-end-insert

// careful: notice the ? and & in between
# leanpub-start-insert
const getUrl = (searchTerm, page) =>
  `${API_BASE}${API_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}`;
# leanpub-end-insert
```

Next, we must adjust all `getUrl` invocations by passing the `page` argument. Since the initial search and last search always fetch the first page (`0`), we pass this page as an argument to the function for retrieving the appropriate URL:

{title="src/App.js",lang="javascript"}

```
const App = () => {
  ...

# leanpub-start-insert
  const [urls, setUrls] = React.useState([getUrl(searchTerm, 0)]);
# leanpub-end-insert

  ...

  const handleSearchSubmit = event => {
# leanpub-start-insert
    handleSearch(searchTerm, 0);
# leanpub-end-insert

    event.preventDefault();
  };

  const handleLastSearch = searchTerm => {
    setSearchTerm(searchTerm);

# leanpub-start-insert
    handleSearch(searchTerm, 0);
# leanpub-end-insert
  };

# leanpub-start-insert
  const handleSearch = (searchTerm, page) => {
    const url = getUrl(searchTerm, page);
# leanpub-end-insert
    setUrls(urls.concat(url));
  };

  ...
};
```

To fetch the next page when a button is clicked, we'll need to increment the `page` argument in this new handler:

{title="src/App.js",lang="javascript"}

```
const App = () => {
  ...

# leanpub-start-insert
  const handleMore = () => {
    const lastUrl = urls[urls.length - 1];
    const searchTerm = extractSearchTerm(lastUrl);
    handleSearch(searchTerm, stories.page + 1);
  };
# leanpub-end-insert

  ...

  return (
    <div>
      ...

      {stories.isLoading ? (
        <p>Loading ...</p>
      ) : (
        <List list={stories.data} onRemoveItem={handleRemoveStory} />
      )}

# leanpub-start-insert
      <button type="button" onClick={handleMore}>
        More
      </button>
# leanpub-end-insert
    </div>
  );
};
```

We've implemented data fetching with the dynamic `page` argument. The initial and last searches always use the first page, and every fetch with the new "More" button uses an incremented page. There is one crucial bug when trying the feature, though: the new fetches don't extend the previous list, but completely replace it.

We solve this in the reducer by avoiding the replacement of current `data` with new `data`, concatenating the paginated lists:

{title="src/App.js",lang="javascript"}

```
const storiesReducer = (state, action) => {
  switch (action.type) {
    case 'STORIES_FETCH_INIT':
      ...
    case 'STORIES_FETCH_SUCCESS':
      return {
        ...state,
        isLoading: false,
        isError: false,
# leanpub-start-insert
        data:
          action.payload.page === 0
            ? action.payload.list
            : state.data.concat(action.payload.list),
# leanpub-end-insert
        page: action.payload.page,
      };
    case 'STORIES_FETCH_FAILURE':
      ...
    case 'REMOVE_STORY':
      ...
    default:
      throw new Error();
  }
};
```

The displayed list grows after fetching more data with the new button. However, there is still a flicker straining the UX. When fetching paginated data, the list disappears for a moment because the loading indicator appears and reappears after the request resolves.

The desired behavior is to render the list--which is an empty list in the beginning--and replace the "More" button with the loading indicator only for pending requests. This is a common UI refactoring for conditional rendering when the task evolves from a single list to paginated lists.

{title="src/App.js",lang="javascript"}

```
const App = () => {
  ...

  return (
    <div>
      ...

# leanpub-start-insert
      <List list={stories.data} onRemoveItem={handleRemoveStory} />
# leanpub-end-insert

      {stories.isLoading ? (
        <p>Loading ...</p>
      ) : (
# leanpub-start-insert
        <button type="button" onClick={handleMore}>
          More
        </button>
# leanpub-end-insert
      )}
    </div>
  );
};
```

It's possible to fetch ongoing data for popular stories now. When working with third-party APIs, it's always a good idea to explore its boundaries. Every remote API returns different data structures, so its features may vary, and can be used in applications that consume the API.

### Exercises:

- Confirm your [source code for the last section](https://codesandbox.io/s/github/the-road-to-learn-react/hacker-stories/tree/hs/Paginated-Fetch).
  - Confirm the [changes from the last section](https://github.com/the-road-to-learn-react/hacker-stories/compare/hs/Remember-Last-Searches...hs/Paginated-Fetch?expand=1).
- Revisit the [Hacker News API documentation](https://hn.algolia.com/api): Is there a way to fetch more items in a list for a page by just adding further parameters to the API endpoint?
- Revisit the beginning of this section which speaks about pagination and infinite pagination. How would you implement a normal pagination component with buttons from 1-[3]-10, where each button fetches and displays only one page of the list.
- Instead of having one "More" button, how would you implement an infinite pagination with an infinite scroll technique? Rather than clicking a button for fetching the next page explicitly, the infinite scroll could fetch the next page once the viewport of the browser hits the bottom of the displayed list.
