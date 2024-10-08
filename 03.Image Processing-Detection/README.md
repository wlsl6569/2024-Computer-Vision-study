# Filters as Templates
**정의**: 특정한 패턴이나 특징 또는 이미지 속 오브젝트를 감지하기 위해 필터를 템플릿처럼 사용하는 것으로, 이미지 속 내용을 미리 정의된 패턴과 비교하여 유사한 영역을 식별한다.

### Filters as Templates, 컨볼루션을 사용한 수학적 설명:
수학적으로, 템플릿으로서의 필터는 컨볼루션 연산을 사용하여 설명될 수 있다.
\[
O(x, y) = \sum_{u=-k}^{k} \sum_{v=-k}^{k} I(x-u, y-v) \cdot T(u, v)
\]
- \( O(x, y) \): 필터링 후의 출력 이미지
- \( I(x, y) \): 입력 이미지
- \( T(u, v) \): 필터(템플릿)
- \( u \)와 \( v \)는 필터 크기를 나타내며, 일반적으로 -\( k \)에서 \( k \)까지의 범위를 가짐

### Cross-Correlation 연산법의 Filters as Templates:
그러나 템플릿을 뒤집어 계산하는 컨볼루션보다 템플릿 매칭에서는 필터를 뒤집지 않고 이미지에 사용하는 Cross-Correlation 연산방식이 더 직관적이다.

### Cross-Correlation 연산법의 Filters as Templates의 문제:
단순 Cross-Correlation 연산법은 템플릿 매칭에서 올바른 결과를 얻지 못할 수도 있다. 가장 비슷한 패턴 구역보다 유사하지 않은 패턴이 상관값이 크게 나오는 현상이 있다는 것. 이를 위해 Normalized Cross-Correlation를 사용한다.

### Normalized Cross-Correlation 연산이란:
템플릿과 이미지 각 영역의 밝기 차이와 대비 차이를 보정하여 정밀한 템플릿 매칭 결과를 얻는다.

### Filters as Templates, Normalized Cross-Correlation을 사용한 수학적 설명:
\[
NCC(x, y) = \frac{\sum_{u=-k}^{k} \sum_{v=-k}^{k} (I(x-u, y-v) - I_{mean}) \cdot (T(u, v) - T_{mean})}{\sqrt{\sum_{u=-k}^{k} \sum_{v=-k}^{k} (I(x-u, y-v) - I_{mean})^2 \cdot \sum_{u=-k}^{k} \sum_{v=-k}^{k} (T(u, v) - T_{mean})^2}}
\]
- \( I(x, y) \): 입력 이미지
- \( T(u, v) \): 필터(템플릿)
- \( I_{mean} \): 필터 아래에 있는 이미지 영역의 평균 강도
- \( T_{mean} \): 필터의 평균 강도
- 분모는 이미지 영역과 필터의 표준 편차로 교차 상관을 정규화함

---

# Edge Detection – 기본
**Edge detection 의미**: 작은 영역 내에서 이미지 강도가 급격히 변하는 부분

### Edge의 발생:
1. **surface normal discontinuity**: 표면 방향의 불연속
2. **depth discontinuity**: 객체끼리 깊이 불연속성
3. **surface color discontinuity**: 표면 색상 불연속
4. **illumination discontinuity**: 그림자나 반사 같은 조명 효과 불연속성

### Real Image에서의 Edge 발생:
1. **cast shadows**: 그림자
2. **depth discontinuity**: 객체끼리 깊이 불연속성
3. **Reflectance change**: 반사율 변화: 외관 정보, 질감
4. **Discontinuous change in surface orientation**: 표면 방향의 불연속

### Edge Detection 기본 원리:
이미지에서 강한 변화의 징후가 있는 영역을 찾는 것으로, 일반적으로 픽셀 강도 변화가 큰 영역을 탐지하는 방식.
- 고려할 점 1. Neighborhood size를 정하기(이미지에서 중심 픽셀 주변 픽셀들을 이웃이라 부른다)
- 고려할 점 2. 변화를 어떻게 감지할지

