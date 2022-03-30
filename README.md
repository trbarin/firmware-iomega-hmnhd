# Iomega Home Media Network Hard Drive

Preparação de novo disco para instalação de firmware

## Arquivos necessários
 * [mbr+uboot+kernel.gz](files/mbr+uboot+kernel.gz)
 * [sda1-2064.tgz](files/sda1-2064.tgz)
 * [hmnhd-firmware-update-2104.zip](files/hmnhd-firmware-update-2104.zip)

##  Sistema operacional
 *  Alguma distribuição Linux

##  Procedimento
 1. Descobrir o nome do dispositivo (aqui vamos considerar que é a 'sdb')
    ```
    cat /proc/partitions
    ```

 1. Entrar em modo root
    ```
    sudo su
    ```

 1. Escrever os dados do mbr+uboot+kernel.gz no dispositivo
    ```
    gzip -dc /full/path/to/mbr+uboot+kernel.gz | dd of=/dev/sdb
    ```

1.  Alterar o tamanho da partição 2 para usar todo o espaço restante do dispositivo
    ```
    fdisk /dev/sdb              (inicia o fdisk no dispositivo)

    d{enter}2{enter}    	    (apaga a partição 2)
    n{enter}p{enter}2{enter}	(cria uma nova partição primária, no espaço 2)
    {enter}{enter}	            (valor de inicial de setor padrão - valor final de setor padrão)
    t{enter}2{enter}fd{enter}	(troca o tipo da partição 2 para 'fd')
    w{enter}	                (grava as alterações no dispositovo)
    ```

1.  Formatar a partição 1
    ```
    mke2fs -j /dev/sdb1
    ```

1.  Monta a partição 1
    ```
    mkdir /tmp/sdb1
    mount /dev/sdb1 /tmp/sdb1
    ```

1.  Instala o firmware 2064 na partição 1
    ```
    cd /tmp/sdb1
    tar -xzf /full/path/to/sda1-2064.tgz
    ```

1.  Instalar o xfsprogs
    ```
    apt-get install xfsprogs
    ```

1.  Formatar a partição 2
    ```
    mkfs -t xfs /dev/sdb2
    ```

1.  O processo foi concluído, basta montar o Home Media Network e usar.
    * O endereço MAC padrão é 00:d0:b8:09:68:1e, no entanto é possível altera-lo no arquivo '/etc/modules'

##  Fonte
*   [El Blog de BADBOY](http://bad3000.blogspot.com/2014/01/replace-internal-sata-drive-or.html)
