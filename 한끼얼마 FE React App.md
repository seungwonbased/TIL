# 한끼얼마 Frontend React Application Report

> [📹 한끼얼마 Demo Video](https://youtu.be/bordHSSc1G4)

## 1. 👥 팀 소개
### 1.1. 멤버

- 배승원 (https://github.com/seungwonbased)
	- Team Leader
	- Frontend Dev
- 서종훈 (https://github.com/seojonghon)
	- Frontend Dev
- 이지윤 (https://github.com/jiiiyunn)
	- Frontend Dev
- 정지환 (https://github.com/hwan515)
	- Frontend Dev
- 이진욱 (https://github.com/dljinook)
	- Frontend Dev
- 김지훈
	- Project Manager

### 1.2. 팀 문화: 조금 더 개발자스러운 Workflow, 커뮤니케이션
#### 1.2.1. Git, GitHub를 이용한 협업
- 1일차에 회의를 통해 Git, Github Workflow와 컨벤션 정의
#### Workflow
1. 원본 리포지토리를 각자의 계정으로 `fork`
2. `git clone` 명령어로 `fork` 떠온 리포지토리를 로컬 환경으로 가져감
3. 원본 원격 리포지토리를 `remote add` 해 연결
4. 로컬 환경에 네이밍 컨벤션을 따라 `branch` 생성
5. 기능 구현 및 개발
6. 메세지 컨벤션에 따라 `commit`
7. 현재 작업 중인 `branch`에 `push`
8. `pull request`
9. 다 같이 코드 리뷰 후 `main branch` 에 `merge`
#### Convention
- Branch Naming Convention
	- feature-기능_이름: 기능 개발 시 사용
	- fix-기능_이름: 버그 수정 시 사용
	- docs-문서_이름: 문서를 생성하거나 수정할 때 사용
	- refactor-기능_이름: 기능을 리팩터링할 때 사용
- Commit Message Convention
	- 간결히 한 줄로 표현
	- Header
		- feat: 새로운 기능에 대한 커밋
		- fix: 버그 수정에 대한 커밋
		- docs: 문서 수정에 대한 커밋
		- refactor: 리팩토링에 대한 커밋
	- 예시
```bash
git commit -m "fix: 로그인 토큰 오류 수정"
```
#### Pull Request Message Convention
- PR의 목적을 한 문장으로 표현
- 피드백 받기를 원하는 지점을 명시

#### 1.2.2. 커뮤니케이션 툴: Discord

```
- CHAT CHANNEL
	# 라운지
	# 공지
	# 아이디어
	# 레퍼런스
	# 이슈
	# 질문
	# 파일-및-코드
- MEMO CHANNEL
	# 승원
	# 지훈
	# 종훈
	# 진욱
	# 지윤
	# 지환
- VOICE CHANNEL
	# 미팅
```

- \# 라운지: 자유롭게 토론
- \# 공지: 미팅, 일정, 회의록 공지
- \# 아이디어: 번뜩이는 아이디어
- \# 레퍼런스: 참고할만한 서비스, 자료 레퍼런스
- \# 이슈: 개발 및 프로젝트 진행에 있어 생기는 이슈
- \# 질문: 진짜 아무 질문이나
- \# 파일-및-코드: 파일 및 코드 공유
- \# 미팅: 보이스 & 페이스 미팅

- 불필요한 회의를 줄이는 효율적인 커뮤니케이션 구축
- 목적에 맞는 채널에 자유롭게 의견 공유 및 업무 공지

## 2. 📁 디렉터리 구조 

```
📁 recipe-book
├──── 📁 node_modules
├──── 📁 src
│      ├──── 📁 api
│      ├──── 📁 assets
│      ├──── 📁 atom
│      ├──── 📁 components
│      ├──── 📁 layout
│      ├──── 📁 pages
│      ├──── 📁 routes
│      ├──── 📁 style
│      ├──── 📄 app.js
│      └──── 📄 index.js
├──── 📄 package.json
└──── 📄 README.md
```

- **node_modules**
	- 프로젝트의 종속성 및 패키지가 저장되는 디렉터리
	- 이 디렉터리는 일반적으로 자동으로 생성되며, 프로젝트에서 사용하는 라이브러리와 패키지를 관리
- **src**
	- 소스 코드 및 리소스 파일을 저장하는 주요 디렉터리
	- **api**
	  - 서버와 통신하거나 데이터를 가져오는 API 요청을 수행하는 코드를 포함하는 디렉터리
	- **assets**
	  - 이미지, 폰트 및 기타 정적 자원을 저장
  - **atom**
	  - Recoil 상태 관리에서 사용하는 Atom 상태를 정의하는 코드를 저장
	- **components**
	  - 재사용 가능한 컴포넌트 파일을 저장, 모듈화 목적
	- **layout**
	  - 애플리케이션의 전체 레이아웃 및 공통 컴포넌트를 정의하는 코드를 포함하는 디렉터리
	- **pages**
	  - 각 페이지에 대한 컴포넌트 코드를 저장
	- **routes**
	  - 라우팅 구성 및 경로 관리를 담당하는 코드를 포함하는 디렉터리
	- **style**
	  - 전역 스타일 시트를 저장
	- **app.js**
	  - 애플리케이션의 진입점이 되는 파일
	- **index.js**
	  - 리액트 애플리케이션을 실제 DOM에 렌더링하는 파일
- **package.json**
  - 프로젝트 설정, 스크립트 및 종속성을 관리하는 파일

## 3. 🔑 애플리케이션 특징
### 3.1. 상태 관리: Recoil

- Meta에서 개발한 상태 관리 라이브러리
- React를 사용할 때 상태의 중앙 집중화, 상태의 불변성 유지, 데이터 흐름의 제어와 같은 역할
- 사용 목적
	- 데이터 관리
	- 데이터 동기화
	- 상태 변경
	- 상태 표현
- Atom은 상태를 저장하는 데 사용하는 기본적인 단위
- 본 프로젝트의 Atom 예시

```jsx
// Atom.jsx
import { atom } from 'recoil';
import { recoilPersist } from 'recoil-persist';

// recoil-persist를 사용하여 상태를 로컬 스토리지에 지속적으로 저장하는 설정
const { persistAtom } = recoilPersist();

// 엑세스 토큰 상태
export const accessTokenAtom = atom({
  key: 'accessToken',
  default: '', // 초기값은 빈 문자열
  effects_UNSTABLE: [persistAtom], // recoil-persist로 지속적인 상태 저장 설정 적용
});

// CSRF 토큰 상태
export const csrfTokenAtom = atom({
  key: 'csrfToken',
  default: '', // 초기값은 빈 문자열
  effects_UNSTABLE: [persistAtom], // recoil-persist로 지속적인 상태 저장 설정 적용
});

// 로그인 현황 상태
export const isLoginAtom = atom({
  key: 'isLogin',
  default: false, // 초기값은 false (로그아웃 상태)
  effects_UNSTABLE: [persistAtom], // recoil-persist로 지속적인 상태 저장 설정 적용
});
```

- 본 프로젝트에서 Recoil 훅 함수를 사용하는 예시
	- accessToken에 토큰을 저장하고, setAccessToken으로 토큰 값을 업데이트
	- csrfToken에 토큰을 저장하고, setCsrfToken으로 토큰 값을 업데이트
	- isLogin에 로그인 상태를 저장하고, setIsLogin으로 로그인 상태를 업데이트

```jsx
const [accessToken, setAccessToken] = useRecoilState(accessTokenAtom);
const [csrfToken, setCsrfToken] = useRecoilState(csrfTokenAtom);
const [isLogin, setIsLogin] = useRecoilState(isLoginAtom);
```

- 본 프로젝트에서는 사용자의 로그인 상태, 액세스 & CSRF 토큰의 상태 등을 Recoil로 상태 관리함
- 쉬운 문법으로 컴포넌트 간 데이터 공유를 할 수 있어 선택함

### 3.2. Styled Components

- React에서 CSS 스타일을 정의하고 관리하기 위한 라이브러리
- JS 파일에서 스타일을 정의하고 컴포넌트화할 수 있어 스타일 관리와 재사용성 향상
- 기존 CSS의 문법과 거의 유사해 쉬움
- 본 프로젝트에서 Styled Components를 사용한 예시

```jsx
function PostList() {

  // 생략

  return (
    <LayoutWrapper>
      <HeadWrapper>
        {/* 레시피 등록 버튼 */}
        <Button
          onClick={formButton}
          content='레시피 등록하기'
          backgroundcolor='white'
          color='black'
          style={{ border: '3px solid black' }}
        />
        <InputContainer>
          {/* 검색 입력 필드 */}
          <Input width='250px' style={{ padding: '0' }} />
          {/* 검색 아이콘 */}
          <SearchIcon
            style={{ position: 'absolute',
                     right: '0px', 
                     top: '30px',
                     width: "20px", 
                     height: "20px" }}
          />
        </InputContainer>
      </HeadWrapper>
      <StyledTable>
        <ListHeadWrapper>
          <th style={{ width: "10%" }}>번호</th>
          <td style={{ width: "40%" }}>요리 이름</td>
          <th style={{ width: "25%" }}>공유자</th>
          <th style={{ width: "25%" }}>작성일</th>
        </ListHeadWrapper>
        {data.posts.length > 0 ? (
          data.posts.map((res, i) 
            => <PostListContent key={i} data={res} />)
        ) : (
          // 로딩 컴포넌트
          <Loading />
        )}
      </StyledTable>
      <Paging
        totalItem={totalDataLength}
        page={page}
        setPage={setPage}
        style={{ width: "100%", display: "inline", margin: "0 auto" }}
      />
    </LayoutWrapper>
  );
}

const StyledTable = styled.table`
  border: 0;
  border-collapse: collapse;
  width: 100%;
  margin-top: 20px;
`;

const InputContainer = styled.div`
  position: relative;
  display: flex;
  justify-content: space-between;
`;

const HeadWrapper = styled.div`
  display: flex;
  height: 40px;
  justify-content: space-between;
  align-items: center;
  padding: 0;
`;

export const ListHeadWrapper = styled.tr`
  background-color: var(--main-color);
  color: white;
  height: 40px;
  margin-top: 20px;
  padding: 0 15px;
  text-align: center;
  border-radius: 30px;
`;

export default PostList;

```

- 코드의 가독성과 재사용성이 높음
- 간단한 스타일링은 객체를 삽입하는 방법을 사용

### 3.3. async - await

- 비동기 함수 내에서 동기적인 코드를 사용해야 할 때 사용
- 흔히 비동기 작업을 할 때 콜백 함수를 사용해왔음
	- 본 패턴이 콜백 함수보다 직관적이라 가독성이 높은 것 같음
- async 함수 내에서 await 키워드를 사용해 비동기 작업의 완료를 기다리고, 이후의 코드를 동기적으로 실행 가능
- 주의점: async 함수 내에서만 await 키워드를 실행할 수 있음
- 본 프로젝트에서 async - await 패턴을 사용한 예시

```jsx
// 게시물 및 댓글 데이터 가져오기
async function fetchData() {
  const res = await getPostDetail();
  setData(res.post);
  setCommentData(res.comments);
  return res;
}
```

- await 키워드를 사용해 레시피 상세 페이지를 기다리고 이후 동기적으로 댓글 추가할 수 있음

```jsx
const postComment = async () => {
  try {
    // 서버의 댓글 엔드포인트로 POST 요청을 보냄
    const res = await fetch(`${URL}/posts/${id}/comments`, {
      method: 'POST',
      headers: {
        'content-type': 'application/json', // JSON 데이터로 설정
        Authorization: `Bearer ${accesstoken}`, // 인증 토큰을 헤더에 추가
        'X-CSRF-Token': `${csrfToken}`, // CSRF 토큰을 헤더에 추가
      },
      body: JSON.stringify({
        content: `${comment}`, // 댓글 내용을 JSON 데이터에 포함
      }),
    });
    
    // 응답 데이터를 JSON 형식으로 파싱
    const data = res.json();
    console.log(data);
    
    return data;
  } catch (error) {
      console.error('Post Comment 데이터 전송 실패', error); // 오류 메시지 출력
  }
};
```

- 비동기 작업의 예외 처리를 위해 async - await 패턴을 사용하면 try - catch 문을 사용해 비동기 작업의 에러를 핸들링할 수 있음
- fetch 함수가 비동기적으로 네트워크 요청을 수행
	- 네트워크 요청을 보내고 응답을 기다리는 동안 코드 실행이 멈추지 않고 다음 줄로 진행
	- 이때 await 키워드를 사용해 해당 라인에서 응답이 올 때까지 기다림
- then () 블록을 사용해 응답 Promise를 처리할 수 있지만 async - await 패턴이 더 직관적인 것 같음

### 3.4. Export Base URL

```jsx
// 개발 환경 BaseURL.js
export const URL = "https://rest-recipe-book-dptb.run.goorm.site";

// 운영 환경 BaseURL.js
export const URL = "http://10.0.0.3:5000";
```

- BaseURL을 내보내 환경 구성을 용이하게 함
- export한 URL을 Import 해서 사용하는 코드 예시

```jsx
// PostListAPI.jsx
import { URL } from "../BaseURL";

// PostListAPI - 레시피 목록을 가져오는 API
const PostListAPI = (pageNo) => {
  // 레시피 목록을 가져오는 함수
  const getPostList = async () => {
    try {
      const res = await fetch(`${URL}/posts?pageNo=${pageNo}&pageSize=10`, {
        method: "GET",
      });
      const data = await res.json(); // JSON 응답 파싱
      return data;
    } catch (error) {
      console.error("레시피 목록 데이터 전송 실패", error);
    }
  };

  return getPostList;
};

export default PostListAPI;
```

### 3.5. 테스트 API 서버

> Test API Server URL: https://rest-recipe-book-dptb.run.goorm.site

- 테스트 API 서버를 배포해 개발 시 팀원 모두 같은 데이터로 개발 진행

## 4. 🔧 Issue & Troubleshooting

> ✅: 해결 이슈
> ❓: 미해결 이슈

### 4.1. 비동기 함수 흐름 제어의 문제

- JavaScript의 비동기 처리 방식을 제대로 이해하지 못한 채로 작성한 코드에서 데이터가 로딩되지 않았을 때 다른 함수가 실행되었고, 오류 처리도 제대로 되지 않았음
> ✅ 해결: async - await 패턴을 사용한 흐름 제어를 통해 해결

### 4.2. 전역 상태 저장

- 상태를 Props로 전달하려고 할 때 복잡도가 증가하는 문제 발생, 중첩 레벨이 깊어져 헷갈림
- 로그인 상태를 업데이트하려고 할 때 로직에서 꼬여 충돌이 나는지 파악되지 않음
> ✅ 해결: 리코일을 사용해 상태를 중앙 집중화하여 컴포넌트 간 데이터 공유를 단순화

### 4.3. localhost 쿠키 저장이 안됨

- API 서버에서 발행된 토큰을 Set-Cookie로 전송해 브라우저의 쿠키 저장소에 자동으로 저장하도록 의도
- 의도와 다르게 보낸 쿠키가 저장소에 저장되지 않음
- 조사해보니, 로컬 개발 환경에서는 쿠키를 로컬 도메인(localhost)에 설정할 때 특별한 구성이 필요
	- 이는 보안 정책과 관련이 있음
	- 브라우저에서 쿠키를 로컬 도메인에 설정하려면 "SameSite" 속성을 "None"으로 설정해야하며, "Secure" 속성도 설정되어야 함
	- 이것은 HTTPS로 서비스되고 있는 경우에만 해당되며, 로컬 개발 환경은 기본적으로 HTTPS를 사용하지 않기 때문에 저장 불가능
> ✅ 해결: 쿠키 대신 Local Storage를 사용해 해결, 최근 보안적으로 더 권장되는 옵션이며 쿠키 대신 상태 정보를 저장할 수도 있음, 또한 도메인 또는 프로토콜과 무관하게 사용할 수 있음
