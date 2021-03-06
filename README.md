## SQL'DE DOSYA TİPLERİ
SQL'de dosya tipleri ikiye ayrılır. 
1. MDF (Meta Data File)
2. LDF (Log Data File)
Mdf : Veritabanına girilmiş olan verilerin aslını tutar. Yani veritabanına eklediğimiz bilgilerin kendisi tutar. 
Ldf : Log dosyasıdır. Veritabanında yapılan işlemlerin loglarını tutar. (Şu kullanıcı şu ip’den giriş yaptı, şu bilgiyi güncelledi gibi bilgiler yer alır) 
 


## CONSTRAINTS
Constraint tablolardaki alanlara girilen verilerin kontrollerini yapan ve kısıtlamalar getiren tekniklerin bütünüdür. Constraint ile amaçlanan tamamen veri bütünlüğünü, doğrulamasını ve tutarlılığını sağlamaktır.</br>
Not Null, Primary key, Foreign Key, Unique Key, Check ve Default, Constraintlerdir.**


## TRANSACT SQL (T-SQL)
T-sql, sql dilinin sql servera uyarlanmış halidir. T-sql Sql'i referans almış ve daha gelişmiş bir dildir. T-sql Microsoft Sql Server ile kullanılan bir sorgu dilidir. T-sql dilini genellikle 3 başlık altında inceleriz. Bunları;
1. Data Definition Language (DDL) = (create, alter, drop)
2. Data Manipulation Language (DML) = (select, insert, update, delete)
3. Data Control Language (DCL)** = (grant, deny, revoke) </br>
olarak ayırabiliriz.


### Örneklere geçmeden önce ilk önce veritabanımızı ve tablolarımızı oluşturalım.
create database SQLExercise

create table Employees</br>
(<br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;EmployeeId int identity(1,1),</br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Name nvarchar(50) not null,</br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Surname nvarchar(50) not null,</br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;TcNo nvarchar(11) not null,</br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Salary money not null,</br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DepartmentId int not null</br>
)

create table Departments</br>
(</br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DepartmentId int identity(1,1),</br>
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DepartmanName nvarchar(50) not null</br>
)

## UNIQUE CONSTRAINTS
Bir tabloda primary key kullanarak sadece bir tane benzersiz alan elde edebilirsiniz. Primary key tanımlamamızdaki amaç alanı benzersiz yapmak değil bütün alanların o alana bağımlı olmasını sağlamaktır. Unique Key ise alanı sadece benzersiz yapmak için kullanılır. Unique Key Primary Key'in aksine birden fazla olabilir. Tablo tasarımını yaparken bir alanın UNIQUE olup olmayacağına iyi karar vermemiz gerekir. Çünkü daha sonradan ALTER komutu ile alanın özelliklerini değiştirirken, ilgili alanda tekrarlayan kayıtlar varsa UNIQUE değerini veremeyiz. </br> 
Aşağıdaki kod ile TcNo alanını unique key olarak tanımlıyoruz. 

alter table Employees add constraint uniqueTc unique (TcNo) 


## FOREIGN KEY CONSTRAINT
Sql'de ikincil anahtarları, ilişkisel tablolar içerisindeki alanlar için veri tutarlılığını ve bağımlılığını sağlamak amaçlı kullanırız. Foreign key olarak tanımlayacağımız alan için kesinlikle referans alacağı bir alan ve tablo belirtmek zorundayız.</br>
Aşağıdaki komut ile Employees ve Departments tablolarıyla DepartmentId'ler üzerinden bir ilişki kuruyoruz.

alter table Employees
   add constraint forkeyDepartmentId
   foreign key(DepartmentId) references Departments(DepartmentId)


## CHECK CONSTRAINTS
Check Constraint, tablonun ilgili sütununa veri girerken veriyi bizim belirlediğimiz şekilde kontrol eder. Tc kimlikler 11 haneden oluşur. Bunu sql'de ayarlamak ise şöyledir;

alter table Employees
add constraint tcNoCheck check(Len(TcNo) = 11) 

## DEFAULT CONSTRAINTS
Default constraint, adından da anlaşılacağı üzere bir sütuna değer girilmediğinde o sütun için default bir değer ataması gerçekleştirir. Örneğin Employees tablosuna SalaryDay sütunu ekleyelim ve default değer atayalım.

alter table Employees
add SalaryDay date,
constraint defaultValue default(getdate()) for SalaryDay

## Tabloya veri ekleme (INSERT)
İlk önce Departments daha sonra Employees tablolalarımza veri ekleyelim.</br></br>

insert Departments (DepartmanName) values ('Software') </br>
insert Departments (DepartmanName) values ('Accounting') </br>
insert Departments (DepartmanName) values ('Sales')</br>

