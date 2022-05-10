Computed Properties (Hesaplanan Özellikler) Vue'nun verilerimizi dönüştürmemize veya hesaplamalar yapmamıza ve ardından sonucu template'imizde güncel bir variable (değişken) olarak kolayca yeniden kullanmamıza olanak tanıyan bir başka güçlü özelliğidir. Computed Properties çok kullanışlıdır ve complex in-template expressions (karmaşık template içi ifadeler) yerini almalıdır. Herhangi bir kod örneği içermeyen bu açıklama şu anda pek bir anlam ifade etmese de, hesaplanan özelliklerle ilgili [şu dersi](https://vueschool.io/lessons/vue-fundamentals-capi-computed-properties-in-vue-with-the-composition-api) izledikten sonra kesinlikle olacaktır.

İzlemeniz için yukarıda bağlantı verdiğimiz bu videonun açıklamasına gelir isek; Computed Properties için en iyi örneklerden birisi kullanıcıya input alanına yazarken kaç karakter daha yazabileceğini göstermektir. Öncelikle `ref` veya `reactive` gibi `computed` yardımcı fonksiyonunu sağlamalıyız. Ardından input ile birlikte computed fonksiyon yazmalıyız. Örnek şu şekildedir:

```javascript
<script setup>
import { ref, computed } from 'vue'
 
const yeniOge = ref('')
const karakterSayisi = computed(() => {
  return yeniOge.value.length
})
</script>

<template>
<input v-model.trim="yeniOge" type="text" placeholder="Bir metin ekleyin" /><br>
Metninizin uzunluğu {{ karakterSayisi }} karakterdir.
</template>
```

`computed` referans veriler ile her zaman senkronize bir şekilde çalışır.

Şimdi de başka bir örnek deneyelim. Computed ile öğelerimizi döndürelim yani listemize yeni eklenen öğeleri başa çıkaralım. Ancak computed properties lerin yalnızca template (sunum) katmanımızdaki verileri dönüştürmek için olduğunu hatırlayın. Yani mevcut verileri değiştirmemelidir. Bundan kaçınmak ve computed prop ta bir değeri değiştirmemek ya da mutasyona uğratmamak için spread operatörünü kullanabiliriz. Çünkü bu tür bir durum büyük verilerde hatalara neden olabilir. Bu computed lar ile çalışırken unutmamanız gereken önemli bir şeydir.
```javascript
<script setup>
import { ref, computed } from 'vue'

const yapilacaklar = ref([])

function yapilacaklaraEkle(e) {
    const value = e.target.value.trim()
    if (value) {
        yapilacaklar.value.push({
            id: yapilacaklar.value.length+1,
            baslik: value
        })
        e.target.value = ''
    }
}
  
const yapilacaklariTersCevir = computed(()=>{
  return [...yapilacaklar.value].reverse()
})

</script>
<template>
  	<div><input @keyup.enter="yapilacaklaraEkle"></div>
		<div v-for="yap in yapilacaklariTersCevir" :key="yap.id">{{ yap.id }} {{ yap.baslik }}</div>
</template>
```
# Basitçe

Aslında template içerisinde expressions (ifadeler) oldukça kullanışlıdır ancak bunlar basit işlemler içindir. Template içerisine çok fazla logic (mantık) koymanız, onları şişirir ve bakımını zorlaştırır. Örneğin şu şekilde nested (iç içe) kullanılan bir objemiz vardır:
```javascript
const yazar = reactive({
  yazarAdSoyad: 'Saniye Ünlü',
  yayinlanmisKitaplari: [
    'Vue 2 - Gelişmiş Rehber',
    'Vue 3 - Basit Rehber',
    'Vue 4 - Ne Olduğu Belli Değil'
  ]
})
```
Bu obje içinde yazarın yayınlanmış kitabı olup olmadığına bakmak istiyorsunuz.
```javascript
<p>Yayınlanmış kitapları:</p>
<span>{{ yazar.yayinlanmisKitaplari.length > 0 ? 'Var' : 'Yok' }}</span>
```
Bu noktada, template (şablon) biraz darmadağın oluyor. Bu şablonu anlayabilmek için nasıl bir mantık yürütüldüğünü anlamadan önce şablona biraz bakmak gerekiyor. Fakat daha da önemlisi, hesaplamayı şablona bir defadan daha fazla dâhil etmemiz gerekirse kendinizi tekrar etmek istemezsiniz.

Bu nedenle reaktif veriler içeren karmaşık mantık hesaplamalarında `computed property` kullanılması daha doğru bir yaklaşım olacaktır. Şimdi de bu örneğimizin yeniden düzenlenmiş hâline bir bakalım.
```javascript
<script setup>
import { reactive, computed } from 'vue'
const yazar = reactive({
  yazarAdSoyad: 'Saniye Ünlü',
  yayinlanmisKitaplari: [
    'Vue 2 - Gelişmiş Rehber',
    'Vue 3 - Basit Rehber',
    'Vue 4 - Ne Olduğu Belli Değil'
  ]
})

// computed ref
const yazarinYayinlanmisKitaplari = computed(()=>{
    return yazar.yayinlanmisKitaplari.length > 0 ? 'Var' : 'Yok'
})
</script>
<template>
<p>Yayınlanmış kitapları:</p>
<span>{{ yazarinYayinlanmisKitaplari }}</span>
</template>
```
`computed()` fonksiyonu bir getter function (alıcı/alım fonksiyondan) geçirilmeyi bekler ve fonksiyon neticesinde döndürülen (return) değer artık computed ref (hesaplanmış ref) olur. Ayrıca artık `computed` *değerine* `script setup` içinde başka bir fonksiyon veya işlevden `yazarinYayinlanmisKitaplari.value` şeklinde ulaşabilirsiniz. Template kısmında ise `{{yazarinYayinlanmisKitaplari}}` şeklinde .value olmadan kullanabilirsiniz.

Computed property reaktif bağımlılıkları otomatik olarak izler ve Vue artık `yazarinYayinlanmisKitaplari` hesaplamasının (mantığı) `yazar.yayinlanmisKitaplari` obje özelliğine bağlı olduğunun farkındadır. Artık `yazar.yayinlanmisKitaplari` değiştiğinde tüm güncellemeleri takip edecektir.

# Computed Ön Belleğe Alma ile Metodlar

Expression (ifadede/kullanımda) bir metod (yöntem) çağırarak da aynı sonucu elde edebileceğimizi fark etmiş olabilirsiniz.
```javascript
<p>{{ yazarinYayinlanmisKitaplari() }}</p>
```
```javascript
// in component (komponentte)
function yazarinYayinlanmisKitaplari() {
  return yazar.yayinlanmisKitaplari.length > 0 ? 'Var' : 'Yok'
}
```
Computed property yerine bunu bir fonksiyon olarak da tanımlayabiliriz. Sonuç olarak, iki yaklaşım gerçekten de tamamen aynıdır. Ancak buradaki fark, *computed property nin reaktif bağımlılıklara göre bunu önbelleğe almasıdır*. Computed property ile kullanımda, yalnızca bazı reaktif bağımlılıklar değiştiğinde durum yeniden değerlendirilecektir. Bu `yazar.yayinlanmisKitaplari` değişmediği sürece `yazarinYayinlanmisKitaplari` na erişimde getter function (alıcı/alım fonksiyon) gerek kalmadan sonucu döndürebileceği anlamına gelir.

Bu aynı zamanda aşağıdaki computed property nin asla güncellenmeyeceği anlamına gelir. Çünkü `Date.now()` reaktif bir bağımlılık değildir.
```javascript
const simdi = computed(() => Date.now())
```
Karşılaştırıldığında; bir metot çağırmada, yeniden oluşturma her gerçekleştiğinde fonksiyon çalışacaktır.

Peki neden önbelleğe alma ihtiyacımız doğuyor? Çok sayıda computed property ile oluşturulmuş bir `liste` olduğunu hayal edin. Böyle bir işlem yaptığınız anda, bir dizide çok fazla döngü oluşturmuş ve sonuç olarak çok sayıda hesaplama yapmış olacaksınız. Bu tür bir işlemde yine `liste`ye bağlı olan başka computed propertieslerimiz olabilir. Eğer önbelleğe alma olmasaydı, `liste`deki tüm getter işlemlerini gereğinden fazla sayıda yürütmüş olacaktık. İşte bu nedenlerden dolayı önbelleğe alınmasını istemediğiniz durumlarda bunun yerine bir `call metodu` (fonksiyon) kullanın.

# Yazılabilir Computed

Computed properties varsayılan olarak yalnızca getter dir (alıcıdır/alımdır). Computed property yeni bir değer atamaya çalışırsanız, bir `runtime` (çalışma zamanı) uyarısı alırsınız. `Yazılabilir` bir `computed property` ihtiyaç duyduğunuz nadir durumlarda, `getter` ve `setter` ayarlayarak hem alıcı (`getter`) hem de ayarlayıcı (`setter`) olmasını sağlayabilirsiniz.
```javascript
<script setup>
import { ref, computed } from 'vue'

const adi = ref('John')
const soyadi = ref('Doe')

const yazilabilirTamAdi = computed({
  // getter
  get() {
    return adi.value + ' ' + soyadi.value
  },
  // setter
  set(yeniDeger) {
    // Not: burada destructuring assignment syntax (yıkıcı atama sözdizimini) kullanıyoruz .
    [adi.value, soyadi.value] = yeniDeger.split(' ')
  }
})

const adiSoyadi = computed(()=>{
  return adi.value + ' ' + soyadi.value
})

const degistirilmisYazilabilirTamAdi = yazilabilirTamAdi.value = 'Bütün Yılmaz'
const degistirilmisAdiSoyadi = adiSoyadi.value = 'Bahadır Cinali'

</script>
<template>
<b>adi:</b> {{ adi }} <b>soyadi:</b> {{ soyadi }} (<small>Değişti ve buranın John Doe olması gerekiyordu.</small>)<br>
{{yazilabilirTamAdi}}<br>
{{degistirilmisYazilabilirTamAdi}}<br>
{{adiSoyadi}}<br>
{{degistirilmisAdiSoyadi}} (<small>Ancak bu kullanımla değişmedi</small>)<br>
</template>
```
Artık böylelikle `degistirilmisYazilabilirTamAdi.value='Bütün Yılmaz'` çalıştırdığınızda setter çağrılır ve adi ile soyadi buna göre güncellenir. Yani hem adiSoyadi değeri için hem de ayrı ayrı `adi` ve `soyadi` da kalıcı olarak artık `'Bütün Yılmaz'` olur.

# Pratik kullanım önerileri ve dikkat edilmesi gerekenler

## Getters side effect (Alımda/Alıcıda yan etki) olmamalıdır

Computed getter fonksiyonların yalnızca saf hesaplama yapması ve yan etkilerden (side effect) arınmış olması gerektiğini hatırlamak önemlidir. Örneğin; zaman uyumsuz (asenkron/async) requestlerde bulunmayın veya computed getter içinde DOM'u değiştirmeyin. Computed property'i yalnızca diğer değerlere dayalı başka yeni bir değerin nasıl türetileceğini deklaratif (bildirimsel) olarak açıklayan bir özellik olarak düşünün. Öyle ki onun tek sorumluluğu yani ona verebileceğiniz tek sorumluluk bu değeri hesaplamak size geriye döndürmek olmalıdır. Vue guide'ın ilerleyen bölümlerinde watchers (gözlemci/bekçi/bakıcı) ile state (durum) değişikliklerine reaksiyon olarak side effect (yan etkileri) nasıl gerçekleştirilebileceği üzerinde durulacaktır.

## Computed value (değerini) mutasyona uğratmaktan (değiştirmekten) kaçının

Computed property den döndürülen bir değer, türetilmiş bir state tir (durumdur). Bu nedenle bunu kafanızda anlık bir görüntü olarak hayal edin. Yani kaynağın durumu her değiştiğinde yeni bir anlık görüntü oluşturulur. Anlık bir görüntüyü değiştirmek çok da mantıklı olmayan bir şey. Bu nedenle computed return (dönüş) değeri salt okunur olarak ele alınmalı ve hiçbir zaman mutasyona (değişime) uğratılmamalıdır. Bunu yapmak yerine yeni oluşturulmasını istediğiniz hesaplamaları (computed sonuçları gibi) tetiklemek (veya değiştirmek) için computed'in bağlı olduğu kaynağın durumunu güncelleyin.

# Kaynakça
Bu sayfadaki tüm bilgiler daha basit anlayabilmem/anlayabilmeniz için bir miktar bilgi daha eklenerek [bu sayfadan](https://vuejs.org/guide/essentials/computed.html#computed-caching-vs-methods) çevrilmiştir. 
