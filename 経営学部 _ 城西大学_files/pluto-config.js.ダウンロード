
/**
 * pluto.js - アーティスWebサイト制作用Javascriptライブラリ 設定ファイル
 *
 * Copyright (C) Artis Inc. | http://www.artisj.com/
*/

/*=============================================================================================
=========== 機能設定 ==========================================================================
===============================================================================================*/

/* 使用したい機能を"true"に、使用しないものを"false"にして下さい。 */

pluto.autoExec = {

    urlBreak: false, // url改行機能

    scrollUp: false, // ページスクロール機能

    rollOver: false, // ロールオーバー機能

    flashversion: false, // flash設置機能

    fontcontroller: false, // フォントサイズ変更機能

    bgcolorcontroller: false, // 背景色変更機能

    printbtn: false, // 印刷設定機能

    autoicon: false, // オートアイコン機能

    tabs: false, // トップページ新着エリアタブ機能

    bgchanger: false, // フォーム背景変更機能

    error: false, // フォームエラー用

    defaultvalue: false, // テキストフィールドのデフォルト文字表示機能

    pagenation: true, // 新着カテゴリページ送り機能

    smartphoneguide: false, // スマフォ誘導

    coop: false, // googleカスタム検索窓設定機能

	movie: true // 動画要素設定機能（CMS3なら必ずtrueにする）


};



/*=============================================================================================
=========== 機能詳細設定 ======================================================================
===============================================================================================*/

if(pluto.autoExec.fontcontroller){ pluto.html.headerhack('newsite', 100); }; // フォントサイズ変更用


