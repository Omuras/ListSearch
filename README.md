package odev;

import com.sun.org.apache.bcel.internal.generic.GOTO;
import static com.sun.org.apache.xpath.internal.axes.HasPositionalPredChecker.check;
import static java.awt.SystemColor.text;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;
import java.io.Reader;
import static java.lang.Math.pow;
import java.nio.charset.Charset;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Locale;
import java.util.Scanner;
import java.util.logging.Level;
import java.util.logging.Logger;
import static jdk.nashorn.internal.runtime.regexp.joni.encoding.CharacterType.ASCII;
import static sun.nio.ch.IOStatus.check;


public class Odev {
    
    
    
    public static String[] readText(String file){
        //Dizi olustur ve elemanlari ona aktar
        String[] Words = new String[100];
        try{
            Scanner scan =  new Scanner (new File(file));  //Dosya tarayici
            String Temp; 
                           //Ilk dizimiz
            for(int k=0; k<100;k++ )
            {
                
                Temp=scan.next();
                //Aktardıgımız kelimeleri kucuk harf yapmak icin
                Words[k]=Temp.toLowerCase(Locale.ENGLISH);
                
            }
            
        }
        catch(FileNotFoundException e){
            System.exit(1);
        
        }
        return Words;
  
    }
    
    public static int toASCII(String str) {           //String to ASCII

        char[] ch=str.toCharArray();            //String to Char array harfleri tek tek almak için
        int ascii=0;
        int last=0;
        for(int k =0; k<ch.length; k++){        
            ascii = ch[k];
            last+=ascii*Math.pow(k+1, 4);
        }    
        return last;
}
     public static int toASCII(char[] str) {           //String to ASCII 2.method       pow(k+1,4)
            
        int ascii=0;
        int last=0;
        for(int k =0; k<str.length; k++){        
            ascii = str[k];
            last+=ascii*Math.pow(k+1, 4);
        }    
        return last;
}
     
    public static void Nbul(int words,String GKelime){              //Arama fonksiyonu bunun gibi 2 tane daha fonk olcak 
        int Ascii=toASCII(GKelime);                             //Dogru calisiyo
        int net=0;

            if(words==Ascii)
                net++;

        if(net==1){
           System.out.println(GKelime);
        }
    }
    
    public static void CikarAra(int words,String GKelime){              //Harf cikararak arama            Sikinti var
                                                                            //Fonksiyonlar bastan yazilacak
            String bulunan = new String();
            int k=0;
            int Ascii;
            char[] temp = null;
            for(int s=0;s<GKelime.length();s++){
                    char[] Array=GKelime.toCharArray();
                    
                    for (int i = s+1; i <= GKelime.length(); i++) 
                    {
                        if(i==GKelime.length())
                        {
                            temp = Arrays.copyOfRange(Array, 0, i-1);           //javada charda null atayamadım ondan yeni diziye 
                        }                                                       //kopyalıyorum 1 eksik olarak yani Dizi[5] ise
                        else                                                    //Dizi[4] kopyalıyorum bu sayede son harf gidiyor
                        {
                            Array[i-1]=Array[i];
                        }

                    }
                    
                    if(temp!=null)
                    {
                        Ascii=toASCII(temp);
                    }
                    else{
                    Ascii=toASCII(Array);
                    }
                   if(words==Ascii){
                        for(int sayac=0;sayac<Array.length-1;sayac++){
                            System.out.print(Array[sayac]);
                        }
                        System.out.println();
                        }       
                    }

    }
        
        
    public static void DegistirAra(int words,String GKelime){              //Harflerin yerni degistirerek arama
            String bulunan = new String();
              char temp;

                for(int s=1;s<GKelime.length();s++){
                    char[] Array=new char[GKelime.length()];
                    
                    Array=GKelime.toCharArray();
                    
                                                                   // Char array yapıp kelimenin harflerinin yeri değişiyor
                    
                    temp=Array[s-1];
                    Array[s-1]=Array[s];
                    Array[s]=temp;    

                    int Ascii=toASCII(Array);
                    
                    if(words==Ascii){
                        for(int sayac=0;sayac<Array.length;sayac++){
                            System.out.print(Array[sayac]);
                        }
                        System.out.println();
                        }       
                    }

    
    }    
    
    
    public static void main(String[] args) throws IOException {

        int[] SonListe = new int[211];
        //Dosyadan okuma methodu
        String[] words = readText("Words.txt");
        int Sira=0;
        int Hashtemp=0;
        Scanner sin =  new Scanner(System.in);             //giris
        boolean ex=true;
        boolean again=true;
        String enter = null;                   //Giris degerlerin
        String sorgula;
                                                //Cikis degiskenleri
        int varyok = 0;
        
        
        
        
        // listeye sıralanıp koyuluyor
        for (String word : words) {           
            int temp=1;
            Hashtemp = toASCII(word);
            Sira=Hashtemp%211;
            while(SonListe[Sira]!=0){
                Sira+=temp^2;
                temp++;  
                if(Sira>=211)
                    Sira=0;
            }
            SonListe[Sira]=Hashtemp;
        }
        

        while(ex==true){
            again=true;
            System.out.println("Hangi kelimeyi aramak isterdiniz ?");
            enter=sin.nextLine().toLowerCase();

            System.out.println(" ");
            System.out.println("Bulunan kelimeler : ");
            
            //Aramalar yapılıyor
                for(int Sayac=0;Sayac<SonListe.length;Sayac++){
                    if(SonListe[Sayac]!=0)
                        Nbul(SonListe[Sayac],enter);
                }
            
            for(int Sayac=0;Sayac<SonListe.length;Sayac++){        
                if(SonListe[Sayac]!=0)
                    CikarAra(SonListe[Sayac], enter);
            }
                
            for(int Sayac=0;Sayac<SonListe.length;Sayac++){        
                if(SonListe[Sayac]!=0){
                    DegistirAra(SonListe[Sayac], enter);
            }}
 

            //Switch  case tekrar arama sorgulaması
                   
            System.out.println(" ");
            System.out.println("Tekrar arama yapmak ister misiniz ? (E/H)");
            while(again){
            again=false;
            sorgula=sin.nextLine();
            
            switch(sorgula){
                case "E":
                    break;
                case "e":
                    break;
                case "h":
                    ex=false;
                    break;
                case "H":
                    ex=false;
                    break;
                default:
                {
                    System.out.println("Lütfen tekrar deneyiniz.Tekrar kelime aramak için E cikmak icin H tuşlayınız.");
                    again=true;
                }
                             }
            
            }
            
        }
    }

    }
  
