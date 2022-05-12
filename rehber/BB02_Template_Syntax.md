Vue render'lanan (işlenerek oluşturulan) temel komponent örneğinin (instance) verilerine DOM'u deklaratif (bildirimsel) olarak binding yapmanıza (bağlamanıza) izin veren HTML tabanlı bir template (şablon) sinteksi (sözdizimi) kullanır. Tüm Vue template'leri, HTML ve uyumlu tarayıcılar tarafından pars edilebilen (ayrıştırılan) geçerli sintekslerdir (sözdizimleridir). 

Vue kendi yapısı içinde template'leri, yüksek seviyede optimize edilmiş JavaScript kodunda kompile eder (derler). Reaktivite sistemi ile birlikte Vue, App (uygulama) durumu her değiştiğinde çok düşük miktarda DOM manipülasyonu ile onu yeniden oluşturur ve app içinde yapılan değişikliği, gerçekleştirebileceği en az değişiklikle gerçekleştirerek sunar. 

Eğer Virtual DOM (Sanal Belge Obje/Nesne Modeli) kavramlarına bir aşinalığınız varsa yani JavaScript'i saf olarak kullanmayı tercih ediyorsanız, template'ler kullanmak yerine oluşturma fonksiyonlarını kendiniz de yazabilirsiniz. Ayrıca isteğe bağlı olarak JSX desteğini de kullanabilirsiniz. Fakat kullanacağınız bu saf JavaScript kodlarında compile-time (derleme zamanı) optimizasyonları yaparsanız, bunun hoş bir tepki vermeyeceğini de lütfen unutmayın. 

# Metin Enterpolasyonu (metne ekleme yapılması)

Bir veriyi template içinde kullanmanın en temel biçimi "Bıyık" (çift süslü parantez) sinteksi (sözdizimi) adı verilen ekleme ile yapılmasıdır: 

```html
<span> Mesaj : {{ msj }} </span>
```
Parantez içinde verilen `msj` etiketi, script içinde kendisine verilmiş `msj` değişkeninin özelliği (değeri) ile değiştirilecek ve burada sunulacaktır. Ayrıca daha sonra öğreneceğiniz üzere, bu değişkeni reaktif olarak tanımladıysanız artık `msj` değeri, değişken her değiştiğinde güncellenecektir.

# Raw (Saf) HTML
Çift süslü parantez, verileri HTML olarak değil düz metin olarak yorumlar ve gösterir. Eğer gösterilecek sonucun bir HTML çıktısı olarak gösterilmesini istiyorsanız `v-html` direktifini (yönergesini??) kullanmanız gerekir:

```javascript
<script setup>
import { ref } from 'vue'

const metin = ref('<b>Kalın yazılmasını istiyorum</b>')
</script>

<template>
  <p>
    {{ metin }}
  </p>
  <p>
    <span v-html="metin"></span>
  </p>
</template>
```

```diff 
Metin interpolasyonunun (ekleme yapılmasının) kullanımı: <span style="color:red">Bu metin kırmızı olacak</span>
v-html direktifi kullanımı ile aldığımız sonuç: 
-Bu metin kırmızı olacak 
```
Elbette yine doğal olarak HTML ve CSS etiketlerinin kullanımı ile basit şekilde birçok şey yapabiliriz. Fakat burada karşımıza yeni bir şey çıkıyor. `v-html` attribute (öznitelik) direktif adını verdik ve bu şekilde kullanıldığını gösterdik. Sizce direktif adı verilen bu özniteliğin başına yalnızca `v-` eklediğimiz için mi bu gerçekleşti? Elbette Vue'nin özniteliğin başına `-v` koyması kendisini temsil eder ve bu türdeki tüm direktiflere bu harf ile tire eklenir fakat HTML'de böyle bir öznitelik de yoktur. Yani bir HTML etiketine (tag) `html`  özniteliğini yazarak da böyle bir sonuca ulaşamazdık. Sonuç olarak yani tahmin edebileceğiniz üzere oluşturulan bu öznitelik artık Vue'da direktif adını alır ve DOM'a özel reaktif davranış da sergiler (eğer değişkeni reaktif hale getirdiyseniz). Mantığınızında elverdiği ölçüde böyle bir kullanımda direktif adını verdiğimiz etiket özniteliklerini de kendimizin tanımlaması gerektiğini fark etmişsinizdir. Fakat burada temel olarak anlatılmak istenen husus saf HTML etiket ve öznitelikleri gibi Vue ile birlikte, biz de kendimize özel etiket ve öznitelikler oluşturabileceğimizdir. Reaktif kullanım ile birlikte biz bu `saf HTML` kodlarımıza artık bu elementin iç özelliklerini "güncel tut" diyebiliyoruz.   

