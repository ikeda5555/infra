# Istio Install

## 環境要件
### kindの場合
* 複数ノード、外部ポート開ける
* Docker for Mac、またはDocker for WindowsのPreferences⇨Advancedで割り当てリソース増やす

## インストール
### 設定ファイルダウンロード
* https://istio.io/docs/setup/kubernetes/#downloading-the-release
### CRDのapply
* https://istio.io/docs/setup/kubernetes/install/kubernetes/
* for i in install/kubernetes/helm/istio-init/files/crd*yaml; do kubectl apply -f $i; done
* Macでエラーの場合、以下リンクの内容試す
* https://stackoverflow.com/questions/55417410/kubernetes-create-deployment-unexpected-schemaerror
### Istioコンポーネントをデプロイ
* kubectl apply -f install/kubernetes/istio-demo.yaml
* 数分かかる
* ロードバランサーない場合、istio-ingressgatewayをNodePortに変更
* kindの場合、http2、httpsのノードポートを外部に開けた番号に変更
## デモ
### デモアプリデプロイ
* https://istio.io/docs/examples/bookinfo/
* kubectl label namespace default istio-injection=enabled
* kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
* リソースきつければこれも時間かかる
### gatewayリソース作成
* kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
* 下記を参考にIngressポート確認（istio-ingressgatewayのhttp2とhttpsのNodePort）
* https://istio.io/docs/tasks/traffic-management/ingress/ingress-control/#determining-the-ingress-ip-and-ports
* kindの場合、　export GATEWAY_URL=localhost:【http2のポートに割り当てたホスト側ポート】
* curlでアクセス　curl -s http://${GATEWAY_URL}/productpage | grep -o "<title>.*</title>"
* 画面でも同URLで確認
### トラフィックコントロール
* DestinationRule作成
* kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml
* VirtualService作成
* https://istio.io/docs/tasks/traffic-management/request-routing/
* kubectl apply -f samples/bookinfo/networking/virtual-service-all-v1.yaml
### モニタリング
* https://istio.io/docs/tasks/telemetry/metrics/using-istio-dashboard/