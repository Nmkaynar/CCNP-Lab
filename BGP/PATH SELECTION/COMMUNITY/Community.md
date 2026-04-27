BGP Community, route’ları etiketlemek (taglemek) için kullanılan bir attribute’tur.
Bu etiketler sayesinde router’lar belirli politikaları uygulayabilir.

AA:NN şeklinde prefixlere etiketlenir ve gönderilen komşunun bu etikete göre aksiyon almasını sağlanır. 

Komşu AS'da bu community'ler önceden belirlenmiştir. Ve bizde komşu AS'ı belirlediği community'lere göre etiketleme yaparız.

cisco da eski IOS'larda AA:NN algılaması için ``ip bgp-community new-format`` komutu aktif edilmeli.

Community direkt path selection tetikleyicisi değildir ancak attributeları tetikleyerek path selection yapar.
Burada standart community'ler incelenecek.

``Additive`` ile daha önce eklenenen communityleri silmeden yenisini ekler.

### Well-Know Communities  

|no-export | AS dışına duyurulmaz|
|:-------- |:------------|
|no-advertise | Kimseye anons etme|
|local-AS  | Sub-AS dışına çıkmaz.|
|blackhole|Trafik drop edilir.|


## ip bgp-community new-format
Eski IOS'larda defaultta AA:NN şeklinde gözükmüyor.
<img width="660" height="237" alt="image" src="https://github.com/user-attachments/assets/9f74991b-cab6-4e77-9b81-070c192f10fe" />
Community: 32768100 şeklinde görüldüğü için ``ip bgp-community new-format`` komutunu aktif etmeliyiz.

Komutu aktif ettikten sonra istediğimiz formata geldi.
<img width="639" height="234" alt="image" src="https://github.com/user-attachments/assets/0564298c-2109-4e91-a7d6-89a9391e3814" />
