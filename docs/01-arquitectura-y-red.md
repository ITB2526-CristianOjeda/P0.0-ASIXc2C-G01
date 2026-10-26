Para organizar nuestra arquitectura de red de este proyecto hemos tenido en cuenta cuántas redes son necesarias para poder organizar todos y cada uno de los servidores basandonos en la coherencia y la seguridad que otorga dividir los servidores en las siguientes redes:
[Upl<mxfile host="app.diagrams.net">
  <diagram name="Pàgina-1" id="4kYu3nM5JxbX8REtMIVM">
    <mxGraphModel grid="1" page="1" gridSize="10" guides="1" tooltips="1" connect="1" arrows="1" fold="1" pageScale="1" pageWidth="827" pageHeight="1169" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />
        <mxCell id="ADTn_hC_l_CTP9jVdpqL-13" connectable="0" parent="1" style="group" value="" vertex="1">
          <mxGeometry height="100" width="180" x="350" y="100" as="geometry" />
        </mxCell>
        <mxCell id="ADTn_hC_l_CTP9jVdpqL-6" parent="ADTn_hC_l_CTP9jVdpqL-13" style="rounded=0;whiteSpace=wrap;html=1;fillColor=light-dark(#FFFFFF,#F0452A);strokeColor=light-dark(#000000,#FF6666);" value="" vertex="1">
          <mxGeometry height="100" width="180" as="geometry" />
        </mxCell>
        <mxCell id="ADTn_hC_l_CTP9jVdpqL-11" parent="ADTn_hC_l_CTP9jVdpqL-13" style="sketch=0;outlineConnect=0;fontColor=#232F3E;gradientColor=none;strokeColor=#232F3E;fillColor=#ffffff;dashed=0;verticalLabelPosition=bottom;verticalAlign=top;align=center;html=1;fontSize=12;fontStyle=0;aspect=fixed;shape=mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.nat_gateway;" value="NAT gateway" vertex="1">
          <mxGeometry height="60" width="60" x="60" y="9" as="geometry" />
        </mxCell>
        <mxCell id="ADTn_hC_l_CTP9jVdpqL-12" parent="ADTn_hC_l_CTP9jVdpqL-13" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=18;" value="NAT" vertex="1">
          <mxGeometry height="20" width="40" as="geometry" />
        </mxCell>
        <mxCell id="ADTn_hC_l_CTP9jVdpqL-23" parent="1" style="rounded=0;whiteSpace=wrap;html=1;fillColor=light-dark(#FFFFFF,#EEF28E);" value="" vertex="1">
          <mxGeometry height="432" width="372" x="-151" y="238" as="geometry" />
        </mxCell>
        <mxCell id="ADTn_hC_l_CTP9jVdpqL-25" parent="1" style="rounded=0;whiteSpace=wrap;html=1;fillColor=light-dark(#FFFFFF,#AAEDAA);" value="" vertex="1">
          <mxGeometry height="432" width="372" x="659" y="238" as="geometry" />
        </mxCell>
        <mxCell id="ADTn_hC_l_CTP9jVdpqL-34" parent="1" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=18;fontColor=light-dark(#000000,#000000);" value="DMZ-192.168.2.0/24" vertex="1">
          <mxGeometry height="40" width="372" x="-160" y="238" as="geometry" />
        </mxCell>
        <mxCell id="ADTn_hC_l_CTP9jVdpqL-35" parent="1" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=18;fontColor=light-dark(#000000,#000000);" value="INTRANET-192.168.1.0/24" vertex="1">
          <mxGeometry height="40" width="372" x="670" y="238" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-14" connectable="0" parent="1" style="group" value="" vertex="1">
          <mxGeometry height="121" width="130" x="375" y="-90" as="geometry" />
        </mxCell>
        <mxCell id="ADTn_hC_l_CTP9jVdpqL-3" parent="RjAdubwmxspqf5qblBbC-14" style="sketch=0;pointerEvents=1;shadow=0;dashed=0;html=1;strokeColor=none;fillColor=#505050;labelPosition=center;verticalLabelPosition=bottom;outlineConnect=0;verticalAlign=top;align=center;shape=mxgraph.office.clouds.public_cloud;" value="" vertex="1">
          <mxGeometry height="81" width="101" x="14.5" y="40" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-11" parent="RjAdubwmxspqf5qblBbC-14" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=18;fontColor=default;" value="Internet" vertex="1">
          <mxGeometry height="40" width="130" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-15" connectable="0" parent="1" style="group" value="" vertex="1">
          <mxGeometry height="100" width="80" x="400" y="300" as="geometry" />
        </mxCell>
        <mxCell id="ADTn_hC_l_CTP9jVdpqL-1" parent="RjAdubwmxspqf5qblBbC-15" style="image;html=1;image=img/lib/clip_art/networking/Router_Icon_128x128.png" value="" vertex="1">
          <mxGeometry height="80" width="80" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-5" parent="RjAdubwmxspqf5qblBbC-15" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=18;" value="R-N01" vertex="1">
          <mxGeometry height="30" width="60" x="10" y="70" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-18" connectable="0" parent="1" style="group" value="" vertex="1">
          <mxGeometry height="150" width="130" x="-30" y="307" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-1" parent="RjAdubwmxspqf5qblBbC-18" style="shape=image;html=1;verticalAlign=top;verticalLabelPosition=bottom;labelBackgroundColor=#ffffff;imageAspect=0;aspect=fixed;image=https://icons.diagrams.net/assets/infrastructure/1/Server-107.svg" value="" vertex="1">
          <mxGeometry height="100" width="50" x="40" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-7" parent="RjAdubwmxspqf5qblBbC-18" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=16;fontColor=light-dark(#000000,#000000);" value="&lt;div&gt;W-N01&lt;/div&gt;&lt;div&gt;192.168.2.2/24&lt;/div&gt;" vertex="1">
          <mxGeometry height="50" width="130" y="100" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-19" connectable="0" parent="1" style="group" value="" vertex="1">
          <mxGeometry height="150" width="130" x="-30" y="485" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-3" parent="RjAdubwmxspqf5qblBbC-19" style="shape=image;html=1;verticalAlign=top;verticalLabelPosition=bottom;labelBackgroundColor=#ffffff;imageAspect=0;aspect=fixed;image=https://icons.diagrams.net/assets/infrastructure/1/Server-107.svg" value="" vertex="1">
          <mxGeometry height="100" width="50" x="40" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-17" parent="RjAdubwmxspqf5qblBbC-19" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=16;fontColor=light-dark(#000000,#000000);" value="&lt;div&gt;F-N01&lt;/div&gt;&lt;div&gt;192.168.2.3/24&lt;/div&gt;" vertex="1">
          <mxGeometry height="50" width="130" y="100" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-20" connectable="0" parent="1" style="group" value="" vertex="1">
          <mxGeometry height="150" width="130" x="695" y="307" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-21" parent="RjAdubwmxspqf5qblBbC-20" style="shape=image;html=1;verticalAlign=top;verticalLabelPosition=bottom;labelBackgroundColor=#ffffff;imageAspect=0;aspect=fixed;image=https://icons.diagrams.net/assets/infrastructure/1/Server-107.svg" value="" vertex="1">
          <mxGeometry height="100" width="50" x="40" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-22" parent="RjAdubwmxspqf5qblBbC-20" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=16;fontColor=light-dark(#000000,#000000);" value="&lt;div&gt;B-N01&lt;/div&gt;&lt;div&gt;192.168.1.2/24&lt;/div&gt;" vertex="1">
          <mxGeometry height="50" width="130" y="100" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-23" connectable="0" parent="1" style="group" value="" vertex="1">
          <mxGeometry height="150" width="130" x="865" y="307" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-24" parent="RjAdubwmxspqf5qblBbC-23" style="shape=image;html=1;verticalAlign=top;verticalLabelPosition=bottom;labelBackgroundColor=#ffffff;imageAspect=0;aspect=fixed;image=https://icons.diagrams.net/assets/infrastructure/1/Server-107.svg" value="" vertex="1">
          <mxGeometry height="100" width="50" x="40" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-25" parent="RjAdubwmxspqf5qblBbC-23" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=16;fontColor=light-dark(#000000,#000000);" value="&lt;div&gt;D-N01&lt;/div&gt;&lt;div&gt;192.168.1.3/24&lt;/div&gt;" vertex="1">
          <mxGeometry height="50" width="130" y="100" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-32" connectable="0" parent="1" style="group" value="" vertex="1">
          <mxGeometry height="110" width="130" x="694.995" y="505" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-27" parent="RjAdubwmxspqf5qblBbC-32" style="points=[[0.13,0.02,0],[0.5,0,0],[0.87,0.02,0],[0.885,0.4,0],[0.985,0.985,0],[0.5,1,0],[0.015,0.985,0],[0.115,0.4,0]];verticalLabelPosition=bottom;sketch=0;html=1;verticalAlign=top;aspect=fixed;align=center;pointerEvents=1;shape=mxgraph.cisco19.laptop2;fillColor=#fafafa;strokeColor=#005073;" value="" vertex="1">
          <mxGeometry height="60" width="85.71" x="22.144999999999982" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-29" parent="RjAdubwmxspqf5qblBbC-32" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=16;fontColor=light-dark(#000000,#000000);" value="&lt;div&gt;CLI-WIN-N01&lt;/div&gt;&lt;div&gt;192.168.1.100/24&lt;/div&gt;" vertex="1">
          <mxGeometry height="50" width="130" y="60" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-33" connectable="0" parent="1" style="group" value="" vertex="1">
          <mxGeometry height="110" width="130" x="864.995" y="505" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-28" parent="RjAdubwmxspqf5qblBbC-33" style="points=[[0.13,0.02,0],[0.5,0,0],[0.87,0.02,0],[0.885,0.4,0],[0.985,0.985,0],[0.5,1,0],[0.015,0.985,0],[0.115,0.4,0]];verticalLabelPosition=bottom;sketch=0;html=1;verticalAlign=top;aspect=fixed;align=center;pointerEvents=1;shape=mxgraph.cisco19.laptop2;fillColor=#fafafa;strokeColor=#005073;" value="" vertex="1">
          <mxGeometry height="60" width="85.71" x="22.144999999999982" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-30" parent="RjAdubwmxspqf5qblBbC-33" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=16;fontColor=light-dark(#000000,#000000);" value="&lt;div&gt;CLI-LNX-N01&lt;/div&gt;&lt;div&gt;192.168.1.101/24&lt;/div&gt;" vertex="1">
          <mxGeometry height="50" width="130" y="60" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-34" parent="1" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=16;fontColor=light-dark(#000000,#FFFFFF);" value="&lt;div&gt;ASIXc-ITB1&lt;/div&gt;&lt;div&gt;192.168.2.1/24&lt;/div&gt;" vertex="1">
          <mxGeometry height="50" width="130" x="280" y="290" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-35" parent="1" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=16;fontColor=light-dark(#000000,#FFFFFF);" value="&lt;div&gt;ASIXc-ITB2&lt;/div&gt;&lt;div&gt;192.168.1.1/24&lt;/div&gt;" vertex="1">
          <mxGeometry height="50" width="130" x="470" y="290" as="geometry" />
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-37" edge="1" parent="1" source="ADTn_hC_l_CTP9jVdpqL-23" style="endArrow=classic;startArrow=classic;html=1;rounded=0;exitX=1;exitY=0.5;exitDx=0;exitDy=0;entryX=0;entryY=0.5;entryDx=0;entryDy=0;edgeStyle=orthogonalEdgeStyle;" target="ADTn_hC_l_CTP9jVdpqL-1" value="">
          <mxGeometry height="50" relative="1" width="50" x="0.0002" as="geometry">
            <mxPoint x="320" y="440" as="sourcePoint" />
            <mxPoint x="380" y="590" as="targetPoint" />
          </mxGeometry>
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-39" edge="1" parent="1" source="ADTn_hC_l_CTP9jVdpqL-25" style="endArrow=classic;startArrow=classic;html=1;rounded=0;exitX=0;exitY=0.5;exitDx=0;exitDy=0;edgeStyle=orthogonalEdgeStyle;" target="ADTn_hC_l_CTP9jVdpqL-1" value="">
          <mxGeometry height="50" relative="1" width="50" x="0.0002" as="geometry">
            <mxPoint x="370" y="634" as="sourcePoint" />
            <mxPoint x="490" y="340" as="targetPoint" />
          </mxGeometry>
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-40" edge="1" parent="1" source="ADTn_hC_l_CTP9jVdpqL-3" style="endArrow=classic;startArrow=classic;html=1;rounded=0;entryX=0.5;entryY=0;entryDx=0;entryDy=0;exitX=0.5;exitY=0.864;exitDx=0;exitDy=0;exitPerimeter=0;" target="ADTn_hC_l_CTP9jVdpqL-6" value="">
          <mxGeometry height="50" relative="1" width="50" as="geometry">
            <mxPoint x="560" y="50" as="sourcePoint" />
            <mxPoint x="610" as="targetPoint" />
          </mxGeometry>
        </mxCell>
        <mxCell id="RjAdubwmxspqf5qblBbC-41" edge="1" parent="1" style="endArrow=classic;startArrow=classic;html=1;rounded=0;entryX=0.5;entryY=1;entryDx=0;entryDy=0;" target="ADTn_hC_l_CTP9jVdpqL-6" value="">
          <mxGeometry height="50" relative="1" width="50" as="geometry">
            <mxPoint x="440" y="310" as="sourcePoint" />
            <mxPoint x="620" y="140" as="targetPoint" />
          </mxGeometry>
        </mxCell>
        <mxCell id="tBIBYolfv5RpAuy-j7M3-2" parent="1" style="text;html=1;whiteSpace=wrap;strokeColor=none;fillColor=none;align=center;verticalAlign=middle;rounded=0;fontSize=16;fontColor=light-dark(#000000,#FFFFFF);" value="&lt;div&gt;Default Isard&lt;/div&gt;192.168.121.242" vertex="1">
          <mxGeometry height="50" width="130" x="440" y="233" as="geometry" />
        </mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
oading Tarea 1.2_ Diseño del Diagrama de Arquitectura de Red…]()

