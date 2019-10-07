# API

app üzerinde bannerları kullanabilmek için tüm bannerların template yapısında olması gerekiyor. Sonrasında bu bannerlara API üzerinde getBannerList ile ulaşabiliriz. Ayrıca Bannerların entegrayon kodu kısmına bannerid bilgisini girmemiz gerekiyor.

# Page Speed Kuralları

#### 1- JS ve Css sıkıştırmaya konulmalı Burada dikkat edilmesi gereken ilk anda yüklenmesi gerekli olan jsler senkron, kalan jsler asenkron seçilmeli. 

ex: https://kartalyuvasi.proj-e.com/admin/moduls/scriptManager/scriptFileManager_liste.aspx?ps=80

```HTML
/*
    SENKRON OLACAKLAR
*/

jquery-base.min.js
ascMustScripts.js
customEvents-99.1.0.min.js
unveil-lazyload.js
formValidation.js
jquery.cookie.js
emos.css
```

```HTML
/*
    ASENKRON OLACAKLAR
*/

cartScripts.js
popupDimensions.js
comboBox.js
minus.popup.js
jquery.maskedinput.js
ajaxtooltip.js
fb-connect.js (ÖNEMLİ: sitede facebook connect kullanılmıyorsa bu script eklenmemeli)
google-connect.js (ÖNEMLİ: sitede google connect kullanılmıyorsa bu script eklenmemeli)
jquery-ui.min.js
swiper.min.js
mediaElement.js
custom-plugins.js
config.js
allScripts.js
custom.js
genel.css
```

#### 2- PWA için manifest.json hazılanmalı

a. Dil dosya yönetimi - site bilgilerine eklenecek kod.

ex: https://kartalyuvasi.proj-e.com/admin/moduls/mlng/lngFile_guncelle.aspx?ID=868&
```HTML
<link rel="manifest" href="/manifest.json">
```

b. E-MOS script içerisine eklenecek örnek kod

ex: https://kartalyuvasi.proj-e.com/admin/moduls/export/exportclient_guncelle.aspx?ID=3335&

```JSON
{
"short_name": "Kartal Yuvasi",
"name": "Kartal Yuvasi",
"icons": [
    {
    "src": "/images/frontend/favicon/bjk-192.png",
    "type": "image/png",
    "sizes": "192x192"
    },
    {
    "src": "/images/frontend/favicon/bjk-512.png",
    "type": "image/png",
    "sizes": "512x512"
    }
],
"start_url": "/?source=pwa",
"background_color": "#FFFFFF",
"display": "standalone",
"scope": "/",
"theme_color": "#000000"
}
```

#### 3- PWA için Service Worker eklenmeli

a. Dil dosya yönetimi - site bilgilerine eklenecek kod.

ex: https://kartalyuvasi.proj-e.com/admin/moduls/mlng/lngFile_guncelle.aspx?ID=868&

```HTML
<script>
    if ('serviceWorker' in navigator) {
      window.addEventListener('load', function() {
        navigator.serviceWorker.register('/sw.js').then(function(registration) {
          console.log('Registration was successful');
        }, function(err) {
            console.log('registration failed :(');
        });
      });
    }
</script>
```
b. E-MOS script içerisine eklenecek örnek kod

ex: https://kartalyuvasi.proj-e.com/admin/moduls/export/exportclient_guncelle.aspx?ID=3334&

