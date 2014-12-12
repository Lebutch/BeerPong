

#BeerPong // Projet final d'électro  
##Documentation


**Concept**

Automatic Beer Pong launcher, à pour objectif de pouvoir lancer automatiquement 6 balles de Beer Pong dans 6 verres en une seule séquence.  Contrôlé par arduino et par une patch MAX,  le lanceur alterne entre différentes données pré programmer pour être en mesure de rentrer la balle à toute les fois.

Pour s’assurer que la machine soit toujours efficace, sous chaque verre, il y a un capteur de vélocité. Ces capteurs permettent d’envoyer une rétroaction au lanceur.  Lorsqu’une balle entre dans un verre et que le verre est ensuite retiré pour être bu, le capteur aura une vélocité de plus ou moins zéro, ce qui enverra comme message au lanceur de ne plus essayer de lancer la balle à cette endroit.  Une fois tous les verres retirés, l’utilisateur peut redémarrer la machiner avec l’application OSC pour iPhone ou iPad.

**Fonctionnement**

Comme illustrer dans les images et les vidéos du making of, nous avons été capable de faire fonctionner la table et le lanceur parfaitement.  La machine arrivait à entrer les balles un peu plus de 3 fois sur 5.  

Nous avons eu un problème lors du montage de la table.  En finalisant la décoration de la table et en rendant notre réseau de câbles plus minimaliste et plus efficace, nous avons accroché le moteur, ce qui à abimer le roulement à l’intérieur.  Depuis ce moment, il y a un petit délai lors de la monté du bras du lanceur ce qui enlève de la force au lancer.  La balle n’atteint plus les coordonnées désirés.  Nous avons essayer de changer l’angle du moteur, démontrer le moteur et le remonter pour s’assurer que tout était bien à sa place et nous avons aussi essayer de changer l’angle de départ pour réussir tout de même à atteindre les verres.  Malheureusement, rien de ces solutions n’ont plus résoudre notre problème.
Making Of  

![](image/1.JPG)
![](image/2.JPG)
![](image/4.JPG)
![](image/6.JPG)
![](image/7.JPG)

###Code arduino

```
// Pour contrôler les capteurs de pression

/* 
 * Arduino2Max
 * Send pin values from Arduino to MAX/MSP
 * 
 * Arduino2Max.pde
 * ------------ 
 * This version: .5, November 29, 2010
 * ------------
 * Copyleft: use as you like
 * by Daniel Jolliffe
 * Based on a sketch and patch by Thomas Ouellet Fredericks tof.danslchamp.org
 * 
 */

int x = 0; // a place to hold pin values
int ledpin = 13;

void setup()
{
  Serial.begin(9600); // 115200 is the default Arduino Bluetooth speed
  digitalWrite(13,HIGH); ///startup blink
  delay(600);
  digitalWrite(13,LOW);
  pinMode(13,INPUT);
}

void loop()
{ 

  if (Serial.available() > 0){ // Check serial buffer for characters

    if (Serial.read() == 'r') { // If an 'r' is received then read the pins

      for (int pin= 0; pin<=5; pin++){ // Read and send analog pins 0-5
        x = analogRead(pin);
        sendValue (x);
      }

      for (int pin= 2; pin<=13; pin++){ // Read and send digital pins 2-13
        x = digitalRead(pin);
        sendValue (x);
      }

      Serial.println(); // Send a carriage returnt to mark end of pin data. 
      delay (5); // add a delay to prevent crashing/overloading of the serial port

    }

  }
}

void sendValue (int x){ // function to send the pin value followed by a "space". 
  Serial.print(x);
  Serial.write(32); 
}
```
//Pour contrôler les moteurs.
```
#include <Massenger.h>
#include <Servo.h>
#include <Servo.h>

//Creer une instance de la logitheque
Servo myservo;
Servo myservo2;
Massenger massenger = Massenger( &Serial , massageReceived);

int angle = 500;
int angle2 = 500;

void setup()
{
  //Activer la fonction servo de la broche 9
  myservo.attach(9);
  while(!Serial){
  }
   //Activer la fonction servo de la broche 10
  myservo2.attach(10);
  while(!Serial){
  }
  Serial.println("code entrer");
}

void loop() {

  massenger.update();
  //Tourner le servomoteur selon l'angle
  //qui incrÃ©mente
}

void massageReceived() {
  Serial.println("massageReceived");

  if ( massenger.checkAddr("A") ) { // If the massage starts with "pwm9"...
    int value = massenger.getInt(); // ...read the value of the data as an int...
    myservo.write(value);
  }
  if ( massenger.checkAddr("B") ) { // If the massage starts with "pwm9"...
    int value = massenger.getInt(); // ...read the value of the data as an int...
    myservo2.write(value);
  }

}
```
###Code MAX MSP


**Patch principal**

