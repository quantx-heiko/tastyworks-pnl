Tastyworks PNL
--------------

Create a PnL-document for a tax income statement in Germany for the US-broker tastyworks.
Download all transactions from Tastyworks into a csv-file and create with this
python-script a new enhanced csv-file. This new csv-file adds a currency conversion
from USD to Euro and computes the realised profits and losses based on the FIFO method.
Also all currency gains for the USD-cash-account are added.
A summary for all stock, fond, dividend, option and future gains/losses is output.

Details on German taxes are written up
at <https://laroche.github.io/private-geldanlage/steuern.html>.

## Installation & Setup

### Dependencies Installation

Before running the project, ensure all dependencies are installed. Execute the following command:

```bash
pip install -r requirements.txt
```

## Execution

Run the following command to convert from the tastyworks csv file (transaction_history.csv)
to a 2023 tax statement as csv file (tastyworks-tax-2023.csv):
<pre>
python3 tw-pnl.py --assume-individual-stock --tax-output=2023 --output-csv=tastyworks-tax-2023.csv transaction_history.csv
</pre>

Continue with [Google Spreadsheets](https://docs.google.com/spreadsheets/) or
[libreoffice](https://libreoffice.org/) spreadsheet to finalize your tax report:
<pre>
soffice tastyworks-tax-2023.csv
</pre>

The following command outputs a more detailed csv report for your personal review together with a
yearly summary for all years in the extra file tastyworks-summary.csv:
<pre>
python3 tw-pnl.py --assume-individual-stock --summary=tastyworks-summary.csv --output-csv=tastyworks-tax.csv transaction_history.csv
soffice tastyworks-tax.csv
soffice tastyworks-summary.csv
</pre>

[RUN.sh](RUN.sh) contains an example on how to generate all reports.

If the script does not complete successfully, please add the option __--debug__ to see
additional information on where it stops.

[In German: Erstellt eine Gewinn- und Verlustberechnung für eine Steuererklärung
in Deutschland für den US-Broker Tastyworks.
Lade von Tastyworks alle Transaktionen in eine CSV-Datei herunter und generiere
mit diesem Python-Skript eine neu aufbereitete CSV-Datei. Diese neue CSV-Datei
fügt eine Umrechnung von USD in Euro hinzu und berechnet die realisierten
Gewinne und Verluste nach FIFO-Methode. Auch eine Berechnung aller Währungsgewinne
für das USD-Cashkonto wird erstellt.
Eine Übersicht über die verschiedenen Gewinne/Verluste für Aktien/Fonds/Dividenden/Optionen
und Futures wird ausgegeben.

Details zur Steuererklärung in Deutschland sind unter
<https://laroche.github.io/private-geldanlage/steuern.html> zusammengestellt.

Eine Web-Applikation für dieses Python-Skript wird in Zukunft
unter <https://knorke2.homedns.org/depot-pnl> aufgebaut.]

Starte folgenden Kommandozeilen-Aufruf für eine Konvertierung von einer Tastyworks csv-Datei (transaction_history.csv)
zu einer Steuerausgabe für 2023 als CSV-Datei (tastyworks-tax-2023.csv):
<pre>
python3 tw-pnl.py --assume-individual-stock --tax-output=2023 --output-csv=tastyworks-tax-2023.csv transaction_history.csv
</pre>

Für einen schönen Ausdruck lade ich die csv-Ausgabedatei in [Google Spreadsheets](https://docs.google.com/spreadsheets/).
Dabei einfach keine Konvertierung von Zahlen zulassen. Dann die Überschrift groß und in Fettdruck darstelllen,
einige Spalten auf Rechts-Formaierung stellen und fürs Finanzamt unnötige Daten löschen.

Als Alternative kann man die Tabellenkalkulation [libreoffice](https://de.libreoffice.org/) mit der
CSV Ausgabedatei tastyworks-tax-2023.csv starten:
<pre>
soffice tastyworks-tax-2023.csv
</pre>

Folgender Befehl gibt einen detaillierten CSV Report für den persönlichen Review zusammen
mit einem Jahresüberblick für alle Jahre in die extra Datei tastyworks-summary.csv:
<pre>
python3 tw-pnl.py --assume-individual-stock --summary=tastyworks-summary.csv --output-csv=tastyworks-tax.csv transaction_history.csv
soffice tastyworks-tax.csv
soffice tastyworks-summary.csv
</pre>

[RUN.sh](RUN.sh) enthält Beispielaufrufe zur Generierung aller Reports.

Falls das Skript nicht erfolgreich läuft, bitte die Option __--debug__ hinzufügen, damit man
zusätzliche Informationen sieht, an welcher Stelle genau abgebrochen wird.


How to use
----------

Clone or download this repository onto your local computer. If you don't have git installed,
you can download the current zip file from <https://github.com/laroche/tastyworks-pnl/archive/refs/heads/main.zip>.

Download your trade history as csv file from the web-browser via
<https://trade.tastyworks.com/index.html#/transactionHistoryPage>.
(Choose "Activity" and then "History" and then setup the filter for a
custom period of time and download it as csv file.
Do not download this file from the separate Tastyworks application,
but use the website and your browser.)

Newest entries in the csv file should be on the top and it should contain the complete
history over all years. (You can at most download data for one year, so for several years
you have to download into several files and combine them into one file with a text editor.)
The csv file has the following first line:

<p><code>
Date/Time,Transaction Code,Transaction Subcode,Symbol,Buy/Sell,Open/Close,Quantity,Expiration Date,Strike,Call/Put,Price,Fees,Amount,Description,Account Reference
</code></p>

If you delete the __eurusd.csv__ file or your Tastytrade transaction history contains more recent transactions than the available data in this file, a current version is downloaded automatically directly from <https://www.bundesbank.de/de/statistiken/wechselkurse>.
(Link to the data: [eurusd.csv](https://www.bundesbank.de/statistic-rmi/StatisticDownload?tsId=BBEX3.D.USD.EUR.BB.AC.000&its_csvFormat=en&its_fileFormat=csv&mode=its&its_from=2010))

The option __--usd__ can be used to prevent converting pnl data into Euro.

Per default the script stops on unknown trading symbols (underlyings) and you have
to hardcode into the source code if it is an individual stock or some ETF/fond.
You can use the __--assume-individual-stock__ option to assume individual stock for all unknown symbols.

Currency gains are computed via FIFO as with other assets. They are tax free if hold for more than one year
or if credit is paid back (negative cash balance). They are also tax free for dividends, credit payments and
sold options as well as account fees.
Currency gains need to go into the 'Anlage SO' within a private German tax statement.

The summary output lists all assets at the end of each year. 'account-usd' contains a FIFO list of all
USD buys and the conversion price for Euro. This entry might be very long and look complicated. You might
want to ignore this line and just look over the list of other assets.

The option __--show__ gives some summary graphs.

The option __--debug__ adds debug information on what data is currently processed to aid with
debug reports on bugs and problems.

The option __--verbose__ outputs the csv output data also as text on the screen. Just a way to see data
differently, you normally should use the csv data for further processing.


If you work on Linux with Ubuntu/Debian, you need to make sure
<https://pandas.pydata.org/> is installed:

<p><code>
sudo apt-get install python3-pandas
</code></p>

Or use pip or pip3 for a local install:

<p><code>
pip install -r requirements.txt
</code></p>
<p><code>
pip3 install -r requirements.txt
</code></p>

For graphical output (--show), you need matplotlib:

<p><code>
sudo apt-get install python3-matplotlib
</code></p>


CSV and Excel Output
--------------------

The script can also output all data again as CSV file or as Excel file.
(CSV should be most robust, I don't have much experience with Excel. I'd recommend CSV
and just reading it into a new Excel sheet yourself. Both data types contain the same output data.)

The options for this are __--output-csv=file.csv__ and __--output-excel=file.xlsx__.

The output contains the important original data from the Tastyworks csv file plus
pnl generated data as well as eurusd conversion data. You probably do not have to
provide all data in a tax statement, some is only added for further data processing
convenience in your spreadsheet program.

If you provide the option __--tax-output=2023__, your output file will be
optimized for tax output for a special year. Only transactions for that year are output.
The datetime will only contain the day, but no time information. Fewer other data is output.

The CSV file contains floating point numbers in English(US) notation where a decimal
point (".") and no decimal comma (",") is used as decimal separator for floating point
numbers like in "-0.2155".
Usually Tastyworks provides floating point numbers with 4 decimals after the decimal separator.
For yearly summaries I only output with 2 decimal digits.

Here the output transaction data in detail:

- __datetime__: Date and time (Tastyworks gives minutes for this, no exact seconds)
  of the transaction
- __type__: type of transaction (German names): Aktie, Aktienfond, Mischfond,
  Immobilienfond, Sonstiges, Option, Future, Dividende, Quellensteuer, Zinsen,
  Ein/Auszahlung, Ordergebühr, Brokergebühr
- __pnl__: pnl for tax payments for this transaction based on FIFO
- __term_loss__: how much does this transaction contribute to losses in future
  contracts ('Verlustverrechnungstopf Termingeschäfte'). If you use the option __-b__ then
  also all losses from option writing are added here.
- __eur_amount__: 'amount - fees' converted into Euro currency
- __usd_amount__: transaction amount in USD
- __fees__: cost of transaction in USD that needs to be subtracted from amount (Tastyworks fees and
  all exchange, clearing and regulatory fees)
- __eurusd__: official eurusd conversion rate for this transaction date from bundesbank.de
- __quantity__: number of buys or sells
- __asset__: what is bought (stock symbol or something like 'SPY P310 20-12-18' for an option
- __symbol__: base asset (underlying) that is traded. This is included to be able to
  generate summary overviews for e.g. all transactions in SPY with stocks and options combined.
- __description__: additional informational text for the transaction
- __cash_total__: account cash balance in USD after this transaction. This is the previous account total plus
  'amount - fees' from this transaction. (Cash amount at Tastyworks.)
  This is purely informational and not needed for tax data.
- __net_total__: Sum in USD of cash_total plus all assets (stocks, options)
  in your account.
  This does not use current market data, but keeps asset prices at purchase cost.
  Best looked at to check if this script calculates the same total sum as shown in your
  Tastyworks current total.
- __tax_free__: are further currency changes tax free (German: steuerneutral)
- __usd_gains__: currency conversion gains for the account in USD. Based on cash changes
  in USD due to this transaction.
- __usd_gains_notax__: as above, but not part of German tax law


FAQ
---

- Either github issues or email works for me to enhance/fix this program. Sample data
  is best to resolve issues.
- This script needs the input CSV file to be in encoding format UTF-8 with numbers
  in US-format. Output is also in US-format, so you need to convert this into German
  style of numbers. libreoffice is doing this very easily.
- If you read in the csv-file from Tastyworks into Excel, the floating point price values
  can get converted from US style to German notation. This breaks this script, so please
  stay with original data from Tastyworks or use a normal editor instead of Excel to change data.
- Even the tax report is currently in US notation. I recommend to use libreoffice to read in this
  data and then store it in German notation plus output it into a pdf for tax authorities.
- One check is to look at the computed value of "account cash balance" from this script and compare it
  to the official value "Cash Balance" from Tastyworks at <https://manage.tastyworks.com/#/accounts/balances>.
  After 550 transactions the final cash balance value is on the cent identical to the
  computed value.
- The output-csv file should have the same amount of lines as the input csv. The resulting output
  is just an enriched form of the input. Please verify all summary output by computing it again from
  the output-csv file yourself.


Currency gains in German tax law
--------------------------------

This description has moved to <https://laroche.github.io/private-geldanlage/steuern.html#fremdwaehrungskonten>.


TODO
----

- Tastytrade API: <https://support.tastyworks.com/support/solutions/articles/43000700385-tastytrade-open-api>
- stocks/ETFs
   - Stock splits and spinoffs are not fully supported. (Option strike prices also need to be adjusted.) Example entry:
      - Assumption: stock/option splits are tax neutral.
      - stock splits are now implemented, but not tested at all. Options are not yet supported. Please send in more test data.
   - In German: Bei Aktien-Leerverkäufen (über eine Jahresgrenze hinaus) wird 30 % vom Preis mit der KapESt
     als Ersatzbemessungsgrundlage besteuert (§ 43a Absatz 2 Satz 7 EStG) und erst mit der Eindeckung ausgeglichen.
   - Complete support for Investmentsteuergesetz (InvStG) 2018: Zahlungen am Anfang vom Jahr (Vorabpauschale).
   - Check if withholding tax is max 15% for dividends for US stocks as per DBA.
     Warn if e.g. 30% withholding tax is paid and point to missing W8-BEN formular.
   - Complete the list of non-stocks.
   - Does Tastyworks use BRK.B or BRK/B in transaction history?
     Adjust the list of individual stocks accordingly.
   - If you select to automatically reinvest Dividends, then also partial amounts of stocks are bought.
     This is currently not supported.
- Options:
   - If a long option is assigned, the option buy price should be added to
     the stock price. This is currently not done, but we print a warning
     message for this case for manual adjustments in this rather rare case.
   - If you sell one option and then buy two of the same options, this transaction should be split for
     a short option trade and a long option trade for German taxes. This is currently not done, but maybe
     the checks for the yearly tax report will not work in this case. A real check and split of the
     transactions needs to be added.
   - A separate bucket for all long-options that expire worthless should be created.
   - If option writing is cash-settled, the cash-settlement needs to go into "Termingeschäftsverluste".
   - Termingeschäftsverluste are capped at 20.000 Euro per year. Make this configurable as couples
     have double the amount. Depend this automatically on the account name (contains "joint").
- Does not work with futures.
   - A first quick implementation is done, further checks are needed on real account data.
     Let me know if you can provide full account data to check/verify.
   - As "Mark-to-Market"-payments/transactions are done, we immediately pay taxes for them, this is
     not stacked up until futures are sold again. What is a correct way to tax these transactions?
     Adding this up needs to be done to correctly implement "20k€ Terminverlustgeschäfte" for futures.
- Cryptos:
   - I am not trading cryptos, so there is only minimal support for them.
   - Cryptos have their own group. pnl is calculated as with normal stocks. No 1-year-taxfree or any other stuff is done.
- Currency gains:
   - Done: For currency gains, we could also add all fees as tax free by adding a separate booking/transaction.
   - For currency gains tax calculation you can reorder all transactions of one day and use the best
     order to minimize tax payments. This is currently not done with the current source code.
   - For currency gains you don't pay taxes for negative cash. Review on how this is computed in detail, is it
     done correctly if going from negative cash to positive cash by splitting a transaction at 0 USD?
   - If you transfer USD to another bank account, you need to choose between tax-neutral and normal tax transaction.
   - In German: Stillhalterpraemien gelten auch nicht als Währungsanschaffung, sondern
     als Zufluss und sind daher steuer-neutral. Im Source wird dazu die Auszeichnung von Tastyworks
     als "Sell-To-Open" verwendet.
   - If you are short stock, the additional cash is not really part of "settled cash". Interest payments are not
     changed by this.
- Output
   - Output all decimal digits, even if they are 0.
   - For --summary allow excel output depending on filename extension.
   - Output yearly summary data at the beginning of the file, before all individual transactions.
   - Done: Tax csv output: Sort transactions in the order they appear in the summary report.
   - Add columne with automatic annotations. (warnings and error messages from conversion.)
   - Done: Add columne with underlying symbol (dividends and options) and Put/Call (for options).
   - Instead of datetime in one columne, output date and time in two separate columns.
   - Print header with explanation of transaction output. Or additional page for the tax athority with explanations.
   - Generate documentation that can be passed on to the German tax authorities on how
     the tax is computed and also document on how to fill out the official tax documents.
      - Check this for docx reporting: <https://github.com/airens/interactive_brokers_tax>
   - Done: More German words in the output csv file for the German Finanzamt.
   - Can Excel output also include yearly summary data computed from Excel? (How are formulas output?)
     Can transactions also be grouped per year on different sheets/tabs? A yearly tab.
   - Output open positions (assets) at start/end of year. Also include summary of unrealized gains (including currencies).
   - Is the time output using the correct timezone?
   - Are we rounding output correctly?
      - The EUR amount is internally stored with 4 digits, so if pnl computations are
        done, we can have slightly different results. Maybe the EUR amount should already
        be stored rounded to 2 digits for all further computations. As tax data should be
        computed from the spreadsheet file, this is not really an issue.
      - Since we store some data as float, then output is not always output with 2 digits.
        E.g. 5.80 is output as 5.8. Storing numbers as string could change this.
   - Output report as pdf in addition to csv: <https://github.com/probstj/ccGains>
   - Allow new strategy field / cetegory to be filled out.
   - Add an extra notes/comment field that can be edited.
   - Verlustverrechnungstöpfe für Aktien, Sonstiges, Termingeschäfte. Berechnung der zu zahlenden Steuer.
   - If data might be stored locally, you can also use parquet dataformat: pd.to_parquet() and pd.read_parquet().
- Statistics:
   - "fees" output should also be correct if only one tax year is output
   - Options
      - Add DIT (Days In Trade), average days in trade
      - Compute actual premium per day over the DITs. Graph all sold options over all days in the year
        to show distribution of sold premium over the course of the year.
      - number of winning trades (total and percentage), average gains
      - cagr = Decimal((quantity * price) / basis) **  Decimal(Decimal(1) / (Decimal(length_held)/Decimal(365.25))) - 1
   - Statistics for stocks/options combined for the same symbol/underlying
   - Specify non-realised gains to know how much tax needs to be paid for current net total.
   - Add performance reviews, graphs based on different time periods and underlyings.
   - How much premium can you keep for option trades. Tastyworks says this should be
     about 25 % of the premium sold. I am mostly selling only premium (and no spreads) and can keep
     about 79 % of it as average over many years.
   - How much fees do you pay for your option trades to the broker/exchange compared
     to your overall profit? This seems to be less than 1.5 % for me in 2021.
   - Show number of trades/transactions history.
   - List of best and worst trades of the year.
   - Check stats output from: <https://github.com/Jcuevas97/OrderParser>
   - We use the current day while running this script to compute premium/day statistic for
     the current year. Maybe using the last day of trading activity from the logs would be
     better?
- Docu
   - Add images on how to download csv-file within Tastyworks into docu.
   - Translate text output into German.
   - Add docu in German.
- Demo data and tests
   - Add test data for users to try out.
   - Add testsuite to verify proper operation.
   - Use pandas.isna(x)?
- If we move currency conversion gains to the end of the loop, we can defer the computation
  of tax_free into the complete loop. E.g. detection of interest payments as tax_free is
  currently made too complex.
- CPU profiling to improve python code
   - How to run:
      - sudo apt-get install graphviz
      - python3 -m cProfile -s cumtime -o profile.pstats tw-pnl.py
      - git clone https://github.com/jrfonseca/gprof2dot
      - ln -s "$PWD"/gprof2dot/gprof2dot.py ~/bin
      - gprof2dot.py -f pstats profile.pstats | dot -Tsvg -o callgraph.svg
   - get_summary and append_yearly_stats(df_append_row) take too much CPU time, how to improve?
      - Keep data in dictionary until final data is complete?
- Look at other libraries for currency conversion:
  <https://github.com/alexprengere/currencyconverter> or
  <https://github.com/flaxandteal/moneypandas>
- Stock splits example:
<pre>
01/21/2021 12:39 PM,Receive Deliver,Forward Split,TQQQ,Buy,Open,108,,,,,0.00,-9940.86,Forward split: Open 108.0 TQQQ,xxx...00
01/21/2021 12:39 PM,Receive Deliver,Forward Split,TQQQ,Sell,Close,54,,,,,0.00,9940.86,Forward split: Close 54.0 TQQQ,xxx...00
</pre>
- Similar/related projects:
   - <https://github.com/avion23/tastyworksTaxes>
   - <https://github.com/elch78/tastyworks-tax-calculator>
   - <https://github.com/leonk2210/tastyworks-tax-de>
   - <https://github.com/neogeo/tasty-trade-csv-import-profit-loss>
   - <https://github.com/jblanfrey/tastyworks-matlab-cli>
   - <https://github.com/Jcuevas97/OrderParser>
   - SQL import of all trades: <https://github.com/Dtrodler/TastyPlTracker>
   - MySQL import of all trades: <https://github.com/jx666jx/TastyTrader>
   - Convert from Interactive Brokers to Tastyworks: <https://github.com/cmer/interactive_brokers_2_tasty_works>
   - Tastyworks API:
      - <https://github.com/boyan-soubachov/tastyworks_api>
      - <https://github.com/c4syner/tastyscrape>
   - Tastyworks risk simulator: <https://github.com/iloire/tastyworks-risk>
   - Google Sheets portfolio analysis: <https://github.com/jarthursquiers/FireByArthurTradingEngine>
   - <https://github.com/TylerSafe/Tastyworks-tax-calculator>