insert Employees (Name, Surname, TcNo, Salary, DepartmentId) values ('Ali','Veli','12345678912',4250,1) </br>
insert Employees (Name, Surname, TcNo, Salary, DepartmentId) values ('Hasan','Hseyin','12345678923',3500,1)  </br>  
insert Employees (Name, Surname, TcNo, Salary, DepartmentId) values ('Ömer','Faruk','12345678934',1700,2)   </br>
insert Employees (Name, Surname, TcNo, Salary, DepartmentId) values ('Hatice','Fadime','12345678945',750,3)</br>
insert Employees (Name, Surname, TcNo, Salary, DepartmentId) values ('Ayşe','Fatma','12345678956',750,3)</br>

## INSERT INTO SELECT
Bir tablomuzdaki kayıtları, bir başka tabloya (aynı alanlara sahip) kopyalayabiliriz. Bunu da Insert Into Select ile yapıyoruz. Örnek olarak Employees tablomuz ile aynı alanlara sahip EmployeeCopyOne isimli bir tablo oluştup, aşağıdaki kodu çalıştırdığımızda, Employee tablosundaki veriler kopya tablomuza eklenir.</br>

Insert Into EmployeesCopyOne</br>
Select *</br>
From Employees</br>

Bu işlem ile, tablomuzdak bütün kayıtları direkt kopyaladık.</br>
Peki sadece bir iki alanı kopyalamak istersek? İşte bunun içinde örnek bir senaryo oluşturalım ve ilk olarak şöyle bir tablo oluşturalım.</br>

create table EmployeesNames </br>
(</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Id int identity(1,1),</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;NameSurname nvarchar(max),</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DepartmentId int</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constraint pkEmployeesNames primary key(Id)</br>
)</br>

Daha sonra Id'si 1 olmayan verileri, name surname alanlarını concat ile birleştirerek yeni tablomuza kopyalayalım. Bunun için ise aşağıdaki kodları yazmalıyız.</br>

insert EmployeesNames</br>
(NameSurname, DepartmentId)</br>
select CONCAT(Name, ' ' , Surname), DepartmentId from Employees</br>
where DepartmentId <>1</br>

Artık yeni oluşturmuş olduğumuz tablomuza, Employees tablomuzdan Id'si 1 olmayan kayıtlar name ve surname alanları concat ile birleştirilerek alınmıştır.

## IDENTITY INSERT
Biz bir tablo oluşturduğumuzda tanımladığımız Id'ye primary key ve bir bir artan özelliğini veririz. Daha sonra bu tabloya bir veri eklerken Id alanına da kendimiz değer vermeye kalkarsak hata alırız. Bir önceki örnekte oluşturduğumuz EmployeesNames tablosu için aşağıdaki kodu çalıştırarak hatayı alabilirsiniz.</br>

insert EmployeesNames</br>
(Id, NameSurname, DepartmentId)</br>
values</br>
(50,'test', 2)</br><br>
Evet şimdi hatayı aldık ve gözlemledik. Peki illa Id alanına kendim veri eklemek istiyorum derseniz, identity_insert komutu ile bunu yapabilirsiniz. Aşağıdaki kodu çalıştırarak Id alanına kendiniz veri ekleyebilirsiniz.<br><br>
set identity_insert EmployeesNames on <br>
insert EmployeesNames<br>
(Id, NameSurname, DepartmentId)<br>
values<br>
(50,'test', 2)<br>
set identity_insert EmployeesNames off<br> 

## DELETE VE TRUNCATE TABLE
Delete bir tablodan satır veya satırları silmek için kullanılır. Özellikle tüm satırı etkileyeceği için dikkatli kullanılması gereken bir komuttur. Biraz önce oluşturduğumuz EmployeesNames tablosundaki verileri silmek için şu kodu çalıştıralım.</br></br>
delete from EmployeesNames</br></br>
Bu komut ile tüm verileri sildik fakat yeni bir veri eklediğimizde en son kalan Id'den artan şekilde devam eder. Yani bu komut verileri tablodan kalıcı olarak siliyor ama transaction-log'da kayıtlı olarak kalıyor. Eğer Id'imizde birden başlasın demek isterseniz, şu kodu yazmalısınız.</br></br>

truncate table EmployeesNames

## UPDATE
Update komutu tablodaki var olan bir verinin değiştirilmesi için kullanılır. Bu komutta delete komutu gibi tüm tabloyu etkileyen bir komut olduğu için dikkatli kullanılması gerekmektedir. Tüm çalışanların maaşına %15 zam yaptığımızı düşünelim. Bu güncellemeyi şu kod ile yaparız;</br></br>
update Employees set Salary = Salary + (Salary/100) * 15</br></br>
Eğer sadece departmenId'si 1 olan yani software departmanındakilerin maaşını arttırmak istersek ise;</br></br>
update Employees set Salary = Salary + (Salary/100) * 15 where DepartmentId = 1</br></br>
komutunu çalıştırırız.

## ALIASES (TAKMA AD)
Aliases result tablosunda bir tablonun veya sütunun ismini anlık olarak değiştirmek için kullanılır. Özelikle kolon isimlerinin daha anlaşılır olması, tablo isimlerinin kısaltılması için tercih edilir.</br>

select  Name, Surname as [Soy Adı] from Employees </br>

