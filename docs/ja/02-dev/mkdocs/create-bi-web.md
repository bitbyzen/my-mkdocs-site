# バイリンガルサイトを構築する方法
## はじめに
英日のバイリンガルサイトを構築する際の前準備は、ざっくりと以下のとおり。

## 手順
1. **docs**フォルダ内に**en/ja**フォルダを配置する。

      ```bash title="terminal"
      mkdir en && ja
      ```
       
1. とりあえず、テーマは**Material**を使用する。
    * **Material**以外の方法は未調査。

      ```yaml title="mkdocs.yml"
      theme:
        name: material
        language: en
        features:
          - navigation.tabs
          - navigation.tabs.sticky
          - navigation.sections
      ```

1. **i18n**プラグインを使用する。
    - **i18n**プラグインは**plugins**リストの最上部に記述する。</br>
    これにより、サイトのトップメニューアイテムの位置を制御できるようになる。

      ```yaml title="mkdocs.yml"
      plugins:
        - i18n:
        - awesome-pages
        - search
        - glightbox
      ```

1. ナビメニューは、**nav**ではなく、フォルダ階層を利用して作成する。
    - これにより、**YAML**ファイル内にナビメニューアイテムを一切記述しなくても、自動的にアイテムをアルファベット順に並べることができる。

      ```yaml title="mkdocs.yml"
      plugins:
        - i18n:
            docs_structure: folder
      ```

1. メインにしたい言語を`default:true`にする。

      ```yaml title="mkdocs.yml"
      plugins:
        - i18n:
            docs_structure: folder
            fallback_to_default: false
            languages:
              - locale: en
                default: true
                name: EN
                build: true
              - locale: ja
                name: JA
                build: true
                theme:
                  language: ja
      ```

1. 日本語サイトのトップメニューアイテムの位置を制御するために、**CSS**ファイルを作成する。
    - 英語サイトの場合、**Material**だけで問題ないけど、日本語サイトの場合、**Material**だけだとうまくいかない。

      ```css title="tabs-ja.css"
      /* Apply only on Japanese pages */
      html[lang="ja"] .md-tabs__link {
        font-size: 0 !important;
      }

      html[lang="ja"] .md-tabs__item:nth-child(1) .md-tabs__link::after {
        content: "ホーム";
        font-size: .7rem;
      }
      html[lang="ja"] .md-tabs__item:nth-child(2) .md-tabs__link::after {
        content: "アプリメモ";
        font-size: .7rem;
      }
      ```

1. **CSS**ファイルを**en/ja**フォルダの両方に配置する。

    - **ja**フォルダだけに配置するとうまくいかない。

      ```yaml title="mkdocs.yml"
      extra_css:
        - stylesheets/tabs-ja.css
      ```

---