# 제 3회 Outta 부트캠프
## 올리브영 추천 시스템 구현 

<hr>

### 목차
1. [프로젝트 개요]
2. [데이터 크롤링](## 2. 데이터 크롤링)
3. [데이터 전처리](#프로젝트-구조)
4. [데이터 임베딩](#프로젝트-환경)
5. [데이터 필터링](#데이터와-모델)
6. [추천 시스템](#웹-시뮬레이터-개발)
7. [추천 시스템 실행](#추후-연구-방향)
8. [피드백 및 결론](#참고-문헌)
9. [사용 방법]

<hr>

## 1. 프로젝트 개요 


## 2. 데이터 크롤링

올리브영 사이트에서 스킨케어와 관련된 카테고리의 제품들을 크롤링 했다. 
각 항목(스킨/토너 , 크림 , 로션 , 에센스/세럼/앰플)들을 인기 순으로 24개씩 8페이지, 총 192개의 제품의 정보들을 수집 했고 리뷰는 체험단을 제외한 도움 순으로 상위 리뷰들을 수집 했다.

## 3. 데이터 전처리

수집된 데이터를 통합한 후, 잘못된 크롤링 데이터와 결측치를 처리하였다. '할인율'이라는 파생 컬럼을 계산하여 새로 추가했고, 가격과 리뷰 수를 텍스트 형태로 변환하여 임베딩 준비를 했다.

## 4. 데이터 임베딩

`SentenceTransformer`를 활용하여 텍스트 데이터를 벡터 형태로 변환 했다. 이를 통해 텍스트 데이터의 의미를 모델 학습에 활용할 수 있도록 했다.

## 5. 데이터 필터링

사용 유저에게 질문을 통해 수치 데이터를 필터링하고 상위 50%의 제품을 필터링 했다. 올리브영의 기본적인 유저 정보와 관련 되어 쉽게 응답할 수 있는 질문들로 구성하였고 유저가 원하지 않으면 필터링을 적용하지 않는다.

## 6. 추천 시스템

추천 시스템은 코사인 유사도를 기반으로 유저 쿼리와 제품 간의 유사도를 계산한다. 데이터의 범위를 정규화하여 모델 학습을 안정화했고, 리뷰에 지수 가중치를 적용하여 수치가 높은 리뷰의 중요도를 올려줬다. 최종 점수는 리뷰와 제품명에 우선 순위를 두고, 가격, 할인율, 리뷰 수는 보조적으로 가중치를 설정 했다.

## 7. 추천 시스템 실행

유저는 쿼리를 입력하여 제품을 추천받을 수 있으며, 추천 결과가 마음에 들지 않을 경우 새로운 쿼리를 입력하여 다시 추천받을 수 있다. 단순 필터링과 쿼리 기반 추천을 결합하여 유저가 편리하게 사용할 수 있도록 설계하였으며, 할인율과 같은 조건도 반영하고, 추천된 제품의 상세 정보와 대표 리뷰를 제공한다. 

## 8. 피드백 및 결론

`SentenceTransformer` 모델을 사용하였지만 유저 쿼리가 복잡해질 경우 이를 상세히 반영하기 어려워 보였다. 한국어로 된 리뷰들을 더 잘 임베딩 해줄 수 있는 모델을 사용하고 더 많은 리뷰와 제품 정보 데이터를 크롤링하여 구축한다면 이보다 나은 성능을 보일 것으로 기대 된다. 

## 9. 사용 방법 및 추후 발전 방향 

1. 데이터 크롤링 코드는 참고만 하고 넘어가도 된다. 추가로 해보고 싶은 분들은 올리브영 사이트에서 base_url 체크하고 page 수를 조정하면 된다. 크롤링 시기에 따라 올리브영 사이트 관리자가 html을 변경했다면 SELECTOR들이 변경된 정보들을 찾을 수 있게 개발자 도구에서 html을 찾아 코드를 수정해야 할 수도 있다. 
2. github에 올라와 있는 파일들을 다운로드 한 후 '데이터 전처리' 섹션으로 넘어가 '추천 시스템' 섹션까지 코드를 실행시킨다.
3. '추천 시스템 실행' 섹션에서 모델을 다운로드 하고 데이터를 임베딩 한 뒤 함수를 실행하면 된다.
4. 임베딩 데이터는 용량이 커서 github에 업로드 하지 못했다. 한번 임베딩 한 후 pickle파일로 저장하고 다시 불러와서 사용하면 편리하다.
5. 밑에는 추천 받은 예시 결과이다. 

![image](https://github.com/user-attachments/assets/39550fa0-b709-4a67-a4e2-00eb34f02af0)

![image](https://github.com/user-attachments/assets/23a327a0-18c7-4132-8932-ef880c386b88)