### Edge의 수학적 정의:
1. 엣지는 이미지의 강도를 계산하는 함수에서의 급격한 변화가 발생하는 위치로 정의 – 미분
2. 강도의 변화를 측정하며 이는 이분 연산자를 사용하여 계산된다. - 그래디언트
    - 이미지의 그래디언트란 강도가 가장 빠르게 증가하는 방향을 가르키는 벡터로 x와 y의 편미분으로 구성
    - 그래디언트의 크기는 엣지의 강도를, 방향은 엣지의 방향을 나타낸다.

---

# Edge Detection 연관 기법들
### Non-Maximal Suppression:
엣지나 코너를 찾은 후, 이웃 픽셀들 중에서 가장 강한 값만 남기기 위한 방법
- 각 픽셀에 대해 주변 픽셀과 비교하고 강도가 가장 센 픽셀만 유지하고 나머지 제거

### Gaussian Smooth:
엣지 검출 중 이미지를 부드럽게 처리하여 노이즈를 줄인 더욱 정확한 엣지 검출 가능하게 한다.

### Edge Detection의 다른 방법:
미분 연산자에 따라 그래디언트를 계산하는 방법들이 달라진다.
- 대표적으로 Sobel, Prewitt, Roberts 등의 미분 연산자가 있다.

### 일반적인 엣지 검출 과정:
1. **스무딩**: 이미지의 노이즈를 줄인다. 노이즈는 엣지 검출의 정확도에 심각한 영향을 미칠 수 있어, 엣지 검출 방법을 적용하기 전에 가우시안 스무딩과 같은 전처리 단계가 필요하다.
2. **그래디언트 계산**: 엣지를 감지하기 위해 그래디언트 필터 적용
3. **Non-max suppression (비최대 억제)**: 감지된 엣지를 단일 픽셀 선으로 얇게 만든다
    - 각 픽셀에 대해 주변 픽셀과 비교
    - 강도가 가장 센 픽셀만 유지하고 나머지 제거
4. **임계값 처리**: 엣지 픽셀을 분류하여 최종 검출 완료

---

# 이차도함수를 이용한 엣지 검출:
이미지의 두 번째 도함수를 사용하여 엣지를 찾습니다. 2차 도함수는 밝기의 변화가 급격하게 바뀌는 지점을 식별하는 데 유용
- **2차 도함수를 계산하는 방법**: Laplacian as Edge Detector (엣지 검출기로서 라플라스 변환). 이 연산자는 높은 주파수(급격한 밝기 변화)에 민감하여 엣지를 잘 찾아낸다.
- **2차 도함수를 이용하는 엣지 검출 커널**: Discrete Laplacian Operator(이산 라플라스 연산자). 이미지를 처리하기 위한 수학적 필터로, 픽셀 강도의 변화를 수치적으로 계산하는데 특정 픽셀과 그 주변 픽셀 관계를 수식으로 표현한 커널

### 노이즈가 포함된 이미지에서의 엣지 검출 알고리즘:
1. 이미지를 가우시안 스무딩하여 노이즈 최소화한다.
2. 가우시안 필터로 스무딩한 이미지 결과에 대해 미분 수행하여 엣지 찾는다: 노이즈를 줄이며 밝기 변화의 경계도 정확히 감지한다.
3. 가우시안의 라플라스: 라플라스 연산 수행하여 엣지 찾는다.

---

# Gradient vs. Laplacian
- **그래디언트**: 이미지의 밝기 변화의 방향과 크기를 알 수 있습니다.
    - 엣지의 강도와 방향성을 알 수 있다.
    - 최대 임계값을 사용한 검출
    - 비선형 연산으로 두 번의 컨볼루션 연산 필요
- **라플라스**: 밝기 변화의 두 번째 도함수를 보여주며, 엣지의 존재를 판단하는 데 사용됩니다.
    - 엣지의 위치만 알 수 있다
    - 영점 교차에 기반한 검출
    - 선형 연산으로 한번의 컨볼루션 연산 필요

---

# Edge 응용: Corner 검출
**corner의 정의**:
1. 이미지에서 두 개 이상의 엣지가 만나는 지점
2. Local 이웃이 뚜렷한 가장자리 방향을 나타내는 점들
3. 매칭하기 좋은 local features

