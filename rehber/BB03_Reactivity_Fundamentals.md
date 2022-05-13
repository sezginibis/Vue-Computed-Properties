# Reaktivite Temelleri
|BİLGİ: |
|---|
|Vue'nun kendi rehberinin tüm bölümlerinde olduğu [bu bölümün eğitimlerini de](https://vuejs.org/guide/essentials/reactivity-fundamentals.html) Options API ya da Composition API olarak seçip okuyabilirsiniz. Fakat bu Türkçeleştirmede yalnızca Composition API kullanılmaktadır.|

# Reaktif Durumun (State) Bildirilmesi (Deklare Edilmesi)
`reactive()` fonksiyonu ile reaktif bir obje/nesne ya da dizi (array) oluşturabilirsiniz.
```javascript
import { reactive } from 'vue'

const durum = reactive({ sayi: 0 })
```
Reaktif objeler [JavaScript Proksi](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)'leridir ve normal objeler gibi davranırlar. Buradaki fark ise Vue'nun reaktif bir objenin özelliğine erişim ve mutasyonlarını (değişimlerini) izleyebilmesidir. İleride [Reaktivite'nin Derinlikleri](https://vuejs.org/guide/extras/reactivity-in-depth.html) bölümünde bu sistemin nasıl çalıştığı açıklanmaktadır fakat öncelikle bu bölümü okuyup bitirmeniz tavsiye edilir.  

Options API kullandığınız bir komponentin template bölümünde reaktif durum (state) kullanmak için `script` bölümünde `setup()` fonksiyonunu bildirmelisiniz:
```javascript
<script>
import { reactive } from 'vue'

export default {
  // `setup` composition API için ayrılmış özel bir kancadır (hook).
  setup() {
    const durum = reactive({ sayi: 0 })

    // durumu şablona çıkarmak için 
    return {
      durum
    }
  }
}
</script>
```
```html
<template>
<div>{{ durum.sayi }}</div>
</template>
```
Benzer şekilde aynı scope (kapsam) içinde durumu (state) değiştirebilen fonksiyonları da kullanabiliris ve böylece durumun (state) yanında bir metot olarak kullanabiliriz:
```javascript
<script>
import { reactive } from 'vue'

export default {
  setup() {
    const durum = reactive({ sayi: 0 })

    function artir() {
      durum.sayi++
    }

    // kullandığınız fonksiyonu da dışarıya çıkartmayı unutmayın
    return {
      durum,
      artir
    }
  }
}
</script>
```
Expose (açık) edilmiş metotlar genellikle olay dinleyicileri (event listeners) ile birlikte kullanılırlar. 
```html
<template>
<button @click="artir">
  {{ durum.sayi }}
</button>
</template>
```
# <script setup>
Durumu (state) ve metodları `setup()` aracılığı ile manuel olarak kullanmak biraz yorucu olabilir. Tek Dosya Komponentlerini (SFC) kullanırken `<script setup>` ile kullanırız ve böylelikle işlerimiz büyük ölçüde basitleşir:
```vue
<script setup>
import { reactive } from 'vue'

const durum = reactive({ sayi: 0 })

function artir() {
  durum.sayi++
}
</script>

<template>
  <button @click="artir">
    {{ durum.sayi }}
  </button>
</template>
```
`<script setup>` üst seviye içe aktarmalar ve değişkenlerin bildiriminde komponent içinde rahatlıkla kullanılmalıdır. 
  
|BİLGİ: |
|---|
|Rehberin geriye kalan bölümlerinde SFC + <script setup> sinteksi (sözdizimi) kullanılacaktır. Ayrıca istenildiği takdirde Vue rehberinde Options ve Composition API'ler arasında geçiş yapılarak anlatım bulunduğunu da bu bölümün başında belirtmiştik.|

## DOM Güncelleme Zamanlaması


  
