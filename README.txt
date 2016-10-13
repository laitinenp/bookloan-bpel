T‰ss‰ kansiossa on OpenESB-v‰lineell‰ laadittu BPEL-malli BPMN-standardista lˆytyv‰st‰ BookLoan-prosessista.

Opiskelemme suorituskelpoisen BPMN-mallin eli BPEL-mallin tekemist‰ OpenESB-v‰lineell‰.

Lis‰ksi koneeseen tulee olla asennettuna java jdk (esim. Java JDK 1.7.0).

1. Rekisterˆidy palvelussa open-esb.net.

2. Lataa OpenESB-palvelin ja OpenESB Studio koneellesi ja pura zip-paketti esimerkiksi C-kansioon (Win).

http://www.open-esb.net/index.php?option=com_content&view=article&id=113

3. K‰ynnist‰ komentorivi ja siirry kansioon C:\OpenESB-SE-3.0.5\OE-Instance\bin

4. Suorita komento .\setenv.bat

5. Suorita komento .\openesb.bat

Kokeile hallintasovellusta selaimella http://localhost:4848/plugin/webui/#/login (k‰ytt‰j‰tunnus admin, salasana admin).

6. Perehdy dokumentteihin

770-001 Startup with OpenESB Standalone

ja

770-002 OpenESB Standalone Edition Hello World

sivulla [http://www.open-esb.net/index.php?option=com_content&view=article&id=86].

7. Laadi OpenESB Standalone Edition Hello World -esimerkkimalli ja kokeile sen toimintaa palvelimessa.

----------------- OSA 2: Ohjausrakenteet ---------------

8. BPMN-oppimateriaaleissa esiintyy seuraava kirjaston lainausprosessiin liittyv‰ liiketoimintaprosessi:

Prosessim‰‰ritys ei ole aivan ongelmaton. Kriittist‰ tarkastelua lˆytyy esimerkiksi blogista [http://brsilver.com/bpmn-method-and-style-an-example/]. Blogia kannattaa k‰ytt‰‰ hyv‰ksi teht‰v‰n ratkaisussa.

Prosessilla on yksi asiakas: Borrower. Lainaaja l‰hett‰‰ kirjan lainauspyynnˆn (Book Request: bookid, customerid). Prosessi k‰ytt‰‰ tietokantaa (olkoon sen nimi vaikkapa BookStatus). BookStatus-tietokannasta k‰ytet‰‰n seuraavia palveluja:

    GetBookStatus(bookid) => (On Loan / On Hold / Available / NotAvailable)
    UpdateBookStatus(bookId, customerId) -- t‰m‰n j‰lkeen On Loan
    HoldBook(bookId, customerId) -- t‰m‰n j‰lkeen On Hold

Lainausprosessi l‰hett‰‰ lainaajalle seuraavia viestej‰ (musta kirjekuori -symboli):

    Offer to Hold
    Hold Confirmation
    Book Request Cancelled

Laadi BookStatus-tietokannan WSDL-liittym‰kuvaus ja perusperaatiot BPEL-prosesseilla. Meill‰ ei ole viel‰ aitoa tietokantaa k‰ytett‰viss‰mme, mutta voimme simuloida testiaineistoa yksinkertaisesti seuraavasti:

    jos bookid on 1, niin kirjan status on alussa Available
    jos bookid on 2, niin kirjan status on alussa OnLoan
    jos bookid on 3, niin kirjan status on alussa OnHold
    jos bookid on joku muu kuin 1 - 3, niin kirjan status on NotAvailable.

SetStatus-operaation ei tarvitse tehd‰ yht‰‰n mit‰‰n, mutta sen on hyv‰ olla olemassa BookLoan-prosessin kehitt‰mist‰ ja testausta varten.
------------VIIKKO 39---------------
9. Viikon koodaushaaste: toteuta kirjanlainausprosessi l‰hteen [http://brsilver.com/bpmn-method-and-style-an-example/] parannatun prosessikuvauksen mukaisesti. K‰yt‰ hyv‰ksesi teht‰v‰ss‰ 8 m‰‰ritelty‰ BookStatus-prosessia. T‰m‰n tekemisess‰ joudut tekem‰‰n v‰hint‰‰n seuraavia asioita:

    prosessin wsdl-kuvaus sanomam‰‰rityksineen (esim. BookRequest, OfferToHold, HoldRequest)
    BookStatus-prosessin hyˆdynt‰minen
    eksklusiivisen tietoon perustuvan p‰‰tˆksen koodaaminen (valitse elementti ja vaihda v‰lilehti "Mapper")
    ajastimen koodaus (valitse timer-elementti ja vaihda v‰lilehti "Mapper")
    eksklusiivisen dataan perustuvan p‰‰tˆksen koodaaminen (valitse if-elementti ja vaihda v‰lilehti "Mapper")
    eksklusiivisen tapahtumaan perustuvan p‰‰tˆksen koodaaminen

Viimeisen kohdan tekemiseksi joudut opiskelemaan viel‰ kuinka prosessin korrelaatio m‰‰ritell‰‰n. Aloita vaikka OpenESB-v‰lineeseen liittyv‰st‰ dokumentista [https://blogs.oracle.com/jason/entry/constraints_on_bpel_correlation_in]
