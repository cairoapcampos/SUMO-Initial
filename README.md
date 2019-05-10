# VANET-Initial
## Criação de simulação no SUMO e OMNET++


### Geração do mapa no SUMO:


1- Selecionar um trecho do mapa do OpenStreetMap e fazer o download dele no formato osm.

2- Converter o arquivo *.osm para *.net.xml:

`netconvert --osm-files map.osm -o map.net.xml`

3- Copiar arquivo osmPolyconvert.typ.xm:

`cp /home/veins/src/sumo-0.32.0/data/typemap/osmPolyconvert.typ.xml .`

4- Gerar o arquivo *.poly.xml:

`polyconvert --net-file map.net.xml --osm-files map.osm --type-file osmPolyconvert.typ.xml -o map.poly.xml`

5- Criar o arquivo *.sumo.cfg:

`nano map.sumo.cfg`

Conteúdo:

`<configuration>`
             `<input>`
             `<net-file value="map.net.xml"/>`
             `<additional-files value="map.poly.xml"/>`
             `</input>`
`</configuration>`

6- sumo-gui map.sumo.cfg - pegar coordenadas.

7- nano fluxo.rou.xml

<routes>
        <flow id="fluxo1" begin="0" end="3600" number="5000" from="497075096#0" to="252010647"/>
        <flow id="fluxo2" begin="0" end="3600" number="5000" from="252024721" to="9653489#4"/>
</routes>

8- duarouter -n map.net.xml -r fluxo.rou.xml  --randomize-flows -o map.rou.xml

9- nano map.sumo.cfg 

Adicional a linha:  <route-files value="map.rou.xml"/>

<configuration>
             <input>
             <net-file value="map.net.xml"/>
             <route-files value="map.rou.xml"/>
             <additional-files value="map.poly.xml"/>
             </input>
</configuration>


10- sumo-gui map.sumo.cfg - Iniciar simulação.



------------------------
# Configuração Veins
------------------------

1- Copiar pasta Veins:

cp -R $HOME/src/veins $HOME/workspace.omnetpp

2- Renomear arquivos:

mv $HOME/workspace.omnetpp/veins/examples/veins/erlangen.net.xml   $HOME/workspace.omnetpp/veins/examples/veins/erlangen.net.xml.old
mv $HOME/workspace.omnetpp/veins/examples/veins/erlangen.poly.xml  $HOME/workspace.omnetpp/veins/examples/veins/erlangen.poly.xml.old
mv $HOME/workspace.omnetpp/veins/examples/veins/erlangen.rou.xml   $HOME/workspace.omnetpp/veins/examples/veins/erlangen.rou.xml.old


cp map.net.xml erlangen.net.xml   && mv erlangen.net.xml $HOME/workspace.omnetpp/veins/examples/veins
cp map.poly.xml erlangen.poly.xml && mv erlangen.poly.xml $HOME/workspace.omnetpp/veins/examples/veins
cp map.rou.xml erlangen.rou.xml   && mv erlangen.rou.xml $HOME/workspace.omnetpp/veins/examples/veins

------------------------
# Configuração OMNET++
------------------------


