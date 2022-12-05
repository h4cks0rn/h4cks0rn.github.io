---
title: Reconocimiento de os  por ( TTL ) 
author: IRVING  ST (ak) th3hostname
date: 2021-12-1 12:00
categories: [Hacking,Reconocimiento,Programacion]
tags: [Hacking, Reconocimiento,Programacion]
---
> Github  : htttps://github.com/th3hostname    -    Twitter : https://twitter.com/th3hostname

## Reconoce el sistema instalado en un host a través del (TTL)


### Él (TTL) es el tiempo de vida de donde lo veas, pero ala hora de determinar el OS dé un host se usan Rangos.

# Rangos
    

     TTL  ->  0-64  - Maquina  (LINUX)
     TTL  ->  64-128  - Maquina  (WINDOWS)

### Para  saber  el TTL  del host  objetivo   podemos  usar  el comando  ping  
> Example  

     ping  -c1  192.168.1.64 


### Obtendremos así él (TTL)  y ya sabiendo  los  rangos podemos  determinar  el host     NOTA: Por cada  nodo intermedio  él  (TTL) disminuirá  en una unidad, si no pasara por ningún nodo intermedio siempre él (TTL) siempre  seria  (64 en  caso de  una máquina  Linux)  y (128  en caso  deuna  máquina Windows)
>  Para ubicar directamente podemos usar el siguiente (Comando)
     
     ping  -c1  google.com  | grep  ttl  |  cut -d  ' ' -f 7


### Pero podemos  hacer esta tarea más fácil  creando un simple script en (Bash)

> whichSystem
     
     
     #!/bin/bash 

     # Author :  (ak) th3hostname - IRVING ST. 

     # nmap y  pa dentro  ->  happy  hacking  

     ip_address=$1

     #  Control_C 
     trap  ctrl_c INT 

     function ctrl_c(){
         echo  "exit ..."
         rm ping.log > /dev/null  2>&1
         exit 1 
     }

     # System  recognition  function

     function detecSystem(){
         ping  -c 1  $ip_address >  ping.log 
         for ((i=30;  i<=64;  i++)); do
               grep ttl=$i  ping.log >  /dev/null 2>&1 
               if [ "$(echo $?)"  == "0" ];  then
                    echo  "ip  addres -> $ip_address - LINUX  - TTL($i) "  
               fi     
         done    

         for ((i=70; i<=150; i++));do  
              grep  ttl=$i ping.log > /dev/null 2>&1 
              if  [ "$(echo $?)" ==  "0"  ];  then  
                   echo "ip addres -> $ip_address - WINDOWS - TTL($i) "               
              fi

         done  

     } 

     #  Execution check  

     if [ "$#"  == "1" ];then
         detecSystem
         rm ping.log  >  /dev/null  2>&1 
     else  
         echo "usague: whichSystem  192.168.1.64"  
         rm ping.log  >  /dev/null  2>&1
     fi

###  Sin  nada  mas que  decir HAPPY HACKING  espero  a verte ayudado. 
