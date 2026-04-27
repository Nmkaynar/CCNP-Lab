## Community ile MED Değiştirme

Bunun için ISP1'in vermiş olduğu 500:400 communitysini kullanacağız.

R2 de yapılacak olan işlemler.


````
ip prefix-list Change-MED permit 192.168.10.0/24

route-map RM-ISP-Out permit 10 
 match ip address prefix-list Change-MED
 set community 500:400

route-map RM-ISP-Out permit 20
exit

router bgp 200
 neighbor 10.0.12.1 send-community
 neighbor 10.0.12.1 route-map RM-ISP-Out out
 do clear ip bgp 10.0.12.1 soft out
````


Community ile İşlem yapmadan önce ki hali. Metric default olarak 0 dır.

<img width="651" height="400" alt="image" src="https://github.com/user-attachments/assets/fbb3d84e-5b3c-42b9-b48d-a84e4c3e2e0a" />




İşlem yapıldıktan sonra ki hali 
ISPRouter1 düşük metric değerine sahip ibgp komşuluğunu seçti. 192.168.10.0/24 networküne ISP1 den gelen trafik gateway olarak ISPRouter2'yi tercih edecek.. Tek bağlantı üzerinden trafik AS200 gelecek.


<img width="649" height="437" alt="image" src="https://github.com/user-attachments/assets/b59222bb-6e9d-4729-ad21-35dcceaf2416" />

