```python
# Loading the libraries 
import requests 
from bs4 import BeautifulSoup
import pandas as pd 
import validators #to test if the link is valid or not 
```


```python
#loding the data from the excel file 
```


```python
df = pd.read_excel(r'atelier_stat.xlsx',header=0) # Lecture du fichier excel en un dataframe
```


```python
df 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>custumer name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>LARS SPENGELER -</td>
    </tr>
    <tr>
      <td>1</td>
      <td>SAN PIETRO SERVIZI SRL</td>
    </tr>
    <tr>
      <td>2</td>
      <td>ILLUMINANDO FIRENZE SRLS</td>
    </tr>
    <tr>
      <td>3</td>
      <td>SAVI IMMOBILIER</td>
    </tr>
    <tr>
      <td>4</td>
      <td>SERENA</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>95</td>
      <td>STUDIO LEGALE CUCCIATTI E ASSOCIATI DEGL</td>
    </tr>
    <tr>
      <td>96</td>
      <td>ULRICH BRAUN</td>
    </tr>
    <tr>
      <td>97</td>
      <td>GARAGE ROUSSET CHAVEROT</td>
    </tr>
    <tr>
      <td>98</td>
      <td>ENERGIES ET INNOVATIONS</td>
    </tr>
    <tr>
      <td>99</td>
      <td>SARL ARSAPRIM</td>
    </tr>
  </tbody>
</table>
<p>100 rows × 1 columns</p>
</div>




```python
l= [x for x in df['custumer name']] # Convert the column customer name into a list
```


```python
for i in l:
    print(i)
