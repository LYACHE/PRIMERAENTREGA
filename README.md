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
			

                        
                            



