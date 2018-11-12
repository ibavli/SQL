# SQL'DE DOSYA TİPLERİ
SQL'de dosya tipleri ikiye ayrılır. 
1. MDF (Meta Data File)
2. LDF (Log Data File)
Mdf : Veritabanına girilmiş olan verilerin aslını tutar. Yani veritabanına eklediğimiz bilgilerin kendisi tutar. 
Ldf : Log dosyasıdır. Veritabanında yapılan işlemlerin loglarını tutar. (Şu kullanıcı şu ip’den giriş yaptı, şu bilgiyi güncelledi gibi bilgiler yer alır) 
 


# CONSTRAİNTS
Constraint tablolardaki alanlara girilen verilerin kontrollerini yapan ve kısıtlamalar getiren tekniklerin bütünüdür. Constraint ile amaçlanan tamamen veri bütünlüğünü, doğrulamasını ve tutarlılığını sağlamaktır.
Primary key, Foreign Key, Unique Key, Check ve Default, Constraintlerdir.


# TRANSACT SQL (T-SQL)
T-sql, sql dilinin sql servera uyarlanmış halidir. T-sql Sql'i referans almış ve daha gelişmiş bir dildir. T-sql Microsoft Sql Server ile kullanılan bir sorgu dilidir. T-sql dilini genellikle 3 başlık altında inceleriz. Bunları;
1. Data Definition Language (DDL) = (create, alter, drop)
2. Data Manipulation Language (DML) = (select, insert, update, delete)
3. Data Control Language (DCL) = (grant, deny, revoke) 
olarak ayırabiliriz.


# Örneklere geçmeden önce ilk önce veritabanımızı ve tablolarımızı oluşturalım.
create database SQLExercise

create table Employees
(
	EmployeeId int identity(1,1),
	Name nvarchar(50) not null,
	Surname nvarchar(50) not null,
	TcNo nvarchar(11) not null,
	Salary money not null,
	DepartmentId int not null
)

create table Departments
(
	DepartmentId int identity(1,1),
	DepartmanName nvarchar(50) not null
) 

# UNİQUE CONSTRAİNTS
Bir tabloda primary key kullanarak sadece bir tane benzersiz alan elde edebilirsiniz. Primary key tanımlamamızdaki amaç alanı benzersiz yapmak değil bütün alanların o alana bağımlı olmasını sağlamaktır. Unique Key ise alanı sadece benzersiz yapmak için kullanılır. Unique Key Primary Key'in aksine birden fazla olabilir. 
Aşağıdaki kod ile TcNo alanını unique key olarak tanımlıyoruz. 

alter table Employees
add constraint uniqueTc unique (TcNo) 


# FOREİGN KEY CONSTRAİNT
Sql'de ikincil anahtarları, ilişkisel tablolar içerisindeki alanlar için veri tutarlılığını ve bağımlılığını sağlamak amaçlı kullanırız. Foreign key olarak tanımlayacağımız alan için kesinlikle referans alacağı bir alan ve tablo belirtmek zorundayız. Aşağıdaki komut ile Employees ve Departments tablolarıyla DepartmentId'ler üzerinden bir ilişki kuruyoruz.

alter table Employees
   add constraint forkeyDepartmentId
   foreign key(DepartmentId) references Departments(DepartmentId)


# CHECK CONSTRAİNTS
Check Constraint, tablonun ilgili sütununa veri girerken veriyi bizim belirlediğimiz şekilde kontrol eder. Tc kimlikler 11 haneden oluşur. Bunu sql'de ayarlamak ise şöyledir;

alter table Employees
add constraint tcNoCheck check(Len(TcNo) = 11) 

# DEFAULT CONSTRAİNTS
Default constraint, adından da anlaşılacağı üzere bir sütuna değer girilmediğinde o sütun için default bir değer ataması gerçekleştirir. Örneğin Employees tablosuna SalaryDay sütunu ekleyelim ve default değer atayalım.

alter table Employees
add SalaryDay date,
constraint defaultValue default(getdate()) for SalaryDay

# Tabloya veri ekleme (INSERT)
İlk önce Departments daha sonra Employees tablolalarımza veri ekleyelim.

insert Departments (DepartmanName) values ('Software') 
insert Departments (DepartmanName) values ('Accounting') 
insert Departments (DepartmanName) values ('Sales')