# print the name of the customers 
```

    LARS SPENGELER -
    SAN PIETRO SERVIZI SRL
    ILLUMINANDO FIRENZE SRLS
    SAVI IMMOBILIER
    SERENA
    MEYER WERBUNG
    SISTEMA DESIGN DI CARLI FEDERICO
    CAFE DALI
    AUX DELICES DE L ABBAYE
    MME FLORENCE YOONGRAM
    ARKASS VERSICHERUNGSMAKLER
    AERA CONCEPT
    MAINTECH SANTE
    CHEVALLIER CONSEIL
    EARL DU VERNET
    BUISSONNIERE ACTIVITES GITES SEJOURS
    ELICICOLTURA UMBRA
    STAATLICHE BERUFSSCHULE II BAYREUTH
    ELETTRA SNC DI VISMAN ANDREA E RIODA ANDREA
    MAIRIE DE BRETTNACH
    DR. MED.DENT. KNUT KARST ZAHNARZT
    ANDREAS FLEER
    BESTATTUNGEN JUNKER GMBH
    MAURICE JOEL
    MROTZEK ELEKTRO GMBH
    STUDIO PULIX GIORGIO S.A.S.
    A. HILL GMBH
    DINAMICA BIESSE S.R.L.
    MR CATALDO LASORSA
    SERTECO SRL
    ALPHADI AKADEMIE
    ASSOCIATION AIDE MENAGERE MILIEU RURAL
    RISTORANTE AUGUSTA
    MR N TIZRA
    CHRISTIAN HEIDER BAUGEWERBE
    DITTA GIUGLIANI VERONICA
    MONSIEUR CHRISTOPHE DEBEAULIEU
    BURKHARD SUDHOFF
    TOSKANAWORLD
    MANFRED KANTORSKI
    MAREIKE MARTIN PD09 PODOLOGIE
    SNC CASCAVEL SALVIGNAC - AU REND
    ALFRED SUPPAN
    VIOLA DHONAU GASTHAUS ZUR SCHAABE
    ABC NEON SNC
    MR DANIEL BRANDAZZI
    LE BAR DES AMIS
    MY VANITY DI MY ALESSIA
    PRO-TOPO
    CHAUSSURES DAVID
    TRANSKOM SAS
    TRANSPORT JLG EXPRESS
    CARAVANTOURS S.P.A.
    AP. PE.L.
    PARADOX BOUTIQUE
    PICCIRILLO ROCCO
    EDEN SERVICES ANIMALIERS
    STUDIO RECUPERO SALVATORE
    CORNELIA GERLACH LANDWIRTSCHAFT
    ASSOCIAZIONE NUOVA VITA ONLUS
    ZILS CONSULTING
    ODISIN
    MARTIN PUBLICITE
    DI PASQUALE FRANCESCO
    EURL HAMELIN PHILIPPE PIERRES ET TRADITIONS
    TAXI KIM GMBH
    BRESSION SEBASTIEN
    AUTOSERVIZI PIERSIGILLI
    BOTT SILVIO
    STUDIO DENTISTICO ALTOBELLI SERVIDEI
    STEUERBERATER SIEGFRIED SCHADL
    SICLET MANDOT CHRISTINE
    COIFFURE FANNY
    MÃœLLICH SICHERHEITSTECHNIK
    M.A.V. S.R.L.
    DR. ANGELIKA WENZEL
    SV MOTOR WILDAU E.V.
    AUTOSERVICE DALICHOW MEISTERBETRIEB
    FRISEUR IN DER KLOSTERPASSAGE
    KRAUS DENTAL-LABOR
    DIRSTAM SAS DI DI RUSSO M. & C.
    ARNOLD SZCZEPANSKI
    DAS FESTESSEN COUTELLE &
    BUCINO MARZIA
    FUCHS COHANA REBOUL & ASSOCIES
    FDST WOHNHEIM AM QUERSCHLAG
    YAMAHA XS 650 CLUB DE FRANCE
    BAR ROMA DI LIGORIO GIUSEPPE
    K. UND W. BERNAUER BAU-GMBH
    ETTEBA SARL
    LILIANE HEBERLE
    ELEKTRO GÃ–TZ
    CO GE I T SRL
    SOC NADINE ARNAU ET CIE
    JAN KOCANEK
    STUDIO LEGALE CUCCIATTI E ASSOCIATI DEGL
    ULRICH BRAUN
    GARAGE ROUSSET CHAVEROT
    ENERGIES ET INNOVATIONS
    SARL ARSAPRIM
    


```python
result = pd.DataFrame(columns=['Customer Name','Website','Facebook','Instagram','LinkedIn']) #creation of the dataframe that contains the informations to scrape
```


```python
for i in l : 
    
    new_row= {"Customer Name":"","Website":"","Facebook":"","Instagram":"","LinkedIn":""} 
    
    new_row["Customer Name"]=i 
    
    url = 'https://google.com/search?q='+urllib.parse.quote_plus(i) 
    
    request = urllib.request.Request(url) 
    
    request.add_header('User-Agent', 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36')
    
    raw_response = urllib.request.urlopen(request).read() 
    
    html = raw_response.decode("utf-8") 
    
    soup = BeautifulSoup(html, 'html.parser') 
    
    wb=soup.find('link',href=True) 
    
    if (wb!=None and validators.url(wb['href'], public=False)): 
        new_row["Website"]=wb['href']
        
        fb=soup.select_one('a[href*=facebook]') 
        if(fb!=None): 
            new_row["Facebook"]=fb['href']
        else: new_row["Facebook"]="Page Facebook Introuvable"

        ig=soup.select_one('a[href*=instagram]') 
        if(ig!=None): 
            new_row["Instagram"]=ig['href']
        else: new_row["Instagram"]="Profil Instagram Introuvable"

        li=soup.select_one('a[href*=linkedin]') 
        if(li!=None):  
            new_row["LinkedIn"]=li['href']
        else:new_row["LinkedIn"]="Profil LinkedIn introuvable"
    else:
        continue
    result = result.append(new_row, ignore_index=True) 
    
```


```python
result 
#print the final data frame 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Customer Name</th>
      <th>Website</th>
      <th>Facebook</th>
      <th>Instagram</th>
      <th>LinkedIn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>LARS SPENGELER -</td>
      <td>https://www.amw-spengeler.de/</td>
      <td>https://www.facebook.com/public/Lars-Spengler</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>1</td>
      <td>ILLUMINANDO FIRENZE SRLS</td>
      <td>http://illuminando.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>2</td>
      <td>SAVI IMMOBILIER</td>
      <td>https://www.savi-immobilier.fr/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>3</td>
      <td>MEYER WERBUNG</td>
      <td>https://meyer-werbung.de/</td>
      <td>Page Facebook Introuvable</td>
      <td>https://www.instagram.com/neubertwerbung_neust...</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>4</td>
      <td>ARKASS VERSICHERUNGSMAKLER</td>
      <td>https://arkass-stein.de/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://de.linkedin.com/in/annette-ahmed-stein...</td>
    </tr>
    <tr>
      <td>5</td>
      <td>CHEVALLIER CONSEIL</td>
      <td>https://chevallierconseil.fr/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://fr.linkedin.com/company/d--chevallier-...</td>
    </tr>
    <tr>
      <td>6</td>
      <td>STAATLICHE BERUFSSCHULE II BAYREUTH</td>
      <td>https://www.kfm-berufsschule-bayreuth.de/</td>
      <td>https://m.facebook.com/profile.php?id=11550960...</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://at.linkedin.com/school/staatliche-beru...</td>
    </tr>
    <tr>
      <td>7</td>
      <td>DR. MED.DENT. KNUT KARST ZAHNARZT</td>
      <td>https://www.dr-karst.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>8</td>
      <td>DINAMICA BIESSE S.R.L.</td>
      <td>http://www.dinamicabiesse.it/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>9</td>
      <td>SERTECO SRL</td>
      <td>https://www.serteco.biz/</td>
      <td>https://www.facebook.com/sertecosrl/</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://it.linkedin.com/company/serteco-s-r-l-</td>
    </tr>
    <tr>
      <td>10</td>
      <td>ASSOCIATION AIDE MENAGERE MILIEU RURAL</td>
      <td>https://www.admr.org/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>11</td>
      <td>MAREIKE MARTIN PD09 PODOLOGIE</td>
      <td>https://www.podologie-koenitz.de/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>12</td>
      <td>CARAVANTOURS S.P.A.</td>
      <td>https://www.caravantours.it/</td>
      <td>https://www.facebook.com/caravantourstouropera...</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>13</td>
      <td>EDEN SERVICES ANIMALIERS</td>
      <td>http://www.eden-servicesanimaliers.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>14</td>
      <td>ZILS CONSULTING</td>
      <td>https://www.zils-consulting.com/</td>
      <td>https://ar-ar.facebook.com/ZilsConsulting</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://fr.linkedin.com/in/jean-fran%C3%A7ois-...</td>
    </tr>
    <tr>
      <td>15</td>
      <td>BRESSION SEBASTIEN</td>
      <td>https://champagnebression-s.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>16</td>
      <td>AUTOSERVIZI PIERSIGILLI</td>
      <td>https://autoservizipiersigilli.it/</td>
      <td>https://www.facebook.com/Piersigilliviaggisrl/</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>17</td>
      <td>M.A.V. S.R.L.</td>
      <td>https://www.mavsrl.net/</td>
      <td>https://www.facebook.com/MachinesAgricolesVald...</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://ae.linkedin.com/company/m.a.v.-s.r.l.</td>
    </tr>
    <tr>
      <td>18</td>
      <td>SV MOTOR WILDAU E.V.</td>
      <td>https://www.svmotorwildau.de/</td>
      <td>https://www.facebook.com/pages/SV-Motor-Wildau...</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>19</td>
      <td>AUTOSERVICE DALICHOW MEISTERBETRIEB</td>
      <td>https://www.autoservice-dalichow.de/</td>
      <td>https://de-de.facebook.com/Autoservice-Dalicho...</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>20</td>
      <td>KRAUS DENTAL-LABOR</td>
      <td>https://www.oralelegance.de/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>21</td>
      <td>FUCHS COHANA REBOUL &amp; ASSOCIES</td>
      <td>http://reboulassocies.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://fr.linkedin.com/in/s%C3%A9bastien-roug...</td>
    </tr>
    <tr>
      <td>22</td>
      <td>ETTEBA SARL</td>
      <td>http://www.etteba.com/</td>
      <td>https://fr-fr.facebook.com/etteba.elec/</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://fr.linkedin.com/in/veillard-monique-28...</td>
    </tr>
    <tr>
      <td>23</td>
      <td>CO GE I T SRL</td>
      <td>https://cogeit.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>24</td>
      <td>STUDIO LEGALE CUCCIATTI E ASSOCIATI DEGL</td>
      <td>http://www.studiocucciatti.eu/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>25</td>
      <td>ENERGIES ET INNOVATIONS</td>
      <td>https://www.energies-innovations.fr/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
    </tr>
    <tr>
      <td>26</td>
      <td>SARL ARSAPRIM</td>
      <td>https://arsaprim.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://fr.linkedin.com/company/sarl-arsaprim</td>
    </tr>
  </tbody>
</table>
</div>




```python
l = [x for x in result["Website"]] #creation of list containing the official websites 
```


```python
for i in l : print(i) #printing the links
```

    https://www.amw-spengeler.de/
    http://illuminando.com/
    https://www.savi-immobilier.fr/
    https://meyer-werbung.de/
    https://arkass-stein.de/
    https://chevallierconseil.fr/
    https://www.kfm-berufsschule-bayreuth.de/
    https://www.dr-karst.com/
    http://www.dinamicabiesse.it/
    https://www.serteco.biz/
    https://www.admr.org/
    https://www.podologie-koenitz.de/
    https://www.caravantours.it/
    http://www.eden-servicesanimaliers.com/
    https://www.zils-consulting.com/
    https://champagnebression-s.com/
    https://autoservizipiersigilli.it/
    https://www.mavsrl.net/
    https://www.svmotorwildau.de/
    https://www.autoservice-dalichow.de/
    https://www.oralelegance.de/
    http://reboulassocies.com/
    http://www.etteba.com/
    https://cogeit.com/
    http://www.studiocucciatti.eu/
    https://www.energies-innovations.fr/
    https://arsaprim.com/
    


```python
#search of the logos
s=0
links=[]
for i in l:
    request = urllib.request.Request(i)
    
    request.add_header('User-Agent', 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36')
    
    response = urllib.request.urlopen(request).read()
    
    html=response.decode("utf-8")
    
    soup = BeautifulSoup(html, 'html.parser')
    
    tags=soup.select_one('img[src*=logo]')
    
    x=soup.find('img',src=True)
    if (tags==None and x==None):
        links.append('No Logo')
    
    else:
        if(tags!=None):
            links.append(tags)
            print(links[s])
            s=s+1  
    
        else:
            if(x!=None and tags==None):
                links.append(x)
                print(links[s])
                s=s+1
                
```

    <img alt="" id="emotion-header-logo" src="https://www.amw-spengeler.de/s/misc/logo.jpg?t=1616307417"/>
    <img alt="Logo" class="normal" itemprop="image" src="http://illuminando.com/wp-content/uploads/2016/03/logo_illuminando.png"/>
    <img alt="SAVI IMMOBILIER" src="https://www.meilleursagents.com/static/mypro/images/logo.svg?c07c1b494788581f1b1b6070062b3a9c" width="144"/>
    <img alt="Werbeatelier Meyer" height="80" src="/images/meyer/WM_Logo_Relaunch_RGB_grau.svg"/>
    <img alt="" class="attachment-large size-large" height="137" loading="lazy" sizes="(max-width: 900px) 100vw, 900px" src="https://arkass-stein.de/wp-content/uploads/arkass-logo3.jpg" srcset="https://arkass-stein.de/wp-content/uploads/arkass-logo3.jpg 960w, https://arkass-stein.de/wp-content/uploads/arkass-logo3-300x46.jpg 300w, https://arkass-stein.de/wp-content/uploads/arkass-logo3-768x117.jpg 768w" width="900"/>
    <img alt="Merck" class="" src="https://chevallierconseil.fr/wp-content/uploads/2020/03/logo_249x207__0006_merck-e1586248769984.jpg"/>
    <img src="styles/images/spacer.gif"/>
    <img alt="Dr. Knut Karst - Ilmenau" src="/images/logo_karst.png"/>
    <img alt="Dinamica Biesse" class="logo-main scale-with-grid" src="http://www.dinamicabiesse.it/wp-content/uploads/2015/06/DinamicaBiesse_logo.png"/>
    <img height="1" src="https://www.facebook.com/tr?id=2017101355196726&amp;ev=PageView&amp;noscript=1" style="display:none" width="1"/>
    <img alt="logo personia" src="/system/files/webmaster/structure/logo-personia.png" style="width: 203px; height: 64px;"/>
    <img alt="Logo" class="uk-overlay-scale" src="/media/widgetkit/logo_mm-74474ac45e636f086a96d802439ff19b.png" width="270px"/>
    <img alt="Caravantours Tour" id="logo" src="/assets/img/logo.png"/>
    <img alt="" id="svgImg-11006" src="ewExternalFiles/logo_edendog-3.svg" style="width:282px"/>
    <img alt="ZILS Consulting" src="//www.zils-consulting.com/formation/wp-content/uploads/2019/03/logo-zils-consulting.png"/>
    <img alt="Champagne BRESSION Sébastien" data-height-percentage="85" id="logo" src="https://champagnebression-s.com/wp-content/uploads/2019/05/logo-header.png"/>
    <img src="https://autoservizipiersigilli.it/wp-content/uploads/2017/10/Logo-slogan2.jpg" style="height:100px; width:413px;"/>
    <img alt="" class="attachment-large size-large" height="181" sizes="(max-width: 302px) 100vw, 302px" src="https://cdn.shortpixel.ai/client/q_glossy,ret_img,w_302/https://www.mavsrl.net/mav-rolling-form-machines/wp-content/uploads/2018/01/logomav.png" srcset="https://cdn.shortpixel.ai/client/q_glossy,ret_img,w_302/https://www.mavsrl.net/mav-rolling-form-machines/wp-content/uploads/2018/01/logomav.png 302w, https://cdn.shortpixel.ai/client/q_glossy,ret_img,w_300/https://www.mavsrl.net/mav-rolling-form-machines/wp-content/uploads/2018/01/logomav-300x180.png 300w" width="302"/>
    <img alt="header_logo.png" src="cache/84951.png"/>
    No Logo
    <img alt="Dentallabor Horst-Dieter Kraus GmbH - Logo" src="https://www.oralelegance.de/images/layout/branding.png"/>
    <img alt="Cabinet Reboul &amp; Associé" src="img/logo.svg"/>
    <img border="0" height="278" src="http://www.etteba.com/images/01_JPGWeb.jpg" width="677"/>
    <img alt="COGEIT" class="img-responsive center-block" height="60" src="http://cogeit.com/wp-content/uploads/2018/01/logo-cogeit_web-1.png" width="179"/>
    No Logo
    


```python
# extraction of the links source for the logos 

logo_src=[]
s=0
for i in links:
    if (i!='No Logo'):
        x=i['src']
        if (l[s])in x :
            print(s+1,x) 
            logo_src.append(x)
            s=s+1
        elif ((('http://' in x) or ('https://' in x) )  and (l[s] not in x)):
            print(s+1,x)
            logo_src.append(x)
            s=s+1
        elif (x[0]=='/' and l[s] not in x):
            print(s+1,l[s]+x[1:])
            logo_src.append(l[s]+x[1:])
            s=s+1
        elif (x[:1]=='//' and l[s] in x):
            print(s+1,x[2:])
            logo_src.append(x[2:])
            s=s+1
        else:
            print(s+1,l[s]+x)
            logo_src.append(l[s]+x)
            s=s+1
            
    else:
        logo_src.append(i)
```

    1 https://www.amw-spengeler.de/s/misc/logo.jpg?t=1616307417
    2 http://illuminando.com/wp-content/uploads/2016/03/logo_illuminando.png
    3 https://www.meilleursagents.com/static/mypro/images/logo.svg?c07c1b494788581f1b1b6070062b3a9c
    4 https://meyer-werbung.de/images/meyer/WM_Logo_Relaunch_RGB_grau.svg
    5 https://arkass-stein.de/wp-content/uploads/arkass-logo3.jpg
    6 https://chevallierconseil.fr/wp-content/uploads/2020/03/logo_249x207__0006_merck-e1586248769984.jpg
    7 https://www.kfm-berufsschule-bayreuth.de/styles/images/spacer.gif
    8 https://www.dr-karst.com/images/logo_karst.png
    9 http://www.dinamicabiesse.it/wp-content/uploads/2015/06/DinamicaBiesse_logo.png
    10 https://www.facebook.com/tr?id=2017101355196726&ev=PageView&noscript=1
    11 https://www.admr.org/system/files/webmaster/structure/logo-personia.png
    12 https://www.podologie-koenitz.de/media/widgetkit/logo_mm-74474ac45e636f086a96d802439ff19b.png
    13 https://www.caravantours.it/assets/img/logo.png
    14 http://www.eden-servicesanimaliers.com/ewExternalFiles/logo_edendog-3.svg
    15 https://www.zils-consulting.com//www.zils-consulting.com/formation/wp-content/uploads/2019/03/logo-zils-consulting.png
    16 https://champagnebression-s.com/wp-content/uploads/2019/05/logo-header.png
    17 https://autoservizipiersigilli.it/wp-content/uploads/2017/10/Logo-slogan2.jpg
    18 https://cdn.shortpixel.ai/client/q_glossy,ret_img,w_302/https://www.mavsrl.net/mav-rolling-form-machines/wp-content/uploads/2018/01/logomav.png
    19 https://www.svmotorwildau.de/cache/84951.png
    20 https://www.oralelegance.de/images/layout/branding.png
    21 https://www.oralelegance.de/img/logo.svg
    22 http://www.etteba.com/images/01_JPGWeb.jpg
    23 http://cogeit.com/wp-content/uploads/2018/01/logo-cogeit_web-1.png
    24 https://cogeit.com/_media/img/thumb/20150613-122946.jpg
    25 https://i2.wp.com/arsaprim.com/wp-content/uploads/2020/11/logo-arsaprim-1.png?fit=482%2C158&ssl=1
    


```python
result.insert(5, "Logo Source", logo_src, True) 
#adding a new column to the result table with the logos  
```


```python
result #final table
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Customer Name</th>
      <th>Website</th>
      <th>Facebook</th>
      <th>Instagram</th>
      <th>LinkedIn</th>
      <th>Logo Source</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>LARS SPENGELER -</td>
      <td>https://www.amw-spengeler.de/</td>
      <td>https://www.facebook.com/public/Lars-Spengler</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>https://www.amw-spengeler.de/s/misc/logo.jpg?t...</td>
    </tr>
    <tr>
      <td>1</td>
      <td>ILLUMINANDO FIRENZE SRLS</td>
      <td>http://illuminando.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>http://illuminando.com/wp-content/uploads/2016...</td>
    </tr>
    <tr>
      <td>2</td>
      <td>SAVI IMMOBILIER</td>
      <td>https://www.savi-immobilier.fr/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>https://www.meilleursagents.com/static/mypro/i...</td>
    </tr>
    <tr>
      <td>3</td>
      <td>MEYER WERBUNG</td>
      <td>https://meyer-werbung.de/</td>
      <td>Page Facebook Introuvable</td>
      <td>https://www.instagram.com/neubertwerbung_neust...</td>
      <td>Profil LinkedIn introuvable</td>
      <td>https://meyer-werbung.de/images/meyer/WM_Logo_...</td>
    </tr>
    <tr>
      <td>4</td>
      <td>ARKASS VERSICHERUNGSMAKLER</td>
      <td>https://arkass-stein.de/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://de.linkedin.com/in/annette-ahmed-stein...</td>
      <td>https://arkass-stein.de/wp-content/uploads/ark...</td>
    </tr>
    <tr>
      <td>5</td>
      <td>CHEVALLIER CONSEIL</td>
      <td>https://chevallierconseil.fr/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://fr.linkedin.com/company/d--chevallier-...</td>
      <td>https://chevallierconseil.fr/wp-content/upload...</td>
    </tr>
    <tr>
      <td>6</td>
      <td>STAATLICHE BERUFSSCHULE II BAYREUTH</td>
      <td>https://www.kfm-berufsschule-bayreuth.de/</td>
      <td>https://m.facebook.com/profile.php?id=11550960...</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://at.linkedin.com/school/staatliche-beru...</td>
      <td>https://www.kfm-berufsschule-bayreuth.de/style...</td>
    </tr>
    <tr>
      <td>7</td>
      <td>DR. MED.DENT. KNUT KARST ZAHNARZT</td>
      <td>https://www.dr-karst.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>https://www.dr-karst.com/images/logo_karst.png</td>
    </tr>
    <tr>
      <td>8</td>
      <td>DINAMICA BIESSE S.R.L.</td>
      <td>http://www.dinamicabiesse.it/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>http://www.dinamicabiesse.it/wp-content/upload...</td>
    </tr>
    <tr>
      <td>9</td>
      <td>SERTECO SRL</td>
      <td>https://www.serteco.biz/</td>
      <td>https://www.facebook.com/sertecosrl/</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://it.linkedin.com/company/serteco-s-r-l-</td>
      <td>https://www.facebook.com/tr?id=201710135519672...</td>
    </tr>
    <tr>
      <td>10</td>
      <td>ASSOCIATION AIDE MENAGERE MILIEU RURAL</td>
      <td>https://www.admr.org/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>https://www.admr.org/system/files/webmaster/st...</td>
    </tr>
    <tr>
      <td>11</td>
      <td>MAREIKE MARTIN PD09 PODOLOGIE</td>
      <td>https://www.podologie-koenitz.de/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>https://www.podologie-koenitz.de/media/widgetk...</td>
    </tr>
    <tr>
      <td>12</td>
      <td>CARAVANTOURS S.P.A.</td>
      <td>https://www.caravantours.it/</td>
      <td>https://www.facebook.com/caravantourstouropera...</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>https://www.caravantours.it/assets/img/logo.png</td>
    </tr>
    <tr>
      <td>13</td>
      <td>EDEN SERVICES ANIMALIERS</td>
      <td>http://www.eden-servicesanimaliers.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>http://www.eden-servicesanimaliers.com/ewExter...</td>
    </tr>
    <tr>
      <td>14</td>
      <td>ZILS CONSULTING</td>
      <td>https://www.zils-consulting.com/</td>
      <td>https://ar-ar.facebook.com/ZilsConsulting</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://fr.linkedin.com/in/jean-fran%C3%A7ois-...</td>
      <td>https://www.zils-consulting.com//www.zils-cons...</td>
    </tr>
    <tr>
      <td>15</td>
      <td>BRESSION SEBASTIEN</td>
      <td>https://champagnebression-s.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>https://champagnebression-s.com/wp-content/upl...</td>
    </tr>
    <tr>
      <td>16</td>
      <td>AUTOSERVIZI PIERSIGILLI</td>
      <td>https://autoservizipiersigilli.it/</td>
      <td>https://www.facebook.com/Piersigilliviaggisrl/</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>https://autoservizipiersigilli.it/wp-content/u...</td>
    </tr>
    <tr>
      <td>17</td>
      <td>M.A.V. S.R.L.</td>
      <td>https://www.mavsrl.net/</td>
      <td>https://www.facebook.com/MachinesAgricolesVald...</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://ae.linkedin.com/company/m.a.v.-s.r.l.</td>
      <td>https://cdn.shortpixel.ai/client/q_glossy,ret_...</td>
    </tr>
    <tr>
      <td>18</td>
      <td>SV MOTOR WILDAU E.V.</td>
      <td>https://www.svmotorwildau.de/</td>
      <td>https://www.facebook.com/pages/SV-Motor-Wildau...</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>https://www.svmotorwildau.de/cache/84951.png</td>
    </tr>
    <tr>
      <td>19</td>
      <td>AUTOSERVICE DALICHOW MEISTERBETRIEB</td>
      <td>https://www.autoservice-dalichow.de/</td>
      <td>https://de-de.facebook.com/Autoservice-Dalicho...</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>No Logo</td>
    </tr>
    <tr>
      <td>20</td>
      <td>KRAUS DENTAL-LABOR</td>
      <td>https://www.oralelegance.de/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>https://www.oralelegance.de/images/layout/bran...</td>
    </tr>
    <tr>
      <td>21</td>
      <td>FUCHS COHANA REBOUL &amp; ASSOCIES</td>
      <td>http://reboulassocies.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://fr.linkedin.com/in/s%C3%A9bastien-roug...</td>
      <td>https://www.oralelegance.de/img/logo.svg</td>
    </tr>
    <tr>
      <td>22</td>
      <td>ETTEBA SARL</td>
      <td>http://www.etteba.com/</td>
      <td>https://fr-fr.facebook.com/etteba.elec/</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://fr.linkedin.com/in/veillard-monique-28...</td>
      <td>http://www.etteba.com/images/01_JPGWeb.jpg</td>
    </tr>
    <tr>
      <td>23</td>
      <td>CO GE I T SRL</td>
      <td>https://cogeit.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>http://cogeit.com/wp-content/uploads/2018/01/l...</td>
    </tr>
    <tr>
      <td>24</td>
      <td>STUDIO LEGALE CUCCIATTI E ASSOCIATI DEGL</td>
      <td>http://www.studiocucciatti.eu/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>No Logo</td>
    </tr>
    <tr>
      <td>25</td>
      <td>ENERGIES ET INNOVATIONS</td>
      <td>https://www.energies-innovations.fr/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>Profil LinkedIn introuvable</td>
      <td>https://cogeit.com/_media/img/thumb/20150613-1...</td>
    </tr>
    <tr>
      <td>26</td>
      <td>SARL ARSAPRIM</td>
      <td>https://arsaprim.com/</td>
      <td>Page Facebook Introuvable</td>
      <td>Profil Instagram Introuvable</td>
      <td>https://fr.linkedin.com/company/sarl-arsaprim</td>
      <td>https://i2.wp.com/arsaprim.com/wp-content/uplo...</td>
    </tr>
  </tbody>
</table>
</div>




```python
result.to_csv(r'Output.csv') # exporting the result to a csv file 
```


```python
# Les étapes suivantes sont facultatives et servent à juste télécharger les logos sur votre pc.
```


```python
files_names= [x for x in result["Customer Name"]] #pour créer les noms des logos à télécharger
```


```python
#to downlaod the logo 
i=0
for i in range(len(logo_src)):
    try :
        url = logo_src[i]
        filename= files_names[i]
        urllib.request.urlretrieve(url, filename)
    except: 
        pass
```


```python

```
