# SUMO-Initial
Criação de simulação no SUMO e OMNET++

--------------------------
# Geração do mapa no SUMO:
--------------------------


`1- netconvert --osm-files map.osm -o map.net.xml`

`2- cp /home/veins/src/sumo-0.32.0/data/typemap/osmPolyconvert.typ.xml .`

`3- polyconvert --net-file map.net.xml --osm-files map.osm --type-file osmPolyconvert.typ.xml -o map.poly.xml`

`4- Criar o arquivo: map.sumo.cfg`

`<configuration>`
            ` <input>`
            ` <net-file value="map.net.xml"/>`
            ` <additional-files value="map.poly.xml"/>`
             ` </input>`
`</configuration>`

5- sumo-gui map.sumo.cfg - pegar coordenadas.

6- nano fluxo.rou.xml

<routes>
        <flow id="fluxo1" begin="0" end="3600" number="5000" from="497075096#0" to="252010647"/>
        <flow id="fluxo2" begin="0" end="3600" number="5000" from="252024721" to="9653489#4"/>
</routes>

7- duarouter -n map.net.xml -r fluxo.rou.xml  --randomize-flows -o map.rou.xml

8- nano map.sumo.cfg 

Adicional a linha:  <route-files value="map.rou.xml"/>

<configuration>
             <input>
             <net-file value="map.net.xml"/>
             <route-files value="map.rou.xml"/>
             <additional-files value="map.poly.xml"/>
             </input>
</configuration>


6- sumo-gui map.sumo.cfg - Iniciar simulação.



------------------------
# Configurando OMNET ++:
------------------------

1- Renomear arquivos:

mv $HOME/workspace.omnetpp/veins/examples/veins/erlangen.net.xml   $HOME/workspace.omnetpp/veins/examples/vein /erlangen.net.xml.old
mv $HOME/workspace.omnetpp/veins/examples/veins/erlangen.poly.xml  $HOME/workspace.omnetpp/veins/examples/veins/erlangen.poly.xml.old
mv $HOME/workspace.omnetpp/veins/examples/veins/erlangen.rou.xml   $HOME/workspace.omnetpp/veins/examples/veins/erlangen.rou.xml.old


cp map.net.xml erlangen.net.xml   && mv erlangen.net.xml $HOME/workspace.omnetpp/veins/examples/veins
cp map.poly.xml erlangen.poly.xml && mv erlangen.poly.xml $HOME/workspace.omnetpp/veins/examples/veins
cp map.rou.xml erlangen.rou.xml   && mv erlangen.rou.xml $HOME/workspace.omnetpp/veins/examples/veins
