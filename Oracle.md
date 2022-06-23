Ceate table tblCategory(
CategoryID int not null Primary Key,
CategoryName nvarchar2(50),
Cat_Desc nvarchar2(50)
);
--=============
Create Table tblBook(
BookID int not null Primary Key,
BookTitle nvarchar2(50) not null,
BookPage int,
BookType nvarchar2(30),
PublishDate date,
Publisher nvarchar2(50),
CategoryID int,
constraint fk_Cat_Book Foreign Key(CategoryID) Refernces tblCategory(CategoryID)
);