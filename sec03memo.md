# ライフサイクル
画面に表示されるまでの流れ

## 全体の流れ
WEBサーバーがpublic/index.phpにリダイレクト
1. autoload読み込み
2. Applicationインスタンス作成(サービスコンテナ)
3. HttpKernelインスタンス作成
4. Requestインスタンス作成
5. HttpKernelがリクエストを処理してResponse取得
6. レスポンス送信
7. terminate()で後片付け

1 autoload
requireなしで別ファイルのクラスを利用可能

2 Application(サービスコンテナ)
Bootstrap/app.phpを読み込み
app.php内
$app = new Illuminate\Foundation\Application
Applicationインスタンスが通称サービスコンテナ

サービスプロバイダも読み込まれる
registerConfiguredProviders(){}

3. Kernel
Bootstrap/app.php
singletonでKernelをサービスコンテナに登録

Public/index.php
HttpKernel
$kernel = app()->make(Kernel::class)

4. Requestインスタンス作成
Public/index.php
Request::capture()
SymfonyRequest::createFormGlobals()
-> この中で、$_GET, $_POSTなどを使っている

self::createRequestFormFactory()
->この中でRequestクラスをインスタンス化


# サービスコンテナ
たくさんのサービスを入れておく箱、と考えてOk
app()->bind()
app()->singleton()
app()->make()

dd(app())で中身を確認できる
bindings: array:65

## サービコンテナに登録する
app()->bind('lifeCycleTest', function(){
  return 'ライフサイクルテスト';
})

引数('取り出すときの名前', 機能)

## サービスコンテナから取り出す
$test = app()->make('liceCycleTest');

他の書き方
$test = app('lifeCycleTest');
$test = resolve('lifeCycleTest');
$test = App::make('lifeCycleTest');

## 依存関係の解決
依存した2つのクラス
それぞれのインスタンス化後に実行
$message = new Message();
$sample = new Sample($message);
$sample->run();

サービスコンテナを使ったパターン
app()->bind('sample', Sample::class)
$sample = app()->make('sample');
$sample->run();
サービスコンテナを使用すると、関連するクラスも同時にインスタンス化してくれる



# サービスプロバイダー(提供者)
サービスコンテナにサービスを登録する仕組み
register() 登録
boot() 登録後に実行したい処理
app()

サービスプロバイダの読み込み箇所
illuminate\Foundation\Application

```
registeredConfiguredProviders(){
  $providers =  Collection::make($this->config('app.providers'));
}
```