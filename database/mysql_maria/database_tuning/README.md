- DB 튜닝 (RDB)
    - `explain`  키워드 → 쿼리를 실행하기 전 실행 계획에 대해 보여주는 명령어
    - `mysql`의 경우 캐시가 생성되는 데  `2가지 캐시`가 존재
        - `InnoDB 버퍼 풀` ( innoDB )
            - InnoDB 엔진에서 사용하는 데이터와 인덱스를 메모리에 저장하여 디스크 I/O를 줄이고 쿼리 성능을 향상
        - `쿼리 캐시`
            - SELECT 쿼리의 결과를 메모리에 저장하여 동일한 쿼리가 다시 실행될 때 디스크 I/O 없이 바로 결과를 반환합니다.
    - `index` ,  `limit` , `where`  쿼리 성능 향상에 영향을 줌, where도 문자열이 null인 경우
