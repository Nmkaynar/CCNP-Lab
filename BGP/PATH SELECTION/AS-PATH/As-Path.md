## AS-PATH

- Bir prefixin geçtiği AS(Autonomous System) listesidir.
- Best Path selectionda 4.adımda bakılan bir attribute'dur.
- BGP’de loop prevention sağlar (kendi AS’ini görürse route’u reddeder).
- Kısa olan AS-PATH tercih edilir.
- eBGP’de her hop’ta AS numarası eklenir, iBGP’de değiştirilmez (preserve edilir).Ne geldiyse o gider
- Incoming trafiği etkilemek için kullanılır.
- Genellikle AS-PATH prepend ile yol uzatılarak daha az tercih edilmesi sağlanır.
- Regex ile filtering yapılabilir (policy control).
- Vendor bağımsızdır.
- En sağdan sola doğru sırayla eklenir. En sağdaki AS networkün duyurulduğu AS no dur.



<img width="1211" height="543" alt="image" src="https://github.com/user-attachments/assets/4712ee8f-2963-43e3-9320-c62d2382d936" />







## As-Path Prepending

R5 routerından bakıldığında R1 arkasında ki networke erişmek için R2 rotasını kullanmaktadır. 
<img width="722" height="160" alt="image" src="https://github.com/user-attachments/assets/ec07cbc7-c8ae-4d10-a1ac-2eae85af8386" />



R1'in Lan tarafına gelen trafik R2 tarafından gelmesini istimiyor isek, R1 den duyurduğumuz prefixlere daha fazla As ekleyerek sanki yol uzunmuş gibi algılanmasını sağlamalıyız. 

Aş.daki işlemi R1 de ve out yönünde yapmalıyız. Çünkü networkü duyuran R1 Router'ı. Ve update paketleri outbound yönünde olduğundan.
````
route-map RM-PREPEND permit 10
 set as-path prepend 65100 65100 

router bgp 65100
 neighbor 10.0.10.2 route-map RM-PREPEND out
 do clear ip bgp 10.0.10.2 soft out

````

Artık 172.16.10.0/24 ve 172.16.20.0/24 networkünden 192.168.10.0/24 networküne gelen trafik R2 üzeinden gelmeyecek. R5 routerından artık best path R3 router'ı olarak gözükmektedir. R5 Router As-Pathi daha az olan R3'ü seçti.
<img width="815" height="168" alt="image" src="https://github.com/user-attachments/assets/1cf271e2-8380-4c70-ab81-c3ecfb915a12" />



## AS-Path Filtering

R1 65300 AS-No dan gelen rotaları reddetsin. 

Aş.daki işlemi R1'de uygulamalıyız.

````
ip as-path access-list 10 deny _65300_
ip as-path access-list  10 permit  .*

router bgp 65100
 neighbor 10.0.11.2 filter-list 10 in
 do clear ip bgp 10.0.11.2 soft in

````

Şu an da 65300 AS-path içeren rotalar mevcut
<img width="720" height="197" alt="image" src="https://github.com/user-attachments/assets/6e67d754-5ed1-443b-bffe-12c43363759a" />


Config aktif edildikten sonra R3 ten gelen rotalar engellendi.

<img width="728" height="157" alt="image" src="https://github.com/user-attachments/assets/e26f43ab-8ed2-484c-94ce-b820373f211a" />




## REGEX
````
^$                  Router'ın Kendi ürettiği Rotalar.
^65100_             AS 65100 den direkt gelen rotalar
_65100$             Origin AS'ı 65100 olan rotalar
_65100_             As_Path içinde 65100 geçen rotalar
.*                  Tüm Rotalar
````
