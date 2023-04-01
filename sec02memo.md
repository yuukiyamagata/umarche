# ブラウザに表示されるまでの流れ
1. index.php
(サービスプロバイダ、サービスコンテナ、etc...)
2. middleware
3. routing
4. controller <-> model DB
5. view


# サービスプロバイダ
config/app.php内のproviders,aliasにAuthと記載

# ルーティング
routes/web.php
route/auth.php

# controllerとViewの分離
DBとデータのやり取り
変数の受け渡し

componentクラス
Bladeコンポーネント
=> ビュー

# componentのパターン
1つのコンポーネント(部品)を複数のページで使える
コンポーネント側を修正すると全て反映される

## $slot
Blade側
<x-app>この文章が差し込まれる</x-app>
Component側 component/app.blade.php
{{ $slot }}
-> マスタッシュ構文

## 名前付きスロット
スロットが複数ある時に使われる
Blade側 
<x-slot name="header">この文章が差し込まれる<x-slot>
Component側 component/app.blade.php
{{ $header }}

## 属性
Blade側
<x-card title="タイトル" />
Component側 component/card.blade.php
{{ $title }}

## 変数
Blade側
<x-card :message="$message" />
Component component/card.blade.php
コントローラなどに指定 $message
{{ $message }}

## 初期値
Blade側
設定しない場合、初期値が反映される
Component側 component/card.blade.php
@pops({'message' => '初期値です'})

## クラスの設定 属性バック

<div {{ $attribute }}>
既存のClassを使用しつつ、新たにClassを加えたいときは
$attribute->merge(['class' => 'text-sm' ])


## Bladeコンポーネント
クラスベース php artisan make:component xxx
インライン php artisan make:component xxx --inline
匿名コンポーネント 直接ファイル作成












