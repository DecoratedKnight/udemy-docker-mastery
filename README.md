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

## Section 5: Container Lifetime & Persistent Data: Volumes, Volumes, Volumes
volume作ったときのパスは直接アクセスできない
Docker for Macは実際にはLinux VMを立ち上げていて、その中のパスだから

コンテナ側のマウントさせることができるパスはVOLUMEとしてDockerfileに記載しないとダメなのかな？
→ 違った、マウントはできる
→ VOLUMEに書いたパスは勝手に名前なしのボリュームでマウントされる、名前を付ければそのNamed Volumeがつく

マウント時のオプションは -vと--mountがあるが、公式は--mount推しっぽい
data volumeの時 -vの左側にボリューム名
docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=yes -v mysql_data:/var/lib/mysql mysql
docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=yes --mount source=mysql_data,target=/var/lib/mysql mysql

bind mountの時 -vなら左側をパスにする
ホスト側にあるファイルと同期するので、開発の時に使う
docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx
docker container run -d --name nginx -p 80:80 --mount type=bind,source=$(pwd),target=/usr/share/nginx/html nginx

デタッチしてるコンテナのログを見るときは docker container logs -f 

## Selction 6: Making It Easier with Docker Compose: The Multi-Container Tool
volumeをread-onlyにすることもできる
docker-composeで立ち上げるとネットワークが勝手に作られる

assingment1
namedvolume にしないと、毎回新しいvolumeができちゃって引き継がれない
docker-compose down -v とするとvolumeも削除される

docker-composeでビルドする時にimageを省略することもできる
docker-compose down --rmi local とすると、down時にimageを消す

## Selection 7: Swarm Intro and Creating a 3-Node Swarm Cluster
http://docs.docker.jp/engine/swarm/key-concepts.html

ノード（node） とは、Swarm 内に参加する Docker Engine インスタンスです。

サービス（service） とは、アプリケーションを作り上げるための様々なタスクを、どのように実行するかという定義です。たとえば、Swarm 内で Redis イメージをデプロイするサービスを作成します。
サービスの作成時に指定するのは、どのコンテナ・イメージを使い、コンテナ内でどのようなコマンドを実行するかです。

タスク（task） とは Docker コンテナを運び、コンテナ内でコマンドを実行します。これは Swarm における最小スケジューリング単位です。マネージャ・ノードはワーカ・ノードに対してタスクを割り当てます。割り当てる数はサービスのスケールで設定されたレプリカ数に応じます。

マネージャーノードはワーカーも兼ねる

docker service ls ... 立ち上がってるサービスがリストで出る
docekr service ps \<id> ... そのサービスの実行状況(タスク)がわかる。どのノードにいるかもわかる

ノード ... デプロイ対象になる場所、それを管理するもの（マネージャーノード）
サービス ... やること
タスク ...実際にやってるもの

サービスを削除しても、立ち上げたコンテナが自動で止まるわけじゃない

docker-machine
env設定してdockerコマンドのアクセス先を変える
eval "$(docker-machine env default)"

戻すときは再起動するか新しいシェル開くか以下
eval $(docker-machine env -u)

ターミナルタイプがないって言われたら
https://www.geek.sc/archives/2408