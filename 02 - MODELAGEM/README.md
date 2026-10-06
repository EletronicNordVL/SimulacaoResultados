# 02 - Modelagem do Banco de Dados

Nesta pasta estão localizados os diagramas e especificações do modelo relacional construídos para o banco de dados .

## 🛠️ Ferramentas Utilizadas
* **brModelo**: Utilizado para a criação e visualização do modelo lógico.
* **Bloco de Notas / Editor de Texto**: Para documentação da estrutura DDL física.

## 📄 Conteúdo da Pasta
* ****: Diagrama de Entidade-Relacionamento (DER) lógico exportado do **brModelo**, apresentando as tabelas ,  e , seus atributos, chaves e cardinalidades (1:N).
* ****: Documentação da estrutura física com comandos DDL, definindo os tipos de dados (, , , ) e integridade referencial.

## 🏗️ Estrutura do Modelo
* ****: Armazena os metadados da simulação (data, quantidade de corpos inicial, número de interações e tempo).
* ****: Mapeia cada etapa/interação de uma simulação (possui restrição  para evitar duplicidade).
* ****: Registra as propriedades físicas e dinâmicas (massa, coordenadas X/Y, velocidades X/Y e densidade) dos corpos celestes em cada resultado.
