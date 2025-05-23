# Akciju cenu scraper

## Projekta apraksts

### Projekta mērķis

Šī projekta mērķis bija izstrādāt kodu, kas ar Python palīdzību reālā laikā iegūst dažādu akciju cenas.  
Šāda programma atver iespēju lietotājam novērot izmaiņas akcijās un pašam veidot akciju vērtību grafikus, kā arī to nākotnes ceļu prognozes.

---

## Koda darbība

### 1. Bibliotēku ielāde

| Bibliotēka     | Tās izmantošanas skaidrojums |
|----------------|------------------------------|
| `json`         | Tiek izmantots, lai ielādētu atsevišķā `.json` failā saglabātos akciju nosaukumus un ticker kodus. |
| `requests`     | Lai nosūtītu HTTP pieprasījumu Yahoo Finance un saņemtu HTML saturu. |
| `BeautifulSoup`| Tiek izmantots HTML satura pārskatei un vērtību atrašanai. |
| `time`         | Tiek izmantots priekš laika atsauces terminālī un Excel failā, kā arī lai mākslīgi uzstādītu aizkavi (samazina IP adreses bloķēšanas iespējamību). |
| `openpyxl`     | Tiek izmantots, lai ielādētu, modificētu un veiktu darbības ar Excel failiem. |

---

### 2. Akciju saraksta ielāde

- Tiek atvērts `stocks.json` fails un piešķirts mainīgajam `stock_dictionary`.

---

### 3. Klases `ListTicker` definīcija

- Metode `__init__` — izveido tukšu sarakstu.
- Metode `add_ticker` — pievieno jaunu ticker sarakstam (novērš dublikātus).

---

### 4. Lietotāja ievade

- Lietotājam tiek prasīts ievadīt 3 akciju nosaukumus vai ticker kodus, atdalītus ar komatiem.
- Tickeri tiek pārbaudīti:
  - Vai tie ir zināmi;
  - Vai tie atbilst kompānijas nosaukumam `stocks.json` datubāzē;
  - Ja nav atpazīti, tiek prasīts lietotājam apstiprinājums, vai ievadītais ir ticker kods.

- Tikai derīgie tickeri tiek pievienoti gala sarakstam `tickers`.
- Ja neviens ticker nav derīgs, programma beidz darbību.

---

### 5. Excel faila sagatavošana

- Tiek mēģināts atvērt `stock_prices.xlsx` failu.
- Ja fails neeksistē, tas tiek izveidots un tiek pievienotas kolonnas: `Time`, kā arī visi tickeri.

---

### 6. Datu iegūšana (`get_price` funkcija)

- Izveido Yahoo Finance URL katram tickerim.
- Izsūta HTTP pieprasījumu.
- Ar `requests` un `BeautifulSoup` palīdzību tiek iegūtas un izvilktas akciju cenas no HTML koda.

---

### 7. Datu apstrāde (`scraped_data` funkcija)

- Fiksē laika zīmogu.
- Iegūst cenas visiem tickeriem.
- Izvada datus terminālī.
- Pieraksta tos `stock_prices.xlsx` failā ar `openpyxl`.

---

### 8. Cikla palaišana

- Tiek izmantots `while` cikls, kas ik pēc **15 sekundēm** izsauc `scraped_data()` funkciju, tādējādi iegūstot jaunas cenas.

---

## Koda darbības piemērs

**Ievade:**

```
Apple, MSFT, Tesla
```

**Termināļa izvade:**

```
[2025-05-22 15:00:30] AAPL: 182.12, MSFT: 426.90, TSLA: 171.25
```

**Excel fails (`stock_prices.xlsx`):**

| Time                | AAPL  | MSFT  | TSLA  |
|---------------------|-------|--------|--------|
| 2025-05-22 15:00:30 | 182.12| 426.90 | 171.25 |
