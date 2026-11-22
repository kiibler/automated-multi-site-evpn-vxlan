# automated-multi-site-evpn-vxlan
Automatisoitu multi-tenant multi-site VXLAN fabric -konesaliverkon toteutus Ciscon Nexus 9000v -virtuaalikytkimillä. Toteutettu XAMKin VirtualLab-alustalla. Tämä projekti pohjautuu aikaisempaan [BGP EVPN VXLAN](https://github.com/kiibler/cisco-nexus-bgp-evpn-vxlan) projektiini. Tämän projektin kuvaus keskittyy verkon multi-site osa-alueeseen ja sen toteutuksen automatisointiin Ansiblella.

## Verkon topologia

![Verkon topologia](topologia/multi-site-new.png)

Alueiden internal-verkot on rakennettu hyödyntäen Spine Leaf -topologiaa. Niitä on laajettennu toisella Leaf -tasolla, johon alueiden rajoilla toimivat Border Gateway -laitteet on sijoitettu. Super Spine -taso muodostaa datakeskuksen runkoverkon, jonka kautta alueet yhdistetään toisiinsa. Leaf-tason Nexus 9000v -kytkimet on yhdistetty Trunk linkeillä Proxmox Virtual Environment -palvelimiin. Muut linkit ovat L3 reititettyjä portteja.

## Toteutuksen automatisointi

Verkon toteutus on automatisoitu Ansiblen avulla. Toteutuksen automatisointi suunniteltiin siten, että laitteille määriteltiin ID ja rooli. Roolin perusteella laitteet jaettiin ryhmiin: `vteps`, `spines`, `bgws`, `nves` ja `dc_core`. Lisäksi laitteet jaettiin ryhmiin: `dc1`, `site1_nves` ja `site2_nves` niiden sijainnin perusteella. Laitekohtaisen Id:n, roolin sekä datakeskus- ja aluekohtaisten muuttujien avulla laitteille määriteltiin dynaamisesti tarvittavat muuttujat, kuten: `ASN`, `hostname`
 ja IPv4 osoitteet.
 
Laitekohtaiset ID:t jaettiin seuraavan taulukon mukaan:

| Rooli | Device-Id |
| :--- | :--- |
| SERVER |  1-99 |
| VTEP | 100-129 |
| SPINE | 130-159 |
| BGW | 160-189 |
| CORE | 190-219 |
| Reserved | 220-254 |

IPv4 osoitteet muodostettiin seuraavan taulukon mukaan:

| Use | Prefix | 2nd Octet | 3rd Octet | 4th Octet |
| :--- | :--- | :--- | :--- | :--- |
| MGMT | `192.168.0.0/16` | - | DC-Id + Site-Id | Device-Id |
| RID | `10.0.0.0/8` | 0 | DC-Id + Site-Id | Device-Id |
| VTEP PIP | `10.0.0.0/8` | 1 | DC-Id + Site-Id | Device-Id |
| Multi-site VIP | `10.0.0.0/8` | 100 | DC-Id + Site-Id | 1 |
| Site-external links | `10.0.0.0/8` | BGW Device-Id | Core Device-Id | 1 (BGWs) / 2 (COREs)

Laitteiden käyttöönotto ja konfigurointi on jaettu viiteen osa-alueeseen:

1. Alustavat asetukset. Tässä vaiheessa laitteille konfiguroidaan hostnimet, tarvittavat asetukset ja IPv4 osoitteet. Lisäksi tarvittavat prosessit, kuten **BGP**, laitetaan päälle.
2. Internal-underlay. Tässä vaiheessa laitteille konfiguroidaan alueiden rajojen sisällä toimiva **OSPFv2** underlay-verkko.
3. Internal-overlay. Tässä vaiheessa laitteille konfiguroidaan alueiden rajojen sisällä toimiva **iBGP EVPN** overlay-verkko. Lisäksi laitteille konfiguroidaan asiakasverkkojen segmentointiin tarvittavat **VLANit** ja **L2VNI:t** sekä asiakasverkkojen eristykseen tarvittavat reitityskontekstit eli **VRFt** ja **L3VNI:t**.
4. External-underlay. Tässä vaiheessa laitteille konfiguroidaan alueiden yhdistämiseen tarvittava **eBGP IPv4** underlay-verkko.
5. External-overlay. Tässä vaiheessa laitteille konfiguroidaan alueiden yhdistämiseen ja asiakasverkkojen jatkamiseen tarvittava **eBGP EVPN** overlay-verkko.

Laitteet konfiguroitiin datakeskuksen erillisen hallintaverkon (`MGMT`) ja laitteiden hallintarajapinnan (`mgmt0`) kautta.
