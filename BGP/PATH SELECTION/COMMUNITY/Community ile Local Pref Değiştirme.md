## Local Pref Artırma 
Bunun için ISP'nin bize vermiş olduğu 500:200 community'sini kullanacağız.


AS200 de yapılan işlem
````
ip prefix-list Pref_200 permit 192.168.10.0/24


route-map RM-ISP-Out permit 10
 match ip address prefix-list Pref_200
 set community 500:200

route-map RM-ISP-Out permit 20
 exit

router bgp 200
 neighbor 10.0.12.1  send-community
 neighbor 10.0.12.1 route-map RM-ISP-Out out
 do clear ip bgp 10.0.12.1 soft out
````

 ISPRouter1 de community olarak 500:200 görülmektedir.
<img width="661" height="213" alt="image" src="https://github.com/user-attachments/assets/06289f77-69fd-4cc9-80d6-1d898fb3d505" />
işlem sonrasında ISP tarafında artık 192.168.10.0/24networkü için local_pref 200 yapılmış oldu.
ISPRouter2 artık best rotayı ibgp tarafından alıyor. 
Burada 10.0.14.2 ebgp komşuluğu iken locPref 200 olduğundan ibgp komşuluğunu tercih ediyor. 
<img width="716" height="228" alt="image" src="https://github.com/user-attachments/assets/1523aaba-2131-432b-bf1c-eaa875efd532" />



## Local Pref Azaltma

Yeni bir network duyuralım ve ısp tarafında local pref i 50 olsun.

R2de 

````
ip route 192.168.20.0 255.255.255.0 null0 
router bgp 200
network 192.168.20.0 mask 255.255.255.0
````
community ekleme

````
ip prefix-list Pref_50 permit 192.169.20.0/24

route-map RM-ISP-Out permit 30
exit

route-map RM-ISP-Out  permit 20
match ip address prefix-list Pref_50
set community 500:201
exit

do clear ip bgp 10.0.12.1 soft out
````
Not: Önce networkü belirlemek için prefix list oluşturdum.Sonra route map'e permit 30 yapıp boş bir  sınıf açtım ki 20 de işlem yapacağım için anlık kesinti olmasın. çünkü 20 de match işlemini aktif edersem match olmayan diğer prefixler drop edilir.
<img width="641" height="325" alt="image" src="https://github.com/user-attachments/assets/bf525efa-7c92-4043-87aa-94bc70151454" />
<img width="649" height="120" alt="image" src="https://github.com/user-attachments/assets/ef762ef8-3465-4c6c-a812-e984348b9178" />

Yukarıdaki çıktılarda görüldüğü gibi hem community eklendi hem de local pref 50 oldu. ISPRouter1 192.168.20.0/24 networkü için best rotayı internal tarafından aldı.

ISPRouter2 üzerinden 192.168.20.0/24 networkü için baktığımızda, artık iki rota var birisi direkt R2 ile olan komşuluğu diğer R3 -> ISP2->R2 şeklinde giden rota. Farkettiğiniz üzere ibgp üzerinden öğrendiği rota gitti.

<img width="746" height="135" alt="image" src="https://github.com/user-attachments/assets/40374ce7-064c-4b74-a7a0-e30d2944962f" />

