Bu sorguyu çalıştırdığımızda result penceresinde Surname sütunu Soy Adı olarak gelir.<br>
Şimdi de iki tablomuzun isimlerini as komutu ile değiştirerek (sadece sorgu sürecinde) iki tablodan da bir kaç alanı yine as komutları ile değiştirerek result penceresine getirelim.

select calisanlar.EmployeeId,</br>
calisanlar.Name AS isim,</br> 
calisanlar.Surname as soyisim,</br>
meslekler.DepartmanName as meslek</br>
from Employees as calisanlar, Departments AS meslekler</br>
where calisanlar.DepartmentId = meslekler.DepartmanId</br>
</br>
Bu yukarıdaki kod ile result penceresinde EmployeeId, isim, soyisim, meslek alanları ile sonuçlar geldi.


## CONCAT
select Concat(Name,' - ',Surname) as [Adı ve SoyAdı] from Employees </br>
Concat komutu iki ifadeyi birleştirmek için kullanılır. Bu değişiklik sadece result penceresinde olur. Bu kodu çalıştırınca result penceresinde tek sütunlu bir sonuç geldi. Sütunun adı [Adı ve soyadı] içerisindeki değerler ise isim-soyisim şeklinde.


## WHERE komutu operatörleri
1) Karşılaştırma Operatörleri
	* Eşittir ( = )
	* Büyüktür ( > )
	* Küçüktür ( < )
	* Büyük eşit ( >= )
	* Küçük eşit ( <= )
	* Eşit değil ( != veya <> )
	
2) Mantıksal Operatörler
	* And
	* Or

3) Diğer Operatörler
	* Between
	* In
	* Like

## IN Operatörü
In operatörü where komutu ile kullanılır. Birden fazla eşitlik durumu yazmak yerine tercih edilir.</br>

select * from Employees where Name in ('ali','hasan') </br>

Bu sorgu ile isimleri ali ve hasan olanları listeledik.


## NOT IN Operatörü
In operatörünün tam tersidir. İçinde eşit olmayanları listeler.</br>

select * from Employees where Name not in ('ali','hasan') </br>

Bu sorgu ile isimleri ali ve hasan olmayanları listeledik.

## LIKE ve NOT LIKE Operatörü
Filtreleme yaparak aramak için kullanırız.</br>

select * from Employees where Name like '%ha%'    </br>
Bu sorgu isimlerinin içerisinde 'ha' geçenleri arar.</br></br>

select * from Employees where Name like 'h%' </br>
Bu sorgu isimleri 'h' ile başlayanları listeler.</br></br>

select * from Employees where Name like '%r' </br>
Bu sorgu isimleri 'r' ile bitenleri listeler.</br>

NOT : Bu yukarıdaki kodları not like şeklinde yazarsanız tam tersi işlemleri yapacaktır.

## ORDER BY ve CONCAT kullanımı
Order by bize tablolarımızı sıralamamızı sağlar. Concat ile birlikte kullanımı da aşağıdaki gibidir.</br>
select Concat(Name,' ',Surname) as [Adı ve SoyAdı]  from Employees  order by [Adı ve SoyAdı] desc

## DISTINCT
Distinct komutu farklı kayıtları elde etmemizi, yani tekrar eden kayıtları tekil olarak listelememizi sağlar.</br>
select Distinct DepartmentId from Employees </br>
select distinct DepartmentId, SalaryDay from Employees


## TOP
Tablomuzdan kaç kayıt getirmek istiyorsak sorgumuza onu ekleriz.</br>
select top 2 * from Employees 

## Önemli not (Top kullanımı ile ilgili)
select top 2 * from Employees order by Salary asc </br></br>
Bu yukarıdaki sorguyu çalıştırdığımızda en düşük maaşlı iki kişi geldi. Fakat komutu şöyle yaparsak,</br></br>
select top 1 * from Employees order by Salary asc => bu komut ile ekrana bir kişi geldi. Fakat maaşı bu maaşa eşit olan biri daha var. İşte bu gibi durumlar için, with ties kullanırız.</br></br>
select top 1 with ties * from Employees order by Salary asc

## AGGREGATE FUNCTIONS
Aggregate functionlar select ifadesiyle kullanılan geriye tek hücre olarak sonuç dönen fonksiyonlardır.</br>

select COUNT(*) from Employees -- bütün satırları sayar.</br>
select COUNT(EmployeeId) from Employees -- EmployeeId alanına göre null olmayan satırları sayar</br>
select SUM(Salary) from Employees -- maaş verilerini toplar</br>
select AVG(Salary) from Employees -- maaş verilerinin ortalamasını verir </br>
select Min(Salary) from Employees -- maaş verilerinden minimum olanını verir</br>
select MAX(Salary) from Employees -- maaş verilerinin maximum olanını verir </br>


##  CASE WHEN
Case When ifadesi select içerisinde koşullu ifadeler için kullanılır. Örneğimizde DepartmentId alanı 1 olanlar için Software, 2 olanlar için Accounting, 3 olanlar için Sales yazmasını istiyoruz.</br></br>

