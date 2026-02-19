# **Exec format error**を解決する方法

## 現象
`explorer.exe .`を使用してWindows Explorerを開こうとしたら、以下のエラーが表示される。
```
-bash: /mnt/c/WINDOWS/explorer.exe: 　
cannot execute binary file: Exec format error
```
## 環境 
- VS Code
- WSL2
- Ubuntu terminal

## 結論
設定を見直しても問題なさそうなら、おそらく一時的な問題。  
多くの場合、WSL2を再起動すれば直る。

## 手順
1. VS Codeを閉じる。
1. PowerShellで以下のコマンドを実行する。
    ```
    wsl --shutdown
    ```
1. VS Codeを再起動する。

---