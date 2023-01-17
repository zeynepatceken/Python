# Python
Global ai hub bootcamp projesi

Original file is located at
    https://colab.research.google.com/drive/1FQFkJ8l_vC07uB4hMa-tmMSuMWuTMlFH
"""
#Öğrenci Not Sistemi
from asyncio import wait_for
import pandas as pd
#Notlar durumu hesaplanır ve csv, excel dosyasına dönüştürülür.
def not_hesapla(satir):
   satir = satir[:-1]
   liste = satir.split(':')
   ogrenci_bilgiler = liste[0]
   not_durum = int(liste[1])
   ogrenci_ciktilari={'Ogrenci Bilgileri':[ogrenci_bilgiler],'Not Durumu':[not_durum]}
   pd.DataFrame(ogrenci_ciktilari).to_csv('sonuclar.csv',index=False)
   csv = pd.read_csv('sonuclar.csv')
   excelWriter=pd.ExcelWriter('Öğrenci_Bilgileri.xlsx')
   csv.to_excel(excelWriter,
                index_label='Öğrenci Sayısı',
                sheet_name='Öğrenci Bilgileri')
   excelWriter.save()
  
   if not_durum>=40 and not_durum<=100:
      durum = " Geçti"
   else:
      durum = " Kaldı"
    
   return ogrenci_bilgiler+ " :"+ durum + "\n"

#Notların okunması için yazılan fonksiyon  
def notlar():    
  with open("sinav_notları.txt","r",encoding="UTF8") as file:
    for satir in file:
        print(not_hesapla(satir))

#Notların kullanıcıdan alındığı ve .txt dosyasının oluşturulduğu fonksiyon bloğu. 
def notlar_gir():
  ad= input('Öğrencinin adı:')
  soyad= input('Öğrencinin soyadı:')
  numara= input('Öğrencinin numara:')
  not1= input('Öğrencinin not:')

  with open("sinav_notları.txt","a", encoding="UTF8") as file: 
   file.write(ad+' '+ soyad+' '+ numara+':'+not1+'\n')
            
#Notların kayıt edildiği fonksiyon.            
def notlar_kaydet():
  with open('sinav_notları.txt',"r",encoding="UTF8") as file:  
   liste= []
   for i in file:
        liste.append(not_hesapla(i))
   with open("sonuclar.txt","w",encoding="UTF8") as file2:
        for i in liste:
            file2.write(i)   
         
#Kullanıcı için oluşturulmuş menüdür. While True işlemi ile kullanıcının çıkış işlemine kadar sürekli karşısına menünün gelmesini sağlar.            
while True:
  islem=input('1- Notları Oku\n2- Not Gir\n3- Notları Kayıt Et\n4-Çıkış\n')
  if islem == '1':
    notlar()
  elif islem == '2':
   notlar_gir()
  elif islem == '3':
    notlar_kaydet()
  else:
    break
