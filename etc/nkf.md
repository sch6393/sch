nkf Network Kanji Filter (Shift JIS의 특수문자 처리)
===

>[https://osdn.net/projects/nkf/](https://osdn.net/projects/nkf/)

### nkf Network Kanji Filter
1. `nkf-2.1.5.tar.gz` 설치
    ```sh
    gzip -d nkf-2.1.5.tar.gz
    tar xvf nkf-2.1.5.tar
    cd nkf-2.1.5
    make
    sudo make install
    ```

1. 사용 예시
    ```sh
    # CP932를 UTF8로 변환
    nkf -w --cp932 ccc_cp932.csv > ccc_utf8.csv

    # UTF8을 CP932로 변환
    nkf -s --cp932 ccc_utf8.csv > ccc_cp932.csv

    # 문자 인코딩을 직접 지정 (CP932를 UTF8로 변환)
    nkf --ic=CP932 --oc=UTF-8 ccc_cp932.csv > ccc_utf8.csv
    ```

1. 파일 인코딩 확인
    ```sh
    nkf --guess ccc.csv
    ```

1. 참고
    * 대응되는 문자
      ```
              CP932 → Unicode  Shift_JIS → Unicode
      0x8160    ～     U+FF5E      〜        U+301C
      0x8161    ∥     U+2225      ‖        U+2016
      0x817C    －     U+FF0D      −         U+2212
      0x8191    ￠     U+FFE0      ¢         U+00A2
      0x8192    ￡     U+FFE1      £         U+00A3
      0x81CA    ￢     U+FFE2      ¬         U+00AC
      ```
      >[https://sites.google.com/site/fudist/Home/vim-nihongo-ban/mojibake/utf8-cp932conv](https://sites.google.com/site/fudist/Home/vim-nihongo-ban/mojibake/utf8-cp932conv)
