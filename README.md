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
	DepartmanId int not null
)

create table Departments
(
	DepartmanId int identity(1,1),
	DepartmanAdi nvarchar(50) not null
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
   foreign key(DepartmanId) references Departments(DepartmanId)

