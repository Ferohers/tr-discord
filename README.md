Evet, ilk kez GitHub üzerinde Türkçe bir şey yazıyorum. Çünkü Türkiye ve Rusya haricinde ihtiyacı olan pek çıkmayacak.

Ne yapmaya çalışıyoruz?
Belirli bir programın (örneğin Discord) farklı bir yoldan ilerleyerek VPN üzerinden bağlanmasını sağlıyoruz. Bu sayede sadece belirlediğiniz uygulamaların trafiği VPN üzerinden geçerken, diğer internet trafiğiniz doğrudan bağlantınızı kullanır.

Neden normal bir VPN ile bağlanmıyoruz da bu yöntemle uğraşıyoruz?
Örneğin, Discord'a normal bir VPN ile erişim 70-80ms gecikme ile olurken, bu yöntemle 20-30ms gibi daha düşük gecikmelerle oyununuzu oynayabilirsiniz. Bu, özellikle düşük ping gerektiren uygulamalar için büyük bir avantaj sağlar.

Neye İhtiyaç Var?
Bir VPN'e ihtiyacınız olacak. Kendi VPN sunucunuzu kurmanız çok daha mantıklı ve güvenli olacaktır.

VPN Kurulumu: Kendi VPN sunucunuzu kurmak için bir repomuz mevcut. Temel server bilginiz varsa, birkaç komutla kendi VPN sunucunuzu hazır hale getirebilirsiniz.

Başlayalım!
1) VPN'iniz Hazır! Ama Komple Bağlantınız Oradan Geçiyor. Bunu Sonlandıralım.
VPN bağlantınızın tüm internet trafiğinizi yönlendirmesini engellemek için "Split Tunneling" özelliğini etkinleştirmemiz gerekiyor.

Bunun için PowerShell'i yönetici olarak açın ve aşağıdaki komutu tırnak işaretleriyle birlikte yazın:

```powershell
Set-VpnConnection -Name "VPN İSMİNİZ" -SplitTunneling $True
```

Örnek:

```powershell
Set-VpnConnection -Name "benim-vpn" -SplitTunneling $True
```

Bu adımı tamamladığınızda, VPN'inize bağlanacaksınız ancak hiçbir veri varsayılan olarak bu bağlantı üzerinden geçmeyecek.

Ek Bilgi: Bu adımın görsel bir anlatımı için Windows 11'de VPN Split Tunneling Kurulumu videosuna göz atabilirsiniz.

2) Geçmesi Gereken IP'leri Bulup Ekleme
Şimdi Discord gibi belirli uygulamaların VPN üzerinden geçmesini istediğiniz IP adreslerini bulup eklememiz gerekiyor.

Kaynak Monitörü'nü açın: Başlat menüsüne %windir%\system32\perfmon.exe /res yazıp Enter'a basarak Kaynak Monitörü'nü açın.

Ağ kısmına geçin: Kaynak Monitörü'nde "Ağ" sekmesine gidin.

Discord'u bulun: Burada Discord uygulamasını bulun. "Adres" kısmında bağlanmaya çalıştığı IP adreslerini göreceksiniz.

3) IP Adreslerini VPN'e Yönlendirme
Kaynak Monitörü'nde bulduğunuz IP adreslerini VPN bağlantınız üzerinden yönlendirmek için `netsh` komutunu kullanacağız. Her bir IP adresi için aşağıdaki komutu uygulayın:

```bash
netsh interface ipv4 add route x.x.x.0/24 "benim-vpn-ismim"
```

Sıra ile benim görüp bulduklarım:

```bash
netsh interface ipv4 add route 172.217.20.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 162.159.134.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 162.159.128.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 162.159.136.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 162.159.137.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 162.159.135.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 162.159.133.0/24 "benim-vpn-ismim"
netsh interface ipv4 add route 142.250.187.0/24 "benim-vpn-ismim"
```

Not: IPsec, IKEv2 vb. modern VPN protokollerinde bir sorun yaşamazsınız. Çok eski teknolojilerde küçük bir sıkıntı yaşama şansınız olabilir.

Peki Neden BypassDPI Benzeri Bir Şey Kullanmıyoruz?
Güvenlik Riskleri: Üçüncü şahısların tüm kodunu anlayamadığımız yazılımları kullanmak kendi içinde sakıncalıdır. Bu yazılımların kim tarafından yapıldığı ve/veya uluslararası adaptasyonunun olmaması daha korkutucudur.

Yasal Erişim: Halen erişimi kısıtlı noktalara yasal olmayacak şekilde ulaşmak bir sorundur. VPN, istediğiniz veriyi bu erişimin yasal olduğu noktadan alıp size taşımaktadır. Paylaşımsız bir VPN ile verileriniz tam güvenlidir.
