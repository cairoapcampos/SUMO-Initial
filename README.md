# VANET-Initial

## Geração do mapa no SUMO:


1- Selecionar um trecho do mapa do OpenStreetMap e fazer o download dele no formato osm.

2- Converter o arquivo map.osm para map.net.xml:

`netconvert --osm-files map.osm -o map.net.xml`

3- Editar modelo para remover vias ou partes de vias que não seram utilizadas:

`netedit map.net.xml`

4- Copiar arquivo osmPolyconvert.typ.xml:

`cp /home/veins/src/sumo-0.32.0/data/typemap/osmPolyconvert.typ.xml .`

5- Gerar o arquivo map.poly.xml:

`polyconvert --net-file map.net.xml --osm-files map.osm --type-file osmPolyconvert.typ.xml -o map.poly.xml`

6- Criar o arquivo map.sumo.cfg:

`nano map.sumo.cfg`

* Conteúdo:

```
<configuration>
               <input>
               <net-file value="map.net.xml"/>
               <additional-files value="map.poly.xml"/>
               </input>
</configuration>

```

7-Abrir o arquivo map.sumo.cfg e pegar o nome das vias:

`sumo-gui map.sumo.cfg`

* Exemplo: Em um trecho de transito que começa e termina em uma mesma rodovia, cuja a pista é dupla e o sentido do transito é o mesmo, as vias podem se chamar:

`179369580#1_1`
`179369580#1_0`

* Dessa nomenclatura pegaremos a parte`179369580#1`.

8- Criar o arquivo  fluxo.rou.xml:

`nano fluxo.rou.xml`

* Conteúdo:

```
<routes>
        <flow id="fluxo1" color="0,0,255" begin="0" end="3600" number="2000" departLane="random" from="179369580#1" to="179369580#1"/>
</routes>
```

```
Descrição:

* flow: Indica um fluxo de transito. Varios fluxos podem serem criados, 
cada um em uma linha com um inicio e um destino diferente.

* Id: nome do fluxo

* begin e end: Tempo de inicio e fim de um fluxo em segundos. 
O tempo final de 3600s equivale a 1 hora.

* number: Numero de veiculos

* departLane: Forma como os veiculos serão distribuidos em uma pista dupla.

* from e to: Inicio e fim de uma via. Numeros iguais indicam inicio e 
fim de transito em uma mesma via, numeros diferentes 
indicam incio de transito em uma via e fim de transito em outra via.
```

9- Gerar arquivo map.rou.xml:

`duarouter -n map.net.xml -r fluxo.rou.xml  --randomize-flows -o map.rou.xml`

10- Editar o arquivo map.sumo.cfg:

`nano map.sumo.cfg`

* Adicional a linha:  

```
<route-files value="map.rou.xml"/>
```

* Exemplo:

```
<configuration>
             <input>
             <net-file value="map.net.xml"/>
             <route-files value="map.rou.xml"/>
             <additional-files value="map.poly.xml"/>
             </input>
</configuration>
```


11- Iniciar a simulação no sumo-gui:

`sumo-gui map.sumo.cfg`


## Configuração Veins

1- Copiar pasta Veins:

`cp -R $HOME/src/veins $HOME/workspace.omnetpp`

2- Renomear arquivos que serão substituídos em `$HOME/workspace.omnetpp/veins/examples/veins`:

`mv $HOME/workspace.omnetpp/veins/examples/veins/erlangen.net.xml   $HOME/workspace.omnetpp/veins/examples/veins/erlangen.net.xml.old`

`mv $HOME/workspace.omnetpp/veins/examples/veins/erlangen.poly.xml  $HOME/workspace.omnetpp/veins/examples/veins/erlangen.poly.xml.old`

`mv $HOME/workspace.omnetpp/veins/examples/veins/erlangen.rou.xml   $HOME/workspace.omnetpp/veins/examples/veins/erlangen.rou.xml.old`

`mv $HOME/workspace.omnetpp/veins/examples/veins/erlangen.sumo.cfg   $HOME/workspace.omnetpp/veins/examples/veins/erlangen.sumo.cfg.old`

3- Renomear arquivos gerados e movê-los para o diretório `$HOME/workspace.omnetpp/veins/examples/veins`:

`cp map.net.xml erlangen.net.xml   && mv erlangen.net.xml $HOME/workspace.omnetpp/veins/examples/veins`

`cp map.rou.xml erlangen.rou.xml   && mv erlangen.rou.xml $HOME/workspace.omnetpp/veins/examples/veins`

`cp map.poly.xml erlangen.poly.xml && mv erlangen.poly.xml $HOME/workspace.omnetpp/veins/examples/veins`

`cp map.sumo.cfg erlangen.sumo.cfg && mv erlangen.sumo.cfg $HOME/workspace.omnetpp/veins/examples/veins`

4- Editar o arquivo erlangen.sumo.cfg:

`nano $HOME/workspace.omnetpp/veins/examples/veins/erlangen.sumo.cfg`

* Alterar suas configurações para as seguintes:

```
<configuration>
               <input>
               <net-file value="erlangen.net.xml"/>
               <route-files value="erlangen.rou.xml"/>
               <additional-files value="erlangen.poly.xml"/>
               </input>
</configuration>

```


## Configuração OMNET++


1- Para importar o projeto no OMNET++ clicar em `File > Import > General: Existing Projects` e selecionar a pasta do VEINS copiada para `workspace.omnetpp`. Aguardar a importação, indicada por uma barra de progresso no canto inferior do software.

2- Para que o projeto seja construído deve-se selecionar a opção `Project > Build All`. Da mesma forma do passo anteior deve-se aguardar a construção do projeto, indicada por uma barra de progresso no canto inferior do software.



### Fontes:

* https://www.udemy.com/ferramenta-de-microssimulacao-de-trafego-sumo
* https://sumo.dlr.de/wiki/Definition_of_Vehicles,_Vehicle_Types,_and_Routes
* https://sumo.dlr.de/wiki/NETCONVERT
* http://cst.fee.unicamp.br/sites/default/files/sumo/sumo-roadmap.pdf



