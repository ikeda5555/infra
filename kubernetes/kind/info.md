# 参考
### 公式
* https://kind.sigs.k8s.io
### 公式リポジトリ
* https://github.com/kubernetes-sigs/kind


### バイナリ（Mac以外はここから。MacならReadmeのcurl以下コマンド）
* https://github.com/kubernetes-sigs/kind/releases


### 日本語解説
* https://blog.cybozu.io/entry/2019/07/03/170000

# 手順
## インストール
### Mac

```
curl -Lo ./kind-darwin-amd64 https://github.com/kubernetes-sigs/kind/releases/download/v0.4.0/kind-darwin-amd64
chmod +x ./kind-darwin-amd64
mv ./kind-darwin-amd64 /some-dir-in-your-PATH/kind
```

### Windows
* 下記からWindows用バイナリを取得して、Path通ったとこに配置。
* https://github.com/kubernetes-sigs/kind/releases

## クラスタ構築
### シングルノード

```
kind create cluster
```

### マルチノード

```
kind: Cluster
apiVersion: kind.sigs.k8s.io/v1alpha3
nodes:
- role: control-plane
- role: worker
- role: worker
- role: worker
```

```
kind create cluster　--config multiNodeCluster.yaml
```

### クラスタ外部にポート開ける

```
kind: Cluster
apiVersion: kind.sigs.k8s.io/v1alpha3
nodes:
- role: control-plane
- role: worker
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    listenAddress: "127.0.0.1" # Optional, defaults to "0.0.0.0"
    #protocol: udp # Optional, defaults to tcp
```

```
kind create cluster　--config outerPort.yaml
```

## CLIでのクラスタへのアクセス
* export KUBECONFIG="$(kind get kubeconfig-path --name="kind")"
* インストール時に出力されるexportを実行

## クラスタ削除

```
kind delete cluster
```
