# Playback — 프론트엔드

축구 경기 영상을 업로드하고 AI가 분류한 하이라이트를 확인·편집하는 React 웹 화면입니다. 충북대학교 졸업작품 팀의 [원본 프론트엔드](https://github.com/CBNU-playback/playback)를 포크했습니다.

프로젝트 전체 구조와 제 담당 역할은 **[백엔드 저장소](https://github.com/LDeHa/playback-backend)**에 정리했습니다. 이 저장소에는 팀이 함께 만든 화면 코드와 별도 `train` 브랜치의 학습 코드를 보존했습니다.

## 사용자 흐름

1. 축구 경기 영상을 업로드합니다.
2. 백엔드가 만든 하이라이트 구간과 이벤트 라벨을 확인합니다.
3. 구간을 선택하거나 추가·삭제하고 영상으로 미리 봅니다.
4. 텍스트 표시와 전환 옵션을 설정한 뒤 결과 영상을 다운로드합니다.

영상 분석과 최종 영상 내보내기는 Django 백엔드와 연결됩니다.

## 실행 화면

팀 저장소에 있던 실행 화면입니다. 아래 이미지는 이번 정리 과정에서 새로 실행한 결과가 아닙니다.

### 업로드

![영상 업로드 화면](docs/images/upload.png)

### 분석

![AI 분석 화면](docs/images/inference.png)

### 결과

![하이라이트 결과 화면](docs/images/output.png)

## 주요 파일

| 파일 | 내용 |
| --- | --- |
| [src/pages/Home.js](src/pages/Home.js) | 영상 업로드, 구간 편집, 결과 내보내기 |
| [src/components/VideoPlayer.js](src/components/VideoPlayer.js) | 영상 재생 화면 |
| [src/components/AIVideoEditor.js](src/components/AIVideoEditor.js) | 업로드·자막 요청 등 편집 관련 컴포넌트 |
| [src/App.js](src/App.js) | 페이지 라우팅 |
| [package.json](package.json) | 의존성과 실행 명령 |

기술: React 17 · React Router · Material UI · Axios

## 모델 학습 코드

`train` 브랜치에 `LDeHa` 계정으로 올린 세 파일이 있습니다. 기본 브랜치의 화면 코드와 별도로 보관된 작업입니다.

- [train_dataset.py](https://github.com/LDeHa/playback-frontend/blob/train/train_dataset.py): 자막과 이벤트 라벨을 시간 구간으로 연결해 학습 데이터 구성
- [bert_train.py](https://github.com/LDeHa/playback-frontend/blob/train/bert_train.py): BERT 분류 모델 학습, 분류 리포트와 혼동 행렬 저장
- [predict_action.py](https://github.com/LDeHa/playback-frontend/blob/train/predict_action.py): 구간별 이벤트 예측과 결과 비교

[원본 커밋 확인](https://github.com/LDeHa/playback-frontend/commit/b8b38bc)

## 실행 준비

```bash
git clone https://github.com/LDeHa/playback-frontend.git
cd playback-frontend
npm ci
npm start
```

화면의 기본 주소는 `http://localhost:3000`입니다. 영상 업로드·분석·내보내기에는 [백엔드](https://github.com/LDeHa/playback-backend)가 필요합니다.

### 백엔드 연결 설정

현재 `Home.js`와 `AIVideoEditor.js`에는 팀 개발 환경의 API 주소가 직접 들어 있습니다. 로컬 실행 전 두 파일의 주소를 실행 중인 백엔드 주소로 맞춰야 합니다.

`.env`의 `REACT_APP_API_BASE_URL`은 `src/api/videoApi.js`에서 사용하지만, 모든 요청에 적용된 상태는 아닙니다. `package.json`의 proxy 설정만 바꿔도 위의 직접 호출 주소는 바뀌지 않습니다.

## 현재 상태

- README에 존재하지 않던 이미지 경로를 정리하고 실제 화면 세 장을 연결했습니다.
- 이번 정리에서는 영상 처리 환경을 실행하지 않았습니다.
- `src/App.test.js`에는 Create React App의 기본 테스트가 남아 있어 현재 화면의 기능 검증 결과로 사용할 수 없습니다.

화면 코드는 팀 공동 결과물입니다. 제 담당 역할과 확인 가능한 개인 커밋은 [백엔드 기여 문서](https://github.com/LDeHa/playback-backend/blob/main/docs/contributions.md)에 구분해 정리했습니다.

