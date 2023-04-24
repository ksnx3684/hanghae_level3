# hanghae_level3
항해99 Node.js Level3

## ERD
https://drawsql.app/teams/ksnx3684s-team/diagrams/hanghae-level3

---

### 1. mongoose에서 sequelize로 변경했을 때, 많은 코드 변경이 있었나요? 있었다면 어떤 코드에서 변경사항이 많았나요?

- 구조적인 변경점으로는 기존에 사용하던 schemas 폴더를 지워지고 새로 models 폴더와 migrations 폴더를 생성했습니다. 이 과정에서 app.js나 각 라우터 파일들에서 불러오던 schemas 파일들을 sequelize에 맞게 수정해야 했습니다.
또한 routes파일에서 각 API마다 DB에서 조회할 때 쓰는 코드부분에서도 변경점이 많았습니다. 대표적으로 게시글 조회 시 기존에는 Posts.find() 메서드를 사용했다면 sequelize에서는 Posts.findAll() 메서드를 사용하여 조회할 컬럼들을 attributes에 기입해주는 형식으로 바뀌었습니다.
테이블을 생성하고 맵핑하는 과정에서도 기존과는 다르게 CLI에서 명령어로 실행하는 부분도 차이점입니다. 때문에 sequelize를 보다 제대로 이용하기 위해서는 기본적인 명령어 또한 습득해야 한다는 중요성을 알게 되었습니다.

---


### 2. 닉네임과 비밀번호에 대한 요구사항 검증은 어떻게 진행하였나요? 만약 정규표현식을 사용하였다면 어떤 정규표현식으로 구현하였나요?

- 닉네임은 정규 표현식을 이용하여 검증하였습니다. 조건이 최소 3자 이상, 알파벳 대소문자, 숫자 형태로 구성하는 것이었기 때문에 다음과 같은 /^[a-zA-Z0-9]{3,}$/ 표현식을 사용하였습니다.

- 비밀번호는 length를 이용하여 문자열 길이 검사를 통해 4자 이상인 경우 에러 처리 되도록 구현하였고 또한 includes() 메서드를 이용하여 닉네임과 같은 값이 포함되었을 경우 에러로 처리될 수 있도록 구성했습니다.

---


### 3. ERD를 먼저 작성 후 개발을 진행했을 때, 좋은 점은 어떤 것들이 있었나요?

- 개발자 입장에서 각 테이블의 상세 구조와 테이블 간의 의존 관계가 한 눈에 보이기 때문에 데이터베이스를 분석할 때 편리합니다. 또한, 추후 유지 보수 등의 관리적인 측면에서도 효율적일 수 있기 때문에 ERD를 구성해 놓는 것이 좋습니다.

---


### 4. JWT를 이용해 사용자 인증은 진행하였을 때, 어떤 장점과 단점이 존재하였나요?

- 장점으로는 공격자가 데이터를 변조하여 재전송을 하는 행위를 방지 할 수 있다는 점과 stateless한 특징으로 인해 서버가 죽었다가 다시 재기동해도 그대로 사용할 수 있다는 장점이 있습니다.

- 단점으로는 누구나 디코딩이 가능하다는 점 때문에 민감한 데이터를 넣었을 경우 유출의 위험이 있으며, 토큰의 길이가 길다는 점 때문에 서버 자원의 낭비가 심할 수 있습니다.

---


### 5. 게시글 조회와 댓글 조회 API Response를 어떻게 구성하였나요? 왜 그렇게 구성하였나요?

- 게시글 조회와 상세 조회 둘 다 비슷한 구조로 구성하였습니다. 게시글 조회에 성공 시 status 200으로, DB에서 조회하는 도중 에러 또는 서버 내부에서 예기치 못한 에러로 인한 조회 실패 시 status 400으로 각각 구성하였습니다. 