### Harris Corner Detection:
**정의**: 특정 픽셀 위치에서 강도 변화가 어떻게 이루어지는지를 분석. 이는 데이터 포인트 주위의 지역적으로 이동할 때의 강도 변화 패턴을 조사하여 수행하는 것으로 이미지에서 밝기가 바뀌는 점을 찾아내서 이 정보를 바탕으로 강한 코너를 찾아내는 구조

### 알고리즘:
1. **강도 변화 측정**: 특정 점 ((x, y)) 주위에서 조금 이동해 보세요. 이때 이 점의 밝기(강도)가 얼마나 변하는지를 측정합니다. 예를 들어, 위쪽으로 조금 이동했을 때의 밝기와 원래의 밝기를 비교합니다.
2. **가중치 적용**: 이동하면서 사용하는 값에 가중치를 줘서, 주변 밝기 변화가 노이즈에 영향을 덜 받도록 합니다. 간단히 말해, 주변 점들에 더 신경을 써서 안정성을 높이는 것입니다.
3. **행렬 만들기**: 주변의 밝기 변화 데이터를 수집해 (M)이라는 작은 테이블(행렬)을 만듭니다. 이 테이블은 각 방향으로의 변화 정보를 담고 있습니다.
4. **코너 응답 계산**: 만들어진 (M)의 정보를 바탕으로 코너가 얼마나 강한지를 계산합니다. 이 값이 높을수록 그 점이 코너일 확률이 높습니다.
5. **최고 점 찾기**: 전체 이미지에서 계산된 값을 비교해, 가장 높은 값(강한 코너)을 찾습니다. 주변에서 가장 강한 점만 남기고 나머지는 지웁니다.
6. **임계값 설정**: 마지막으로, 찾은 포인트 중에서 일정 기준(임계값)을 넘는 점들만 코너로 인정합니다.

---

# Blob
**Blob의 정의**: 둘러싼 주변 영역에 비해 밝거나 어두운 이미지의 영역

### Blob Detection의 기본적인 진행:
1. 이미지 스무딩
2. Laplacian of Gaussian 또는 Difference of Gaussian 적용
3. 최적의 scale과 orientation parameters을 찾기 (크기, 방향성)

### Edge Detection vs. Blob Detection:
- **Edge detection**: 이미지에서 갑작스러운 픽셀 값의 변화를 검출하는 것을 목적으로 가우시안 필터를 이미지에 먹여 노이즈를 제거하여 더 정확한 미분값을 낼 수 있고 이를 통해 그라디언트 계산이 가능해진다.
- **Blob detection**: 이미지에서 둥글거나 일정한 형태의 큰 특징을 나타내는 부분을 검출하는 것이 목적이며 가우시안의 라플라시안을 통해 물체의 밝기가 바뀌는 곳(blob)을 찾아낸다.
    - **라플라시안**: 함수의 2차 미분을 합산한 값으로 함수의 변동성을 평가
    - 주로 2차원 함수인 이미지에서 픽셀 값이 어떻게 변화하는지, 즉 밝기 변화가 있는지 탐지하는데 사용
    - **Laplacian of Gaussian (LoG)**: 라플라시안과 가우시안의 조합으로 가우시안으로 이미지를 먼저 스무딩하고 (노이즈 제거) 그 결과에 라플라시안을 적용하는 방식

---

# SIFT
**SIFT 검출의 정의**: 이미지에서 스케일 및 회전 불변성을 갖는 특징점을 찾고, 이를 활용해 이미지 매칭을 수행하는 알고리즘, 블롭과 같은 특징을 갖는 지점들을 찾는다.

### SIFT의 주요 단계:
1. **Fast Normalized LoG Approximation: DoG**:
    - DoG는 LoG의 근사값으로, 두 가우시안 필터 간의 차이를 이용해 빠르게 특징점을 검출. SIFT의 특징 검출 과정에서 속도를 높이는 데 사용.
2. **Extracting SIFT Interest Points**:
    - SIFT 알고리즘은 강한 특징점을 갖는 지점을 찾아 키포인트를 생성. 이 과정은 여러 스케일과 위치에서 잠재적인 관심 지점을 검토하여 결정.
