
## 파이어베이스(Firebase) 배포

완전한 리액트 어플리케이션을 만든후, 마지막 과정은 배포입니다. 코드를 작성하는 배우는 것부터 어플리케이션을 제작하는 과정까지 중 핵심은 세상에 아이디어를 펼치는 것입니다. 파이어베이스(Firebase) 호스팅을 통해 어플리케이션을 배포합니다.

파이어베이스는 create-react-app 뿐 만 아니라, 대부분의 라이브러리들과 Angular, Vue 같은 프레임워크에도 적용됩니다. 처음으로, 파이어베이스 CLI를 노드 모듈들에 global하게 설치합니다:

{title="Command Line",lang="javascript"}
~~~~~~~
npm install -g firebase-tools
~~~~~~~

파이어베이스 CLI의 global 설치를 사용하면 프로젝트의 의존성을 신경쓰지 않고 어플리케이션을 배포 할 수 있습니다. global하게 설치된 노드 패키지는, 커맨드를 가능한 자주 업데이트 해 최신버전으로 유지하는 것을 잊지맙시다:

{title="Command Line",lang="javascript"}
~~~~~~~
npm install -g firebase-tools
~~~~~~~

만약 아직 파이어베이스 프로젝트가 없다면, [파이어베스 계정](https://console.firebase.google.com/?pli=1)을 만들고 새로운 프로젝트를 생성합니다. 그렇게 하면 파이어베이스 CLI와 파이베이스 계정(구글 계정)과 연결할 수 있습니다: 
{title="Command Line",lang="javascript"}
~~~~~~~
firebase login
~~~~~~~

브라우져로 열 수 있는 URL이 커맨드라인에 표시되거나 파이어베이스 CLI가 해당 URL을 엽니다. 구글 계정 선택해 파이어베이스 프로젝트를 선택하고 구글의 권한요청을 승인합니다. 컨맨드라인으로 돌아와 로그인이 성공적으로 이루어졌는지 확인합니다.

다음으로, 프로젝트의 폴더로 이동해, 파이어베이스의 호스팅 feature들과 관련된 파이어베이스 프로젝트를 초기화하는 아래 컨맨드를 실행합니다.

{title="Command Line",lang="javascript"}
~~~~~~~
firebase init
~~~~~~~

다음으로, 호스팅 옵션을 선택합니다.  만약 파이어베이스 호스팅과 같은 다른 도구들에도 관심이 있다면 다른 옵션들도 추가합니다 :

{title="Command Line",lang="javascript"}
~~~~~~~
? Which Firebase CLI features do you want to set up for this folder? Press Space to select features, then Enter to confirm your choices.
 ? Database: Deploy Firebase Realtime Database Rules
 ? Firestore: Deploy rules and create indexes for Firestore
 ? Functions: Configure and deploy Cloud Functions
-> Hosting: Configure and deploy Firebase Hosting sites
 ? Storage: Deploy Cloud Storage security rules
~~~~~~~

로그인을 하면 구글이 계정과 관련된 모든 파이어베이스 프로젝트들을 인식하고, 인식된 프로젝트는 파이베이스 플랫폼에서 선택 가능합니다.

{title="Command Line",lang="javascript"}
~~~~~~~
First, let's associate this project directory with a Firebase project.
You can create multiple project aliases by running firebase use --add,
but for now we'll just set up a default project.

? Select a default Firebase project for this directory:
-> my-react-project-abc123 (my-react-project)
i  Using project my-react-project-abc123 (my-react-project)
~~~~~~~

아직 몇 개의 환경설정이 남았습니다. 기본적으로 설정된 *public/* 폴더 대신 create-react-app의 *build/*폴더를 사용할 수 있습니다. Webpack과 같은 번들링 툴로 설정 한 경우 빌드 폴더에 적절한 이름을 선택할 수 있습니다 :

{title="Command Line",lang="javascript"}
~~~~~~~
? What do you want to use as your public directory? build
? Configure as a single-page app (rewrite all urls to /index.html)? Yes
? File public/index.html already exists. Overwrite? No
~~~~~~~

create-react-app 어플리케이션은 `npm run build` 를 처음 실행하고 나면 *build*/ 폴더를 생성합니다. *build/*  폴더에는 *public/* 폴더와 *src/* 폴더에서 합쳐진 모든 자료들이 있습니다.  이것은 한 페이지로 이루어진 어플리케이션이기 때문에 사용자를 *index.html* 파일로 리디렉트해 리액트 라우터가 클라이언트-사이드를 처리하도록 합니다.

이제 파이어베이스 초기화는 끝났습니다. 이 과정은 몇개의 파이어베이스 포스팅을 위한 설정 파일들을 프로젝트 폴더에 생성했습니다.  [파이어베이스 도큐멘트](https://firebase.google.com/docs/hosting/full-config) 에서 리디렉트 동작구성, 404 페이지, 헤더 등과 관련해 더 읽을 수 있습니다. 끝으로, 커맨드라인을 통해 파이어베이스로 리액트 어플리케이션을 배포합니다.

{title="Command Line",lang="javascript"}
~~~~~~~
firebase deploy
~~~~~~~

성공적으로 배포를 하고 나면, 작성한 프로젝트와 결과가 같은지 확인을 해야합니다:

{title="Command Line",lang="javascript"}
~~~~~~~
Project Console: https://console.firebase.google.com/project/my-react-project-abc123/overview
Hosting URL: https://my-react-project-abc123.firebaseapp.com
~~~~~~~

프로젝트 콘솔의 주소와 호스팅 URL의 주소 결과를 모두 확인합니다. 첫번째 링크는 새로운 파이어베이스 호스팅 패널을 볼 수 있는 파이어베이스 프로젝트 대쉬보드로 이동합니다. 두번째 링크는 배포한 리액트 어플리케이션으로 이동합니다. 

만약 두번째 링크에서 빈페이지가 보인다면,  *firebase.json*의 `public` key/value pair가 `build` 폴더 혹은 이름이 변경된 폴더에 올바르게 설정되었는지 확인합니다. 다음으로, 리액트 앱을  `npm run build`로 빌드 스크립트를 실행했는지 체크합니다. 마지막으로, [공식적인 파이어베이스를 통한  create-react-app배포의 문제해결(official troubleshoot area for deploying create-react-app applications to Firebase)](https://create-react-app.dev/docs/deployment)을 확인해봅니다.  `firebase deploy`로 배포를 다시 시도합니다.

### 읽어보기

- [파이어베이스 호스팅](https://firebase.google.com/docs/hosting/) 더 읽어보기

* [파이어베이스로 배포한 어플리케이션에 컴스텀 도메인 연결하기](https://firebase.google.com/docs/hosting/custom-domain)
* 선택 사항: 만약 클라우드 관리 서버를 원한다면, [DigitalOcean](https://www.digitalocean.com/?refcode=fb27c90322f3&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=CopyPaste)을 통해 확인 가능합니다. 이것은 해야할 일은 늘어나지만, 더 많은 통제를 할 수 있도록 합니다. [저는 그것을 통해 저의 웹 사이트, 웹어플리케이션, 백엔드 API들을 호스트 합니다.](https://www.robinwieruch.de/deploy-applications-digital-ocean/)
