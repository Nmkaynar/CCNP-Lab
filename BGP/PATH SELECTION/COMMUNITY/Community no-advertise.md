## No-Advertise

Bir rotaya NO_ADVERTISE eklerseniz, o rotayı hiçbir BGP komşunuza anons etmezsiniz. 
Sadece kendi router'ınızın BGP tablosunda kalır. 
iBGP komşularına bile gönderilmez (bu yönüyle NO_EXPORT'tan daha kısıtlayıcıdır).


ISPRouter1 R2 den öğrendiği 192.168.10.0/24 prefixini hem AS100(R1) hem de ibgp ile ISPRouter2'ye duyurmaktadır.

<img width="640" height="105" alt="image" src="https://github.com/user-attachments/assets/db1c098b-dbcc-423e-a6a3-5699121780d0" />
<img width="672" height="104" alt="image" src="https://github.com/user-attachments/assets/baba6bd4-3086-4651-9847-ba17c1abd8d6" />

R2 de no-advertise community ile etiketledikten sonra sonuçlara bakalım.

````
ip prefix-list no-advertise permit 192.168.10.0/24

route-map RM-ISP-Out permit 10
  match ip address no-advirtise
  match ip address no-advertise
  set community no-advertise
  exit

route-map RM-ISP-Out permit 20
  exit
do clear ip bgp 10.0.12.1 soft out

````

Config aktif edildikten sonra ISPRouter2 sadece R2 den öğrenmektedir.
<img width="639" height="73" alt="image" src="https://github.com/user-attachments/assets/49fd3dd9-e3d2-4d40-ad2a-ed32bfc0d8af" />
ISPRouter1 aynı şekilde AS100'e(R1) de advertise etmedi. Bu sebepten ötürü R1 de sadece AS600 den bu rotayı öğrendi.
<img width="660" height="78" alt="image" src="https://github.com/user-attachments/assets/62248fd5-2ddc-407d-9d54-f0ae9ef4d82b" />

