# Prova-Final-Modelagem-Banco-De_Dados 💻 
Um comercio local entrou em contato solicitando a criação de um sistema para gestão de estoque, pedidos e funcionários. Lembrando que, cada funcionário possui um armário, os armários não são compartilhados e, um pedido pode ter mais de um funcionário. O sistema deve ser feito com MySQL. 

# Criação do banco de dados 

## Entidades:
Produto;<br>
Pedido;<br>
Funcionário;<br>
Armário;<br>
Cliente;<br>

## Atributos:
Produto (SKU, Dimensões, EAN, Preço, Estoque)<br>
Pedido (Data da compra, Previsão de Entrega, pedidoID);<br>
Funcionário (ID, Data de Nascimento, E-mail, Telefone, Endereço)<br>
Armário (Data de ocupação, armarioID)<br>
Cliente (CPF, Idade, E-mail, Telefone, Endereço);<br>

# D.E.R (Diagrama Entidade Relacionamento)
![image](https://github.com/victorsoaresho/Prova-Final-Modelagem-Banco-De_Dados/assets/136899628/bb8db5e9-3dd1-4b06-a7e1-75b0d902a407)

# Modelo Lógico
![Captura de Tela (9)](https://github.com/victorsoaresho/Prova-Final-Modelagem-Banco-De_Dados/assets/136899628/a78f63d5-dc1e-4ea0-b0c0-afa395a65ca9)

# Dados
Criação banco de dados e tabelas:
SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

ALTER SCHEMA `comercial sistema`  DEFAULT CHARACTER SET utf8  DEFAULT COLLATE utf8_general_ci ;

CREATE DATABASE comercial sistema;
USE comercial sistema;

CREATE TABLE IF NOT EXISTS `comercial sistema`.`funcionario` (
  `funcioID` INT(11) NOT NULL AUTO_INCREMENT,
  `DataNascimento` DATE NOT NULL,
  `Rua` VARCHAR(45) NOT NULL,
  `Cidade` VARCHAR(45) NOT NULL,
  `Bairro` VARCHAR(45) NOT NULL,
  `Telefone` VARCHAR(45) GENERATED ALWAYS AS () VIRTUAL,
  `Email` VARCHAR(45) GENERATED ALWAYS AS () VIRTUAL,
  PRIMARY KEY (`funcioID`),
  UNIQUE INDEX `funcioID_UNIQUE` (`funcioID` ASC) VISIBLE)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `comercial sistema`.`Armario` (
  `armarioID` INT(11) NOT NULL AUTO_INCREMENT,
  `DataOcupacao` VARCHAR(45) NOT NULL,
  `Funcionario_funcioID` INT(11) NOT NULL,
  PRIMARY KEY (`armarioID`, `Funcionario_funcioID`),
  INDEX `fk_Armário_Funcionário1_idx` (`Funcionario_funcioID` ASC) VISIBLE,
  CONSTRAINT `fk_Armário_Funcionário1`
    FOREIGN KEY (`Funcionario_funcioID`)
    REFERENCES `comercial sistema`.`funcionario` (`funcioID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `comercial sistema`.`Cliente` (
  `CPF` INT(11) NOT NULL,
  `DataNascimentoCliente` DATE NOT NULL,
  `E-mail` VARCHAR(45) NOT NULL,
  `Telefone` VARCHAR(45) NOT NULL,
  `Rua` VARCHAR(45) NOT NULL,
  `Bairro` VARCHAR(45) NOT NULL,
  `Cidade` VARCHAR(45) NOT NULL,
  `Pedido_pedidoID` INT(11) NOT NULL,
  PRIMARY KEY (`CPF`, `Pedido_pedidoID`),
  INDEX `fk_Cliente_Pedido1_idx` (`Pedido_pedidoID` ASC) VISIBLE,
  CONSTRAINT `fk_Cliente_Pedido1`
    FOREIGN KEY (`Pedido_pedidoID`)
    REFERENCES `comercial sistema`.`Pedido` (`pedidoID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `comercial sistema`.`Pedido` (
  `pedidoID` INT(11) NOT NULL AUTO_INCREMENT,
  `DataCompra` DATE NOT NULL,
  `PrevisaoEntrega` VARCHAR(45) NOT NULL,
  `Produto_SKU` VARCHAR(50) NOT NULL,
  `clienteID` INT(11) NOT NULL,
  PRIMARY KEY (`pedidoID`, `Produto_SKU`, `clienteID`),
  INDEX `fk_Pedido_Produto1_idx` (`Produto_SKU` ASC) VISIBLE,
  INDEX `fk_ClienteID_idx` (`clienteID` ASC) VISIBLE,
  CONSTRAINT `fk_Pedido_Produto1`
    FOREIGN KEY (`Produto_SKU`)
    REFERENCES `comercial sistema`.`Produto` (`SKU`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_ClienteID`
    FOREIGN KEY (`clienteID`)
    REFERENCES `comercial sistema`.`Cliente` (`CPF`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `comercial sistema`.`Produto` (
  `SKU` VARCHAR(50) NOT NULL,
  `EAN` INT(11) NOT NULL,
  `Estoque` INT(11) NOT NULL,
  `Altura` FLOAT(11) NOT NULL,
  `Largura` FLOAT(11) NOT NULL,
  `Comprimento` FLOAT(11) NOT NULL,
  `Preço` FLOAT(11) NOT NULL,
  PRIMARY KEY (`SKU`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

CREATE TABLE IF NOT EXISTS `comercial sistema`.`Funcionario_has_Cliente` (
  `Funcionario_funcioID` INT(11) NOT NULL,
  `Cliente_CPF` INT(11) NOT NULL,
  PRIMARY KEY (`Funcionario_funcioID`, `Cliente_CPF`),
  INDEX `fk_Funcionário_has_Cliente_Cliente1_idx` (`Cliente_CPF` ASC) VISIBLE,
  INDEX `fk_Funcionário_has_Cliente_Funcionário1_idx` (`Funcionario_funcioID` ASC) VISIBLE,
  CONSTRAINT `fk_Funcionário_has_Cliente_Funcionário1`
    FOREIGN KEY (`Funcionario_funcioID`)
    REFERENCES `comercial sistema`.`funcionario` (`funcioID`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_Funcionário_has_Cliente_Cliente1`
    FOREIGN KEY (`Cliente_CPF`)
    REFERENCES `comercial sistema`.`Cliente` (`CPF`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;






