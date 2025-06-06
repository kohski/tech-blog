# 技術記事のガイドライン

## 修正方針

- passwordやsecretなどの機微情報は伏字にする。
- 各IDも伏字にする。その際は元のID体系が類推できる形にする
    - 例: abcd1234.ap-northesat-1.rds.com -> xxxxxxxx.ap-northesat-1.rds.com
- コードブロック
    - 言語を明記する
        - 例
            ```typescript
            console.log('Hello World')
            ```
    - cliコマンドの場合は先頭に$をつける
        - 例:
            ```bash 
            $ aws s3 ls
            ```