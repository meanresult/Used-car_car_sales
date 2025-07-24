
# 🚗 중고차 가격 예측 분석 보고서

## 📌 프로젝트 개요

본 프로젝트는 **UAE 중고차 시장 데이터**를 활용하여,
차량의 다양한 특성(제조사, 모델, 연식, 상태 등)에 따라 **합리적인 가격을 예측하는 회귀 모델**을 구축하는 데 목적이 있습니다.

- 데이터 출처: Dubizzle 중고차 플랫폼 크롤링 데이터
- 타겟 변수: `price_in_aed` (중고차 가격)
- 분석 도구: Python, Pandas, Scikit-Learn, XGBoost

## 📊 데이터 전처리 및 정제

1. **결측치 처리**
   - `year`, `no_of_cylinders`: 최빈값으로 대체
   - `horsepower`, `model`, `motors_trim`: 결측치가 많은 경우 제거

2. **파생 변수 생성**
   - `date_posted` → `year_posted`, `month_posted`, `dayofweek_posted` 추출
   - `body_condition`, `mechanical_condition`: 텍스트 → 가중치 인코딩

3. **범주형 변수 인코딩 전략**

| 컬럼명             | 인코딩 방식      | 이유 요약 |
|------------------|------------------|-----------|
| `seller_type`    | 원-핫 인코딩       | 순서 없음 |
| `fuel_type`      | 가중치 인코딩     | 가격 영향력 반영 |
| `company`        | 군집 기반 가중치 | 제조사별 가격 차이 반영 |
| `model`          | 제조사별 군집화   | 고카디널리티 + 가격 변동 |
| `motors_trim`    | 모델별 군집화     | 트림에 따른 가격차 반영 |

## 🧠 모델링 및 성능

- 사용 모델: `XGBoostRegressor`
- 평가 지표:
  - RMSE (평균 예측 오차): **약 108,052 AED**
  - R² (결정계수): **0.9287**
  - 교차검증 평균 R²: **0.8983 ± 0.017**

→ **92% 이상의 예측 정확도**를 보이며, 과적합 없이 안정적으로 성능을 냄.

## 🔍 하이퍼파라미터 튜닝

- `GridSearchCV` 사용 (5-Fold Cross Validation)
- 최적 파라미터 예시:

```python
{
  'learning_rate': 0.5,
  'max_depth':8,
  'n_estimators': 200,
  'subsample': 0.8
}
```

## ✅ 결론 및 향후 과제

- 모델은 **차량 특성과 가격 간의 관계를 잘 학습**하였고,  
  군집화 및 가중치 인코딩은 고차원 카테고리 문제에 효과적이었음
- 향후 적용 가능성:
  - **신차 대비 감가 예측 모델**
  - **지역별 중고차 시세 비교**
  - **연식/트림/주행거리 기반 시세 추정 시스템**

## 📁 파일 구조 예시

```
중고차 분석/
├── Dubizzle_used_car_sales.csv
├── 중고차 분석.ipynb
└── README.md
```
