# OSPF Network Types


## Broadcast (Multi-Acess)
Ethernet portarında birden fazla router aynı segmentteyse defaultta kullanılır.  


## Temel Özellikleri

- DR/BDR seçimi yapılır
- Neighbor keşfi otomatik yapılır
- Hello/dead süreleri 10/40 dır. Hello paketi 10 sn de bir gönderilir. 41. sn de hello paketi gelmez ise komşuluk düşer.
- Paketleri Multicast olarak gönderir. 
- Multicast ip adresleri 224.0.0.5 (tüm routerlara) /224.0.0.6 (sadece DR ve BDR'a)


<img width="498" height="452" alt="image" src="https://github.com/user-attachments/assets/3a21f7ab-8303-41ad-b0d0-d0c0a8d6733e" />





4 router da gigabitethernet portundan aynı segmente. Bu durumda OSPF varsayılan olarak broadcast tipinde DR/BDR seçimi yapar.

DR/BDR seçiminin sebebi, Tüm routerlar birbirilerine DBD,LS REQ, LS UPD, paketlerini göndermek yerine sadece DR/BDR gönderir. OPSF deki tüm routerlar rotaları DR'dan öğrenir.

DR/BDR olmayan routerlar, kendi aralarında DROTHER olarak etiketlenir ve state olarak da 2 WAY de kalır. Bu da iki router'ın birbirine sadece hello paketi gönderdiklerini gösterir.

DR seçilirken önce priorty'e bakılır, eşit ise router-id'si büyük olan DR olur. Bu labda R4 router-id'si büyük olan olduğundan DR seçilir.
R3 de BDR olarak seçilir.


<img width="793" height="142" alt="image" src="https://github.com/user-attachments/assets/b29cd266-d1c5-4e95-9597-94074e17f160" />




R1 ile R2 DR/BDR olamadıklarından DROTHER olurlar ve kendi aralarındaki ospf komşuluğu 2WAY de kalır. 

DROTHER routerlar DR/BDR routerlar ile FULL komşuluk kurarlar.

<img width="775" height="140" alt="image" src="https://github.com/user-attachments/assets/db5d67c2-b543-4e7a-813c-ca8dcbaf9f79" />



show ip ospf interface gigabitEthernet 0/0 ile network  type görebiliriz.

<img width="736" height="501" alt="image" src="https://github.com/user-attachments/assets/576c2978-30dc-47b2-9ffc-adf71821c6af" />



Eğer bir router'ın DR/BDR seçimine katılmasını istemiyor priorty'i 0 yapabilriz.

R3 routerın interface altında priorty 0 yaptık ve akabinde komşulukları resetledik.


<img width="393" height="125" alt="image" src="https://github.com/user-attachments/assets/5ee2fe69-1424-462d-847d-9bb0b0da6980" />


R3 artık DR/BDR seçimine katılmayacak ve DROTHER olarak kalacak.

<img width="767" height="133" alt="image" src="https://github.com/user-attachments/assets/f0318058-4b3a-40f5-bcb5-c1f3b3f722a3" />




## Point to Point (P2P)

İki router arasında serial veya gre tünel bağlantı varsa varsayılan olarak P2P kullanılır.


Temel Özellileri

DR/BDR seçimi  yapılmaz
Neighbor keşfi otomatik yapılır
Hello/dead süreleri 10/40 dır. Hello paketi 10 sn de bir gönderilir. 41. sn de hello paketi gelmez ise komşuluk düşer.
Paketleri Multicast olarak gönderir. Multicast ip adresi 224.0.0.5

<img width="473" height="169" alt="image" src="https://github.com/user-attachments/assets/66ad05ac-08bd-4edc-afa1-8b7ebec0b34f" />




Serial bağlantı varsayılan olarak P2P şeklindedir. Ve DR/BDR seçimi olmaz. P2P bağlantılarda zaten iki router olduğundan DR/BDR seçimine de gerek yoktur.

<img width="698" height="93" alt="image" src="https://github.com/user-attachments/assets/d9f78c93-ac3a-48f4-a51b-4edbb9885532" />



Show ip ospf interface serial 6/1 ile network type P2P olduığunu görürüz.

<img width="793" height="430" alt="image" src="https://github.com/user-attachments/assets/47171f3e-6286-4b7d-b7e1-5727bda06926" />


Eğer multi access ortamda bir router'ın network tpye'ını P2P yaparsak komşuluk kuramayacaktır. Kuramayacak olmasının sebebi, birden fazla routerdan gelen hello paketleri ile her seferinde tekrar tekrar komşuluk kurmaya çalışacaktır. Bu da sürekli komşuluklarını düşürmesine sebep olacaktır.


<img width="878" height="495" alt="image" src="https://github.com/user-attachments/assets/a474b280-7902-4054-b3a4-d0e557c07f68" />



Görüldüğü gibi R2 ile komşuluk kurmaya çalıştı sonra R1 ile tekrar komşuluk kurmaya çalıştı.

<img width="879" height="95" alt="image" src="https://github.com/user-attachments/assets/be281d9b-b8fd-4e09-a49e-69feff7ad297" />

<img width="878" height="104" alt="image" src="https://github.com/user-attachments/assets/1d4d270c-ade8-4fee-96aa-c43d62766477" />


## Point to Multipoint

DMVPN gibi tasarımlarda yaygın olarak kullanılır. Bu mimaride hub spoke yapısı olduğundan, ki tünel interfacelerde defaultta P2P olduğu için ospf kullıldığında spokeler kendi aralarında komşuluk kuramayacaktır. Bu yüzden network type point to multipoint olarak yapılandırıldığında her spoke bir biriyle komşu olacaktır.


Temel özellikleri 
DR/BDR seçimi  yapılmaz
Neighbor keşfi otomatik yapılır
Hello/dead süreleri 30/120 dır. Hello paketi 30 sn de bir gönderilir. 121. sn de hello paketi gelmez ise komşuluk düşer.
Paketleri Multicast olarak gönderir. Multicast ip adresi 224.0.0.5
 
*LAB TOPOLİSİ YAKINDA EKLENECEK*




## Loopback  

Loopback interfaceleri, router-id belirlemek için kullanılır.  Loopback interfacelerini opsf de duyurulduğunda interface subnet olarak ne verirseniz verin /32 olarak duyurulacaktır.


<img width="687" height="162" alt="image" src="https://github.com/user-attachments/assets/33586529-99ac-4d61-a216-4dbdb51be948" />



R1 de Loopback interface /30 olarak verilse bile ospf te duyurusu /32 olarak yapılır. 
<img width="668" height="55" alt="image" src="https://github.com/user-attachments/assets/bc731523-bf35-4040-839c-a885e2068287" />




Eğer interface de verildiği şekilde  duyurulmasını istiyor isek  network type'ını point to point olarak belirlemeliyiz.
<img width="458" height="27" alt="image" src="https://github.com/user-attachments/assets/f21b51de-6d06-49da-a69e-43e300ea8229" />

<img width="655" height="113" alt="image" src="https://github.com/user-attachments/assets/799b4986-f860-4099-9632-a8768e817f15" />



