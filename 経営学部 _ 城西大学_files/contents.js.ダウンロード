$(function(){
    'use strict';

    /*タブレット表示用----------------*/
    const ua = window.navigator.userAgent.toLocaleLowerCase();
    if(ua.indexOf('ipad') > 0 || (ua.indexOf('macintosh') > -1 && 'ontouchend' in document)) {
        $("meta[name='viewport']").attr('content','width='+1260);
    }

    /*スティッキーヘッダー　アンカーポイント設定----------------*/
    //アンカーポイント設定
    var //スクロール数
        ds = 0,
        fixedY = 0,/*fixedのタイミング*/
        // スティッキーヘッダー（余白を含む）高さ
        pcDiffHeight = 100,
        tabDiffHeight = null,
        spDiffHeight = 60,
        // BreakPoint
        pcBreakPoint = 769,
        spBreakPoint = 769,
        // jQuery animate options
        duration = 550,
        animation = 'swing';

    //スティッキーヘッダー
    $(window).scroll(function(){
        ds = $(this).scrollTop();
        if (ds > fixedY) {
            $('#gnavi').addClass('gnavi-area-fixed');
        } else {
            $('#gnavi').removeClass('gnavi-area-fixed');
        }
    });

    // 別ページからのリンク
    setTimeout(function() {
        var targetElemId = location.hash,
            diffHeight = getResponsiveDiffHeight();

        if ($('#body-in').hasClass('home')) return;
        if (targetElemId) {
            $(window).trigger('scroll');
            $(window).scrollTop(
                $(targetElemId).offset().top - diffHeight
            );
        }

    }, 500);

    // ページ内リンクのイベント設定 /*.search-area .btn-area a*/
    $('a[href*="#"]').not(function() {
        // 別ぺージからのアンカーリンクは除外する
        var a = location.href.replace('index.html', '').replace(/#.*$/, '');
        var b = $(this).attr('href').replace('index.html', '').replace(/#.*$/, '');

        return a !== b && !$(this).attr('href').match(/^#/);
    }).on('click', function(e) {
        e.preventDefault();
        e.stopPropagation();

        var href = $(this).attr('href'),
            $elem = $(href.substr(href.indexOf('#')));

        if (!$elem.length) {
            return false;
        }

        var diffHeight = getResponsiveDiffHeight();

        $("html, body").animate(
            {
                scrollTop: $elem.offset().top - diffHeight
            },
            duration,
            animation
        );
    });

    // widthによってPC、Tablet、SPのスティッキーヘッダー計算用高さを返す
    function getResponsiveDiffHeight()
    {
        var w = window.innerWidth ? window.innerWidth : $(window).width();

        if (w >= pcBreakPoint) {
            return pcDiffHeight;
        } else if (spBreakPoint < w && w < pcBreakPoint) {
            return tabDiffHeight;
        } else {
            return spDiffHeight;
        }
    }

    (function($) {
        const userAgent = window.navigator.userAgent.toLowerCase();
        if (userAgent.indexOf('firefox') != -1) {
            $('#body-in').addClass('is-firefox');
        }
    })(jQuery);

    (function($) {
        const userAgent = window.navigator.userAgent.toLowerCase();
        if (userAgent.indexOf('ipad') > 0 || (userAgent.indexOf('macintosh') > -1 && 'ontouchend' in document)) {
            $('#body-in').addClass('is-ipad');
        }
    })(jQuery);

    (function($) {
        const userAgent = window.navigator.userAgent.toLowerCase();
        if (userAgent.indexOf('edge') == -1) {
            if (userAgent.indexOf('chrome') == -1) {
                if (userAgent.indexOf('safari') != -1) {
                    $('#body-in').addClass('is-safari');
                }
            }
        }
    })(jQuery);

    // scroll header for top
    (function() {
        if ($('.home').length === 0) return;
        const mq_sp = window.matchMedia('(max-width: 767px)');
        if (mq_sp.matches) return;

        let isFixed = false;
        const $header = $('header');
        const $logo = $('#js-header-main-logo');
        const w = window.innerWidth ? window.innerWidth : $(window).width();
        const fp = (w > 767) ? 800 : 1000;

        $logo.attr({src: $logo.attr('src').replace('002','001')});

        $(window).scroll(function(){
            const st = $(this).scrollTop();
            if (st > fp) {
                if (isFixed) return;
                isFixed = true;
                $header.addClass('header-fixed');
                $logo.attr({src: $logo.attr('src').replace('001','002')});
            } else {
                if (!isFixed) return;
                isFixed = false;
                $header.removeClass('header-fixed');
                $logo.attr({src: $logo.attr('src').replace('002','001')});
            }
        });
    })();

    // scroll header for under
    (function() {
        if ($('.home').length !== 0) return;

        let isFixed = false;
        const $header = $('header');
        const w = window.innerWidth ? window.innerWidth : $(window).width();
        const fp = (w > 767) ? 100 : 1000;

        $(window).scroll(function(){
            const st = $(this).scrollTop();
            if (st > fp) {
                if (isFixed) return;
                isFixed = true;
                $header.addClass('header-fixed');
            } else {
                if (!isFixed) return;
                isFixed = false;
                $header.removeClass('header-fixed');
            }
        });
    })();

    // サイト内検索のtoggle
    (function() {
        $('#js-toggle-site-search').on('click', function() {
            $('#google-site-search').slideToggle();
        });
    })();

    /*
    // ページの先頭へ戻るボタン
    (function() {
        const body = window.document.body;
        const html = window.document.documentElement;
        const $btn = $('.footer__pagetop');

        function getScrollBottom() {
            const scrollTop = body.scrollTop || html.scrollTop;
            return html.scrollHeight - html.clientHeight - scrollTop;
        }

        if ($btn.length > 0) {
            $(window).scroll(function () {
                if (getScrollBottom() < 170) {
                    $btn.addClass('stop');
                } else if ($(this).scrollTop() > 300) {
                    $btn.removeClass('stop');
                    $btn.fadeIn();
                } else {
                    $btn.fadeOut();
                }
            });
        }
    })();
    */

    // PC float with scroll
    (function() {
        const $float = $('.footer__pagetop');
        let timer;
        $(window).on('scroll', function() {
            clearTimeout(timer);
            if ($(this).scrollTop() < 100) {
                $float.fadeOut();
                return;
            }
            if ($float.is(':visible')) {
                $float.fadeOut();
            }
            timer = setTimeout(function() {
                $float.fadeIn();
            }, 500)
        });
    })();
});
