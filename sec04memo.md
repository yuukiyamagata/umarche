#  モデルとマイグレーション設定
1. モデル、マイグレーション作成
2. ルート設定
3. ルートサービスプロバイダー
4. ガード設定
5. ミドルウェア
6. リクエストクラス設定
7. コントローラ&ブレード設定


# ガード設定
(参照: https://qiita.com/tomoeine/items/40a966bf3801633cf90f)
Laravel標準の認証機能
confing/auth.php

'guards' => [
  'guard-name' => [
    'driver' => 'session',
    'provider' => 'users',
  ],
],

Route::get('test', function(){})->middleware('auth::guard-name')
ルートでauth::ガード名で認証されたユーザーだけにアクセス許可

# Middleware設定
Middleware/Authenticate.php
ユーザーが未認証の場合のリダイレクト処理

URLによって条件分岐
if(Route::is('user.*')){
  return route($this->user_route)
}

Middleware/RedirectAuthenticated
ログイン済みのユーザーがアクセスしてきたらリダイレクト処理

Auth::guard(self::GUARD_USER)->check()
ガード設定対象ユーザーか

if($request->routes('users.*)){}
受信リクエストが名前付きルートに一致するか


# リクエストクラス
App\Http\Request\Auth\LoginRequest.php
ログインフォームに入力された値からパスワードを比較し、認証する

User,Owner,Admin3つのフォームがあるので、routeIs()でルート確認しつつAuth::guard()を追加

Auth::guard($guard)->attempt()