```
----------begin_max5_patcher----------
15427.3oc68ssjiibkXO2yWAbsJzkHplNueQ1qCMxwZ60gGqIzH6ccnYiNPU
DUUbZVDzfjcOsTrePq+MzOlyDI.H.IAYBBjIRzE6QhUA.RV44Rdxy8ye8ad2
cOj9yIatK52F8mid269qey6dW9sz23cEW+t6dM9mebY7l721culrYS7yI2cu
4YaS94s42mRKu0pculta6xjs4e.XwcWLO+sk9vO8dX0a07919k0Ilkvc2E8u
T7nMa+xx76dWsu2EqJ+ZQE2bc71GeYwpm+PVxiaMeID.cF39HJAq+ARNipdE
MCD8un+H+qey2ne49dBtPDvd3E6T3EQygWhLGdwvbndng2MaSWaM.iARmBwb
YNDW.3Xbuf3UIeVslOBfmmrL9KQX..zAvl2JX+P7pmGDPGMigkDjh+FoAalz
E.+xz34ZNA0dot.9r9S0gmAzgBMrJvT8NZAsW.9C61tMck0vFRNPj1KCePQ9
OPjxWGRB6hmh90+hEv+9+9H3uI5W9K0WfTWfJu.qt.WdAQcAo7Bp5BZ4EL0E
reSz1WRVEA6.Rb.DDxNCBTI6ii4.FTgEg7bghTgK1enPiJrXjFMZPBrnjkaR
hp1rrbwpjGS2sJ+sSrGAgb5NHJTIpjAERE9gSlAkJziRPBXFiQ4bEVi4Z7Dc
fvSPmhmHJYqBIEAvmBQ4C7DYfvS.mhmvRC+Dc3Yn5p7Ygv4xmqs6QxlQQbIj
0a4zaSe94kImGNWGmE+Zx1jrOjrJ9AypGbJbP6ZcsPy4L.n.rbFGwYZIrCGN
nyzZlyo00k.DnDatqI1DtxzIFPBwiJwl3bhcoXLT3tyl5bhMzrydTI0X2Spw
JnSJY7gDNs8Pc7vbntvsJIVfh3.3gGpC7fhOnABG4VEDQb9LHixfxgDG008K
T2aSZAfREhwTv.k5d.UY7MgQ.zwDNIPeBmP4P3ngA7XNHG45i4vpMmCt7+AE
E.cNJ.fcvV5AEG.bMN.QHC+t8yeD2+wHH.T3wrA4LNEMzs5APlwHBJjnvQHz
LNCK47deF2PxmvHN2dWkkAXrj.U.th7wlQ...hDT6VXXmaaf.LiIHBNMfwBd
3nC4LLBSCZdAme5AhJlgThE.gLufyO+.Rnyj.rIRCAJVfJb9Nh8mPLjHg1BA
MHp5+5PpFvbZjnob5LtjSzfMFxlIUFBJUJVwDyX5+QF9XO8SoKzwa52sMawy
OmjsI581GANI0oAfqlwEXHVgXTVEqzqBq1sfXXFDN7XirnGRY1C+j9C+.qhP
.iYbJPyvg3Bn297tYHB+JvNeleL36HnmXOzibJzW2Ix9i3isG7gtE726XUFM
2hJsHQswBp+4pM9H6gdfSg95tI6.v2MPt8h7ER2B46E46b.eS6x5OgBP6iH7
PY+m5XLhxJXs3chy.Pp8.H+tA1zNu.fD6AP1cCrUadA.w1Cfz6FXCxHxJsvc
G.hrG.I2Mv1Z4E.DZO.htaXMiZ3fuVyb4HTDNhDQirWuYtayf48I0ZQZ6w3N
Lq8FbOwx3tMq8pcHz.5IVeijvtMk8pcP1AHItT+uIDlxwIsWsSDmtrST2tmq
9op0QRJwyPM2DeBgoHNESU+34FXJgKXmxhxhWMO8UqO4h3VO9T6jq7irnNob
L1F8PzxnkmFnwE28ozUa2r3ujCGPcsgbLtnRDr98tJ9UCL+sYKhWd9DO3dMl
Yfphi8XLlodNbBJ6urb1xjVR8BzIPMmw4P2OLJ4.A7JH2j4EDnqf7zzOtas0
.Oz8.+dpNhkC0Btif8mVrbaRl8vNv8vNthtaJIQR+JcoGSe80D0g.GoXOFFs
IY0lzrMQnjebEDAidMcaxtrHXRVz5zrs+3pSdVB5rl6bYAKHxkErzCEVf.sA
Rjbc3vJsTDPFSasqQiEc4+M7E9G8RE91kwJxqUZ6fHrQBppEJBVNiYxPQlCq
UxA.kwGUTFhOSJLnLlhqRogLO3wXrwDiQojYXAiZTGVIUaJfwniIFCw3yTlp
xyqoBc4KqNYPKKKvwY3QDmI0GZJ4BrR7ufPlQmFnL3XxlwDDkNGlMlB9fgy5
b9LCbeQSWoXovj0IfxWGVmBeNyN6fhRXtK0Tp4o9bX0o9P23o7gBovbKRo94
5LVtaIBbDB0oHjFGaSjSADhSMv.xjnJwkR.au3x.GqfcJVogtJhZ5pD3XEjK
wJMzFg6ZVkrH84jr9pKBBeMZiLD9GEpMXubqEimeDM0gnJZ+QUfwBUgfT0g2
l8aEmS4RTEo2nJjbjPUBc7gK1DxTp4XrT2kHKb+QVhwhuhpLZRoikDKztdDN
iPUZHRcK9B0e7EejvWRcLqJXtHLpq2FB6OlhMVbVPNVoYsQhEhCbGpZy4NGr
C5Ffbph0ThRe.NTxz4tCblPcVhhGhhcGFg1eLhS0rVKtgKT3En2PIj9iRbpZ
0XptJQDJoLdCkf6OJwo5TiTRbQPDAi8FJA0eTBzsljpr8BQkXh2PIv9iR.tD
kr2SeCGBosbjr+VbAcptwmw2uBYUj1KJscGlP9IKU+ci.QU4T5owa7Nf238H
GUr8mVim41vNh019ar.QvmoN+GPoNPaQKx6IMJ6o3GS539V30DRrtTCo1kXH
ngvj2qu+EaChhLVapwzJunvwnJuNgwALxBORHKntiLUE8qZQLLnwVnwBawH7
oGuEbrvVToBCQQDstG.Ygb+fFUAFITkDxzXJJEpUSCT3jtPFUAFKc1f.HsBW
gAU8SN2zY6KRz7+S+8QHcw4Zx0bipQ8EA5TQ9mMFuTzLRQLd0y+gh.eCI8LM
S285CWJYROW+3vFb1UoSw8CWFTz.2g4rlMyvg2Vyus21YBba3wavKsOIJPb2
TNdPQ+22wFMAW0wUjJbkVMhgm24wkoO9wjLcAx.5ON6ZLz7okowaGbDGVIkp
XSGWNoj3CkhwRhOQxphpEUTpIV3KvGJkis.+FnNprL7ogq7dm5WwFnCFsLKl
CYw8il0OMPUUI78TPZOZTk12XGWksPSNg8ik58Rgtq8VjuGJs6oSFY8isx80
wbTkt8fvVTOT5TU6avGUk2PgqjdnbrTruAlhThoBeA8P4npVeisaZwSSR47h
wRodgfVg9D.Qkqnm.R5EisV80wcbMGWAtKfk06T05avKUqDnBYo8ikd8MvU0
CX1DPd+npXeiMc5cZEHtImD+Qyw80S5cIbuXqofH+Q2y80QdBgrZea3JyW3V
W22fapVIKEvB8Eilu6afrpUK5S.o9hw048M12w.U66lbh8QikX+5kElf.lTZ
5CGaw9MPdP3DPUetzsIKcMDhDylB55yEiV9dUGYUqXmm.h84rQUrecLGWfFJ
s86XW0PIhz4cUilfJfueGU+ZuFcFTc+T8twg4iInJcNn1vZU.XzfT2O9tkLx
d+vBvkgz0yvo6mZoJYA5zHon1dqxBBeCnteDc2nTSqkgjtpc+766sJODmVxb
Pc2DEUzoeD3Zt2T3Fc.G.DhaKXNLTiPJrIaZfPbaZsPghpbNntIVgKBA6VqD
P.Q0VFFrL3bAL5P31zdRgN.TFijiN.gO5vssNMjDW0siHfprf1M3ir1vGcvX
H7n01GTXixAVBQRmoNalR0C5PVn59AG2O0.DVUt7VOr0HdvhPHdqygMQPHts
UpUWM8IBBwwpuxPUMpRpB4LH4bqKQGNV4UcPDJNKdRfNbqpqHcaiAWVQR3py
h4t4r3us2mEOZcBMk5IbIBleTLoJ00cz7EHIe5q77e6eKo2LPHGGGXJdOCjZ
ITVcMhd4sfViG.h0eNnwKLv0T7EKpvU8Lb.mI1Ipuap5TvAHd.tsGwgrUBEQ
ThzD8KBbqiWkrz5YOHDIFhsJJopUPxdGqAMF9j+iAUJBJY3Dh.7mNJU9KvYx
P5eLEQiVcAU20J6KFOmJBALPhPFutTRc1qpbE2yRPFh4ypR608.RUOwn2BPN
K4GMPmf.GsSPp6z.c9ZWZRXOY.ZSrKtRrazipmm0eouPlyUgqTsDNgrOwPUb
Xz.UItQq+PorOkTEdr5NXvkhfQCjH3wq0GU2OU9cOn9V8dCHva9rp9FPIzQJ
.06drMDLZIUUifsA4Ne+2.ns330zPTm8UMPV7lvpARgA.czp8956GE6y5SGI
rZ.sSCf8myScucZ8dtIHGMoT0C4KUfmNloIGM0zavbMTVo0JJq2GARFMg5Td
UPgYDriKEI8zq87I4uEnJvUlG+Vitv142dQNWESOyzYNQT9O7O7+7G9C+wen
uxwca9nfIU7OBc2PTK6f0KQ3OsLU8G2k0Bx0kBFWaof7TZ1qw4XK1YlAER7L
LVRfbSIAR.Rv.zM+7.tjFb3RkogyXB09S5TCWRBNbox1nYXDlN83KwAGtDQU
FapjUBlb7knfCWBIzYR.1XK5jBWBCu830NBe3PksnB35n3r46VrJ8691+4Si
mY1iKuZUAO7+O.ozQcrHNWQHci6RRnD8fkxfFq+oSxJPdEXOEPrXYxmRx1rH
cUMZ36tKd85Z29c09HZT9OklUajWnukB4lUawptUVxmVT942e23LELrUA.6x
Lf7OyJO.R+0jNOIa0tE4KEyMUD+uo7KrBrysW2TJcBrYJ8xA6AVEaxy40R37
5+wuKccxpEqVmkrQo4a71CVa2MO4o3cK29gSyCz74GL4LZ9vSxT7t6dNaw7z
U5EQCLs91k+4z1FkGILZcfI+crJd8I9vJNREVokGtQAj617PbllPTHz.U9vs
ooKa9npO2xjm1V730KVs5.r31z0s+vrEO+xY9rOjpd3qm66N+Ia9vtUlm9A0
V6seXS7mZhs2FubYwd8le8+b7pEJgOIaWXHAHP0CMBNeYyiYoKW1.dMO4Sm3
IyU7vOl74Ey29R9en5LCp29h0kLQ2UQkmu34jMaaduswOuo4cNZKu5V6dnXO
5G1l755kJnn4aPAcIaLbIuqwFx5REab+CjNZDKUsaqMy6N1mb0+HGuxa6ffV
UUlmuikPMkBKs70JFd8mqz9z7+Pk2N+zfBgB9EGHGXTfI8DmTn.wvhBvBxjC
EvGXT.AL4PAjgEEnm6XSMT.dfQAloX03fBZnv7g4575zrsKV8TZ6Xn5fVc0l
jyZA2Qu6fOwo0U57td9LH9WVLedSsfZSc5Kb5DDkSUn4BozAJxXOx.g12oHl
6ZGsharDa0xuJ50ioKMZe+m0pnP3.p79bkUTuHvRDTYL.7.NpSoEq0jQVOHi
5gA2gl.YT6L9gjkOtbwie7P5WG2joru30BEkt62ubWhRqxsu79+wUpMQJZ86
+dEas9u985WV78ujpzyL4OrbwmVjj89+oEYIKUpYU978e9uSYaxqU2WoWs1X
kWgDJ7D2DVcSZwOYE+j2.lOCCnYbRCI4hEzc6tFevGdVY41xbB+GJQt2Y3CZ
4ck+JrjKAyYDNTyfnNHFqMfDLibLWxQeAnxu.jDjO8hUeA.o.PL+l5VPK9VJ
r0IGxp+Rqep3UOWX+BGzfor9aZcVZtHqB65lgk9RRYLY.kQBwNVHYmDGVTHM
EQwrFyn2vszgD2RBIbqt5sFWbKdHwsz.B2VXdwXhaQCItkER3VicKiItENj3
Vd.gaKLHZLwsfgD2JBIbaw7B1s315cUx1inyUpa+80d4TZzW425lZIsMawlG
iK9CLS1ho885rwCiVzYnYO7bCfRvT+ie+I+sCgwV04WAgM9R0wgu9qG9E0N6
AlXrGuV2hptAlG9GRXVuH8qnRMnuwOo4mnuw4mLhxuwOMT7Sr217SEpccieZ
n3m3uw4mLlHcieZn3mDus4mJbUzM9ogheR9Fmex3dr.ieZa5yZWQakIxWhep
dfw.WLnIc0Z3KE2Qhon6I4wYvj3cTv.isZVlAGzbo1FmYYzWaIkrNDGBseCy
PD2VS.ZPnCYO8D96OlDOWsjzy3inu0jei+pMQwqhWl9bz5Eq1Ln3VTq31+2I
YyU+UGXrKK28LXVQO0zDVbn+bDVUNi9CIZdmLEx9bGiz4PnRwCJyJxpM6Hnw
CXlImxQAU7fbDsIlq0bEss7E8fO9oxazVxcz1yezKmCoskGoMxkzCxmTXNxg
Xh0ONOBiQBJqI14zIUpEIVpMIW5ESvTKSxzyjno1kroWHgSuXRmdwDO8BIe5
kS.0KlDpVjHp1jLpcIgTOSRodwDS87Im54SP0ymjpmMQUaKYUOcBq1tXnKm3
pmN4UOT5vghrO54WRz8gww3kjkqO7wspp14kaeTTMt6Dex12Pd4fabFz64Bx
QqizhhTHwz9MLIHeybXpw4mMOC0wDlM4oU1ljk5U6YIOfqk7f5N4wRb+gbLM
s7PaxACB4LpIuZ.XI5HKcN6TqHWExbgxLRHQzxrlnc06ovgzdpAjthMZtZ1Q
FVT0MmJCOGxMhDusQ7Tp7ZBvKwnqKMnv7qi1nv7MrmXH2KQGy8RNTFYIIMWw
4BWVXHom5y1zPlioXm0flyYTyI9pZy3lyXfy4MxwNCcNmwNGYvygF8jebivX
5C2nofPbLxrc6drz1Gas+wJaf5fcPWvVH6sGxBahrxtHqrMxB6iryFIqrSxR
akr0dotZyzEraxJamtr8SW1FpKaG0Esk5b1S0tMUmWvpc1V0t8UmRP0oNI6j
uuKUrL1orvkLc5BHfKc5R696x3mqptBywgN370VyI0O3XcDFRLbNPdYDL77H
Xzo9Ft7o6ckN.5Bc.ilPjgSp51ApukknDuOOZiRjjZaYD+5nZW3rvKXfjcJ1
4HxO7jDvqRQuVMbBXTYWVTOV7iCvxzf6f4CtCxaLtCToozfIN2A0GbGz2ZbG
lycJhs8Dl6f3CtC1aLtCnIw2fP4Dm6.6CtC9aMtCB1n2Aehycf7A2g3sF2Qg
VoFGlMg4Nf9f6P9Fi6nPoTxT2jEfOXNff2ZZkZ3NPSMSV1sZc7ieLZSy+6xr
Hh9yh.GRVj6Oy+exv9HLsIHIdpv8joHDI4QZ9xbLn9ywfFZNl9xYf7EmgoAR
EHLFGm3xGyY7cwqheNQIJIOCaizDnn4waimE8OD+3K4rLla9q2sNZapRdRzh
UQaeYwlnGi2j7ahTmZsL9wDcROGketU9iUeIYoqe+7zOuJR2mkhReJ+tqiyT
qnnb7WzuNuIAo+.6eypmGYJWjj4Qfn2Gw+MyNE.nmn.Olty3qZxUEQjipjjN
xDzwME8v2wCerYfLyIeEYFCMOBzLjkLrGGYVM4vxPekuZxGHDVwQuIcW1ikB
CJzmM5zDj4Ia1tXUU7u+yUQ6pk2+9FUFXfkOLewFcjNMwlcPj.zWrl3FV6Jv
Z7aXsq.qwtg0tBrF8FV6JvZjaXsq.qgug0tBrFpqXMzMz14QCsf2twtUgFfc
.uIug1JQanNf1D2PaknMbGPa7ansRzFoCnM1MzVIZi1AzF8FZqDsw5.ZibCs
Uh13c.sgug1NudXsn3F3Fd67ngoq4UG3X3Cqpo6zkVx7OXJ+lODuca1hG1s0
3S3yf+5W4b77xzGhWVTbFUtQuK01w9BB4aZG9Go5SrHe.NQT6FpZTj8UZMJZ
hpAFjGiCSA36spNUw0uUwK2IB1YxzuiQ3G0Qw5CR+jHOSMhXZEKEYHguvdmN
jk0FM1Iayii3l3WWuLIJKdaxuM5gjseNIYUjtNR+RDj9pNXjyKuD.dcyLGUb
0hwr3pwRStrvJlfAdt5pOrm9c4TI3R8hsSGU1y0e+rfZ0Za96R4I2UIL79tu
+7567eWlqoic.vKHR.xMMWRZq07c25DfNj6byxEy6Zo8aC2YZ1h5kX8YDjeT
l00uiNUD47+fziXvuR46ko.WQOd6DEc9ckapDrYfwWenkowyy2a4n9IzPzPg
b8YyHNs1YyEMOx.oeZLOYY7WNpXYFLxCZrIOHKHO.SG9yjbME0vvsCjuxCjg
3amH2Z9agvl7MTflJGI+0G+I4F+Yq7mE5Ubi+bD4Oo23Oam+DbS94Xyextwe
1J+I.eS94Xyexuwe1F+ojeS74XydJtwd1J6I3lzywl87l+zak8TPtI8bjYOQ
farmswdx42jdN1rmvarmsxdBuI8brYOQ2XOai8jQtI8brYOuE4nVYOohaROG
a1yaANpc1S3MomiM64s3F0J6IgdS54XyddKrQsmaehaROGa1yaQMpc1SzMom
iM64snF0d5aRuI8brYOuE0n1yID4MomiL6I9VTiNSJIO0jdd4hMLumn985df
5OnHgJ3cy0WHg4C3914rfiaoDlStDlQ0F1u074kpC5GUHhrzktZlGiBohft8
AINhfLSITuVTIulrYS7yIsRbxGwlNhvLpSiZj8DFjYtsB1O2RCggabb17cKV
khTepO7Rxx0NpBnwi57ktjDTLyG.2lFwlu9qaZDSD3ZyUENzLzt0My7ayi3a
yi3ayi3ayi3qX.Cn3sytxQVbC4BfYWyTMdr5u9HZQu4fVqPsESf4AwlSamiS
nXnPhhQxO4orapPYAAE6jMSnKqj8QJoY0jV4j9jvQXaro7lfRSuFBW9Zvu+3
DpR6j8FjPZuAtvaNLbMW6LEjl8sEFA8cw+bztMlgTSw8hdHUYhTz5rzmyhUe
Syi97hsuj2boJeGOpTiVci3sQeN+71M0e5uRO3ZVuaajReFS+l503OV7VVqG
oMwaiih+T7hkZcLT2NKc2yuDoPpIK9TRTgdgpO3W9bRVhdH37kzcYlIiykm3
MHOvBRCKVvbaRxkZPLiibs+FCdNPk1eKTJylykze4FRGNFgGdZF0jIrXSwXw
ZoO7EfDs3UwKSe1WzLdHQyDfZjLdXPxFHsfDAmVPDDM7zBZa5yOuL45v1VD7
tCoJRqnJKzG68u3IS2Jl8zl1pHr70fe9wV5x2rj3i68ctPvUeFdrcftg6Dcq
zxaAtMeAeN+AeZZwE8K7k7MbKesmyGwWvOwW1Ww16u3K4y3S523C7c76ycwA
oX55ZBmHSGOwSg8Ou6i6fKj6hajs1UxczcxV3R4t4VYKcsr0tW1ZWLaoals2
UyV6t4N3x4t314qw0yV39YqcAsctg1NWQam6nsxkzWxszm20zWV7s8tn97to
tMolscHZqu+CNLM+nkSJEzFkbrwuycT0RKPoGcpHnk2zopHQVt8xltR4ED77
t7NwtwDoJbkdv+pktDo89kwEE2cpO5QJDcZkhlFzUTvSWgPrgvRtZB6SoEtb
JIKpLOa9pmxhslx1lYGNlvxM9YUdEjUXjhhBzaXyC71+9MpieKGt2ZCC1zEx
6oUMo1rd9XahNohh1NbPZS66A4.M6FVH1b5VKCMjSiHCMOhG8TxmiVkpv9+1
n2G8+Z0tMIyMdxdSzmWrbYzSK2oLWPchc0bU3SwK2krYVzeRO02U+OkMIuFu
79n3MQ6VojZnHppuib+fOO0La3SyGB7KxhR+7pYdI5uAU3eMgHgTjAMhoS.S
9CqdLQGHhnWT6XyikQ7CopeKGFuO+IOFuJ5Y0QG6iCRg6R0w.ozc2FVlihtg
han53lG9hNhIJ7VTbzOdWYbQh+69w6zhv1em442IWzw8Qe9E8W2emlKTu3ph
.S9J6ywqxUUQKm6xgPA6AGjDTQ3FZJNProujig0Z7+gsmsxhliuN+Q1MpE95
oVWNwS6JQEXGQkYx4QLtJjXAfC6silh7AMkLAooTiazlfzzXnOnozIHMEhlr
zTfOnoronrWd3QR0SzQmF5tzU0pcK3Eq7tiHzbWFuuCJSNLEII2eheosE5EU
qJ8omZ7WfYC3eFQ7rpg11aSdGXm3cDeEw6T.4rp52raLPk58ETLPmrdeOAOD
pG7Pct1es0YBmsHfOhUTNjmWc+0m1IM3ZK3o3D8fSs9DT8p4YuhZC9BmVZXX
If8tD4Ti83NWhv23zcDmNBDhb5ctD38N6doB+ecvuaiuCeeT8zsNa2pMQOsL
9yKS1rY4WzdDNVAFBbz+0+a+knuK9wGRS+Xz2mk9e3Gyx8M3qKVlD+bRzqwe
I5SwYeILRBZTH5WYSMpvnzISZP+9nuWCMQwyiWqCVvSYouF8mdI803MQ+gcI
K0wh7+RVx7D09fOt4WE8i28CKxmvyemo.zi9gurM40e7tnjeNexOOKpn4L74
rEa0NZNKIZc5l7bV49nG1sUGhin7uB8ZS8WT633eazlDiWs+gYe2reXVdF9q
iqQ7pOtQ643eHI4mTre+2UjVcLLLu2u6a+miVtXiIXo4o2+tkKe+bsurUe26
x+R92EFLqAk+lM1ESLyecFlMgXV++jtKWNjIJHoQOp3PdNwDCihfotV2UPzg
Yc2ByHIO+FKVoqyjYQ+mWp3iKhBVzWT72oeNxjFy4elBtvm0AcK8IyDMeeLZ
yijh5soKnjh+z4bmun9Rx+KkuUZa7RCSasOsdMDHxMwAnbSSQjvLA.YRvJ9s
aUmKtZwq6d0D7sUIJQYZB9OdWir8UIYrLtZwF1w7roSGvrEJdk4oEUlzmSy9
nliS+ozgdqnZkxCtqlSWm52Qa2koN6tdRCXjWWu1nzGmq3RU1zdY9Mt6izFJ
npWNYg43l39hPkktTvyt8mJYbd+KZpaSAW5L.aSMAPaRMEGWYPgim+S6TmRV
VAw2qNzTI+RKzJ8osIqJiL6w7R5+TF9zRgpZYnAAaUPUCbDYQS0x3pYyDrbR
vWUyvfqKYPN1VXABSw4lGAkLfH2p3K3gty2MtNh1yBu5ez7ZQc3hlBkR2eTu
iWuYmUOMQfnpLEoVQzVnIToXgEqTRBTJe6k7GBETUgGJm8rHC.wXRPj+PqiW
kr75nEYJY1yaMG8Oa+o0YICy9f1w3Fuy.ZSvgt1XZasqzNuzSZ.K8OqjIXHi
Cdtf+YWmkpORrr5WlgkiH8usjvsaUgIR5rX39xh4yasDXttLwvzxUMEA34yD
2FYg6cesuMEC741zhdyBqndj0y+qa6SGYF.nmz4sPuGSy4AkyNP4Nl9ClQAR
AHOpuBJQBx8Cur3d2XBZs2o5J0gHEhdMRiQ7hpL8FaPXvFf8T+DwbBLtHUqL
ZG+0jvfi6vj0p0lKwfjivzue6XS1jtK6wRssJZDCQmFVlmrY6hUUEg8ede0+
2xGXuhXmB695h4qSUF2U.WHFZFUuq1r4t1UmdwrXiNn4F9xAYmUeQbhqAwg5
OhinrKLGUYhyh5GkWMQPb3qAwAuBDW3A557csavt7J2sEfvNsivNB70CrS5J
cm+0Cr2486BOA6G3VsCKgUaKe0+5P1ZfsqbUOWeBtkxTsI7ONsm9rnzUoO8j
qlb.zwbxA.rXDQa7igYNaPfdchNbJu3cADecTK6hHtSk90CHtCBMUGOjvNuS
4thtcvHMmYx6pF5vhbvZKZy1cGGe8AaxMvG2I2fwA7lZRExDSoMAhQeSPgIv
HPG1CzsVCwHsW36yTntnXSB2k9T8DapJYmTegQedwp4oe1UaMji6vXxL+knl
VOHA60sFmrYaZ+dCB3h6M55rOqq3ODnH1dG0+TBg4xiR19mRx15HUeHv.eZV
UH4BYZ4oT7sA1y6t9A1SwomlZITfElrBWdad8bad8bad8bad8bzgSFI8WW3f
tzj1wAYQqI6mfrITxWXQGUXy5jkK8PKU.Mf0smGa+1PfQ6Wno9lCiNP+oGCl
MoqPezihBpVTDxx8wltauwOWnVFSlg29zsQODs75pv2NQRGxNTTtoU2O.jVX
WHsPT0.CN.1qNLooHMn5zLF7LG3fSBG0bNnqQBj7USvv5Zb.wCSxVnzoRmj.
l5gSZ7SBlNYxX.RqI.PKQN9qF9kNG6zAkewLX3gENVSLcRwDz3f1fPlKwa2h
Wsmca6ln4G0KWGpfMPPdKXCGmh6XFfKxSsQVQA0AayWrGFFVlo1YLAqnXlPx
CCurqIWXWQtvSRxExTDYAK4B5JxEYZt6BD1jKfqHWzII4BfCZggRWQsXSQpk
jWeuUa8s0QiXIbEwhOIIV.YHSrXthXIlhDKNOnIVbWQrjSQhkfDzhAoNhXQA
SxcVvflXQbEwBNEIVLRPKFzUVFSmjNxfJB5cVtxqSzIoaLnvPdmUryDCNIch
AVDzDKmofwjzEFDZHKFL1YmYMI8fAFEzDKmcl0jzCFHZPKFzUNdmNI8fATFz
6rbka2oxoYPsB2cVUE5yCwO9QGUrOLvznXeJZea7aE6i4q+5J1GpvzCzJ31E
PSWeAeqZetUsO2p1maU6y6thbbW2J127kWeH0GEjBLjpcAaqFESO5nnGaseX
6E5EuvhsowuAqwHbmnpPyIpEylqI.U8urL54rzcqi3BOTXJ3Advu4q5Mpj5R
L0CMbpPc2jrLBBhroNxvgXQGMXEeT21EaheCEGDj4ApBao240S2JJDDV4qWr
vhTPxcS9R7h4sik.chbTH9x9x7ZwpfuPuXuoKzKn+KzK8LhQ2v5MQJP2rzJu
ZBUpWnQ.uI.Sa7VWKTrudJQNeUqWAXmUsiPN8VeU8qz9ppR8kj7dm24C4.5p
C4vfzewtuGgc.YeXGL0KOEERwEpXRxdg.4guZ5CZfnOZ0duue8wxyD4OvLHk
IX4yuu7Q2WwbUnqUyFzDDD4aIJL9MAEtr7ReKRgIuMnvf2t6gouInvkUE4aQ
JL6s.EtrRJeKRf4uIHvf2t6fEuEHvkEA3aQBr7s.AtrjbeCRf4f2DDX3a1cv
b3aABbY0j9Vj.+lvaVkUf5aQB7aBmYUV0puEIvSCeYMTUO4aQJL8MAEtrXle
KRgYuMnvn2t6g4uInvkUh6aQJr3MAEtr7ceKRgkuMnvnvcO7tUqie7iQKt7+
cgBBVbsLAhgvkW0X.78ObesHSQlIMKD62wt7omfP0xbKGUi3hQcfPZexZgJp
BKlWmVmaSe94kIcC0uNNSg81lj8ASApdpFuPcB.xoCKX647gTd3MOTUHxrT0
pwUb+Cg+k507psC6.L4CSXkthEie4Xk.SJB.hDQvKoAyUmaoBxvc3kG1OgYl
cRPPPcPx5rEqb0zEVPC7CSvTyTCDje3NF4UQcqiWkrraMnmrzcqlexlxPCzN
ygCabyTaimetKtnP.omZTxqaaImZgpjNVbH3QrNO7bCmnyfRFH2+4BNgB0+h
TpMDnEs+yR0i88x9SxLr7LDwFe5SWYaMossUyEGPgOpVKjmp1IZoNKzG+bbw
e0dQVbXwHgo4ieIXwjLmCqt53EPaklg0L6WKBQP6BF4jE1WmwHXyvjDhEUWE
RXDRGvH5l3TmvHiLbAsDtnSGvB2EFX3WmvED0uMlEaEQlybQJ0gJuJj1Xh5p
v6IBkF1A3hLg3fAc.tvfoEbYqfTjbZAWHagKwzBtv1BW7oEbQrEtXSK3hZKb
QmVvEyV3hLsfKtsvEdZAWBagKzzBtj1BWSL8Mf1pvABzOUl4RzLFjyHbkvJS
spyE4ZQmeUHo2bNdwVEVfSLEVf1pwBbhowBzVUVfSLUVf1pyBbhoyBzVkVfS
LkVf1p0Bbho0BzV0VfSG0V3cwG2T4f33HFWTyit4WEPG.xEcAiH5GFAAxaca
LAvDV38WERXDdWvH7dhQXh5XjZWERXDVWvHrdFGDbcdj5WERXjtDYHZeiLjr
NOR8qBILRWhLDk1OLBgUmGo9UgDFoKAofh6GFQm7e6wH0uJjvHcIHEzdF1Fp
rNOR8qBILRWBuAE1OLBiVmGo9UgDFoKAFg1W+TfpyiT+pPBizkH6McLdh0EU
wI8TwyBuOURnEAIqOqKphS5owIBZcV+5WERXjtv6S5op3RXcdj5WERXjtnJN
omphKE04QpeUHgQ5hp3jdp3IDPpyjz3xPBmzEkwI8z7DHDVmMowkgDNoKpiS
5aNCA4M3SpeYHgS5hB4jdZhBDQZvmT+xPBmzEUxESmLyg0EEqYSmHaxbYN5N
dfUmNUax.UD3WmTKBviYJtNETKbMZsLEGiCJwn3tXLiX5jfH3tHuYBAVTWRt
NzZbyfngRK4eAEWER7ucQcQ7kPHvvAt5hJeXxzAt5hDX9zIOd5Ba3zIkPPcw
4e7oSxIg5xod7oStIg5hi33SH9vtbZNe53bcTWNNmOczyF0EGawmN4kDpKh4
4SG0JQcQaC9zwoCntXGKe53jHTmbRjbXBJmo6fT+p.xN.XWzTgMczTA1EMUX
SGMUfcQSE1zQSEXWzTgMczTA1EMUXSGMUfcpSILczTA1ICRmNZp.6TnvlNmn
CgtL8gNzyzBRdyQPXZRQlNZR9UgzI5cQGG4fzTWfLdMDR9UgDBoKdDzURpp0
GqpOLksYPJWC9N6.TtsgmrECN4VZKUmZfIuGtp9s8Dspk5AMfq8fv4a7Vs2z
sZogaUuYaQq2iyNMD0ZS15jMc5hNhBmZ5LYEg0+f5a4TcWqV5rVG1Usn.o.j
2JcEEikBvLYw8NPlxYZoVAGYf4Dx.Dl+ChvzKLQxvkLT8INtalsmxbJoY0nO
WNO.NWl12nfeOszqSK45rLSccIeb5Kac0AzxZ9vyio4MqHnnn42s+pl+QcOj
ZIfxNCbB85J937qsspeDXIswCqYgkqYY3rj4VtjEgyR1VIM7vYIa69OR3rjI
VtjwAyRFZ61OR3flg1t+6DrPi1Z11MfDV3rlsdGX3H0.Z6VPR3bdxwtmx5JJ
5JWyEFtVZz5kLXsXw2pgpmxH0KXf5Irg3PCSMq27WMX47kQMqeLKqyz.0q2F
tSVmrZdzuu7ImrEb2n0amaZx2bngPkME81a01moEaeLPeRilNcWNGMiQDTnx
9EJgLCB4Tp52Y4cJZDproZOrnqusunKnXjvWXxdzEhLixvRlNRczdgtdL80W
Sp5m66wW+ORhdHd27n4oK1F829+sMKIZdRDky.fYmFEBrGEZAK20imXfJ7DS
rGOAkEc6hAluJZa5Gh273hE8kw5Z3qpOXOFFVKcfeqXsf6Ysp+gSxNTL5SKV
l7ojrMEhQKsQ7t30qqc6FGanvt+jw4F76qt0hUlaUI4+trjOsn7yu+twYJPX
qZ8uKy.w+La+rcH2GKYq1sHeo7MMcl4c0fZ79FSOyLcXHMpBl6dPIw+wO1zM
R2kpjirXkRdxF09lpyypd77jmh2sb6GNMAu4yeJ9wjV+vmdpGb2yYKlmtRuH
Zfo02t7O2eNJWlP8L4077UwqOwG0bHYKObiBD2s4g3LMYnXXzfpNdKMcYyGU
84Vl7z1hGudwpUGfC2ltt8Gls34WNym8gT0Ce8be24OYyG1sx7zOn1Cu8CZE
BZ99hWtrXScyu9eNd0hWi2lrcgg.f.UOzLPddQoyP5xkMfWyS9zIdxbEG7iI
edw7suzzGecS4i8pRbF8M1r6ghcneXaxqqWpfhluAEz0lm9ryGrFQP14D1Sn
jw.4rUHonQ8XN7q50FJRVdzVi.CL.tgNeU1NF.dZLP8AEzElYI1ff.W.Agol
tKwHfeNZh+r+zxs5VF3YbgesIvRKmXdHZs9fE87CKF6FkOCGyYAI.yap1g6Q
xwaSWXG6ocnXVOPwKWrYamwu3KfeolRUgWz1gNK90IA4feTrK5tez8qA47Im
mGYStUL1yNj9qWWdTpDSocLeJd+e6CezhjOmq+4hkK19k514j9zSaR1VF8z7
gB6d6XL50OOSsXqzXqzlrGWt3wOt8krzcO+R86ejxeMevmN9AO77Ag6UA2Yy
MlOAOo4hnisKDcVq+pe90os2K40GL7P1Y.HGHqL.DiKxfnuRs.jIMo7rYb6c
LvdPL3MiDppWtVSEg2LU7lohuALU7r1A7PzCCoY.HXOTRMe9qd+IlCqCfw.D
SNGhXlF7EzIFCb7H6r1TgeY5ljgzbfFyR2thp6L5EcdzKUXLxprorlKbV3Qz
qNMqh9Ev6MdK+WfFRLMlGPXZh.YvzPSWHyIX5yJxXo5+NqXCR2wv8QrQttW2
6VwGPrwQWXlo8olmI2Cs3iqzmdnANCJolNQAgFbNs5gyy2g6NeGXnNtxQ7cb
yPmFQnV3ikIwwV5dcP3brUwDYW5xisNCK8ljkQvAU+Kx.vPO3GYYFV2HinDr
+Yhieb6hOknjeFsIYaiwZe+YmwAD6LuvI2FkwLu5UsvznWkNXE3a3fptKJjT
ByvQW35aGgn0X0G1orbd0cMKofsoZPqB7eNawxkev3QKkg0Mdum1iGm+nyGd
NcUiJdv3ikhWNpDGhyTKjsJyWM1SenhJ6T7DJ.I8SIYUeovVVi0KiihFUe9w
d38RLNfqqw5DRuuwqvS7AJra2vgdHObiEY92380d4vutkIOG+3WZrJfsK84t
SyVKsR0154WxYXgOmWxNb02pVHFrj8eQJBbCNl5eWEf9kiSZ4FJoYqD.bRJd
oWbgidTjEWLflCbfjQDCxOvzIec7ieLZSzfFmSbH4XiBC+JxZMJzSAQtdjVz
dznEj+1sYFGd7619R1tyoYyUXaDY.rIensGBCY6ymBHDdRwDpST1llU5Wccp
R1bUzsZn8jGv4l5a8rpV8kWeHcYzuXPUpBGRZuxjr7VdCCTiD6UsWcBJNjza
sX6ijNRX3GWlnCXXxieby8Q4+bw1jWUX7g0VATH4vVnw2MLieaglj48qRrdH
ctsd1AlW32.Wh02oz2ZW+s35R1P0hMRzJKjL.a8LDuhFbX.q6.8DM.4zWe0A
PyoVD8y690ZUc9M5ux60ikb045E+Z9Y7E+NDRQfCMQsMAgE6MKs2.44vnLjd
9Cx5q5ZCcDWMtttbNMhbRHS7a3Bfgd3BJTgpPpmiBWv4sVQGH1yXsRdbZcf0
J7.zXkREZgEBYnW1Zk61s4AsWNdERfv6lHlsXqiJe3W949eVYsNJi3f1zR.3
1x5dsjN3dsbYZ5Z+3yxrjmT.3Ks3zR5W0Ns7TAVAUDw+icO43G++AtldPEkf
2nD9+KZFix.l5VyDuNuZh+0aRzR4yOc42Ln1zHBonNYnLzhCRLy8FeFt5kow
yMpuLfX3dqS0PepsHeutw6HL2rSebsarvm1zZV.MvlNBkgloiFAD5uyBYDFq
Eu62ubWx1zzsu79+Q0oApuome+2mqAawyW78ujtJYdxeX4hOsHI68+SKxRVp
z553O+2oUa6tKZ3Iga78F3HrumhXSQCXvH4bPiPuWkUBsBOS4f8oIous9b4.
a8IeHxsGWke0.YMsF7lkm6UXUAwIQYIwyMlX94L01+gLEUgCR3vtFYZjKvoa
5bBjBOcB8NqtBQmMnxQvgjbDiw6jhtVB264t1gI198Q4E7yPpAV3Et8hDxDh
g9VE2+uJ8oxRifQ+NWjDaLeqq6kP2vhTxrP1Acbxu36iJ72v8QqyVrZ6fF+0
PJ+LQ4y3YLpH8Ev9OeiyMS92EuSoypRLRDH52Mea1vlCxvvqUPTj8TEEMCjR
7euJv1FnKJbJjdaafmvIW6U.imb8WgvAIaau6DGNsBZrsLFnvoUgfsFOGNrF
XqaSwnvYMaa+BAFN800Sf9ZgeNfvyPa4M3g0Z1lNLTfglQ1vNSCq0L1l0b.g
mAVxZfCqkrMrFn.aMClXrySNU5Pxo2RVL8N1FwmfqY5zSUCj0pgRCq0rUpZD
P6AwStIrSNWJzVVC3DaMSBK7rUhMBnsfVqsuLrVySL8iPfo2vDCM4TPBZsGm
4g0Z1lcfP1zaNWEPJ0kOmqrxRPQXslsh2.GVylK3jZ33Y83DyZcmOXJbRf44
WImapGRP9OZdErnYGfMCHZBz6SnSqGPYAT.Cf1ZAAjO8FPYv.RPDZxY0CDL4
FRmPacVY.YzCzZeBGNJ1Bmb82easRCRF5F7+UzB8qOqzaYPs0n.Sg8evqAwX
xLJkwHPcwhHmAfbdwnnuZhz6APQN.fBgHlQDLIVnzQPWCwkSQNOCLUMa4dMn
EIXEv.3P7.CLsM7.wIQYwqd9u8ukDouUeGWf1LxE6A5APXJzCGvTrsbBYFn.
8Hg8ZfA1UJ8Pv1BP7YxRPwFJctTploalA1NT3XADdb5k0P+nVsLp465HwgGK
J7Hjs0KHtUKHn+VPTKVPHI9BKnCrlAoozLBPmVmD5LtjPELsXW0VHFEyjm+9
X0+H5o8gxFGD1vvr+96Wr9EK.oMra8pwBDLaFRea94uOTee0uyTGbo+ZDh52
ezvBfqFIfjxYHf5e5RvlIlIHLBR198gDhw7W4LkQuDTwcbAjSrYWfP5uskXq
VPb+sfPVsfn9aAAsgYki72BBX0Bxih1sghg8GKjPZEBB3uEjEqGxkj462ki+
Xm4XaOLvSqGq3lYda8vrRSNFweKHqHXLr+VPVIPj4OVZlUrP96HLpcrP96DC
JwpEj+1zakwIX+wRSrAAgA96LUBtwwSsbFFxuqmKRwfT+tfPWhkF4sC4IHqQ
PdZAYkTZ+4gDa1gI5l2HfXzLXw.Dkx2O.QUFSd7scf0kXaN3g4OgFXaDq5QE
Ev1Py83oNXaDhQ8H8xFYFTRG2TPoyH.btu0jXsiYYTn1ALm79tXWgUFvy72Q
EXfUVLC6HhV.H570fPIyPBnTftORn8hGlQk7522E3XKAI7UARRsCKK.IHTPp
fo8OXDgIzUASPPM5jh6iWAT0dxHBUvqBpP.1LJPHz.HS6rcJhHqc6QDf.W2t
I.dFhlCPRDSCOTJr1sGO.BHuNJjt2tH44t8Fn6DFEPT08cBDYkSUHd7jWfMp
J4Qu7X4BhFZKHRnsfvg1BBEZKHnGWP5oCPe2jcfbr7iUjDcL7DbxLIkP0Byz
2VfkDHu98chvL05kz2MpGlUtJouT.QBO.pz2WovifS8ATg66t8CfJLWLSBoB
HpITouu.go9gVg5qHiCCzLiOCqd4PnReetxLefWnUv9J24DlowwblN6npCU0
MSy8PUHI7xJWB6Oo6HqRoGLPFbqHlGWQVQ0jbOthrwmJ4IDC09MqTjNo1fBo
QucljPkJoQTsgiHNiAqeeGrWEYURQjCUjNbHHCMCKoHfRDDF.q.KhPcLBmHw
x522IfEvVvB2AvBXHVz7kOIe4KzcLV7LNBgzGBVcaW.TVkNDGxmdYVPhrhWC
xUVCyfHhBrpwZt+1iGXI7W.EN7u00hn89JhzQe8zfDClQTjXJ695If29a6DJ
uUdSP1QnhHjy.LfTq.TcF55RkbLGsUvkviGmJXCBl16qntxQ2fFyLz3CYoqt
sSH81kczczGlDjQF8ALz0NOxwLzVAUBhGYeHCBd16qnNyPWPiKsfih3RDpIC
c0scBoG6BvBqmrARISGkDhPYE9wPU0scBTYsx7nNCTbsGsv.bkZuXpPcHpjH
I0uuS.Kq0luK4iufMiwTLgEp8hJ.KkMZyPPFmipeeW.VTtK3AKfKptJIfDzL
oVuc4ALgk21IfE0IfUskuBBEm3Dvpa6BnhXMO30ATHcXi0q9lxK1eam.T1XO
I2egr.YWtPP7n+QrxseJo59aEgrp7AHdzOaHqptPB0iqHqxnXhGMUAwrZE4Q
cMy6yGWJ5WHD0yqHxEWQ9FGgu3JB64UD5hqHjmWQvKthfddEcQMPPdzJJnUU
TpOIZVUgC9zWfPqJaGOVomGl4bsUmdcL+.ZjKcbFcl.VT8+JM9NwCbgdePnU
YvkOo9VolkOCLlkY4lOWQVIVC3wiGsJgdP.OpBAvJ2UA73A1.ajqICr0Smyd
SovLfOQHy.UUZlGhkssuZ4wIG4jNWfsvkGU4GX04GReR6sStu+TL5PnuMbjv
iqHaLlkEXqmNmO7PFGZlf4lVNTwkTfYTOxLyq28WVkp7TfvI6esDNAPOh4sx
IBRhGWQV4DAI0iqH6ri.6wUjMmCvCr0S2K+CY9QqLT911hqn5yYcxdSqrMS5
SohVc1pGaTQP65COdLzsP65yLdLCjfVkuO3.a8z8BcTR0kpjPmOfBb9v7Uj2
yDO3JNJusdUuHHyukK1AaEjB8Xlx.sJahfdL4KfVk6NPO1ZufVk7EPgO2wXU
IgGXqmNWWuPj5jTD1zKVkDyf3VmzffitTP.NZGqc9C0im4JPtndia31SAK2t
CHmyxQtMujWzeTa3AUjirGQXm2eEAF92mRvsRiGAxiqnAnmoLfKGtUG5x8nR
pb6NhyiLQb65ml3faEg5ZsFhpRMPNbFqHYWqmbc6uuKjmwsx2ehtlwiJUYqx
1rZvU8rqKDfKO1kZNrycNPoJFEWkSXLkJGJ7Yyzeq5tNAICrBI6QE+X10op8
nzdlUcrKetfrQCROFP8Cawksrf7nFsLajc3SC9rpUg5SahoVoRkGUwiFVcou
CyyxqMQZNiKj378C6hSeeWHw2N3Bx6Abg.6avT.Jc+Hwn5AiGfAX8AvvJBiD
o6mhPHjrerkT8.m.Xh.KAprbE0U2q1v4LBH2kaBrC.P8..354Wi6..9fjBad
eEAD8vIRbc0RVlTcTkFxbbwHhp5AiGtFP6EjQmwIJaFyGJHBntoTV.YkOvIP
FKvRNOKWQnN51Wo9D0Bg3lhHGKkZlHkUUyPbJUugs8GPv0dfxb+xI1y9GLdD
GDzmnBFnFp.AzR63scWpfV6tP8t0BzV0CbBZyFEIkcULT88jTtXF.h455iDR
T.V0P2q5AiGfw6Cfg4yXTtFTTPAXFnDtJu+nAV9LkkgDRfkfv1shfntR6gh8
ClPLgal6VNgFa25m0m0u.4t0uU82bOZ7NNvrU1pJWzicYUnckIH1mqHqNM2i
CqpCUhpsUjGE7hrJtiXOFVM6BLtraMn.sjJyz9CIjyPZaD0dJACN91tPXlcv
jGYDg1sY0idq0thni3wnoZWIq4wth7gVx2FNxihXA1UI1dTfFvJmZ6wVGsUU
iGwiqGq50oWKWsY5JGud8mRx1T7kluTt603eJMSeI+97KWrxbY923cYIeZQ4
62bm3rGeYw1jG2tKyLZn+YcPAy+noySxVsaQ9ng9az+k+W+l++AeAr9C
-----------end_max5_patcher-----------
</code></pre>




Patch de réception des capteurs
<pre><code>
----------begin_max5_patcher----------
7389.3oc6cs0jihik94r9Un0yCyLQjYtHAR.89Tuar6N6FQEcES06snqIpfz
lLS5BCN.blU1cL+2WcCagMFD1bQos6KtJvFaouyQGcz45u+gal8P52CymA9A
vu.t4le+C2bC+Vrabi75alsL36yiCx4erYoqKhCKlcq3sRVuTbC9aZIuazBw
G8ge8NnU4GMu3s3P98Ud3njxmEJu4pfh4OGk7zWyBmWHFXNtj6stE3f8Y+AB
W9J3uIel4oKWFlTH9xY25u+gOvd41gYN42CSIjqQMk7N8ojsmiQMkb6gojik
QMkbN8oDx2rVKY2CSILbXmRIguRGpkCqhvuK9px.qRyJhRdLs9Ya4P9wzjh7
neiO4XCw8v.7LkOYRvRAL7iYQAwkui3qs3sUgh47rYalc6CbOGsXQXh5PnBT
Z0pjVHhinX9BZnOCJOBXaMEzWWOzXuYHjQmtEgYeMLI3AwzvRAmmmFmlIFZz
UhNtVX+aA70jV26Y6ifD53SgRy.vGClW46oU3mbDveDkc5VFhqRIhCdHLddb
z7uoh8ZxXGUDtTtQ7r+430gEooEOe2+QBk4kRit6STVM1u1srWh9zyoIgKB+
o3nWhByt6+IJKLltMd46u84+X5hvkat+57GVxtAzACq4lvM2DK+Sh7Oc2LGa
fogvW4Ac72x0HenGd5wn3XNw7qkf3LAsslOA+UXIU21k33BYDb5FL1PG1eyo
JUeuGFU9vHeKnOg+vV9dVNh+F8VvV9FDeA7UAUdo1mHH4IAEF4ZsgAS8CrJK
kKpHkunjND76SoPANmn7GncOK.RawMBsfPd7kzH+SRbygwG7ohONSE93hFC7
w9TwG7DgORUNGZ7Acp3CYpvGg9qCM9.OU7wchvGoxvCM9Xcp3i2TgOBMq6I7
Y8xGBypGJPGk9f2p7xtZAVBnvs6HWjEkOOP9kdueMGu5njySUQ5ApF.Mg2O7
TkAtGg9Ot2V6eScdTqdhzYQkuLW7sUdU8K3vjTaGwYlDDVKkm44c+A7DiOD6
UTolWmmze7E.8WHx6J8uN5O47m9KUI3J8uN5u6E.8Wnx7U5eczeuye5u7H2W
o+0Q+8u.n+BSJLAz+hzmXlIqwiB0D8W0f4VMZXVcO4SS9TvwSXLStMMwVkud
Ty7R+Or6QCyKBxZwSIUXkQ057.X6LsmhuUDF0Eg1kcoGwg+ZXvB5OKn34PvO
lsXcTR5eLGDjDDm9DXUTR9IiQnZwn+6vrEzekd.kH7iFaS3LMHoKnf8qgDVA
BDXymCYz1LJncHwoZ65Cr8Iy7fZcQDBJrhfi6dNFf+PgYRvPhFzASTb3KgY4
RClKQ9alErZkxsuQ4QXP3uJjY4d6laQAqLE7fdqrvWhJe9s2MHiNzKni60Yh
Y52IklBf80jRA5j0Q7gh3lTh4GJ+BKmsP9j0Q3yLatm..dXx1YKaef3z4eKb
g5u9rzUgIQIqxByoqMBJ1YvMaQ3iAqiK9Z8b4Ue+cb8U02rVp6MydJKZQZBa
PTApY2t7miN2DRAvpSF9mHIXUMOLk0hBKG3MoR8JVm+PPFiRHkwiJeyhzz3p
u0lmKN7wB4aSkIjrCJVjt5vuYVzSO2vy9PJ8MW1z2M+cx+55Dw69U5Z0hulG
7RUztHHNVt3s5W+2CRhVFTDVDIHAHqMuoXetmymmkFGWY9JdmWp4cVPYhmG9
ZzhhmE5xnPuoe7nUkLQy1PkWD8TXdQ06UD7Td06r2Rc5sV+fbQ5WKBWtJlNK
p9ApD.QpqHUEyU49MItaWam9bX7J02pV0FNrrt8rj5rcdh5WTzrAUO.TcHCq
dHoi9R2l5KLHm3B2x8LtobeCo3l9Gby4guPdXLaDcPH1pqPLReHVCLTkBWUq
Tl5nDHzkfE9F1x1GUQ62Cg6zmwVnFCWnFwYrA9Ls.9NyaaOU718DcwVn0jXU
w3SUx2MZd5iECNC1hg5T2R3fFGgdV3QGAWAxoH3FcR6CdZ7XxSO.xZJIIbE1
jGiTPR1841pDbUz+fJCeHEh24wqSw3Cnb7gUPtckjOjhxUTVdWEl4hg8DpM6
J1EjckJ3TuNyZn2rN5N2p9yZpCcC5QqmtzsnOcq5T2pd0snac65W2pN1Znms
N5Z2E8saPm6V06tYcuaV+6l0AuQ8vOjt30qO9gEb0td40qa9tBG1Up+dueSA
2a6aR1j52ML4ZRx7gs0fvFCjRKTU0rlJB12IVfuY28Fqt+3ohX7IQy.F7v.F
Z2mr4c45BlZoKlZiLLHcOUP1QMjrPp3wEfb5xd5RCfa2P+F1qnAEtaWAkdl7
A2iHzYEVNnh3Vx39VFK3tUM5qYQcICE004Lk5hJOZk06.pKdnnt3yUpqPlsz
uUFN00YnntjyTpKTD7FPn+6.pq8PQccOWotN1h8cceGPcQCE006bk5J0pRXn
CCm5BGJpq+YJ0UpTky6AUlsFJhKz5bUqJA0EYhpLuNYUv7uAxq9uMSh8NdRL
rOHw21v+arjeOQ5k6aaRT+LJ3Fx8bUyTbzwSwQ8EE+XornglxJJb.iLgsZvv
sOk8iAIAOERWJyi1K.CvAKBJBtG7uFL+YNIWby+z5UfhT5ZZPTBn34nbv7f7
v+LfJ0ONXdHKP5.b4972l9kjkt5tEoul.X4mOH8Q9cWEjQGM.NFA9S7jQm8.
a+vz2GHBg2vE.Kvc.2+786N3iiRBmmtVXKPmNYk38ht2NPX6.y7QXyt9yF0P
hXmComtwbOeQPsvvU0KQLHtES5y+kYetl4FySWmMubApT2Lv9f6hv7hnjM9W
6W1XI+Z9raK7DV8zZ1EQ4LuvH7QzQsp7XQCuqngxLz8JZnLCIWQCkYH9JZnL
CcthFJyP6qngxLD0Ez.c1CGvNgGm+rG7oHTS7v+x.NPZBGdWFvgslvg6kAb3
nIbPtLfCrlvA9x.NHZBGNWFvgqlvg8kAbnshGVm+3g06X0zULdlZjlOiEtvK
9pHro+ZPQQVzCqKD1Nqtvr+nBY2mhSeHHVFHtaLinNwu61f88C6OuF37zP5O
vc7ZvolqFj244pgvpr1VbazJRJudO6YnbfET9Ks.8CDwK6CdUp9DGC.VKXHh
2WQ5LK83YeiF66BjsLoeNrf6Wh7fkqhCAYAEg+.3gvhWCCS.r7g4M.Duj4bi
EkWZYsL+9dL4s7Fyj2x1W3iYhrheNPYukZMXoY2D1Ts3ndO1bn5wRKHdskkk
lhajNKb4V8WibbUpklo1cnhszxxQnqn38fqMOxzuxszSbS4wQKzMc+ZiaJMK
RMMtNffvJQXxwsMBkHw+AvUXBOBYikgChr1erShqMqjY2ibu03rudbZvBNed
Olu7mRByOD6KgbwJ6KIKROiXNttHLN3sJAt7ICwnwFhQs.wVhp1hvY0xXR85
FQ0EAAWf6DAQ1hXlwCYRaE89kex4BmeRt+4U9odheBeoyOYcU9TexOQtv4mr
ruJepO4mbur4m7cuJdpOYm7tvYmrtJcpOYmtvsynmyUoS8H6Dx5xlcx08pzo
9jcBdgyNAuJcpOYmPW1rSDmqRm5S1oKbKii8tJcpOYmtvMLNFdU5TexNcgaW
bG7UoS8I6zEtYws8tJcpOYmtvsJtM5pzo9jc5B2p3H7UoS8I6zEtUwg9WkN0
irS1VW5gPmIJcp4jwfWCp9DqlS8YJ4fNex6dhVvaPd0yQ.G2TsfC4dhVEf8v
jaKMk+UyoSvrz39r2Oglpju5vM1Ljinmg5NHAP7xv77fmBqEf4sGkdDbG0Nq
EROvEI5WNVa6kLiUSdR1ERQzm3qM2d95bFWYOp8KqRnTV6Vst1clT6NSNd1J
0qXWnnohwJRhW6OSW6OSW6OSsje2SS+YpshJpZGGucg16qAm08co6NMl0VSj
reGRvJIFlmgTKWy2Wm4dE4QSIxKatgxLWFSFUjeuDpuYE91SgiVqbw6cVydF
8rEgWNzWju81kuZD7s6ndWuxy5Lk7r1xScSrUNBtoHs3GkJX+wfuCVmKJfyx
6AdHkp9MXUV5SYAzukEfWiJdlWnDJ+Dyop4QuQPA3U99O4pu6ejUTmWst.P2
2VT6DVF7M4GYEqbOGTD.BdIHJls2J81Yoqe5Y.E3BidIDH0wg9fu8ZXVHq.Q
+V55LQUit4pAMZfXivSKaDW2XQOdUztzX11wH3hnZwDQU9hSoO90v98baVp+
vdrH5srEA.Ool50xDB9AIAwoOMjXu6Th8dVJPu63B8m3N9dS9N9NH7zsieQ5
SOEG1MzqEmGzToZodDNhskvea.OJfrOZIJkNvxWMhdwSo4yxBCpV6T5SgCGS
i3QS72Va7u7DYd10YWsCYas8w2FswVS1YqlupCYusFr4Vy1cSOau0j821yFb
6XGt6D8GcYGHR35BBy2E6hlG1TbZZNNcMImVlkqCllqEyyouI5zvLcZYpNsL
WmFlrSOy1oko6zz7c5ZButZFuVLkmVlyqcS50tY8Z2zdsZdulLw2gMyWyhH0
ybeG1je0IoptMdp8yoQCGu8M4ayVdcPUoVfpaZoQjevr0fvOeknpD0f.fa1o
ijy+gXMHI1Jc.yhFhiqNa2GqhBA6qTf4QiPFGMBBsEDImihH8XpzTBgYfRej
+tmJYqEUpNUhGHhjqvlW9cjDAATpiEagD2IB+i4zsrJaNYLkYy0gTs+10J8y
pp5mumRQ5TPeqS6wSZyf1KvussyPME528AmoxRifGCeEjjRQwe.bG3+JYcd3
BgEByAuFEGCdLdMUsV5tYap8puDDuNL+dvOy5zbz+ipy7xf3aAA4f0IzUsTh
C86faewEoh9QWJuwyEkAReM49Ay6SSp6mDlP1Q5kbOyxfx+Tx7PlgZAOSWAw
s0avCouDJj0dK+clGj.dhJBdqchklvhYi3RSIJH86Y8WV2ArTr8CuwrnLEa.
AfuLqztwA+guLiI9X6cVvuCeo7sfWel808GXbSrA2FKTyGYuFjv29lIioYSL
aOPGldR8tFTjnE1h5vnMToXiZ.ceTvhSnKu622cpw9qWBaoQGeVDuP11aL6+
HZDz1oKnght3X3zErvTIFJcI.NTzErgSWfHiltXMTzEhoKGyc5HKrt6Qu6Jh
zDk3lG1XlLrGwxsu8cwNoffMF46baM+k5FXMppP5iOV4alz1zrAQkjMMafyO
ZOTaZu26TZubFR1jSK5y.TpCyjv.rW9KUCO.5H3A5TtLoygLOXRMsGKjeeHq
+1t6B5JbZR9AWGVSvQsa3zY9rNlqSsrCifIywZ6Qi2sER0oTd5JmYG3LQVl.
mYmRAuQi8rTwz2O7msYum6.pgPX15jbviwAuFGlmG+FyZbAzgpmM3e+u7afO
FL+gzzuA9TV5+zWx31yYYTbXvSgfkAuAdIH6soIv9Plf87DwzLAiMpP66Nvm
XiXPvhfULis9XV5RvO+b5xfbvOsNLl4Kk+srvEgT90uk+GAeY1mi3cQqOJRZ
NvmeqHb4WlAB+Nu6ZcOPlfmulEUvLvWVHXUZN2m12BdXcAyDw.9WAabQ+EYF
r6G.4gBqI946+38e9ddjmxrKbPx2xYVr6ygg+JkE5+jRxX1.V7Y+3O9+Bhix
EN6gG1oqiiuaAyFhzu607uj+goggaRsym37WNh9NGwlXXLb+eoq4xCDVPNEL
mRkeJTX+WoSfXsSPt6gVGIZca7aDkvhg46A+KwTdQom..uQ4QSeEHBkO9yH4
jdh43gzGEc9ss9VhaEZ5GiErxxeZNG1yzuD9uDe4PQPrfwS4oYigIR9ksAH+
RDfxDgwiMF1oerftGSRzx0KENeHIjJRgQz9xrJQGGUBUoeEBDrT7Hfg4vfHJ
8dQpLx0eMM6aLtF1Swb8fLZ14NohwsxB4QPw5L59fpNrTH2TM14YaMR4znmu
pYdF2gwSCnIMmH7kG+S36JDpLz1MBVletj3e2yLJTUAHrH6HWQPPdpHAHJcr
UvhecMcGmxrv5V5FPT4HLgGoOVDlT5co84GX+TBdsRgaLYYSBqwjlmCN9xhT
gv7fhNrhwvannra2bt79mGyCYis4pyC8IV7dIqUCVo4vU2h8nejoOOUDuJy4
IjojpD+U1JO1hNhpKmgnMdcVIgkjZFTt7LJgthjpP4fESAnIMKKPbVJYj5Xa
6LpwTvpfjv3tgqYTYfKpMVSOXsIq2cV9VmXPbEmj2ptEtrXwttwIUyxRqoXo
gM17IdDjvH.MXisUYorsOJi356s8GA5WcAul9YICxuW8I01lCO7T8tqnTbIR
piCG8ZUhbsYumWtXaMzKWj4uMQlGWrZr+E35kgi.BGP8xj6qKR.eDmbhcG.5
m08XKeOKtWq7vN9Vbad5Ku2E.QDMfay6HEiIjrgbkY5yUxX+SFsGv7QVrajs
LDJjcMv2gKFqV0iThO6CQb4fA6y0LINOcc17RsGjIJJX+w5hvb5oz2jfZ+x1
LZrlO7VEK1EwVFsXUJ8.BxwNhftGyVcIVjob09CfnblC7D7RGE29wBHdcEPP
GOf3POOAGBD1cl9GkWYP.hcWADXG.joaZwhIL8mW9cjyeBmW3NLuPVuelWNc
gd499Yd0o0WdCz7RwbFpo7iNo6yuepkzs1SumCUe2pIsd1NuF1RvYFHMI8wG
6yJbJdLqvoVszVtDmCUTWdcfCR0icWqiz..pBSjFAhcCOvd.KnmRVXzVGxgM
3QGyjxAtVQyyvVlIb242GjWrtpe0N4pEq63VsXEFWTjONPhmIwX5M5Llxi4f
rzjuT+zGcf4O+TFEN.AhfPI8Q0.EXSvCP+x.uFkrH809jc0ebKx4h5ZNVT9a
brGD108JFS5wu5X0H+pt02+tfGHKouA1KumGq5lMUN3KgYE8311NPCppuKkH
fDk0Jr80hosRQ7QtqgHeK7r8DQUn+0Zo80Zo80Zocqm0ZZpk1BIlcyLzMUMr
6wH0RDQ.Phg4DzVxxx7UgwwCTZVh5g7aXfKKgPKgFYPQNWMtUMy8aWJUoMvg
JO9mzz3GowZIQU3TXmATMsSkoa8RA3APb2xBIsIK8QV7yUS+1Sf7.0k7.QaZ
RSi3ZlSKDZvSZVcKvMWqdZGhQymfcwCBNuaLzdW7ef8o4nSptALG4Ih+eewY
dswFkW8bp0QcGvKQuanwcxWJ8BMVz.6fRCa3YVttEMdvADR5a73pOo5n4sxA
KpTmrNUik5fFLikteXIZSrb83gSCQl..5zEOgDQbFKLzpZa7b7fb69DxsMdH
GIBB9IExg8Ij6X9b4VSOja0mPN13gbK6IWvheeh3DSGw8cU4wqqFXM3.tWeB
3tFOfa4O0.NoOAbOSGvccmb.2sOAbeSGv8blbQJ3dDvwVFOGNbxAbm9DvglN
fSblbQJ84oevF+ANwdSNGdedBerwebSLbp4vC5UQJF+gMs8lb.uW2zz3OpoC
dpEoDzqxvM9SZZilb.uWkga7mzDgmbQJ8oACwF+IMg9SNGdeZtPru4aT7okC
eSfM+Pv7u0iA2Lwx7BtYYoxv8ZvMqFbyXOQ8mPxA5AEYTr80na9ZzMeM5lMz
natkXOjUJDyea4CoCU.6Bmx3BUmn0Uj+ox5iv1FjfIDXnQEoAmowQss1TFnX
WGY8Z2PnL+VL3orz0q.tdCTf6Z2SEx+gLlpKoPNh7bBZRTn7vX.zBzV7taaB
AV8IGf05uZRXaYr8nRpNwLrAOavk9KCfVR4qMF.0zQ8LiOrzICp3aKsgVa2N
DZ5QIFQvoSNaCNc33Db5rZOLqnOJrBJqnVTdkgEd5nQBO7rLe7nKA196mv0e
HiO8IrhN0gYE9Z8bxPpmSzsfC40ijCa9TTmMe5IUaHt8HLgJROSnJxIMLZrs
Ssri0zfyAr6LFiNQLlop0sGW844.dRv5dHl3Q38v.d6KPVeP6Rz0CEFv0+bf
BYeVQgJS4jyIJjy4EEx57aMD9rhBUlcEmSTHx4DEpLaLNmHPtmUDHqyuUPdm
SDnxjO3bh.4eNQfJSGmyHBjq0YEABd1sBxEdNQfJy9jyIBzYk0DJyVkyIBzY
kwDJytkyIBj4YKg9H6LNmnP3yJJTYBKcNQgHmWTHz42ZH2yJJTYF5bNQg7Nq
nPkozy4DEx+7hBgl10PqSVEL+afn1+2FRPHutRD8NESNnP.G6+XXxMIrn8uB
g1CSKSZ+JHsRDLzi48k2n1TKzKnEPxnLmLHcMjhzmXM9SsfvUAYTznHL6qhj
ZY2jZTEHQ8dSARONQH1cZ5wJTfIKE.w8I23obl+ipO1nIGovOyieXzHaWRAT
AMXjkEvC.aZm4NG2RdNmtP8Ahu1lH3ngVit.1UYQI8YmCxCaPBYswhNhfETz
.kGDwF61SoaHwvquWRWA9H8by5RTY6c46uXKS5.7tsSsZ5azGnmQua+hl.8I
Vb6C545fg71EsOSQyZzt7fsJZEhvlmX+nseKcotXXUg5zd6f9.wsJSjb0.Vu
9fVc2fs1FyKa1PYG.yEt4pp+n0ElqMx.10IpGV2Y5dIQPmlo1xlmos2lqF6Y
pilyThklyzQZLC0XLiMmgrstLTv2eiYH53VDHY6Qh8SPzsqKuZrWDf5hfMCg
5.0bL6XPbTVZNlssLqwrNBaP9l0XFoyX1yrFy15LlcMqwriNiYhYMlw5Llwl
0XlnyX1wrFyt5LlsMqwrmNiYjYMl80YLaX6CB0YiPj0woZkqOhdjRWhiKcwt
Hetb83ZawuZr0uhOe0YSTngsIJTmcQgF1tnPc1FEZXaiB0YeTngsOJTmMRgF
1FoPc1IEZX6jB0YqTn4rUpqt1JC6eRGdl35oXAI9UirvcWOcmodG2LEYwKIE
DOQefU4pwdl5p6L08HmoDO0YpxUi8Lkn6Lkbj1+zVklpd0XOS00Ru3i0Ru9p
zT0qF6YptV5EiOtYpCQklpd0XOS00vkX6ialxBjisyT0qF6YptFtDejlnE6q
RSUuZrmo5ZtSL73loDrJMU8pwdlpqQRwG6YCQpzT0qF6YptVc2bTflnqZcNG
oxNxSoWRb7lL1Phtp04bjJv5gUYCUuZrmo5xG5bjp04CUoopWM1yTcUqy4HU
qy2Sklpd0XOS0UsNmiTYGnkiJQsxki8bUWE6bNRUXgPnJYsxki8bUWU6bNVe
RCcqPWUubrmq5pbmyQpFKD4Tgtpd4XOW0U8NOywyvDcUTiXNddfz2w0zvOj0
VRtwLhcfu+PYGqANh2Xg2izTOJQ7ls8nKpwVWEa8LGmbZq65VCZHi6aXd2SJ
IJLuXbI+jk7pwleRWURraZhBG2wrtpVX6XNiYckP4ZN9PVWVCywslHcMvgq4
3zajtRzcMGediz0nCtFDugt6B4ZNFrCo61PtlitXHcOPuq43uajth5bMGUTP
5tKnq4b.Njtmsv0bNnLR6CJ6eZFMWjwkpWMx5BB0c2Sh4r6IT2cOIlytmPc2
8jXN6dB0c2Sh4r6IT2cOIlytmPsynMyY2Sn1GTvb18Dps4oMmchfv91ky6Zk
KOGdRrI5vtxr7je0XuSjt645eRIvJj3pLQ4WM1STcsjQesZWly6kMNm1ZZNx
w9AaVN00nbZoI4TS5ruaywQLd4uJ.Y9vPIQ7ECqCm.90m780j38UZ2ek0rf8
Gg0lr80VrwjYCpKVTwAjtlRItV2MK6qIC62M65wV9dV7R1jmr7aZcuu7dJqM
OPp0O5vHo2fQnncn63Ip2KH+oGF4eppUk.Apt6JeI11rOrZJx91jvI6uRe+U
46Qj0c3TMLrzJRCqY7r69HXdxPC8jEZhsWs8Gp+lAZLAHGX7C68QS03I5PQ5
uUK3YONd7zX73OdCGWMFNdi2vQmUiti2vQGdYmwa33nwvwdzFNPcXkcFO3Ap
Cu7NjzAc7nCyrCY7FOZwMOdqtf5vN6LdxBqdrYshD2NLdDJpDrZ0KgY4xuS9
Pgpt2uJTJx8V9kQIhK4eiyxBeIp7yKtSPFU8rBptYqyDZy8cYOUiqCVVx5H9
Voef8K+2+v+e1+b8d
-----------end_max5_patcher-----------
```

