# Inversões 


* Bacia do Iguatu:
* 48 estações.
* Ts1,2 e 3.

 - [x] ig1 (TS4 + TS3) (Rough grid) - (Meio Homogêneo - 100 ohm) / erro do dado
 - [x] ig2 (Rough grid) - (Meio Homogêneo - 100 ohm) / erro do dado
 - [x] ig3 (Finer grid) - (Meio Homogêneo - 100 ohm) / erro do dado
 - [ ] ig4 (combined grid) - (Meio Homogêneo - 1000 ohm) / erro floor (10 xx e yy, 5 xy e yx)
 - [ ] ig5 (combined grid) - (Meio Homogêneo - 500 ohm)  / erro floor (10 xx e yy, 5 xy e yx)
 - [ ] ig6 (combined grid) - (Meio Homogêneo - 200 ohm)  / erro floor (10 xx e yy, 5 xy e yx)
 - [ ] ig7 (combined grid) - (Meio Homogêneo - 50 ohm)   / erro floor (10 xx e yy, 5 xy e yx)
 - [ ] ig8 (combined grid) - (Meio Homogêneo - 10 ohm)   / erro floor (10 xx e yy, 5 xy e yx)
 - [ ] ig9 (combined grid) - (with prior model)

# 
### Rough grid 1

Item \ direction  |     X       |   y        |     Z
------------------|:-----------:|:----------:|:----------:
N of cell         |   57        |  64        |   35         
Cell dimension    |   750       |  750       |   50         
N of padding cells|   11        |      11    |            
increasing Factor |   1.2       |   1.2      |   1.2          

### Rough grid 2

Item \ direction  |     X       |   y        |     Z
------------------|:-----------:|:----------:|:----------:
N of cell         |   70        |  70        |   20         
Cell dimension    |   500       |  500       |   50         
N of padding cells|   18        |     18     |            
increasing Factor |   1.2       |   1.2      |   1.2          

### finer grid

Item \ direction  |     X       |   y        |     Z
------------------|:-----------:|:----------:|:----------:
N of cell         |   100       |  95        |   67         
Cell dimension    |   300       |  300       |   5         
N of padding cells|   25        |   25       |            
increasing Factor |   1.2       |   1.2      |   1.2     

### Combined grid

Item \ direction  |     X       |   y        |     Z
------------------|:-----------:|:----------:|:----------:
N of cell         |   70        |  70        |   30 *         
Cell dimension    |   500       |  500       |   5          
N of padding cells|   11        |   11       |            
increasing Factor |   1.2       |   1.2      |   1.2   

* Colocar um número de células para alcancar no máximo 100 km.
#

## Modelo 1
512 + 32 (Hz)
24 períodos.
* Com topografia
* Resultado da inversão com apenas 3 iterações. 
* Background resistivity: 100 ohm.m
rms=**********
<img src='https://github.com/arturbenevides/MSc_Geophysics/blob/master/ModEM/mod1_3iteracoes.bmp' width=900>

* Resultado da inversão com apenas 59 iterações.
rms= 41
<img src='https://github.com/arturbenevides/MSc_Geophysics/blob/master/ModEM/m1_59it.png' width=900>


## Modelo 2
512 + 32 + 2 (Hz)
~38 períodos.
* Com topografia

* Resultado da inversão com 21 iterações. 

<img src='https://github.com/arturbenevides/MSc_Geophysics/blob/master/ModEM/m2_21it.png' width=900>

* Resultado da inversão com  51 iterações.

<img src='https://github.com/arturbenevides/MSc_Geophysics/blob/master/ModEM/m2_51it.png' width=900>

* Resultado da inversão com  104 iterações. (**última**)
<img src='https://github.com/arturbenevides/MSc_Geophysics/blob/master/ModEM/mod2_104it.bmp' width=900>

## Modelo 3
512 + 32 + 2 (Hz)
~38 períodos.
* sem topografia
* grid refinado

<img src='https://github.com/arturbenevides/MSc_Geophysics/blob/master/ModEM/m3_42it.png' width=900>