select</br>
&nbsp;&nbsp;&nbsp;&nbsp;	Name,</br>
&nbsp;&nbsp;&nbsp;&nbsp;	Surname,</br>
&nbsp;&nbsp;&nbsp;&nbsp;	DepartmentId,</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	case DepartmentId</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	when 1 then 'Software'</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	when 2 then 'Accounting'</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	when 3 then 'Sales'</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	end as [Departmanlar] from Employees </br>
		

## GROUP BY
Group by tablodaki ortak verilerin gruplanmasını sağlar. Önce gruplanacak alanı belirleriz, daha sonra o alan için select sorgusu çekeriz. Sütundaki ortak alan değere göre alanlar gruplanır. </br> </br>
select DepartmentId from Employees
group by DepartmentId</br></br>
Bu kod ile Employees tablosunu DepartmentId alanına göre grupladık ve daha sonrasında bu alana göre select sorgusu çektik.
</br></br>
SELECT DepartmentId, SUM(Salary) as [Maas toplamı]</br>
FROM Employees</br>
Group By DepartmentId</br></br>

SELECT DepartmentId, COUNT(Salary) as [Calisan sayisi]</br>
FROM Employees</br>
Group By DepartmentId</br></br>

SELECT DepartmentId,</br>
&nbsp;&nbsp;&nbsp;&nbsp;COUNT(Salary) as [Calisan sayisi],</br>
&nbsp;&nbsp;&nbsp;&nbsp;SUM(Salary) as [Maas toplamı],</br>
&nbsp;&nbsp;&nbsp;&nbsp;AVG(Salary) as [Ortalama maas],</br>
&nbsp;&nbsp;&nbsp;&nbsp;MAX(Salary) as [Max maas],</br>
&nbsp;&nbsp;&nbsp;&nbsp;MIN(Salary) as [Min maas]</br>
&nbsp;&nbsp;&nbsp;&nbsp;FROM Employees</br>
Group By DepartmentId</br>


## HAVING
Having, group by kullanımında filtreleme yapmak için kullanılır. Group by kullanımından sonra where komutunu kullanamayız. Yukarıdaki group by komutundan sonra Id'si 2 olmayanları listelemek için şu kodu yazarız: </br></br>
select DepartmentId from Employees
group by DepartmentId
having DepartmentId <> 2


## JOINS
Join ifadesi iki veya daha fazla tabloyu result penceresinde birleştirmek için kullanılır. Bu farklı tablolar genellikle ilişkilendirilen tablolardır. Join, ilişkisel veritabanlarında bir tablodaki ikincil anahtar (foreign key) üzerinden  parent tablodaki bütün sütunlara erişmek için kullanılır. Yani birleştirme benzer alanlar üzerinden yapılır.</br>
Dikkat : Join ile tablolar sadece sorgu sonucunda birleştirilerek daha anlamlı sonuçlar elde etmek için kullanılır.</br></br>
T-sql 4 faklı join türü destekler. Bunlar;
 - Inner Join
 - Outer Join
 - Self Join
 - Cross Join
 
 ## INNER JOIN
 İki adet tablomuzdaki kayıtları belli bir kritere göre birleştirmek için INNER JOIN komutu kullanılırız. Genelde en çok tercih edilen join yöntemidir.</br>
NOT: INNER JOIN yerine sadece JOIN kullanılabilir.</br></br>
 select</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	e.Name,</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	e.Surname,</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	e.DepartmentId as EmployeeDepartmentID,</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	d.DepartmanID,</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	d.DepartmanName</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	from Employees as e</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;		inner join</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	Departments as d on e.DepartmentId = d.DepartmanID </br>
</br>
Yukarıdaki örnekte Employees ve Department tablolarını DepartmentId’leri üzerinden birleştirdik. Her bir tabloya ayrı ayrı select çekip incelediğimizde, sadece ilişkili kayıtların geldiğini ve iki alan değerlerininde birbirine eşit olduğunu görürüz. Eğer Departments tablosuna bir kayıt eklersek ve bunu Employee tablosundaki herhangi bir veri ile ilişkilendirmezsek, yukarıdaki sorguda gelmez. Bu aşağıdaki kodu çalıştırıp ardından tekrar join komutunu deneyebilirsiniz. </br></br>
insert Departments (DepartmanName) values ('Human Resources')
<br/></br>
İki tablonun tüm verileri gelsin istersek;<br/>
select * </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	from Employees as e</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	inner join </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	Departments as d</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	on e.DepartmentId = d.DepartmanId
	</br></br>
	Sadece Employees tablosunun bütün verileri gelsin, Departments tablosundan ise sadece DepartmentName alanı gelsin istersek;</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	select</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	e.* , d.DepartmanName</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	from Employees as e</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	inner join </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	Departments as d</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	on e.DepartmentId = d.DepartmanId</br></br>

Bu iki tablodan biraz daha anlamlı bir veri elde edelim;</br>
select</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	CONCAT(e.Name, ' ', e.Surname) as [Name-Surname],</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	d.DepartmanName   </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	from</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	Employees as e</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	join</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	Departments as d</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	on e.DepartmentId = d.DepartmanId</br></br>

