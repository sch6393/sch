CBO, RBO
===

### RBO (Rule Based Optimization)
* 규칙 기반 옵티마이저
* 미리 정해진 규칙에 기반하여 실행 계획을 결정하므로 실제 통계 수집이나 쿼리 수행 시의 발생되는 비용 (Cost) 에 대한 고려가 되지 않음

<br>

### CBO (Cost Based Optimization)
* 비용 기반 옵티마이저
* 데이터에 대한 각종 통계에 기반하여 실행 계획을 결정하지만 실제 성능이 좋게 나오기 위해서는 통계정보 (딕셔너리) 가 정확해야 한다는 전제가 붙음

<br>

### 참고
* 10g 부터는 통계 정보를 자동으로 갱신

<br>