(function(){

pluto.base.addEvent(window,"load",function(){


/*----------------------------------------------------------------------
  スマフォサイト誘導機能
-----------------------------------------------------------------------*/

if(pluto.autoExec.smartphoneguide){
    pluto.html.smartphoneguide(
        {
            bodyNode: "body-in", // ボタンを追加する兄弟要素のID
            spDirName: "sp/", // スマートフォンサイトの格納ディレクトリを「/」つきで指定
            btnClass: "sp-btn", // 誘導ボタンのpタグに設定するクラス名
            range: "all", // JavaScriptの適応範囲（all or top）
            type: 'btn', // JavaScriptの適応パターン（btn or auto）
            image: "common/image/sp-btn.gif", // 誘導ボタンへのパス
            alt: "スマートフォンサイトへ" // 画像のalt
        }

    );
};


/*----------------------------------------------------------------------
  フォントサイズ変更機能
-----------------------------------------------------------------------*/

if(pluto.autoExec.fontcontroller){
    pluto.html.fontController(
        {

            smallID: 'small', // 「小」ボタンのid値
            resetID: 'reset', // 「標準」ボタンのid値
            largeID: 'large', // 「大」ボタンのid値
            current: 100, // 標準フォントサイズ[px] ※同時に上部の1ヶ所を変更してください。
            rate: 10,  // フォントサイズ変化量
            cookiename: 'newsite', // cookieの名前　※サイト毎に変更をお願いします。
            outputID: 'fontController', // フォントサイズエリアのid値
            source: '<dl class="fsize clearfix"><dt class="fsize-title">文字サイズ</dt><dd class="small"><a href="javascript:void(0)" id="small" class="small-btn">小</a></dd><dd class="middle"><a href="javascript:void(0)" id="reset" class="reset-btn">中</a></dd><dd class="large"><a href="javascript:void(0)" id="large" class="large-btn">大</a></dd></dl>' // ソース

        }
    );
};


/*----------------------------------------------------------------------
  トップページ新着エリアタブ機能
-----------------------------------------------------------------------*/

if(pluto.autoExec.tabs){
    pluto.html.tabs(
        {

            node: 'tab-area',
            tabarr: ['all-tab','tab01','tab02','tab03'],
            currenttab: 'all-tab',
            homeID: 'body-in',
            homeClass: 'home',
            setattribute: true //リンクタグの反転
        }

    );
};

/*----------------------------------------------------------------------
  url改行機能
-----------------------------------------------------------------------*/

if(pluto.autoExec.urlBreak){
    pluto.html.urlBreak(

        "break", // urlを改行させるエリアのid値
        "breaknode" // urlを改行させるタグのclass値

    );
};

/*----------------------------------------------------------------------
  新着カテゴリページ送り機能
-----------------------------------------------------------------------*/
if(pluto.autoExec.pagenation){
    pluto.html.pagenation(
        {

            //list: 3, // 1ページに表示させるニュースの数。
            newsList: 20, // 1ページに表示させるニュースの数。
            blogList: 5, // 1ページに表示させるニュースの数。
            range: 8, // 前後に表示させるページ送りの数。
			targetId: 'news-list', // ニュースエリアを囲むdivタグのid値。
            next: '<a href="javascript:void(0);">次へ>></a>', // 「次へ>>」の表示。
            prev: '<a href="javascript:void(0);"><<前へ</a>', // 「<<前へ」の表示。
			currentClass: 'on', // カレントのliタグに設定するクラス。
			pagenationId: 'pagenation-area', // システムで使用します。既に利用しているとき以外は変更しないでください。
            pagenationClass: 'pager clearfix'

        }
    );
};


/*----------------------------------------------------------------------
  ロールオーバー機能
-----------------------------------------------------------------------*/

if(pluto.autoExec.rollOver){
    pluto.html.rollOver(

        "rollover" // ロールオーバさせる要素のclass値

    );
};


/*----------------------------------------------------------------------
  ページスクロール機能
-----------------------------------------------------------------------*/
if(pluto.autoExec.scrollUp){
	pluto.html.scrollUp(
		{

			tag: "a", // イベントを設置するタグ名
			speed: 40, // スクロールスピード
			easing: 3 // スクロールイージング値（値が高くなると、止まるタイミングが遅くなります）

		}
	);
};


/*----------------------------------------------------------------------
  フラッシュ設置機能
-----------------------------------------------------------------------*/

if(pluto.autoExec.flashversion){
    pluto.html.flashVersion(
        {

            version: 8, // flash 書き出しversion
            id: "flashset", // flashが挿入されるブロックのid値
            filepath: "common/flash/top.swf", // flashファイルへのパス
            width: 940, // flashの幅
            height: 363, // flashの高さ
        transparent: false, // flashの背景透過
            source: ' ' // flashのversionが低い時の代替文言（画像でも可）

        }
    );
};


/*----------------------------------------------------------------------
  印刷設定機能
-----------------------------------------------------------------------*/

if(pluto.autoExec.printbtn){
    pluto.html.printBtn(
        {

            node: 'print-btn-area', // 印刷ボタンエリアのid値
            type: 'all' // 'all'=>「全画面プリント」「本文プリント」 'part'=>「本文プリント」

        }
    );
};

/*
挿入されるHTML

<ul>
<li class="print001"><a href="#" id="print-all-btn">全画面プリント</a></li>
<li class="print002"><a href="#" id="print-part-btn">本文プリント</a></li>
</ul>

*/
/*----------------------------------------------------------------------
  背景色変更機能
-----------------------------------------------------------------------*/
if(pluto.autoExec.bgcolorcontroller){
    pluto.html.bgcolorController(
        {

            color: [ 'bgcolor-a',
                     'bgcolor-b',
                     'bgcolor-c'
                                ], // 各背景色ボタンのボタンのid値
            preset: 'default', // 「標準」ボタンのid値。
            cookiename: 'newsite_color', // cookieの名前　※サイト毎に変更をお願いします。
            directory: 'bgcolor/', // 背景色用のCSSを格納するディレクトリへのパス（後ろにスラッシュをつけてください）
            outputID: 'bgcolorController', // 背景色変更エリアのid値
            source: '<dl class="bgcolor clearfix"><dt class="bgcolor-title">背景色変更</dt><dd class="default"><a href="javascript:void(0)" id="default" class="default-btn">白</a></dd><dd class="bgcolor-a"><a href="javascript:void(0)" id="bgcolor-a" class="bgcolor-a-btn">青</a></dd><dd class="bgcolor-b"><a href="javascript:void(0)" id="bgcolor-b" class="bgcolor-b-btn">黄</a></dd><dd class="bgcolor-c"><a href="javascript:void(0)" id="bgcolor-c" class="bgcolor-c-btn">黒</a></dd></dl>'// カレントのクラス名は末尾に「-on」とする

        }
    );
};

/*----------------------------------------------------------------------
  オートアイコン機能
-----------------------------------------------------------------------*/

if(pluto.autoExec.autoicon){
    pluto.html.autoIcon(
        {

            data: ['xls','doc','pdf'], // ファイルの拡張子
            cancel: 'no-icon' // 適用させないタグのclass名

        }
    );
};


/*----------------------------------------------------------------------
  フォーム背景変更機能
-----------------------------------------------------------------------*/
if(pluto.autoExec.bgchanger){
    pluto.form.bgChanger(
        {

            formname: 'mainform', // フォームのname値
            normalbg: '#ffffff', // 通常時の背景色　※cssでの指定より優先されます
            normalborder: 'solid 1px #a5acb2', // 通常時の境界線　※cssでの指定より優先されます
            currentbg: '#FFFFC4', // 選択時の背景色
            currentborder: 'solid 1px #a0a0a0' // 選択時の境界線

        }
    );
};


/*----------------------------------------------------------------------
  フォームエラー用
-----------------------------------------------------------------------*/
if(pluto.autoExec.error){
    pluto.form.error(
        {

            formname: 'mainform', // フォームのname値
            errorname: 'error_name', // エラー要素を格納するinputのname値
            errorbg: '#ffdcdc', // エラー時に変更する背景色
            errorborder: 'solid 1px #a5acb2' // エラー時に変更する境界線

        }
    );
};


/*----------------------------------------------------------------------
  テキストフィールドのデフォル文字表示機能
-----------------------------------------------------------------------*/
if(pluto.autoExec.defaultvalue){
    pluto.form.defaultValue(
        {

            defcolor: '#cccccc', // デフォルト値のフォントカラー
            inputcolor: '#000000', // 入力時のフォントカラー
            classname: 'defaultvalue' // 適用させるinputタグのclass値

        }
    );
};


/*----------------------------------------------------------------------
  googleカスタム検索窓設置機能
-----------------------------------------------------------------------*/
if(pluto.autoExec.coop){
    pluto.google.coop(
        {

        formid: 'cse-search-box', // googleco-opフォームのid値
        searchborder: 'solid 1px #CCCCCC', // 検索窓の境界線
        searchwidth: '150px', // 検索窓の幅
        searchheight: '23px', // 検索窓の高さ
        searchpadding: '3px' // 検索窓の余白（padding）

        }
    );
};

/*----------------------------------------------------------------------
  動画要素設定機能
-----------------------------------------------------------------------*/
if(pluto.autoExec.movie){
    pluto.html.movie(
         "flash-area" // 適応させる要素のclass値（基本変更しない）
    );
};



/*-----------------------------------------------------------------------*/
});
})();
