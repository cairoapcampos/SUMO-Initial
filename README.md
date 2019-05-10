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
        <flow id="fluxo1" begin="0" end="3600" number="1000" departLane="random" from="179369580#1" to="179369580#1"/>
</routes>
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

3- Renomear arquivos gerados e movê-los para o diretório `$HOME/workspace.omnetpp/veins/examples/veins`:

`cp map.net.xml erlangen.net.xml   && mv erlangen.net.xml $HOME/workspace.omnetpp/veins/examples/veins`

`cp map.poly.xml erlangen.poly.xml && mv erlangen.poly.xml $HOME/workspace.omnetpp/veins/examples/veins`

`cp map.rou.xml erlangen.rou.xml   && mv erlangen.rou.xml $HOME/workspace.omnetpp/veins/examples/veins`


## Configuração OMNET++