```JS
var CACHE_NAME = 'kartal-yuvasi-v1.#$ssSCRIPT_FILETIME#$';
var urlsToCache = [
  '/',
  '/styles/fonts/fira/FiraSans-Regular.woff2',
  '/styles/fonts/din/DINPro-Light.woff2',
  '/styles/fonts/din/DINPro-Bold.woff2',
  '/styles/fonts/din/DINPro-CondensedRegular.woff2',
  '/styles/fonts/din/DINPro-CondensedBold.woff2',
  '/styles/js/libs/TweenMax.min.js',
  '/styles/js/libs/ScrollMagic.js',
  '/styles/js/libs/animation.gsap.js',
  '/styles/js/libs/debug.addIndicators.js',
  
  
    '/userControls/kutu/search_suggest.min.js',
    '/styles/js/uniform.js',
    '/styles/genel.css',
    
    '/styles/joinedFiles/compressed/emos_header_#$ssSCRIPT_FILETIME#$.js',
    '/styles/joinedFiles/compressed/emos_header_async_#$ssSCRIPT_FILETIME#$.js',
    '/styles/joinedFiles/compressed/emos_header_async_#$ssSCRIPT_FILETIME#$.css',
    '/styles/joinedFiles/compressed/emos_footer_async_#$ssSCRIPT_FILETIME#$.js'
];

self.addEventListener("install", function(event) {
  event.waitUntil(
    caches.open(CACHE_NAME).then(function(cache) {
      return cache.addAll(urlsToCache);
    })
  );
});
self.addEventListener('activate', function(event) {
    event.waitUntil(
      caches.keys().then(function(cacheNames) {
        return Promise.all(
          cacheNames.map(function(cacheName) {
            if (cacheName != CACHE_NAME) {
              return caches.delete(cacheName);
            }
          })
        );
      })
    );
  });
self.addEventListener("fetch", function(event) {
    event.respondWith(
      fetch(event.request).catch(function() {
        return caches.match(event.request);
      })
    );
  });
```

#### 4- Özel fontlar yüklenirken optimizasyon yapılmalı bunun içinde font-display özelliği kullanılmalıdır.

ex: https://fatihhayrioglu.com/font-display-ozelligi/


```CSS
@font-face {
  font-family: "din";
  src: url(/styles/fonts/din/DINPro-Bold.woff2) format("woff2"), url(/styles/fonts/din/DINPro-Bold.woff) format("woff"), url(/styles/fonts/din/DINPro-Bold.ttf) format("truetype");
  font-weight: 700;
  font-style: normal;
  font-display: swap;
}


.btnDefault{ font-family: din, arial, sans-serif; }

```

#### 5- CSS in asenkron yüklendiği durumlarda sitenin genel kapsayıcısı, header kısmı için appshell.css hazırlanmalı ve kaynak koda eklenmelidir.

ex: https://kartalyuvasi.proj-e.com/admin/moduls/export/exportclient_guncelle.aspx?ID=3323&

#### 6- CSS ve JS asenkron olduğu durumlarda asenkronu desteklemeyen browserlarda LoadCSS.js kaynak koda gömülmeli.

ex: https://kartalyuvasi.proj-e.com/admin/moduls/export/exportclient_guncelle.aspx?ID=3346&

#### 7- Lazyload yapısı için kullanılması gereken html yapıları

```HTML
<!-- 

    NOTLAR: 
    a. lazyload tetiklenmesi için elementler gizli olmamalı yani ekranda gözükenler üzerinde işe yarayacak. Display: none olanlarda işe yaramayacak. 

    b. picture tag kullanılmak isteniyorsa burada <source class="lazy-picture" media="(max-width:5000px)" srcset="/images/frontend/placeholder.gif"></source> çok önemli yani ilk anda browser burda tanımladığın src'i görecek.

-->
<picture>
    <source class="lazy-picture" media="(max-width:5000px)" srcset="/images/frontend/placeholder.gif"></source>
    <source srcset="/upload/banner/menu/koleksiyon-adidas.jpg 1x, /upload/banner/menu/koleksiyon-adidas.jpg 2x" media="(max-width: 960px)"></source>
    <source srcset="/upload/banner/menu/koleksiyon-adidas.jpg" media="(min-width: 961px)"></source>
    <img src="/upload/banner/menu/koleksiyon-adidas.jpg" /> 
</picture>

<!-- image tag kullanılmak isteniyorsa -->
<img data-image-src="/upload/banner/menu/kadin.png" />

<!-- background kullanılacaksa -->
<div data-background="/images/frontend/design-uniform-bg.png"></div>


<!--
    ÖNEMLİ: Yukarıda kullanılan yapılar sitede scroll yaptıkça tetikleniyor. Bazı durumlarda sitedeki swiper içerisinde swipe yaptıkça lazyload tetiklemek gerekebilir. Bu durumda lazy-swiper classını eklemek gerekiyor.
-->

<picture>
    <source class="lazy-picture lazy-swiper" media="(max-width:5000px)" srcset="/images/frontend/placeholder.gif"></source>
    <source srcset="/upload/banner/menu/koleksiyon-adidas.jpg 1x, /upload/banner/menu/koleksiyon-adidas.jpg 2x" media="(max-width: 960px)"></source>
    <source srcset="/upload/banner/menu/koleksiyon-adidas.jpg" media="(min-width: 961px)"></source>
    <img src="/upload/banner/menu/koleksiyon-adidas.jpg" /> 
</picture>

```

