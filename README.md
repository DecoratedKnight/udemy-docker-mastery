## Section 3: Creating and Using Containers Like a Boss
ポートのパブリッシュは ホスト:コンテナ になっているので、nginxを 8080:80 で開ければ、localhost:8080 で繋がる

nginxコンテナに-itオプションでbashとかを指定して入ると、サーバーは立ち上がらない
→ 起動するコマンドの代わりにbashを入れているから当然

Dockerのネットワークについて
https://knowledge.sakura.ad.jp/23899/

自分で作ったbridgeネットワークだとコンテナ名で名前解決できる（デフォルトのbridgeだとできない）