Bu tür durumlarda `span` etiketinin safHTML özelliğinin değeri ile değiştirilecektir. Bildiğimiz normal HTML kullanımları da aynen yorumlanır. Bazı dillerde kullanıldığı üzere `v-html` ile template içinde bölümler oluşturulabilir. Ancak Vue'da böyle bir kullanım yoktur. Çünkü Vue string (dize) tabanlı bir şablonlama motoru değildir. Bu nedenle bunun yerine Vue'da UI kullanımı ve komponizyonlaması (bileşenlerden oluşur hâle getirme) temel birim olarak komponentler kullanılmaktadır. 


```diff 
-GÜVENLİK UYARISI 
``` 
|Elbette tüm bu konuştuklarımız doğrultusunda, template içerisinde Vue direktifleri ile birlikte aynı zamanda normal HTML etiketi attribute'larını kullanabileceğimizi de düşünmüş olmalısınız. Bunda bir problem bulunmamasına rağmen Vue template içinde dinamik olarak rastgele şekilde `v-html` ile HTML kodları oluşturmak tehlikeli olabilir. Bu durum belki XSS güvenlik açıklarına yol açabilir. Bu nedenle `v-html` direktifini yalnızca güvenilen içeriklerde kullanın ve asla kullanıcı tarafından sağlanan içeriklerde bu direktife kodlamanızda yer vermeyin. |
|---|

## Attribute (Öznitelik) Bağlantıları
Çift süslü parantezler HTML özniteliklerinde (attributes) kullanılamaz. Bunun yerine `v-bind` direktifini kullanın.
```html
<div v-bind:id="dinamikId"></div>
```
`v-bind` direktifi böylelikle Vue'ya elementin `id` özniteliğini (attribute) komponentin dinamik özelliği ile senkronize olmuş halde tutması talimatını vermiş olur. Eğer deer `null` (boş) ya da `undefined` (tanımsız) ise attribute (öznitelik) oluşturulmuş bu elementten kaldırılacaktır.  

## Kısa gösterimi/kullanımı
`v-bind` çok yaygın olarak kullanıldığından dolayı bunun için özel bir kısaltma uygulanmıştır:
```html
<div :id="dinamikId"></div>
```
`:` ile başlayan öznitelikler doğal olarak normal HTML'den farklı görünebilir ancak aslında öznitelik adları geçerlidir ve karakter olduğu tüm Vue destekli tarayıcılar tarafından ayrıştırılarak (parse) doğru bir şekilde anlaşılır. Ayrıca render aşamasının ardından görünmezler. Elbette bu kullanım isteğe bağlıdır ancak kullanımı hakkında daha fazla bilgi edindiğinizde siz de bunu sıkça kullanacaksınız.

|Bu rehberin geriye kalan bölümlerinde Vue geliştiricileri tarafından sıklıkla kullanıldığından dolayı `v-bind` için kısa sözdizimini kullanacağız.|
|---|

