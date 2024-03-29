Builder (İnşaatçı) tasarım deseni creational grubununa ait, biden fazla parçadan oluşan nesnelerin üretilmesinden sorumlu bir tasarım desenidir. dofactory.com a göre kullanım oranı 40% larda olan builder tasarım deseni yapı olarak abstract factory desenine benzer.

Bazı nesneler birden fazla nesnenin birleşmesinden(bazı işlemleri yapması sonucu) oluşabilir. Zamanla bu ana nesneyi oluşturan nesnelerin yapısı değişebilir, bu nesnelerin oluşturulması karışık bir hal alabilir veya bu nesnelere başka nesneler de eklenebilir. Builder tasarım deseni bu gibi durumlarda genişletilebilirliği sağlamak ve kod karmaşıklığını engellemek için kullanılır. Builder tasarım deseninde bu nesnelerin oluşturulması Builder denilen sınıfların sorumluluğundadır. Client sadece oluşturulacak nesne türünü belirterek ana nesneyi oluşturan nesnelerin oluşturulmasıyla ilgilenmez. Abstract factory tasarım kalıbı ile benzer bir yapısı vardır. Aralarındaki fark builder tasarım deseni birden fazla nesnenin birleşmesinden oluşan nesnelerin üretilmesinden sorumludur.

Builder desenini oluşturan 4 yapı vardır.

    Product: Oluşturulan nesne.
    Builder: Product oluşturacak nesnelerin (Concrete Builder) uygulaması gereken arayüz.
    Concrete Builder: Product nesnesini oluşturan nesne veya özelliklerin oluşturulduğu sınıflar. Her concrete builder sınıfı aynı arayüzde farklı bir ürünün oluşturulmasını sağlar.
    Director: Verilen builder nesnesine göre product örneği oluşturur.

Örneğin Marka, Model ve Lastik özellikleri olan bir araba nesnemiz olsun. Bu araba nesnemizin özelliklerinin farklı değerler alması ile farklı özelliklerde araba nesnesi üretebiliriz. Builder tasarım deseni ile bu senaryoyu gerçekleştirelim. Uygulamamızın class diyagramı klasörde "diagramBuilderDeseni.png" isminde ki dosyada mevcuttur.

public class Araba
    {
        public string Marka { get; set; }
        public string Motor { get; set; }
        public string Lastik { get; set; }
 
        public override string ToString()
        {
            return String.Format("Marka:{0},Motor:{1},Lastik:{2}", Marka, Motor, Lastik);
        }
    }
 
    // ArabaBuilder arayüzü builder desenindeki Builder yapısıdır. 
    // Bir sınıfın Product ı oluşturan nesneleri oluşturması veya product ın özelliklerini setlemesi ile product ı oluşturan sınıflar bu arayüzü kullanmalıdır.
    public abstract class ArabaBuilder
    {
        protected Araba _araba;
        public Araba araba
        {
            get { return _araba; }
        }
        public abstract void MotorTak();
        public abstract void LastikTak();
    }
 
    // ArabaBuilder arayüzünden türeyen bütün sınıflar Builder desenindeki ConcreteBuilder yapısıdır.
    // ConcreteBuilder yapısı değişik product nesnelerinin oluşturulmasını sağlamaktır.
    public class ArabaConcrete1 : ArabaBuilder
    {
        public ArabaConcrete1()
        {
            _araba = new Araba { Marka = "Concrete1" };
        }
 
        public override void MotorTak()
        {
            _araba.Motor = "1.4 LPG";
        }
 
        public override void LastikTak()
        {
            _araba.Lastik = "15' Çelik jant";
        }
    }
 
    public class ArabaConcrete2 : ArabaBuilder
    {
        public ArabaConcrete2()
        {
            _araba = new Araba { Marka = "Concrete2" };
        }
 
        public override void MotorTak()
        {
            _araba.Motor = "1.8 Dizel";
        }
 
        public override void LastikTak()
        {
            _araba.Lastik = "17' Bor alaşımlı çelik jant";
        }
    }
 
    // ArabaBuilder arayüzündeki metodları çalıştırarak productın oluşturulmasını sağlar.
    // Builder desenindeki Director yapısıdır.
    public class ArabaDirector
    {
        public ArabaDirector(ArabaBuilder ArabaConcrete)
        {
            ArabaConcrete.MotorTak();
            ArabaConcrete.LastikTak();
        }
    }
 
Desenimizi aşağıdaki şekilde kullanabiliriz.
 
class Program
{
    static void Main(string[] args)
    {
        ArabaBuilder araba_builder = new ArabaConcrete1();
        ArabaDirector araba_olusutucu = new ArabaDirector(araba_builder);
        Console.WriteLine(araba_builder.araba.ToString());
 
        araba_builder = new ArabaConcrete2();
        araba_olusutucu = new ArabaDirector(araba_builder);
        Console.WriteLine(araba_builder.araba.ToString());
 
        Console.ReadKey();
    }
}



