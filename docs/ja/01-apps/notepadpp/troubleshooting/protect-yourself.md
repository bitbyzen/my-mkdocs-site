# ハイジャック問題を解決する方法

`2026-02-02`  

Notepad++のアップデーターがハイジャックされたらしい。  
https://www.itmedia.co.jp/news/articles/2602/06/news033.html

公式サイトにもそのことが書かれている。  
https://notepad-plus-plus.org/news/hijacked-incident-info-update/

攻撃のターゲットは、個人ユーザーではないみたい。  
でも念のため、既存版はアンインストールして、最新版をインストールした方がよさそう。  
手順は以下の公式サイトに書かれている。  
https://notepad-plus-plus.org/news/clarification-security-incident/

あと、念のため、以下の処理も行っておく。

* **マルウェアのクイックスキャン**: 再インストール後、Windows Defender等のアンチウイルスソフトを実行する。
* **古いインストーラーファイルの削除**: 以前にダウンロードしたインストーラーは、誤って使用しないように削除する。
* **手動ダウンロードによる更新**: 当面は自動更新に頼らず手動で更新する。
