# PRIMERAENTREGA
# Codigo base de datos Modomoda

CREATE SCHEMA ModoModa;

DROP schema Modomoda;

CREATE SCHEMA primeraentrega;

USE primeraentrega;

CREATE TABLE primeraentrega.dimcliente(
					idcliente INT NOT NULL AUTO_INCREMENT PRIMARY KEY ,
                    usuario VARCHAR(255) NOT NULL,
                    contraseña VARCHAR (255) NOT NULL,
                    direccion VARCHAR (255) NOT NULL,
                    edad INT NOT NULL);
                    
CREATE TABLE primeraentrega.dimfecha(
					idfechaventa INT NOT NULL PRIMARY KEY,
                    anio INT NOT NULL,
                    mes INT NOT NULL,
                    semana INT NOT NULL,
                    diasemana VARCHAR(255) NOT NULL);
                    
CREATE TABLE primeraentrega.dimtiendaonliine(
					idtienda INT NOT NULL PRIMARY KEY,
                    razonsocial VARCHAR (255) NOT NULL,
                    codpostal INT NULL,
                    pais VARCHAR (255) NULL,
					direccion VARCHAR(255) NULL);
                    
CREATE TABLE primeraentrega.dimlogistica(
					idlogistica INT NOT NULL PRIMARY KEY,
                    entregaretiro VARCHAR(255) NOT NULL,
                    empresa VARCHAR(255) NULL,
                    precioporkg DECIMAL(10,2) NULL,
                    pesototal DECIMAL(10,2) NULL,
                    preciototal DECIMAL(10,2) NOT NULL);
                    
CREATE TABLE primeraentrega.dimindumentariadehombre(
					idindhombre INT NOT NULL PRIMARY KEY,
                    tipodeprenda VARCHAR (255) NOT NULL,
                    talles VARCHAR(255) NULL,
                    color TEXT(20) NULL,
                    preciototal FLOAT NULL,
                    costovalor FLOAT NULL);
                    
CREATE TABLE primeraentrega.dimindumentariademujer(
					idindmujer INT NOT NULL PRIMARY KEY,
                    tipodeprenda VARCHAR (255) NOT NULL,
                    talles VARCHAR(255) NULL,
                    color TEXT(20) NULL,
                    preciototal FLOAT NULL,
                    costovalor FLOAT NULL);


ALTER TABLE primeraentrega.dimtiendaonliine RENAME primeraentrega.dimtiendaonline;


CREATE TABLE primeraentrega.factventas(
						idventas INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
                        idcliente INT NOT NULL,
                        idtienda INT NOT NULL,
                        idindhombre INT NOT NULL,
                        idindmujer INT NOT NULL,
                        idfechaventa INT NOT NULL,
                        idlogistica INT NOT NULL,
                        preciototal FLOAT NULL,
                        impuesto VARCHAR (255) NULL,
                        formasdepago VARCHAR (255) NULL,
                        ganancia FLOAT NULL,
                        cantidad INT NOT NULL,
							FOREIGN KEY (idcliente) REFERENCES dimcliente (idcliente),
                            FOREIGN KEY (idtienda) REFERENCES dimtiendaonline (idtienda),
                            FOREIGN KEY (idindhombre) REFERENCES dimindumentariadehombre (idindhombre),
                            FOREIGN KEY (idindmujer) REFERENCES dimindumentariademujer (idindmujer),
                            FOREIGN KEY (idfechaventa) REFERENCES dimfecha (idfechaventa),
                            FOREIGN KEY (idlogistica) REFERENCES dimlogistica (idlogistica));
                            
CREATE TABLE primeraentrega.dimdevoluciones(
					iddevoluciones INT NOT NULL PRIMARY KEY,
                    idventas INT NOT NULL,
                    idindmujer INT NOT NULL,
                    idindhombre INT NOT NULL,
                    fechahora DATETIME NULL,
						FOREIGN KEY (idventas) REFERENCES factventas (idventas),
                        FOREIGN KEY (idindmujer) REFERENCES dimindumentariademujer(idindmujer),
                        FOREIGN KEY (idindhombre) REFERENCES dimindumentariadehombre (idindhombre));
			
			
			