## Boole (doğru/yanlış) attributes (öznitelikleri)
Boole öznitelikleri bir elementin üzerindeki kullanımı ile doğru orantılı olarak bunun yanlış(false)/true(doğru) değerlerini gösterebilen attribute'lerdir. Bunun ile ilgili en sık görülen örneklerden birisi `disabled` (devre dışı) özniteliğidir.  

`v-bind` böyle bir durumda biraz farklı çalışır:
```html
<button :devredisi="butonDevreDisimi">Buton</button>
```
`butonDevreDisimi` true (doğru) bir değere sahip ise `Buton` butonu özniteliği içeriğe [dâhil edilecektir](https://developer.mozilla.org/en-US/docs/Glossary/Truthy). Şayet bu boş bir dize ise false (yanlış) konumunda kalacaktır ve içerikte gösterilmeyecektir ki, bu durum `<button :devredisi="">` şeklindeki kullanım ile aynı anlama gelir. Ayrıca bu durumda attribute [çıkarılacaktır](https://developer.mozilla.org/en-US/docs/Glossary/Falsy). 

# Birden fazla özniteliği dinamik olarak bağlamak
Eğer aşağıdakine benzeyen türde birden çok attribute'ü temsil eden bir JavaScript objeniz/nesneniz varsa:
```javascript
const objeninAttrleri = {
  id: 'container',
  class: 'wrapper'
}
```
Başkaca ek bir argüman kullanmadan bunları tek bir elemente bağlayabilirsiniz/aktarabilirsiniz. Şu şekilde:
```html
<div v-bind="objeninAttrleri"></div>
```

# JavaScript İfadelerini (Expressions) Kullanmak
Rehberimizde şimdiye kadar anlattıklarımızda yalnızca basit özellik anahtarları (property key) ile bağlantıları kullandık. Ancak belirtmek isteriz ki Vue, tüm veri bağlamalarında JavaScript expressions (ifadelerinin) tüm gücünü kullanabilme yeteneğine sahiptir.

```javascript
{{ sayi + 1 }}

{{ ok ? 'EVET' : 'HAYIR' }}

{{ mesaj.split('').reverse().join('') }}

<div :id="`list-${id}`"></div>
```
Bu kullanımlardaki tüm ifadeler (expressions) geçerli komponent (bileşen) örneğinin verileri kapsamında JavaScript kullanımı olarak değerlendirilecektir. 

Vue template'lerinde (şablonlarında) JavaScript ifadeleri (expressions) aşağıda belirtilen pozisyonlarda/konumlarda kullanılabilir.
- Metne ekleme yapıldığında (çift süslü parantez ile)
- Herhangi bir `v-` özniteliği (attribute) ile başlayan Vue direktifi ile kullanımdaki veri değerlerinde

# Yalnızca ifadelerde (expressions)
Her bağlama (binding) yalnızca tek bir ifade (expression) ile çalışır. Yani aşağıdaki kullanımlar ÇALIŞMAYACAKTIR:
```javascript
<!-- bu bir statement'tır (açıklama), bir expression (ifade) değildir: -->
{{ var a = 1 }}

<!-- flow control (akış kontrolü) de çalışmaz bunun yerine ternary expressions (üçlü ifadeler) kullanın -->
{{ if (ok) { return mesaj } }}
```

# Arama/Çağırma (calling) Fonksiyonları

Bir bağlama ifadesinde (binding expression) komponente açık bir metot çağırmak mümkündür:
```html
<span :title="toTitleDate(date)">
  {{ formatDate(date) }}
</span>
```
|İPUCU: |
|---|
|Bağlama ifadeleri içinde çağrılan fonksiyonlar, komponent her güncellendiğinde çağrılır. Bu nedenle söz konusu fonksiyonların/metotların veri değiştirme veya asenkron işlemleri tetikleme gibi yan etkileri olmamalıdır.|

## Korumalı Alana Global Erişim
Template expressions (şablon ifadeleri) korumalı alandadır ve yalnızca [sınırlı globaller listesindekiler](https://github.com/vuejs/core/blob/main/packages/shared/src/globalsWhitelist.ts#L3) bu alana erişebilir. Bu liste `Math` ve `Date` gibi yaygın şekilde kullanılan globallerdir. 

Listede açık bir şekilde belirtimeyen globaller, örneğin `window` özelliği altında kullanıcıya bağlı olanlar template expression'dan (şablon ifadeleri) erişilebilir olmayacaktır. Ancak [`app.config.globalProperties`'e](https://vuejs.org/api/application.html#app-config-globalproperties) ekleyerek tüm Vue ifadeleri için ek globalleri açıkça tanımlayabilirsiniz.

# Direktifler
Direktifler `v-` ön etiketine sahip özel attribute'lerdir. Vue yukarıda tanıttığımız `v-html` ve `v-bind` dâhil olmak üzere bir dizi [yerleşik direktifi](https://vuejs.org/api/built-in-directives.html) size sağlar. Direktif attribue değerlerinin tek bir JavaScript ifadesi olması beklenir. Ancak ilgili `v-for`, `v-on` ve `v-slot` için bu durum geçerlidir ve bu rehberde söz konusu direktifler ayrıntılı olarak anlatılacaktır. Bir direktifin görevi, ifadesinin değeri değiştiğinde DOM'a reaktif olarak güncellemeleri uygulamaktır. Buna bir `v-if` örneği verirsek:
```html
<p v-if="gorulen">Şimdi beni görüyorsun</p>
```
Burada `v-if` direktifi gorulen ifadesinin doğruluğuna bağlı olarak <p> elementini kaldırır/ekler.
  
## Argümanlar
Bazı direktifler bir 'argüman' alabilir ki bu argümanlar direktiften sonra iki nokta üstüste (`:`) ile gösterilir. Örneğin bir HTML attribute'ünü reaktif olarak güncelleyebilmek için bir `v-bind` direktifi kullanırız:
```html
<a v-bind:href="url"> ... </a>

<!-- kısa gösterim -->
<a :href="url"> ... </a>
```
Burada `href` bir argümandır ve böylelikle `v-bind` direktifine elementin `href` attribute'ünün `url` ifadesinin (expression) değerine bağlaması gerektiğini anlatır. Kısa kullanımda argümandan önceki her şey (yani `v-bind`) kullanılarak `:` karakterine yoğunlaştırılır. 
  
Kullanacağımız başka bir örnek ise DOM olaylarını (event) dinleyen `v-on` direktifidir:
```html
<a v-on:click="birSeyYap"> ... </a>

<!-- kısa kullanım -->
<a @click="birSeyYap"> ... </a>
```
Burada argüman `click` ile dinlenecek event'ın adıdır. `v-on` için kısaltma karakteri `@` simgesi olup, kısaltması bulunan birkaç direktiften birisidir. Event handling (olay işleme) ile ilgili daha ayrıntılı bilgiler bu rehberde anlatılacaktır.  
  
## Dinamik argümanlar
Bir JavaScript ifadesini (expression) bir direktif argümanında köşeli parantez içine alarak kullanmak mümkündür:
```html
<!--
Argüman ifadelerinde (expression) aşağıdaki "Dinamik Argüman Değer Kısıtlamaları" ile "Dinamik Argüman Sinteks Kısıtlamaları"
bölümlerinde anlatıldığı üzere bazı kısıtlamalar olduğunu unutmayın,
-->
<a v-bind:[attributeAdi]="url"> ... </a>

<!-- kısa kullanım -->
<a :[attributeAdi]="url"> ... </a>
```
Burada `attributeAdi` dinamik olarak bir JavaScript  ifadesi olarak değerlendirilecektir ve bu son değeri olarak kullanılacaktır. Örneğin; Komponentiniz değeri `href` olan `attributeAdi` isimli bir bir data özelliğine sahipse bunu `v-bind:href` ile bağlamalı ve eşdeğer haline getirmelisiniz.   