#### 8- Siteye eklenen videoların ilk anda yüklenmemesi için video tagına class="is-scrolling" eklenmesi ve videonun ilk anda yüklenmemesi için preload="none", iosta videonun olduğu yerde gözükmesi için playsinline="true" parametrelerinin eklenmesi gerekir. Ayrıca autoplay özelliğini eklenmemesi lazım.

```HTML
<video 
class="is-scrolling" 
src="//player.vimeo.com/external/348070064.sd.mp4?s=fd02b2e7c50467ca51268998272bb0532592b543&profile_id=165"
preload="none" 
muted="true" 
playsinline="true" 
loop="true">
</video>
```

```HTML

<!--
    NOT: Video swiper içerisinde kullanılacaksa aşağıdaki gibi bir html yapısında olması gerekmektedir. İlgili slide aktif olduğu zaman video otomatikman oynamaya başlayacaktır.
-->

<li class="swiper-slide">
    <div class="slide-video">
     <video 
            src="https://player.vimeo.com/external/347762525.hd.mp4?s=c6e7f2ca6a1a1baa8bbda4fcf71124f32fa2520b&profile_id=175"
            poster="/UPLOAD/BANNERS/homepage-slider/bt-design-dion-video-cover.png" 
            preload="none" 
            autobuffer="autobuffer" 
            preload="none" 
            muted="true" 
            playsinline="true" 
            loop="true" 
            class="video-player">
        </video>
    </div>
    <a href="javascript:void(0);" class="slide-video-btn"></a>
</li>

```

#### 9- System widgetları ilk anda yüklenmemesi scroll ettikten sonra yüklenmeleri için system widget "scroller-trigger" classı eklenmeli

