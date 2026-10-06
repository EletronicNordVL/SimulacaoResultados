/* SCRIPT DO BANCO DE DADOS */
DROP DATABASE IF EXISTS UniversoN1;
CREATE DATABASE UniversoN1;
USE UniversoN1;

CREATE TABLE Simulacao (
    NumSimulacao INT PRIMARY KEY AUTO_INCREMENT,
    DataSimulacao DATE NOT NULL,
    QtdCorposInicial INT NOT NULL,
    NumInteracoes INT NOT NULL,
    TempoInteracoes INT NOT NULL
);

CREATE TABLE Resultados (
    NumResultado INT PRIMARY KEY AUTO_INCREMENT,
    NumSimulacao INT NOT NULL,
    NumInteracao INT NOT NULL,
    FOREIGN KEY (NumSimulacao) REFERENCES Simulacao(NumSimulacao),
    UNIQUE (NumSimulacao, NumInteracao)
);

CREATE TABLE Corpos (
    NumCorpo INT PRIMARY KEY AUTO_INCREMENT,
    NumResultado INT NOT NULL,
    NomeCorpo VARCHAR(50) NOT NULL,
    MassaCorpo FLOAT NOT NULL,
    PosX FLOAT NOT NULL,
    PosY FLOAT NOT NULL,
    VelX FLOAT NOT NULL,
    VelY FLOAT NOT NULL,
    DensidadeCorpo FLOAT NOT NULL,
    FOREIGN KEY (NumResultado) REFERENCES Resultados(NumResultado)
);

/* Inserir dados de Testes: */
/* 1. Inserir dados na tabela Simulacao */
INSERT INTO Simulacao (DataSimulacao, QtdCorposInicial, NumInteracoes, TempoInteracoes) 
VALUES ('2025-09-24', 3, 5, 10);

/* 2. Inserir dados na tabela Resultados (atrelados à Simulacao ID 1) */
INSERT INTO Resultados (NumSimulacao, NumInteracao) VALUES 
(1, 1),
(1, 2),
(1, 5); -- Última interação (para os SELECTs 'e' e 'f')

/* 3. Inserir corpos no primeiro resultado (NumResultado = 1) */
INSERT INTO Corpos (NumResultado, NomeCorpo, MassaCorpo, PosX, PosY, VelX, VelY, DensidadeCorpo) VALUES 
(1, 'Terra', 5.97, 0.0, 0.0, 0.0, 30.0, 5.51),
(1, 'Lua', 0.073, 0.384, 0.0, 0.0, 1.0, 3.34);

/* 3b. Inserir corpos no segundo resultado (NumResultado = 2) */
INSERT INTO Corpos (NumResultado, NomeCorpo, MassaCorpo, PosX, PosY, VelX, VelY, DensidadeCorpo) VALUES 
(2, 'Terra', 5.97, 5.2, 6.0, 0.6, 29.9, 5.51),
(2, 'Lua', 0.073, 5.6, 6.1, 0.5, 0.95, 3.34);

/* 4. Inserir corpos no último resultado da simulação (NumResultado = 3) */
INSERT INTO Corpos (NumResultado, NomeCorpo, MassaCorpo, PosX, PosY, VelX, VelY, DensidadeCorpo) VALUES 
(3, 'Terra', 5.97, 10.5, 12.0, 1.2, 29.8, 5.51),
(3, 'Lua', 0.073, 10.9, 12.1, 1.1, 0.9, 3.34);

/* SEÇÃO DOS SELECTS */
/* A) Listar todos os resultados de uma determinada simulação; */
SELECT * FROM Resultados WHERE NumSimulacao = 1;

/* B) Dado um determinado resultado, listar os corpos do resultado; */
SELECT * FROM Corpos WHERE NumResultado = 2;

/* C) Listar todos os resultados de uma simulação, com os respectivos corpos; */
SELECT r.NumResultado, r.NumInteracao, c.NomeCorpo, c.MassaCorpo, c.PosX, c.PosY
FROM Resultados r
JOIN Corpos c ON r.NumResultado = c.NumResultado
WHERE r.NumSimulacao = 1
ORDER BY r.NumInteracao;

/* D) Informar a quantidade de resultados para uma determinada simulação; */
SELECT COUNT(*) AS qtd_resultados FROM Resultados WHERE NumSimulacao = 1;

/* E) Informar a quantidade final de corpos de uma simulação; */
SELECT COUNT(*) AS qtd_corpos_final
FROM Corpos c
JOIN Resultados r ON c.NumResultado = r.NumResultado
WHERE r.NumSimulacao = 1
AND r.NumInteracao = (SELECT MAX(NumInteracao) FROM Resultados WHERE NumSimulacao = 1);

/* F) Listar os corpos do último resultado de uma simulação. */
SELECT c.NomeCorpo, c.MassaCorpo, c.PosX, c.PosY, c.VelX, c.VelY, c.DensidadeCorpo
FROM Corpos c
JOIN Resultados r ON c.NumResultado = r.NumResultado
WHERE r.NumSimulacao = 1
AND r.NumInteracao = (SELECT MAX(NumInteracao) FROM Resultados WHERE NumSimulacao = 1);