3. **SIFT Scale Invariance**:
    - SIFT는 다양한 스케일에서 동일한 특징을 검출하여 스케일 불변성을 확보. 이는 이미지가 확대되거나 축소되어도 특징점이 일관되게 검출되도록 함.
4. **Computing the Principal Orientation**:
    - 각 키포인트 주변의 그라디언트 방향과 크기를 분석하여 주요 방향을 할당. 이 과정은 SIFT의 회전 불변성을 가능하게 함.
5. **SIFT Rotation Invariance**:
    - 할당된 주요 방향을 기준으로 모든 방향을 정렬함으로써 회전 불변성을 확보. 이로써 이미지가 회전해도 동일한 특징점이 유지.

### SIFT 디스크립터 생성:
- SIFT 디스크립터는 각 키포인트 주변의 픽셀 정보를 바탕으로 128차원의 특징 벡터(디스크립터)를 생성한다. 이는 이미지의 로컬 특징을 기술하는 데 사용된다.
- 생성된 디스크립터를 비교하여 이미지의 유사성을 평가한다. 이 과정을 통해 이미지 매칭, 객체 인식 등의 작업이 가능하다.
    - 원리: 생성된 128차원 벡터들 간의 유사성을 측정하는 데 유클리드 거리(Euclidean Distance)를 사용. \( D(D_1, D_2) = \sqrt{\sum (D_{1i} - D_{2i})^2} \)로 이해하면 된다.

---

# SIFT – 요약
**Scale-Invariant Feature Transform (스케일 불변 특징 변환)**
- **스케일 불변성 (Scale invariance)**: 스케일 공간 검색을 통해 다양한 크기의 특징점을 검출합니다.
- **회전 불변성 (Rotation invariance)**: 주된 방향을 추정하여 회전 변형에 불변한 특징점을 생성합니다.
- **다른 작은 변이에 대한 민감도 감소 (Insensitivity to other small variations)**: 히스토그램 기반 디스크립터를 사용하여 작은 변화를 무시합니다.
- **조명 변화에 대한 민감도 감소 (Insensitivity to illumination change)**: 그라디언트 기반 디스크립터를 사용하여 조명 변화에 둔감하게 반응합니다.
- **노이즈에 대한 민감도 감소 (Insensitivity to noise)**: 그라디언트 계산 전에 가우시안 스무딩을 통해 노이즈를 감소시킵니다.

### LoG보다 DoG가 더 효율적인 이유:
- 가우시안 스무딩은 디스크립터 계산에도 필요하다.
    - SIFT 알고리즘에서 특징점을 검출하고 디스크립터(특징 벡터)를 계산할 때, 가우시안 스무딩을 먼저 사용하여 노이즈를 줄인다.. 이 스무딩은 DoG에서도 사용되기 때문에 두 가지 과정을 줄여준다.
- 가우시안 필터링의 분리성과 계단식 적용 가능성 덕분에 효율성이 높다.
    - LoG는 직접 계산하는 경우 2차 미분이 필요하지만, DoG는 단순히 가우시안 필터를 여러 번 적용한 후 그 차이를 계산하는 방식이기 때문에, 가우시안 블러의 계산이 더 간단하고 빠르다.
    - 분리성: 가우시안 필터는 이미지를 한꺼번에 처리하는 대신, 가로 방향으로 한 번, 세로 방향으로 한 번 나눠서 처리할 수 있습니다.
    - 계단식: 여러 단계로 나눠서 가우시안 블러를 점진적으로 적용하는 방식

### Blob은 LoG, SIFT는 DoG를 이용하는 이유?
- 라플라시안은 이미지의 곡률 변화를 강조하여 블롭(둥근 특징)을 잘 검출할 수 있음. 2차 미분을 사용하여 밝거나 어두운 영역 강조하는 점이 둥근 모양에 특화.
- SIFT는 LoG의 근사 방법인 DoG를 사용. DoG는 두 개의 다른 표준 편차를 가진 가우시안 필터를 이미지에 적용한 뒤 그 결과를 빼서 만든 것으로 LoG를 직접 계산하는 것보다 빠르고 간단하면서도 유사한 검출 성능을 제공하나, Blob에 사용하기엔 다소 부정확할 수 있음(곡률 계산 취약).