Birde DepartmentName alanına göre gruplayarak, grup küme elemanlarını saydırıyoruz ve hangi departmanımızda kaç çalışanımız varmış görüyoruz.</br>
select</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;d.DepartmanName,</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;COUNT(*) as EmployeesCount</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;from</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Employees as e</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;join</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Departments as d</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;on e.DepartmentId = d.DepartmanId</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;group by d.DepartmanName</br></br>

## OUTER JOINS
Outer joinler (dış birleşim) Left, Rigth, Full join olarak 3’e ayrılır.

## LEFT OUTER JOINS
Left outer veya left join iki tablodaki birleştirirken öncelikle sol tablonun tamamı sağ tablonun sadece kesişenlerini alır. Sol tablodaki ve sağ tablodaki kesişen kayıtlar ve sol tablodaki kesişmeyen kayıtlar listelenir. Sol taraftan kesişmeyen kayıtların karşısında NULL değerler vardır.</br></br>
select  * from</br>
Departments as d</br>
left join</br>
Employees as e</br>
on d.DepartmanId = e.DepartmentId</br></br>
Sorgu sonucunu incelediğimizde Departments tablosu sol tablo, Employees tablosu sağ tablodur ve sol tablada Human Resources alanının karşı satırlarının NULL olduğunu görürüz. Inner joinden farkıda burada ortaya çıkıyor. </br></br>
Şimdiki örneğimizde ise sol tablomuzda null olan alanları ekrana getirelim</br></br>
select   *  from</br>
Departments as d </br>
left join</br>
Employees as e</br>
on e.DepartmentId = d.DepartmanId</br>
 where e.DepartmentId is null </br>
 
 ## RIGHT OUTER JOIN
 Left join'in tam tersidir.</br></br>
select * from </br>
Employees as e </br>
right join</br>
Departments as d</br>
on e.DepartmentId = d.DepartmanID

## FULL OUTER JOIN
Her iki tablodaki kesişen ve kesişmeyen kayıtların hepsini getirir.</br></br>
select * from</br>
Employees as e</br>
full join </br>
Departments as d </br>
on e.DepartmentId = d.DepartmanID </br>

## SELF JOIN
Tabloların kendisiyle birleştirilmesi işlemidir. Örnek olarak Categories isimli tablo oluşturalım ve senaryomuza başlayalım.</br></br>
create table Categories</br>
 ( </br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Id int identity(1,1), </br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;CategoryName nvarchar(max), </br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;UpperCategoryId int</br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constraint pkCategories primary key(Id), </br>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;constraint fkCategories foreign key(UpperCategoryId) references Categories(Id)</br>
) </br></br>

Bu örnekte aynı tablo içerisinde alt kategori ve üst kategori bilgilerini tutuyoruz. Bu sayede sınırsız kategori ekliyoruz. Üst kategori olan satırların UpperCategoryId’lerini null ekliyoruz. Alt kategori olanların ise UpperCategoryId’sine aynı tablodaki UpperCategoryId alanın Id alanını set ediyoruz. Örnek olarak bir kaç veri ekleyelim.</br></br>

set identity_insert Categories on </br>
insert Categories</br>
(Id,CategoryName,UpperCategoryId)</br>
values</br>
(1,'Kadın Giyim',Null), -- Üst kategori</br>
(2,'Erkek Giyim',Null), -- Üst kategori</br>
(3,'Abiye',1), -- Kadın Giyim kategorisinin alt kategorisi</br>
(4,'Elbise',1), -- Kadın Giyim kategorisinin alt kategorisi</br>
(5,'Deri Ceket',2), -- Erkek Giyim kategorisinin alt kategorisi</br>
(6,'Mont',2), --  Erkek Giyim kategorisinin alt kategorisi</br>
(7,'Deri Mont',6), -- Mont kategorisinin alt kategorisi</br>
(8,'Şişme Mont',6), -- Mont kategorisinin alt kategorisi</br>
(9,'Çocuk giyim',NULL) -- Üst kategori </br>
set identity_insert Categories off</br></br>
Şimdi bir tane self join örneği yapalım </br></br>
select * from</br>
Categories as UpperCat</br>
join</br>
Categories SubCar </br>
on UpperCat.Id = SubCar.UpperCategoryId </br>

## DERIVED TABLE
Derived Table bir veya daha fazla tablonun birleştirilerek select sonucu oluşan sanal tablolardır. Derived table sadece anlık çözüm üretmek için kullanılır. Derived table from komutundan sonra içteki select sorgusunun sonucunu miras alır ve döndürür. Derived table kalıcı olarak saklanan bir tablo değildir. As deyiminden sonra tek satırlık kullanım alanı vardır (where, order by, group by kullanılabilir) Örnek olarak;</br></br>
select * from</br>
(</br>
select * from Employees -- iç sorgu, dış sorguya aktarılır </br>
)</br>
as derivedTable</br>
</br>
Yukarıdaki örneğimizde Employees tablosundaki kayıtları dış sorguya aktardık ve dış sorgu sonucunda oluşacak tabloya derivedTable ismini verdik. </br></br>

