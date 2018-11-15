## SQL'DE DOSYA TİPLERİ
SQL'de dosya tipleri ikiye ayrılır. 
**1. MDF (Meta Data File)
2. LDF (Log Data File)**
Mdf : Veritabanına girilmiş olan verilerin aslını tutar. Yani veritabanına eklediğimiz bilgilerin kendisi tutar. 
Ldf : Log dosyasıdır. Veritabanında yapılan işlemlerin loglarını tutar. (Şu kullanıcı şu ip’den giriş yaptı, şu bilgiyi güncelledi gibi bilgiler yer alır) 
 


## CONSTRAINTS
Constraint tablolardaki alanlara girilen verilerin kontrollerini yapan ve kısıtlamalar getiren tekniklerin bütünüdür. Constraint ile amaçlanan tamamen veri bütünlüğünü, doğrulamasını ve tutarlılığını sağlamaktır.</br>
**Not Null, Primary key, Foreign Key, Unique Key, Check ve Default, Constraintlerdir.**


## TRANSACT SQL (T-SQL)
T-sql, sql dilinin sql servera uyarlanmış halidir. T-sql Sql'i referans almış ve daha gelişmiş bir dildir. T-sql Microsoft Sql Server ile kullanılan bir sorgu dilidir. T-sql dilini genellikle 3 başlık altında inceleriz. Bunları;
**1. Data Definition Language (DDL) **= (create, alter, drop)
**2. Data Manipulation Language (DML)** = (select, insert, update, delete)
**3. Data Control Language (DCL)** = (grant, deny, revoke) </br>
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
**
## UNİQUE CONSTRAİNTS
Bir tabloda primary key kullanarak sadece bir tane benzersiz alan elde edebilirsiniz. Primary key tanımlamamızdaki amaç alanı benzersiz yapmak değil bütün alanların o alana bağımlı olmasını sağlamaktır. Unique Key ise alanı sadece benzersiz yapmak için kullanılır. Unique Key Primary Key'in aksine birden fazla olabilir. Tablo tasarımını yaparken bir alanın UNIQUE olup olmayacağına iyi karar vermemiz gerekir. Çünkü daha sonradan ALTER komutu ile alanın özelliklerini değiştirirken, ilgili alanda tekrarlayan kayıtlar varsa UNIQUE değerini veremeyiz. </br> 
Aşağıdaki kod ile TcNo alanını unique key olarak tanımlıyoruz. 

alter table Employees add constraint uniqueTc unique (TcNo) 


## FOREİGN KEY CONSTRAİNT
Sql'de ikincil anahtarları, ilişkisel tablolar içerisindeki alanlar için veri tutarlılığını ve bağımlılığını sağlamak amaçlı kullanırız. Foreign key olarak tanımlayacağımız alan için kesinlikle referans alacağı bir alan ve tablo belirtmek zorundayız.</br>
Aşağıdaki komut ile Employees ve Departments tablolarıyla DepartmentId'ler üzerinden bir ilişki kuruyoruz.

alter table Employees
   add constraint forkeyDepartmentId
   foreign key(DepartmentId) references Departments(DepartmentId)


## CHECK CONSTRAİNTS
Check Constraint, tablonun ilgili sütununa veri girerken veriyi bizim belirlediğimiz şekilde kontrol eder. Tc kimlikler 11 haneden oluşur. Bunu sql'de ayarlamak ise şöyledir;

alter table Employees
add constraint tcNoCheck check(Len(TcNo) = 11) 

## DEFAULT CONSTRAİNTS
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

## ALİASES (TAKMA AD)
Aliases result tablosunda bir tablonun veya sütunun ismini anlık olarak değiştirmek için kullanılır. Özelikle kolon isimlerinin daha anlaşılır olması, tablo isimlerinin kısaltılması için tercih edilir.</br>

select  Name, Surname as [Soy Adı] from Employees </br>

Bu sorguyu çalıştırdığımızda result penceresinde Surname sütunu Soy Adı olarak gelir.

## Concat kullanımı
select Concat(Name,' - ',Surname) as [Adı ve SoyAdı] from Employees </br>
Concat komutu iki ifadeyi birleştirmek için kullanılır. Bu değişiklik sadece result penceresinde olur. Bu kodu çalıştırınca result penceresinde tek sütunlu bir sonuç geldi. Sütunun adı [Adı ve soyadı] içerisindeki değerler ise isim-soyisim şeklinde.


## Where komutu operatörleri
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

## In Operatörü
In operatörü where komutu ile kullanılır. Birden fazla eşitlik durumu yazmak yerine tercih edilir.</br>

select * from Employees where Name in ('ali','hasan') </br>

Bu sorgu ile isimleri ali ve hasan olanları listeledik.


## Not In Operatörü
In operatörünün tam tersidir. İçinde eşit olmayanları listeler.</br>

select * from Employees where Name not in ('ali','hasan') </br>

Bu sorgu ile isimleri ali ve hasan olmayanları listeledik.

## Like Operatörü
Filtreleme yaparak aramak için kullanırız.</br>

select * from Employees where Name like '%ha%'    </br>
Bu sorgu isimlerinin içerisinde 'ha' geçenleri arar.</br></br>

select * from Employees where Name like 'h%' </br>
Bu sorgu isimleri 'h' ile başlayanları listeler.</br></br>

select * from Employees where Name like '%r' </br>
Bu sorgu isimleri 'r' ile bitenleri listeler.</br>


## Order by ve Concat kullanımı
Order by bize tablolarımızı sıralamamızı sağlar. Concat ile birlikte kullanımı da aşağıdaki gibidir.</br>
select Concat(Name,' ',Surname) as [Adı ve SoyAdı]  from Employees  order by [Adı ve SoyAdı] desc

## Distinct
Distinct komutu farklı kayıtları elde etmemizi, yani tekrar eden kayıtları tekil olarak listelememizi sağlar.</br>
select Distinct DepartmentId from Employees 

## Top
Tablomuzdan kaç kayıt getirmek istiyorsak sorgumuza onu ekleriz.</br>
select top 2 * from Employees 

## Önemli not (Top kullanımı ile ilgili)
select top 2 * from Employees order by Salary asc </br>
Bu yukarıdaki sorguyu çalıştırdığımızda en düşük maaşlı iki kişi geldi. Fakat komutu şöyle yaparsak,</br>
select top 1 * from Employees order by Salary asc => bu komut ile ekrana bir kişi geldi. Fakat maaşı bu maaşa eşit olan biri daha var. İşte bu gibi durumlar için, with ties kullanırız.</br>
select top 1 with ties * from Employees order by Salary asc

## AGGREGATE FUNCTİONS
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
		
