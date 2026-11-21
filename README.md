# automated-multi-site-evpn-vxlan
Automatisoitu multi-tenant multi-site VXLAN fabric -konesaliverkon toteutus Ciscon Nexus 9000v -virtuaalikytkimillä. Toteutettu XAMKin VirtualLab-alustalla. Tämä projekti pohjautuu aikaisempaan [BGP EVPN VXLAN](https://github.com/kiibler/cisco-nexus-bgp-evpn-vxlan) projektiini. Tämän projektin kuvaus keskittyy verkon multi-site osa-alueeseen ja sen toteutuksen automatisointiin Ansiblella.

## Verkon topologia

![Verkon topologia](topologia/multi-site-new.png)

Alueiden internal-verkot on rakennettu hyödyntäen Spine Leaf -topologiaa. Niitä on laajettennu toisella Leaf -tasolla, johon alueiden rajoilla toimivat Border Gateway -laitteet on sijoitettu. Super Spine -taso muodostaa datakeskuksen runkoverkon, jonka kautta alueet yhdistetään toisiinsa. Leaf-tason Nexus 9000v -kytkimet on yhdistetty Trunk linkeillä Proxmox Virtual Environment -palvelimiin. Muut linkit ovat L3 reititettyjä portteja.

## Toteutuksen automatisointi

Verkon toteutus on automatisoitu Ansiblen avulla. Laitteiden käyttöönotto ja konfigurointi on jaettu viiteen osa-alueeseen:

1. Alustavat asetukset. Tässä vaiheessa laitteille konfiguroidaan hostnimet, tarvittavat asetukset ja IPv4 osoitteet. Lisäksi tarvittavat prosessit, kuten **BGP**, laitetaan päälle.
2. Internal-underlay. Tässä vaiheessa laitteille konfiguroidaan alueiden rajojen sisällä toimiva **OSPFv2** underlay-verkko.
3. Internal-overlay. Tässä vaiheessa laitteille konfiguroidaan alueiden rajojen sisällä toimiva **iBGP EVPN** overlay-verkko. Lisäksi laitteille konfiguroidaan asiakasverkkojen segmentointiin tarvittavat **VLANit** ja **L2VNI:t** sekä asiakasverkkojen eristykseen tarvittavat reitityskontekstit eli **VRFt** ja **L3VNI:t**.
4. External-underlay. Tässä vaiheessa laitteille konfiguroidaan alueiden yhdistämiseen tarvittava **eBGP IPv4** underlay-verkko.
5. External-overlay. Tässä vaiheessa laitteille konfiguroidaan alueiden yhdistämiseen ja asiakasverkkojen jatkamiseen tarvittava **eBGP EVPN** overlay-verkko.

Laitteet konfiguroitiin datakeskuksen erillisen hallintaverkon (`MGMT`) kautta.