select DepartmanName, EmployeeCount</br>
from</br>
(</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;select DepartmentId,</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;COUNT(*) as EmployeeCount</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;from</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Employees</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;group by DepartmentId</br>
)</br>
as DepartmentSummary</br>
inner join</br>
Departments as dp</br>
on</br>
DepartmentSummary.DepartmentId = dp.DepartmanId</br>

Örneğimizde iç sorgudan Employees tablosunun DepartmentId alanına göre grupladık ve Count ile alt kümelerini saydırdık. Daha sonra dış sorguya aktarılan veriyi Departments tablosuyla birleştirerek DepartmanName ve EmployeeCount'u listeledik. </br></br>

Derived table'a neden ihtiyaç olduğunu aşağıdaki sorgular ile daha iyi anlarız. </br></br>
select </br>
CONCAT(Name,' ', Surname) as FullName</br>
from Employees</br>
where FullName = 'Ali Veli' </br></br>
Bu yukarıdaki sorguyu çalıştırdığımızda hata alırız. Çünkü aliases kullanımını, where ile kullanamayız. Sadece order by ile kullanabiliriz. Bu sorguyu şimdi derived table ile yapalım.</br></br>
select * from</br>
(</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;select CONCAT(Name,' ', Surname) as FullName from Employees</br>
)</br>
as derivedTable where derivedTable.FullName = 'Ali Veli' </br>


## SUB QUERY
T-Sql'de sorgu içerisinde sorgular yazabiliriz. Bazen filtreleme yapmak için, bazen select ifadesinde başka bir tablodan sütun çıkartmak için kullanırız. Örnek olarak;</br></br>

select * from Employees</br>
where Salary =</br>
(  </br>
select Max(Salary) from Employees  </br>
) </br></br>
 
Veya bir başka örnek.
</br></br>
select * from Departments</br>
where DepartmanId not in</br>
(</br>
select DepartmentId from Employees</br>
)</br>

## SQL PIVOT
Pivot sorgular bir select ifadesiyle satır olarak dönen sonuçların sütunlara çevrilmesi için kullanılır. </br></br>

select</br>
DepartmanName, </br>
Sum (e.Salary) as TotalCost</br>
from Departments as d</br>
inner join</br>
Employees as e </br>
on</br>
d.DepartmanID = e.DepartmentId </br>
group by DepartmanName</br></br>

Yukarıdaki sorguda sonuç satır olarak listelendi. Şimdi bu sorguyu bir derived table içerisine alıyoruz ve pivot yapıyoruz. 
 
</br></br>
select * from</br>
(</br>
select</br>
DepartmanName,</br>
Sum (e.Salary) as TotalCost</br>
from Departments as d</br>
inner join</br>
Employees as e</br>
on</br>
d.DepartmanID = e.DepartmentId</br>
group by DepartmanName</br>
)</br>
as pivotTable</br>
</br>

pivot</br>
(</br>
SUM(TotalCost)</br>
for DepartmanName in ([Accounting],[Sales],[Software])</br>
) as pivotTable </br>


## VIEWS
Sql'de viewlar, sanal tablo olarak kullanılan, gerçek tablodan veriyi alarak özetleyen tablolardır. Derived tablolaların aksine anlık değillerdir, kalıcı olarak saklanırlar. Gerçek tablolardan farklı select sonucu oluşurlar. Viewlar sürekli yazdığımız sql sorgularımızı veya  karmaşık iç içe yazılan (subquery) veya çoklu tablolarımızın (joinler) ihtiyaç halinde sürekli yazmak yerine bir kere yazıp sürekli kullanma kolaylığı sağlar. Aşağıdaki sorguda SQLExercise veritabanına Employees tablomuzdan Name, Surname, Salary alanlarını getiren bir view oluştuyoruz.</br></br>

create view EmployeesSummary</br>
as</br>
select Name,Surname,Salary from Employees</br></br>

Yukarıdaki sorguyu çalıştırdığımızda select sonucu oluşan 3 sütunlu bir EmployeesSummary viewı oluşturduk. Şimdi bu oluşturduğumuz viewı çağıralım.</br></br>

select * from EmployeesSummary</br></br>

Sonucu incelediğimizde view içerisindeki sorgumuzdan sonucun döndüğünü görüyoruz. Bunu bir metot gibi düşünecek olursak her view çağrıldığında gövdesindeki komut çalışacak ve sonuç listelenecek. Aslında sonuç değişmesede bir sonraki kullanımlarda sadece view ismini çağıracağız.</br>
Dikkat : View oluşturuken kesinle select ifadeleri içerisindeki sütun isimleri belli olmalıdır (İsimsiz sütun olmamalıdır) ve ayrıca sütun isimleri farklı olmalıdır. Bu aşağıdaki kodda Employee tablosundaki DepartmentId ile Departments tablosundaki DepartmentId isimleri çakışacağı için böyle bir çözüm üretilir.</br></br></br>

