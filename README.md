# tr-discord

Evet ilk kez türkçe bir şey yazıyorum Github üzerinde. Çünkü Türkiye ve Rusya haricinde ihtiyacı olan adam pek çıkmayacak.

Ne yapmaya çalışıyoruz ?
Belirli bir programın farklı bir yoldan ilerleyerek vpn üzerinden bağlanmasını sağlıyoruz.

Peki neden normal bir VPN ile bağlanmıyoruz da senin bu zımbırtı ile uğraşıyoruz?
Ör: Discord'a erişim VPN'den 70-80ms ile olurken siz 20-30ms ile oyununuzu oynaybiliyorsunuz.

Neye ihtiyaç var ?
Bir VPN'e ihtiyacınız olacak. Kendinizin kurması çok daha mantıklı olur. 

Bir VPN kurulumu içinde repomuz var. Az çok server bilginiz var ise bir kaç komutla hazır hale getirebilirsiniz kendi vpn serverınızı.

Başlayalım. 

1)VPNimiz hazır! Ama komple bağlantınız oradan geçiyor. Bunu sonlandıralım.
https://www.youtube.com/watch?v=S5XCOS-il3c

veya powershelle (admin olarka) şunu yazın tırnaklar ile. Komut ile daha kolay

Type Set-VpnConnection -Name "VPN İSMİNİZ" -SplitTunneling $True
ör: Type Set-VpnConnection -Name "benim-vpn" -SplitTunneling $True

Evet şimdi bir VPN'niniz var bağlanıyorsunuz ama hiçbir data buradan geçmiyor. 

2) Geçmesi gereken IP'leri bulup ekleme
Resourse monitör (kaynak monitörü) açın. Bunu yazıp entera basarsanız açılır başlata.

%windir%\system32\perfmon.exe /res

Network (Ağ) kısmına geçin ve buradan Discord'u bulun. 
Adres kısmında bağlanmaya çalıştığı IP'leri göreceksiniz.


  
3) netsh interface ipv4 add route x.x.x.0/24 "benim-vpn-ismim"
Komutu ile Discord'un kaynak yönetiminde çıkan IP'lerini ekliyoruz.
   
Sıra ile benim görüp bulduklarım:
netsh interface ipv4 add route 172.217.20.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 162.159.134.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 162.159.128.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 162.159.136.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 162.159.137.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 162.159.135.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 162.159.133.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 142.250.187.0/24 "benim-vpn-ismim"

Not: IPsec, Ikev2 vb modern araçlarda bir sorun yaşamazsınız. Çok eski teknolojilerde bir sıkıntı yaşama şansınız azda olsa var.
Peki neden BypassDPI benzeri bir şey kullanmıyoruz? 
3. şahısların tüm kodunu anlayamadığımız yazılımları kullanmak kendi içinde sakıncalı.
Bu yazılımların kim tarafından yapıldığı ve/veya uluslararası adaptasyonunun olmaması daha korkutucu.
Halen erişimi kısıtlı noktalara yasal olmayacak şekilde ulaşmakta bir sorun. 
VPN istediğiniz veriyi bu erişimin yasal olduğu noktadan veriyi alıp size taşımaktadır. Paylaşımsız bir VPN ile verileriniz tam güvenlidir.



