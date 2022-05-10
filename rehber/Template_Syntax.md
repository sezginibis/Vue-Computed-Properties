Vue render'lanan (oluşturulan) temel komponent örneğinin (instance) verilerine DOM'u deklaratif (bildirimsel) olarak binding yapmanıza (bağlamanıza) izin veren HTML tabanlı bir template (şablon) sinteksi (sözdizimi) kullanır. Tüm Vue template'leri HTML ve uyumlu tarayıcılar tarafından pars edilebilen (ayrıştıran) geçerli sintekslerdir (sözdizimleridir). 

Vue kendi yapısı içinde template'leri, yüksek seviyede optimize edilmiş JavaScript kodunda kompile eder (derler). Reaktivite sistemi ile birlikte Vue, App (uygulama) durumu her değiştiğinde çok düşük miktarda DOM manipülasyonu ile onu yeniden oluşturur, app içinde yapılan değişikliği gerçekleştirebileceği en az değişiklikle gerçekleştirerek sunar. 

Eğer Virtual DOM (Sanal Belge Ojbe veya Nesne Modeli) kavramlarına bir aşinalığınız varsa yani JavaScript'i saf olarak kullanmayı tercih ediyorsanız, template'ler kullanmak yerine oluşturma fonksiyonlarını kendiniz de yazabilirsiniz. Ayrıca isteğe bağlı olarak JSX desteğini de kullanabilirsiniz. Fakat burada compile-time (derleme zamanı) optimizasyonları yaparsanız bunun hoş bir tepki vermediğini de unutmayın. 

# Metin Enterpolasyonu (ekleme yapılması)

Bir veriyi template içinde kullanmanın en temel biçimi "Bıyık" (çift süslü parantez) sinteksi (sözdizimi) adı verilen ekleme ile yapılmasıdır: 

```html
<span> Mesaj : {{ msj }} </span>
```
Parantez içinde verilen `msj` etiketi, script içinde kendisine verilmiş `msj` değişkeninin özelliği (değeri) ile değiştirilecek ve burada sunulacaktır. Ayrıca daha sonra öğreneceğiniz üzere, bu değişkeni reaktif olarak tanımladıysanız artık `msj` değeri, değişken her değiştiğinde güncellenecektir.

# Raw (Saf) HTML
Çift süslü parantez, verileri HTML olarak değil düz metin olarak yorumlar ve gösterir. Eğer sonuç olarak bir HTML çıktısı almak istiyorsanız `v-html` direktifini (yönergesini) kullanmanız gerekir:

```html
<p>Text (Metin) eklemesi yapılması (enterpolasyon): {{ safHtml }}</p> <!-- Bu şekilde saf HTML 
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```
