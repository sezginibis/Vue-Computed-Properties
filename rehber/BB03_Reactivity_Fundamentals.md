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

Bir komponentin template bölümünde reaktif durum (state) kullanmak için 
