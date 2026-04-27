## No-Export

No-Export community'si prefixleri komşusuna duyurur ancak komşu AS, başka AS'lere duyurmaz.


AS100 (R1) ve AS300 (R3 ) 192.168.20.0/24 prefixini hem ISP1 hem  de ISP2 den görmektedir

<img width="680" height="89" alt="image" src="https://github.com/user-attachments/assets/10214863-e952-4faf-80ff-97a0f4b0d212" />

<img width="702" height="100" alt="image" src="https://github.com/user-attachments/assets/ed8a9f76-32d0-4b2a-ab99-a9ec6ac257af" />



Herhangi bir community taglenmeden gönderiliyor.

<img width="609" height="94" alt="image" src="https://github.com/user-attachments/assets/0d7a3bee-9dd1-49d7-8a3c-10e5489b40db" />

AS200 de bulunan 192.168.10.0/24 prefixini ISP2'nin diğer AS'lere duyurmasını istemiyoruz.

Bunun için R2 de yapılması gereken config


````
ip prefix-list no-export permit 192.168.10.0/24,

route-map RM-ISP2-Out permit 10
set community no-export
exit
route-map RM-ISP2-Out permit 20
exit

router bgp 200
 neighbor 172.16.20.1 send community
 neighbor 172.16.20.1 route-map RM-ISP2-Out out
do clear ip bgp 172.16.20.1 soft out
````

192.168.10.0/24 prefixi ISP2'ye no-export communtysi ile geldi.  Ve diğer komşu AS'lere duyurmadı

<img width="627" height="125" alt="image" src="https://github.com/user-attachments/assets/bee20174-8d59-4d14-b5a7-8a05e647c7ae" />


AS100 (R1) ve AS300 (R3 ) 192.168.20.0/24 prefixini sadece AS500'den (ISP1) öğrendiler.


<img width="735" height="84" alt="image" src="https://github.com/user-attachments/assets/1d4f7781-e1dc-4800-ba01-359f5655daca" />

<img width="656" height="89" alt="image" src="https://github.com/user-attachments/assets/e77eb001-3ab2-4693-afdd-642620839ff9" />


Wiresharkta Update paketi içerisindeki No-Export Communitysi


<img width="720" height="366" alt="image" src="https://github.com/user-attachments/assets/6eb77537-2c4b-4e13-b74d-83af3ed6a495" />

