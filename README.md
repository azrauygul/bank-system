# bank-system
class Musteri:
    def __init__(self, musteri_numarasi, ad, soyad):
        self.musteri_numarasi = musteri_numarasi
        self.ad = ad
        self.soyad = soyad
        
    def bilgileri_goster(self):
        print("\nMüşteri Bilgileri:")
        print(f"""
            Müşteri Numarası: {self.musteri_numarasi}
            Ad:{self.ad}
            Soyad: {self.soyad}
        """)
        
class Hesap:
    def __init__(self, hesap_numarasi, musteri, bakiye=0):
        self.hesap_numarasi = hesap_numarasi
        self.musteri = musteri
        self.bakiye = bakiye
        self.acik_hesap = True
        
    def para_yatirmak(self, miktar):
        if self.acik_hesap and miktar > 0:
            self.bakiye += miktar
            print(f"""
                Yatırdığınız Miktar: {miktar}
                Şuanki Hesap Bakiyeniz: {self.bakiye}
            """)
        elif not self.acik_hesap:
            print("Hesap Kapalıdır.")
        else:
            print("Girdiğiniz miktar yanlıştır. Lütfen geçerli bir miktar giriniz.")
    
    def para_cekmek(self, miktar):
        if self.acik_hesap and miktar > 0 and miktar <= self.bakiye:
            self.bakiye -= miktar
            print(f"""
                Çekmek İstediğiniz Miktar: {miktar}
                Şuanki Hesap Bakiyeniz: {self.bakiye}
            """)
        elif not self.acik_hesap:
            print("Hesap Kapalıdır.")
        elif miktar > self.bakiye:
            print("Girmiş olduğunuz miktar hesap bakiyenizden fazladır. Lütfen geçerli bir miktar giriniz.")
        else:
            print("Girdiğiniz miktar yanlıştır. Lütfen geçerli bir miktar giriniz.")
            
    def bakiye_sorgulama(self):
        if self.acik_hesap:
            print(f"Şuanki hesap bakiyeniz: {self.bakiye}")
        else:
            print("Hesap Kapalıdır.")
            
    def hesap_kapatmak(self):
        if self.acik_hesap:
            self.acik_hesap = False
            print("Hesap başarılı bir şekilde kapatıldı.")
        else:
            print("Hesap Zaten Kapalıdır.")
            
    def hesap_aktif_etmek(self):
        if not self.acik_hesap:
            self.acik_hesap = True
            print("Hesap başarılı bir şekilde aktif edildi.")
        else:
            print("Hesap Zaten Aktiftir.")
            
    def bilgileri_goster(self):
        print(f"\nHesap Bilgileri: ")
        print(f"""
            Hesap Numarası: {self.hesap_numarasi}
            Hesap Bakiyesi: {self.bakiye}
        """)
        self.musteri.bilgileri_goster()
        
class Banka:
    def __init__(self):
        self.musteriler = []
        self.hesaplar = []
        
    def hesap_olusturmak(self, musteri, ilk_bakiye=0):
        hesap_numarasi = len(self.hesaplar) + 100
        hesap = Hesap(hesap_numarasi, musteri, ilk_bakiye)
        self.hesaplar.append(hesap)
        print(f"{hesap_numarasi} numaralı hesap başarılı bir şekilde oluşturuldu.")
        return hesap
    
    def tum_hesaplar(self):
        print("\nTüm Hesaplar: ")
        for hesap in self.hesaplar:
            hesap.bilgileri_goster()
            
    def tum_musteriler(self):
        print("\nTüm Müşteriler: ")
        for musteri in self.musteriler:
            musteri.bilgileri_goster()
    
banka = Banka()

while True:
    print(20*"*", "Banka Hesap Yönetim Sistemi Menüsü", 20*"*")
    print("""
        1. Hesap Oluşturmak
        2. Tüm Hesaplar Listesi
        3. Tüm Müşteriler Listesi
        4. Para Yatırmak
        5. Para Çekmek
        6. Bakiye Sorgulama
        7. Hesap Kapatmak (Deactive)
        8. Hesap Aktif Etmek (Active)
        9. Çıkış (q)
    """)
    
    islem = input("Lütfen yapacağınız işlemin numarasını giriniz: ")
    
    if islem == "1":
        musteri_numarasi = input("Müşteri Numarası: ")
        ad = input("Ad: ")
        soyad = input("Soyad: ")
        musteri = Musteri(musteri_numarasi, ad, soyad)
        banka.musteriler.append(musteri)
        banka.hesap_olusturmak(musteri)
    
    elif islem == "2":
        banka.tum_hesaplar()
        
    elif islem == "3":
        banka.tum_musteriler()
    
    elif islem == "4":
        hesap_numarasi = int(input("Hesap Numarasını Giriniz: "))
        hesap = next((hesap for hesap in banka.hesaplar if hesap.hesap_numarasi == hesap_numarasi), None)
        if hesap:
            miktar = float(input("Yatıracağınız Miktarı Giriniz: "))
            hesap.para_yatirmak(miktar)
        else:
            print("Hesap Bulunamadı.")
    
    elif islem == "5":
        hesap_numarasi = int(input("Hesap Numarasını Giriniz: "))
        hesap = next((hesap for hesap in banka.hesaplar if hesap.hesap_numarasi == hesap_numarasi), None)
        if hesap:
            miktar = float(input("Çekmek İstediğiniz Miktarı Giriniz: "))
            hesap.para_cekmek(miktar)
        else:
            print("Hesap Bulunamadı.")
            
    elif islem == "6":
        hesap_numarasi = int(input("Hesap Numarasını Giriniz: "))
        hesap = next((hesap for hesap in banka.hesaplar if hesap.hesap_numarasi == hesap_numarasi), None)
        if hesap:
            hesap.bakiye_sorgulama()
        else:
            print("Hesap Bulunamadı.")
            
    elif islem == "7":
        hesap_numarasi = int(input("Hesap Numarasını Giriniz: "))
        hesap = next((hesap for hesap in banka.hesaplar if hesap.hesap_numarasi == hesap_numarasi), None)
        if hesap:
            hesap.hesap_kapatmak()
        else:
            print("Hesap Bulunamadı.")
            
    elif islem == "8":
        hesap_numarasi = int(input("Hesap Numarasını Giriniz: "))
        hesap = next((hesap for hesap in banka.hesaplar if hesap.hesap_numarasi == hesap_numarasi), None)
        if hesap:
            hesap.hesap_aktif_etmek()
        else:
            print("Hesap Bulunamadı.")
            
    elif islem == "9" or islem == "q" or islem == "Q":
        print("Uygulamadan Çıkış Yapıldı.")
        break
    
    else:
        print("Yanlış bir işlem seçimi yaptınız.")
