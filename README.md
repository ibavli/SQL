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
