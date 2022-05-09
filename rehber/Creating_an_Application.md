# Uygulama (App) Örneği

Her Vue uygulaması, `createApp` işleviyle yeni bir uygulama örneği oluşturarak başlar:

```javascript
import { createApp } from 'vue'

const app = createApp({
  /* kök bileşen seçenekleri */
})
```

# Kök Komponent (Root Component)
`createApp`'e aktardığımız obje aslında bir komponenttir (bileşen). Her app (uygulama), children (çocukları) olarak diğer alt komponentlerini de içerebilen bir "root component" (kök bileşen) gerektirir. Böylece `app` bizim kök bileşenimiz olmuş olur.

Single-File Components kullanıyorsanız, genellikle `root component`'i başka bir dosyadan içeriye aktarırız.
```javascript
import { createApp } from 'vue'
// root component App'ini tek dosyalı bir komponentten içeriye aktarırız.
import App from './App.vue'

const app = createApp(App)
```
Şunu da dikkate almanınızı öneririz. Bu rehberdeki birçok örnek yalnızca tek bir komponente ihtiyaç duyarken, çoğu gerçek uygulama iç içe geçmiş (nested), yeniden kullanılabilir komponetlerden oluşan bir ağaç yapısında düzenlenmektedir. Örneğin, bir Todo uygulamasının komponent ağaç yapısı aşağıdaki şekilde görünebilir: 
```
App (root component)
├─ TodoList
│  └─ TodoItem
│     ├─ TodoDeleteButton
│     └─ TodoEditButton
└─ TodoFooter
   ├─ TodoClearButton
   └─ TodoStatistics
```
Rehberimizin sonraki bölümlerinde birden çok komponentin nasıl tanımlanacağına ve oluşturulacağına bakacağız. Ancak bunu yapmadan önce, tek bir komponentin içinde neler olduğuna odaklanacağız.

# App Mount (Uygulamayı Eklemek)

Oluşturduğumuz bir `App` (uygulama örneği) ona `.mount()` metodu ekleleyerek, çağrılıncaya kadan tek başına hiçbir şey oluşturmaz. "Container" argümanı olacak güncel bir DOM elementi (öğesi) veya selector string (seçiçi dize) bekler.  

```html
<div id="app"></div>
```
```javascript
app.mount('#app')
```
Böylelikle App'in root komponentinin içeriği artık onu kapsayan (container) elementin içinde işlenebilecektir. Bununla birlikte container elementin kendisi, App'in bir parçası olarak kabul görmez. 

`.mount()` metodu her zaman tüm app yapılandırmaları ve varlıkların kayıtları yapıldıktan sonra çağrılmalıdır. Ayrıca return (dönüş) değerinin varlık kayıt yöntemlerinden (içerek ekleme) farklı olarak App'in bir örneği değil root komponentinin (kök bileşenin) örneği olduğu da unutulmamalıdır. 

# DOM'daki Root Component Template (Kök Komponent Şablonu)

Vue kodlarımızı build (derleme) adımlarını gerçekleştirmeden önce de root komponentimizin içinde doğrudan doğruya kullanabiliriz.
```html
<div id="app">
  <button @click="say++">{{ say }}</button>
</div>
```
```javascript
import { createApp } from 'vue'

const app = createApp({
  data() {
    return {
      say: 0
    }
  }
})

app.mount('#app')
```
Root komponentimizin zaten bir `template` seçeneği olmadığından dolayı Vue otomatik olarak container'ın (konteyner/kapsayıcı) innerHTML'sini template olarak kullanacaktır.   

# App Konfigürasyonları
App instance (Uygulama örneği) app seviyesinde birkaç yapılandırma ayarı yapabilmemizi sağlayar `.config` objesini (nesnesi) de bize sunar. Örneğin; tüm alt komponentlerden gelecek/gelen hataları app düzeyinde yakalayabilen bir hata işleyicisi (error handler) tanımlayabiliriz:
```javascript
app.config.errorHandler = (hata) => {
  /* handle error (hata işleyici) main.js içinde kullanılır */
}
```
App instance yani uygulama örneği, app-scoped (uygulama kapsamındaki) varlıkları kaydedebilmek için de birkaç metot (yöntem) sağlar. Buna ise örnek olarak bir komponentin kaydedilmesini verebiliriz: 
```javascript
app.component('TodoDeleteButton', TodoDeleteButton)
```
Bu kullanımımızın ardından artık `TodoDeleteButton` komponenti artık App'imizin herhangi bir yerinde kullanılabilir hâle gelecektir. Tabi tüm bunlar rehberin ilerleyen bölümlerinde detaylı şekilde ele alınacaktır. Bununla birlikte dilerseniz tüm App örneklerinin listesine Vue'nun [API Referansları listesinden](https://vuejs.org/api/application.html) de bakabilirsiniz.

App'inizi mount etmeden yani yüklemeden önce tüm app konfigürasyonlarınızın uygulandığından emin olunuz!

# Çoklu app örnekleri

Bir app instance'ını tek bir uygulamaya sığdırmak istemeyebilirsiniz. `createApp` API'si birden fazla Vue app'inin aynı sayfada birlikte kullanılabilmesine izin verir. Yani bunların her birisi genel kapsamlı varlıklara ve yapılandırmalarına ayrı ayrı sahip olabilir.  
```javascript
const app1 = createApp({
  /* ... */
})
app1.mount('#container-1')

const app2 = createApp({
  /* ... */
})
app2.mount('#container-2')
```
Server tarafından render edilen HTML'i Vue ile geliştirmek ya da yalnızca yine Vue'nin büyük bir sayfanın belirli bölümlerinin kontrol edebilmesi için Vue'ya ihtiyacınız varsa tüm sayfaya tek Vue app instance'ını mount etmekten çekinmeyin. Burada anlatılmak istenen husus, belirtilen bu tür kullanımlarda bile birden fazla app instance'ı oluşturabileceğiniz ve bunların kontrolünü gerçekleştiricek elementlere ayrı ayrı bağlayarak kullanabileceğinizin de hatırlatımasına yöneliktir.  

# Kaynakça
Bu sayfadaki tüm bilgiler daha basit anlayabilmem/anlayabilmeniz için bir miktar bilgi daha eklenerek [bu sayfadan](https://vuejs.org/guide/essentials/application.html#multiple-application-instances) çevrilmiştir. 