create view ViewEmployeesAndDepartments</br>
as</br>
select</br>
e.Name,</br>
e.Surname,</br>
d.DepartmanName,</br>
e.DepartmentId as EmployeeDepartmentId,</br>
d.DepartmanId as DepartmentDepartmentId</br>
from Employees as e</br>
inner join</br>
Departments as d</br>
on</br>
e.DepartmentId = d.DepartmanID </br></br>

select * from ViewEmployeesAndDepartments

## USER DEFINED FUNCTIONS
Sql server içerisinde bize sunulan hazır fonksiyonlar vardır. Aggregate functionlar bunlardan sadece bir kaçıdır. Hazır fonksiyonları kullanıdığımız gibi kendimizde sql server ortamında kullanabileceğimiz fonksiyonlar tanımlıyabiliyoruz. Fonksiyonların bir diğer kullanım kolaylığı ise gövdelerine dışarıdan parametre alabilmeleridir. User defined funcionları iki bölümde inceliyor olacağız.
1. Scalar-Valued Functions</br>
2. Table-Valued Functions </br>

### 1) Scalar-Valued Functions
Scalar valued function geriye tek bir hücre dönen fonksiyonlardır. Function tanımlarken function dönüş tipimizi belirliyoruz ve function gövdesinde dönüş tipinde değer çıkarıyoruz. Ekrana mesaj veren bir fonksiyon yazalım;</br></br>

create function fncMessage()</br>
returns nvarchar(50)</br>
as</br>
begin</br>
return 'Test message with functions'  </br>
end</br></br>

Şimdi de fonksiyonumuzu çağıralım;</br></br>

select dbo.fncMessage()</br></br>

Bir başka örnek ; </br></br>

create function fncFullName</br>
(</br>
@name nvarchar(100),</br>
@surname nvarchar(100)</br>
)</br>
returns nvarchar(200) as begin</br>
declare @fullname nvarchar(200)</br>
set @fullname =  Concat(@name,' ',@surname)</br>
return @fullname </br>
end </br></br>

Şimdi de oluşturduğumuz function'ı kullanalım.</br></br>
select * from Employees</br>
select</br>
dbo.fncFullName(e.Name, e.Surname) as FullName,</br>
TcNo</br>
from Employees as e</br>


### 2) Table-Valued Functions
Table-value functions, geriye birden fazla satır ve sütun dönmesi için kullanılır. Tablo dönen fonksiyonları anlamak için viewlar hakkında bilgimiz olmalı. Tablo dönen fonksiyon yerine view çoğu zaman ihtiyacımızı karşılar. Table-valued functionların viewlardan farkı parametre alabilmeleridir. Aşağıdaki kodlarda view ve table-valued function nasıl tanımlanır ona dikkat edelim ve farklılıklarını inceleyelim. Örneğimizde EmployeeId'sini gönderdiğimiz çalışana erişmek için objelerimizi inceleyeceğiz. </br></br>

create view viewEmployees as  select * from Employees </br></br>

Oluşturduğumuz bu view üzerinden EmployeeId'si 3 olan çalışanımızı çağırıyoruz. </br></br>

select * from viewEmployees where EmployeeId = 3</br></br>

create function fncGetEmployee(@employeeId int)
returns table
as
return (select * from Employees  
 where EmployeeId = @employeeId) 


Burada oluşturduğumuz view sadece çalışanları listeliyor. View içerisinde EmployeeId 3 olan çalışanımızı çağırabilirdik fakat bu sefer sadece sürekli EmployeeId'si 3 olan çalışan gelecekti. Bu durumda bu view static çalışacak. Bu durumda diğer çalışanlar içinde ayrı ayrı view yazamayacağımız için view dan bütün satırların gelmesini sağladık ve view üzerinden filtreleme yaptık. Şimdi aynı senaryoyu table function ile yazalım.</br></br>

create function fncGetEmployee(@employeeId int)</br>
returns table</br>
as</br>
return (select * from Employees  </br>
where EmployeeId = @employeeId) </br></br>

Table-Valued function'ı incelediğimizde tıpkı scalar functionlar gibi tanımladık. Returns bölüme table ile dönüş tipinin table olduğunu as den sonra ise return ile select ifademizi tanımladık. </br></br>

SELECT * FROM dbo.fncGetEmployee(3)

## STORED PROCEDURE
Stored procedure server taraflı saklanan bir sql server objesidir. Procedure’ler functionlar gibi parametre alabilir, tablo döndürebilir, insert, update, delete işlemleri için  kullanılabilir. Procedure'ler sadece tanımlandığı isim ile çağrılarak gövdesindeki komutlar hakkında bilgi vermediği için güvenlik objesi olarakda kullanılır. Ayrıca stored procedure’ler önemli performans objeleridir. Server tarafında saklandığı için ağ trafiğini azaltır. T-sql yazılan her sorgu parse, optimize, compile, execute aşamalarından geçer. Stored procedure ilk tanımlandığında bu aşamalardan geçer ve sonraki çalıştırma işleminde sadece execute edilir. Employees tablosuna insert işlemi yapan bir procedure örneği; </br> </br>

