# banco-de-dados3


Crie um banco de dados, adicione tabelas e determine quais são os atributos de cada uma. Em seguida, execute um trigger que se relacione com algum comando, como insert, select, delete ou update.

create database biblioteca;

use database biblioteca;

create table autores (
    autorID serial primary key,
    nome varchar(100),
    nacionalidade varchar(50)
);
<br>
<br>

create table livros (
    livroID serial primary key,
    titulo varchar(255),
    fk_autorID int references autores(autorID),
    anoPublicacao INT
);

<br>
<br>

create table emprestimos (
    emprestimoID serial primary key,
    fk_livroID int references livros(livroID),
    usuario varchar(100),
    dataEmprestimo DATE,
    dataDevolucao DATE
);
<br>
<br>

insert into autores
    (nome, nacionalidade)
values
    ('J.K. Rowling', 'Britânica'),
    ('George Orwell', 'Britânica');
<br>
<br>
insert into livros
    (titulo, fk_autorID, anoPublicacao)
values
    ('Harry Potter e a Pedra Filosofal', 1, 1997),
    ('1984', 2, 1949);
<br>
<br>
insert into emprestimos
    (fk_livroID, usuario, dataEmprestimo, dataDevolucao)
values
    (1, 'Alice', '2023-01-15', '2023-01-22'),
    (2, 'Bob', '2023-02-10', '2023-02-17');
    <br>
    <br>
create table historicoInsercoes (
    historicoID serial primary key,
    fk_livroID int references livros(livrosID),
    dataInsercao timestamp default current_timestamp
);
<br>
<br>

delimiter //

create trigger AfterLivroInserido
after insert on livros
for each row
begin
    insert into historicoInsercoes
    (fk_livroID)
    values
    (new.livroID);
end;
<br>

// delimiter ;

Sempre que um novo livro for inserido na tabela livros, um registro será criado na tabela historicoInsercoes com a fk_livroID e a data e hora da inserção.
