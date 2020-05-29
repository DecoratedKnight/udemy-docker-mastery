## Section 3: Creating and Using Containers Like a Boss
ポートのパブリッシュは ホスト:コンテナ になっているので、nginxを 8080:80 で開ければ、localhost:8080 で繋がる

nginxコンテナに-itオプションでbashとかを指定して入ると、サーバーは立ち上がらない
→ 起動するコマンドの代わりにbashを入れているから当然

Dockerのネットワークについて
https://knowledge.sakura.ad.jp/23899/

自分で作ったbridgeネットワークだとコンテナ名で名前解決できる（デフォルトのbridgeだとできない）

## Section 4: Container Images, Where To Find Them and How To Build Them
イメージのタグ付けは docker image tag \<source> \<target>
リポジトリ名とタグ両方つける感じ ex) docker image tag nginx decoratedknight/nginx:testing
レベルを省略するとlatestが対象になるっぽい

変化がよくある部分はできるだけDockerfileの後ろの方に書く（キャッシュを利用したいので）