create procedure spAddEmployee </br>
( </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@sp_Name nvarchar(50), </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@sp_Surname nvarchar(50), </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@sp_TcNo nvarchar(11), </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@sp_Salary int, </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@sp_DepartmentId int </br>
) </br>
as </br>
begin </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	insert Employees </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	(Name,Surname,TcNo,Salary,DepartmentId) </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	values </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	(@sp_Name, @sp_Surname, @sp_TcNo, @sp_Salary, @sp_DepartmentId) </br>
end </br></br>

Şimdi de oluşturduğumuz procedure'u çalıştıralım. </br></br>
exec spAddEmployee 'sp_name', 'sp_surname', '45678912346', 1333, 1</br></br></br>

Bir başka örnek. Bu aşağıdaki örnekte senaryomuz şu şekilde. Bu stored procedure ile, Departments tablosuna yeni bir kayıt ekleyeceğiz. Fakat ekleyeceğimiz Id ile değer var ise sonuç bize sıfır dönecek. Kayıt yok ise, kayıt işlemi gerçekleştirilecek ve sonuç geriye bir döndürülecek.</br>
NOT : Procedure tanımlarken dönüş tipi belirlemiyoruz. Sadece değer döndürecekse return ile değer döndürüyoruz. Dönüş tipi belirlemememizin nedeni procedureler geriye her zaman int değer döner. Eğer procedure içeriden farklı bir tipte değer dönecekse output parameter kullanılır. 
</br></br>

 create procedure spAddDepartment</br>
 (</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	 @sp_DepartmentId int,</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	 @sp_DepartmentName nvarchar(50)</br>
 )</br>
 as</br>
 begin</br>
</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	declare @IsHasRecord int </br>
 </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	select @IsHasRecord = COUNT(*) from Departments</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	where DepartmanID = @sp_DepartmentId </br>
 </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	declare @result int</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	if @IsHasRecord > 0</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	begin</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;		set @result = 0</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	end</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	else</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	begin</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;		set identity_insert Departments on</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	insert Departments</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;		(DepartmanID, DepartmanName)</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;		values</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;		(@sp_DepartmentId,@sp_DepartmentName) </br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;		set identity_insert Departments off</br></br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;		set @result = 1</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	end</br>
</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	return @result</br>
</br>
end </br></br>
Şimdi de çalıştıralım. İlk önce var olan bir Id deneyelim, daha sonra mevcut olmayan bir Id ile kayıt ekleyelim. </br></br>

declare @IsHasRecord </br></br>
int exec @IsHasRecord = spAddDepartment 5,'sp_departmentName'</br></br>
if @IsHasRecord = 0</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;begin</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print 'Kayıt işlemi başarız'</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;end</br>
else</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;begin</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print 'Kayıt işlemi başarılı'</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;end</br>


## TRIGGER
Triggerlar bir olay için yazılırlar ve otomatik olarak tetiklenirler. Değişiklikleri kontrol etmek, karmaşık iş kurallarını gerçekleştirmek, eposta atmak gibi otomatik işlemler, hata mesajları elde etmek gibi çeşitli ihtiyaçlar için kullanılır. Triggerlar ddl trigger ve dml trigger olmak üzere içi bölüme ayrılır. Dml triggerlar kendi içinde ikiye ayrılır. After trigger ve instead of trigger.</br></br>
GENEL YAPISI</br>
create trigger trigger_name</br>
on</br>
table_name</br>
{after | instead of} {insert-update-delete}</br>
as</br>
begin</br>
--query </br>
end</br></br>
NOT : triggerları enable veya disable ile aktif veya pasif yapabiliriz. Ayrıca kaldırmak istersek, drop ile kaldırabilir.
### After Trigger
İlk olarak design seçeneğincen Employee tablomuza Email alanı açalım. Allow nulls seçeneğini seçelim. Daha sonra aşağıdaki triggerımızı oluşturalım.</br></br>
create trigger trgAddMail</br>
on Employees</br>
after insert</br>
as</br>
begin</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	declare @name nvarchar(max)</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	declare @employeeId int</br></br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	select @name = Name, @employeeId = EmployeeId from inserted</br></br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	update Employees</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	set Email = CONCAT(@name, '@ibavli.com')</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  where EmployeeId = @employeeId</br>
end</br></br>
Daha sonra aşağıdaki kaydı ekleyelim ve email kısmına otomatik olarak verinin eklendiğini gözlemleyelim.</br></br>
insert Employees (Name,Surname,TcNo,Salary,DepartmentId) values ('triggerName','triggerSurname', '12345678948', 1200, 1)</br>

### Instead Of Trigger
Instead of trigger insert, update, delete komutlarının yerine çalışan trigger türüdür.</br></br>

create trigger trgDelete</br>
on Employees</br>
instead of delete</br>
as</br>
begin</br>
print 'yasaklı işlem'</br>
end</br></br>

