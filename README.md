CREATE DATABASE MatingDog;


USE MatingDog;


CREATE TABLE Raca (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL
);

CREATE TABLE Usuario (
    IdUsuario INT IDENTITY(1,1) PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL,
    Email VARCHAR(150) NOT NULL UNIQUE,
    Senha VARCHAR(255) NOT NULL,
    Telefone VARCHAR(20),
    DataNascimento DATE,
    DataCadastro DATETIME DEFAULT GETDATE(),
    FotoPerfil VARCHAR(255)
);


CREATE TABLE FichaAnimal (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    Nome VARCHAR(200),
    Idade INT,
    IdentidadeAnimal INT,
    Sexo VARCHAR(1),
    CorPelagem VARCHAR(100),
    TeveNinhada VARCHAR(1),
    Altura INT,
    Peso INT,
    IdUsuario INT NOT NULL,

    FOREIGN KEY (IdUsuario)
        REFERENCES Usuario(IdUsuario)
);



CREATE TABLE Localizacao(
    IdLocalizacao INT IDENTITY(1,1) PRIMARY KEY,
   IdUsuario INT NOT NULL,
   Coordenadas GEOGRAPHY NOT NULL,
   DataAtualizacao DATETIME2 NOT NULL DEFAULT GETDATE(),

     CONSTRAINT FK_Localizacao_Usuario
 FOREIGN KEY (IdUsuario)
 REFERENCES Usuario(IdUsuario)
);

CREATE TABLE Curtida (
    IdCurtida INT IDENTITY(1,1) PRIMARY KEY,
    IdAnimalOrigem INT NOT NULL,
    IdAnimalDestino INT NOT NULL,
    DataCurtida DATETIME2 NOT NULL DEFAULT GETDATE(),

    FOREIGN KEY (IdAnimalOrigem)
        REFERENCES FichaAnimal(Id),

    FOREIGN KEY (IdAnimalDestino)
        REFERENCES FichaAnimal(Id)
);


CREATE TABLE Bloqueio(
    IdBloqueio INT IDENTITY(1,1) PRIMARY KEY,
    IdUsuarioOrigem INT NOT NULL,
    idUsuarioBloqueado INT NOT NULL,
    DataBloqueio DATETIME2 NOT NULL DEFAULT GETDATE(),

    FOREIGN KEY (IdUsuarioOrigem)
        REFERENCES Usuario(IdUsuario),

    FOREIGN KEY (IdUsuarioBloqueado)
        REFERENCES Usuario(IdUsuario),

    CONSTRAINT CK_Bloqueio_UsuarioDiferentes
        CHECK (IdUsuarioOrigem <> IdUsuarioBloqueado),

    CONSTRAINT UQ_Bloqueio
        UNIQUE (IdUsuarioOrigem, IdUsuarioBloqueado)
);

CREATE TABLE Notificacao (
    IdNotificacao INT IDENTITY(1,1) PRIMARY KEY,
    IdUsuario INT NOT NULL,
    Tipo VARCHAR(50) NOT NULL,
    Mensagem VARCHAR(500) NOT NULL,
    Lida BIT NOT NULL DEFAULT 0,
    DataCriacao DATETIME2 NOT NULL DEFAULT GETDATE(),

    FOREIGN KEY (IdUsuario)
        REFERENCES Usuario(IdUsuario)
);

CREATE TABLE Encontro (
    IdEncontro INT IDENTITY(1,1) PRIMARY KEY
);


CREATE TABLE Avaliacao (
    IdAvaliacao INT IDENTITY(1,1) PRIMARY KEY,
    IdEncontro INT NOT NULL,
    IdUsuario INT NOT NULL,
    Nota INT NOT NULL,
    Comentario VARCHAR(1000),
    DataAvaliacao DATETIME2 NOT NULL DEFAULT GETDATE(),

    FOREIGN KEY (IdEncontro)
        REFERENCES Encontro(IdEncontro),

    FOREIGN KEY (IdUsuario)
        REFERENCES Usuario(IdUsuario)
);


CREATE TABLE Suporte(

);


CREATE TABLE Chat(
    IdChat VARCHAR(1000)
);


CREATE TABLE Mensagem(

);


INSERT INTO Usuario
    (Nome, Email, SenhaHash, Telefone, DataNascimento)
VALUES
    ('Joao Teste', 'joao@teste.com', 'senha_hash_teste', '11992222123', '23-01-2000');
