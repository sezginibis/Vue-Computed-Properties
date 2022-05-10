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
Elbette yine doğal olarak HTML ve CSS etiketlerinin kullanımı ile basit şekilde birçok şey yapabiliriz. Fakat burada karşımıza yeni bir şey çıkıyor. `v-html` attribute (öznitelik) direktif adını verdik ve bu şekilde kullanıldığını gösterdik. Sizce direktif adı verilen bu özniteliğin yalnızca başına `v-` eklediğimiz için mi bu gerçekleşti? Elbette Vue'nin özniteliğin başına `-v` koyması kendisini temsil eder ve bu türdeki tüm direktiflere bu harf ile tire eklenir fakat HTML'de böyle bir öznitelik de yok. Yani bir HTML etiketine (tag) `html`  özniteliğini yazarak da böyle bir sonuca ulaşamazdık. Sonuç olarak yani tahmin edebileceğiniz üzere oluşturulan bu öznitelik artık Vue'da direktif adını alır ve DOM'a özel reaktif bir davranışı da sergiler. Mantığınızında elverdiği ölçüde böyle bir kullanımda direktif adını verdiğimiz öetiket özniteliklerini de kendimizin tanımlaması gerektiğini fark etmişsinizdir. Fakat burada temel olarak anlatılmak istenen husus saf HTML etiket ve öznitelikleri gibi Vue'da biz de kendimize özel etiket ve öznitelikler oluşturabileceğimizdir. Reaktif kullanım ile birlikte biz bu `saf HTML` kodlarımıza artık bu elementin iç özelliklerini "güncel tut" diyebiliyoruz.   