insert Employees (Name, Surname, TcNo, Salary, DepartmentId) values ('Ali','Veli','12345678912',4250,1) 
insert Employees (Name, Surname, TcNo, Salary, DepartmentId) values ('Hasan','Hseyin','12345678923',3500,1)    
insert Employees (Name, Surname, TcNo, Salary, DepartmentId) values ('Ömer','Faruk','12345678934',1700,2)   
insert Employees (Name, Surname, TcNo, Salary, DepartmentId) values ('Hatice','Fadime','12345678945',750,3)
insert Employees (Name, Surname, TcNo, Salary, DepartmentId) values ('Ayşe','Fatma','12345678956',750,3)

# ALİASES (TAKMA AD)
Aliases result tablosunda bir tablonun veya sütunun ismini anlık olarak değiştirmek için kullanılır. Özelikle kolon isimlerinin daha anlaşılır olması, tablo isimlerinin kısaltılması için tercih edilir.

select  Name, Surname as [Soy Adı] from Employees

Bu sorguyu çalıştırdığımızda result penceresinde Surname sütunu Soy Adı olarak gelir.

# Concat kullanımı
select Concat(Name,' - ',Surname) as [Adı ve SoyAdı] from Employees
Concat komutu iki ifadeyi birleştirmek için kullanılır. Bu değişiklik sadece result penceresinde olur. Bu kodu çalıştırınca result penceresinde tek sütunlu bir sonuç geldi. Sütunun adı [Adı ve soyadı] içerisindeki değerler ise isim-soyisim şeklinde.


# Where komutu operatörleri
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

# In Operatörü
In operatörü where komutu ile kullanılır. Birden fazla eşitlik durumu yazmak yerine tercih edilir.

select * from Employees where Name in ('ali','hasan') 

Bu sorgu ile isimleri ali ve hasan olanları listeledik.


# Not In Operatörü
In operatörünün tam tersidir. İçinde eşit olmayanları listeler.

select * from Employees where Name not in ('ali','hasan') 

Bu sorgu ile isimleri ali ve hasan olmayanları listeledik.

# Like Operatörü
Filtreleme yaparak aramak için kullanırız.

select * from Employees where Name like '%ha%'  
Bu sorgu isimlerinin içerisinde 'ha' geçenleri arar.

select * from Employees where Name like 'h%' 
Bu sorgu isimleri 'h' ile başlayanları listeler.

select * from Employees where Name like '%r' 
Bu sorgu isimleri 'r' ile bitenleri listeler.


# Order by ve Concat kullanımı
Order by bize tablolarımızı sıralamamızı sağlar. Concat ile birlikte kullanımı da aşağıdaki gibidir.
select Concat(Name,' ',Surname) as [Adı ve SoyAdı]  from Employees  order by [Adı ve SoyAdı] desc

# Distinct
Distinct komutu farklı kayıtları elde etmemizi, yani tekrar eden kayıtları tekil olarak listelememizi sağlar.
select Distinct DepartmentId from Employees 

# Top
Tablomuzdan kaç kayıt getirmek istiyorsak sorgumuza onu ekleriz.
select top 2 * from Employees 

# Önemli not (Top kullanımı ile ilgili)
select top 2 * from Employees order by Salary asc 
Bu yukarıdaki sorguyu çalıştırdığımızda en düşük maaşlı iki kişi geldi. Fakat komutu şöyle yaparsak,
select top 1 * from Employees order by Salary asc => bu komut ile ekrana bir kişi geldi. Fakat maaşı bu maaşa eşit olan biri daha var. İşte bu gibi durumlar için, with ties kullanırız.
select top 1 with ties * from Employees order by Salary asc

# AGGREGATE FUNCTİONS
Aggregate functionlar select ifadesiyle kullanılan geriye tek hücre olarak sonuç dönen fonksiyonlardır.

select COUNT(*) from Employees -- bütün satırları sayar.
select COUNT(EmployeeId) from Employees -- EmployeeId alanına göre null olmayan satırları sayar
select SUM(Salary) from Employees -- maaş verilerini toplar
select AVG(Salary) from Employees -- maaş verilerinin ortalamasını verir 
select Min(Salary) from Employees -- maaş verilerinden minimum olanını verir
select MAX(Salary) from Employees -- maaş verilerinin maximum olanını verir 


#  CASE WHEN
Case When ifadesi select içerisinde koşullu ifadeler için kullanılır. Örneğimizde DepartmentId alanı 1 olanlar için Software, 2 olanlar için Accounting, 3 olanlar için Sales yazmasını istiyoruz.

select
	Name,
	Surname,
	DepartmentId,
		case DepartmentId
			when 1 then 'Software'
			when 2 then 'Accounting'
			when 3 then 'Sales'
		end as [Departmanlar] from Employees 
		