``` HTML
<div class="system-widget widget swiper-container scroller-trigger" data-uri="/urun_liste.aspx?kat=22949&lang={{lang}}&ps=8">
    <div class="swiper-header"><span>ÖNE ÇIKAN ÜRÜNLER</span></div>
    <div class="swiper-inner">
        <ul class="emosInfinite swiper-wrapper">
        </ul>
    </div>
    <div class="swiper-button-prev"><i> </i></div>
    <div class="swiper-button-next"><i> </i></div>
    <div class="swiper-pagination"></div>
</div>
```
```JS

data-uri paremetresinde kullanabileceğimiz user controller.

/usercontrols/kutu/ajxUrunTab.aspx?lang={{lang}}&tip=indirim&ps=10&rp=1&tur=sepetli 

/usercontrols/kutu/ajxUrunTab.aspx?lang={{lang}}&tip=vitrin&ps=100&rp=1&tur=sepetli

/usercontrols/kutu/ajxUrunTab.aspx?lang={{lang}}&tip=encoksatan&ps=100&rp=1&tur=sepetli

/usercontrols/kutu/ajxUrunTab.aspx?lang={{lang}}&tip=yeniurun&ps=10&rp=1&tur=sepetli

/usercontrols/kutu/ajxUrunTab.aspx?lang={{lang}}&kat={{prdCat}}&tip=onecikan&ps=100&rp=1&tur=sepetli

/usercontrols/urunDetay/ascSonGezilenUrun_ajx.aspx?lang={{lang}}&listeTuru=sepetli&urunSayisi=10&repeatColumns=1&resimTipi=thumb

/usercontrols/urunDetay/ajxIlgiliUrun.aspx?lang={{lang}}&urn={{prdCode}}&ps=100&rp=1

/usercontrols/kutu/ajxUrunTab.aspx?lang={{lang}}&tip=seciliurun&ps=100&rp=1&ukods={{urn}}

NOT: 

{{lang}}: kısmı aktif dilin değerini alıyor. 

{{prdCat}}: <div data-cat="24666" class="system-widget widget swiper-container scroller-trigger" data-uri="/urun_liste.aspx?kat={{prdCat}}&lang={{lang}}&ps=8"></div> burada div üzerinde data-cat attribute görürse onun değerini alır bulamazsa bu sefer urlString de kat geçiyorsa onun değerini alır. Yani ürün listede data-cat eklemeyerek sadece urlStringden okuyarak dinamik bir şekilde widgetı tetikletebilirsiniz. Ürün detay, ana sayfa gibi yerlerde data-cat ile belirlediğiniz kategorilerden ürün çekmesini sağlayabilirsiniz.

{{prdCode}}: <div data-code="889977" class="system-widget widget swiper-container scroller-trigger" data-uri="/usercontrols/urunDetay/ajxIlgiliUrun.aspx?lang={{lang}}&urn={{prdCode}}&ps=100&rp=1"></div> burada div üzerinde data-code attribute görürse onun değerini alır bulamazsa ve ürün detaydaysa hdnURN_KOD değerini alır.

```

#### 10- Eğer Ajax tab menu kullanacaksanız ilk anda istek atmaması için "scroller-trigger" classını eklemeniz gerekir. Böylece scroll edince viewport içerisinde tab gözüktüğü zaman tetikleme gerçekleşecektir.

``` HTML
 <div class="ems-tab scroller-trigger">
        <div class="ems-tab-header">
            <a data-ajx="/urun_liste.aspx?kat=22949&lang=tr-TR&ps=8" href="javascript:void(0);"
                rel="tab-1">ERKEK</a>
            <a data-ajx="/urun_liste.aspx?kat=22928&lang=tr-TR&ps=8" href="javascript:void(0);" rel="tab-2">KADIN</a>
            <a data-ajx="/urun_liste.aspx?kat=22955&lang=tr-TR&ps=8" href="javascript:void(0);" rel="tab-3">ÇOCUK</a>
        </div>
        <div class="ems-tab-content">
            <div rel="tab-1">
                <div class="swiper-container">
                    <div class="swiper-inner">
                        <ul class="emosInfinite">
                        </ul>
                    </div>
                    <div class="swiper-button-prev"><i> </i></div>
                    <div class="swiper-button-next"><i> </i></div>
                    <div class="swiper-pagination"></div>
                </div>
            </div>
            <div rel="tab-2"><div class="swiper-container">
                    <div class="swiper-inner">
                        <ul class="emosInfinite">
                        </ul>
                    </div>
                    <div class="swiper-button-prev"><i> </i></div>
                    <div class="swiper-button-next"><i> </i></div>
                    <div class="swiper-pagination"></div>
                </div></div>
            <div rel="tab-3"><div class="swiper-container">
                    <div class="swiper-inner">
                        <ul class="emosInfinite">
                        </ul>
                    </div>
                    <div class="swiper-button-prev"><i> </i></div>
                    <div class="swiper-button-next"><i> </i></div>
                    <div class="swiper-pagination"></div>
                </div></div>
        </div>
    </div>
```
