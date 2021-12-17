from numpy.random import Generator, MT19937, SeedSequence
from bitarray import bitarray
import array
import sys
import os
suma=0
def cyclic_redundancy_check(filename: str, divisor: str, len_crc: int) -> int:
    """
    This function computes the CRC of a plain-text file
    arguments:
    filename: the file containing the plain-text
    divisor: the generator polynomium
    len_crc: The number of redundant bits (r)
    """
    redundancy = len_crc * bitarray('0')
    bin_file = bitarray()
    p = bitarray(divisor)
    len_p = len(p)
    with open(filename, 'rb') as file:
        bin_file.fromfile(file)
    cw = bin_file + redundancy
    rem = cw[0 : len_p]
    end = len(cw)
    for i in range(len_p, end + 1):
        if rem[0]:
            rem ^= p
        if i < end:
            rem = rem << 1
            rem[-1] = cw[i]
    code=bin_file+rem[len_p-len_crc : len_p]
    return code
def errorG(tCod, e, n, semilla, r):
    "Esta función genera una refaga de errores de tamaño n con e errores de bit y regresa si hay error o no"
    #establecer semilla y crear el generador de numeros
    s=SeedSequence(semilla);
    bit_generator=MT19937(s);
    generador=Generator(bit_generator);
    #Determinar el bit donde comienza la rafaga
    iRafaga=generador.integers(low=0, high=len(tCod)-n+1, size=1)
    #Determinar la posición de los e bits a invertir
    posArray=generador.integers(low=iRafaga[0]+1, high=iRafaga[0]+(n-1), size=e)

    #generar los errores en tcod en las posiciones obtenidas

    #se cambia el primer y último bit de la rafaga
    tCod[iRafaga[0]]=not tCod[iRafaga[0]]
    tCod[iRafaga[0]+(n-1)]= not tCod[iRafaga[0]+(n-1)]
    if(r+1==n):
        tCod[iRafaga[0]]=1
        tCod[iRafaga[0]+(n-1)]= 1



    #Se cambian los demás
    for i in range(e):

        tCod[posArray[i]]=not tCod[posArray[i]]
    return tCod
def descodificador(trama, divisor:str, len_crc: int):

    p = bitarray(divisor)
    len_p=len(p)
    end=len(trama)
    aux=(len_p-1)*bitarray('0')

    rem = trama[0 : len_p]
    for i in range(len_p, end + 1):
        if rem[0]:
            rem ^= p
        if i < end:
            rem = rem << 1
            rem[-1] = trama[i]
    aux2=rem[len_p-len_crc : len_p]
    if(aux2==aux):
        return 0
    else:
        return 1

def validador(val):
    global suma
    suma=suma +val
    return
def main():
    global suma
    s=SeedSequence(int(sys.argv[1]));
    bit_generator=MT19937(s);
    generador=Generator(bit_generator);
    semillas=generador.integers(low=5089, high=35678, size=1000)
    #print(semillas)
    """
    Datos para primer propiedad
    """

    for i in range(1000):
        resultado=cyclic_redundancy_check('cien.txt', '10101', 4)
        resultado=errorG(resultado, 1, 4, semillas[i], 4)
        validador(descodificador(resultado, '10101', 4))

    print("Probabilidad 1:", suma/1000)
    """
    Datos para segunda propiedad
    """
    suma=0
    for i in range(1000):
        resultado=cyclic_redundancy_check('cien.txt', '10101', 4)
        resultado=errorG(resultado, 1, 5, semillas[i], 4)
        validador(descodificador(resultado, '10101', 4))

    print("Probabilidad 2:", suma/1000)
    """
    Datos para tercer propiedad
    """
    suma=0
    for i in range(1000):

        resultado=cyclic_redundancy_check('cien.txt', '10101', 4)
        resultado=errorG(resultado, 5, 8, semillas[i], 4)
        validador(descodificador(resultado, '10101', 4))

    print("Probabilidad 3:", suma/1000)
    return

main()