ENTREGA TRIGGERS DIA 16/12


/*TRABAJO UTILIZACION TRIGGERS*/

/*tabla 1: FACTVENTAS*/

/* Creacion de la tabla AUDITORIA_VENTAS que muestra fecha, hora y usurario de los registros de cada venta*/
USE PRIMERAENTREGA;
CREATE TABLE AUDITORIA_VENTAS (
		IDVENTAS INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
        USUARIO VARCHAR(255) NOT NULL,
        FECHA DATE,
        HORA VARCHAR (255),
        TIPO_MOVIMIENTO VARCHAR(255));
        
/*creacion del trigger tipo AFTER, en el cual se regsitran las insersiones de ventas*/
CREATE TRIGGER `AFT_INS_VENTA`
    AFTER INSERT ON `FACTVENTAS` FOR EACH ROW
    INSERT INTO `AUDITORIA_VENTAS` (USUARIO,FECHA,HORA,TIPO_MOVIEMIENTO)
    VALUES (USER(),CURDATE(),CURTIME(),'INSERT');
    
/*ejemplo de insercion de venta*/
INSERT INTO FACTVENTAS (IDCLIENTE,IDTIENDA,IDINDHOMBRE,IDINDMUJER,IDFECHAVENTA,IDLOGISTICA,PRECIOTOTAL,IMPUESTO,FORMASDEPAGO,COSTO,CANTIDAD)
VALUES (4,1,112255,110050,2,6,4235,21,'mercadopago',2000,1);

/*creacion del trigger tipo BEFORE, en el cual se registran las eliminacion de ventas*/
CREATE TRIGGER `BEF_DEL_VENTA`
    BEFORE DELETE ON `FACTVENTAS` FOR EACH ROW
    INSERT INTO `AUDITORIA_VENTAS` (USUARIO,FECHA,HORA,TIPO_MOVIEMIENTO)
    VALUES (USER(),CURDATE(),CURTIME(),'DELETE');

/*ejemplo de eliminacion de venta*/
DELETE FROM FACTVENTAS
WHERE IDVENTAS = 8;


/*------------------------------------------------------*/


/*tabla 2: DIMINDHOMBRE*/

/*creacion tabla UPDATE_INDHOMBRE , en la cual se registran todos los movimientos en el que se realizaron modificaciones en la tabla*/
CREATE TABLE UPDATE_INDHOMBRE (
	IDINDHOMBRE INT NOT NULL,
    USUARIO VARCHAR(255),
    FECHA DATE,
    HORA VARCHAR(255));
    
    
/*creacion del triger BEFORE update, en la cual se modifica el nombre de un producto*/
CREATE TRIGGER `BEF_UPDATE_IND_HOMBRE`
    BEFORE UPDATE ON `DIMINDHOMBRE` FOR EACH ROW
    INSERT INTO `UPDATE_INDHOMBRE` (IDINDHOMBRE,USUARIO,FECHA,HORA)
    VALUES (IDINDHOMBRE,USER(),CURDATE(),CURTIME());
    
/*ejemplo de update dentro de la tabla DIMINDHOMBRE*/
UPDATE DIMINDHOMBRE
SET COLOR = 'ROJO'
WHERE IDINDHOMBRE = 112233;

/*creacion del triger BEFORE delete, en el cual se registran los productos eliminados*/
CREATE TRIGGER `BEF_DELETE_IND_HOMBRE`
    BEFORE DELETE ON `DIMINDHOMBRE` FOR EACH ROW
    INSERT INTO `UPDATE_INDHOMBRE` (IDINDHOMBRE,USUARIO,FECHA,HORA)
    VALUES (IDINDHOMBRE,USER(),CURDATE(),CURTIME());
    
/*ejemplo de delete dentro de la tabla DIMINDHOMBRE*/
DELETE FROM DIMINDHOMBRE
WHERE IDINDHOMBRE = 112233;
			

                        
                            



