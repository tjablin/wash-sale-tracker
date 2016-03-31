# wash-sale-tracker

Script that calculates wash sale adjustments for US taxes.

Note that the author is not a CPA or tax expert. This program most certainly contains bugs.

This script is inspired by [adlr/wash-sale-calculator](https://github.com/adlr/wash-sale-calculator), but rewritten so that the author could reason about how the replacement lots were being chosen. It also includes keeps track of some additional fields, like the original basis and buy date, to make it easy to correlate the output with a 1099-B.

To use the program from a terminal, run:

`python2 wash.py -w dummy_example.csv -o out.csv`

The csv file must have one buy or buy-sell trade per row. Each row must have all of the following columns, but the optional ones can remain blank:

| Column Header | Type | Description |
|---------------|------|-------------|
| Num Shares | Integer | The number of shares in this lot. |
| Symbol | String | Stock symbol. This is unused by the script, as all lots fed into the script are considered substantially identical. |
| Description | String | An arbitrary description of this lot. |
| Buy Date | Date (mm/dd/yyyy) | The date that this lot was actually bought. |
| Adjusted Buy Date | Date (mmdd/yyyy) | Optional. The adjusted buy date of a loss. Provided by the output. |
| Basis | Integer | The number of cents for the cost basis of this lot. |
| Adjusted Basis | Integer | Optional. The number of cents of the adjusted cost basis of this lot. Provided by the output. |
| Sell Date | Date (mm/dd/yyyy) | Optional. The date that this lot was sold. |
| Proceeds | Integer | Optional. The number of cents that this lot was sold for. |
| Adjustment Code | String | Optional. Will be W if the lot is adjusted for a wash sale. |
| Adjustment | Integer | Optional. The number of cents of this lot's disallowed loss. |
| Form Position | String | Optional. This field is used as a tertiary sorting value, but is primarily used to associate lots before and after the script is run. |
| Buy Lot | String | Optional. Generally can be left blank. If two lots were acquired as part of the same buy order, put the same value here. This may occur if you bought a lot of stock, then sold off the lot in pieces (each piece would get a new line on the 1099b); if the broker automatically divided your buy order into pieces to execute; or if other factors caused the broker to split one buy lot into multiple lines on the 1099b. (This field is used because shares from a given buy lot can't replace shares from the same buy lot in a wash sale). |
| Replacement For | String | Optional. A list of strings, separated by a \| character, of buy lots that this lot is a replacement for. Populated by the script. |
| Is Replacement | Boolean (the strings true/false) | Optional. True if the lot was used as a replacement lot. |
| Loss Processed | Boolean (the strings true/false) | Optional. True if the lot is a loss and has been processed. |


If you commit changes to this software, make sure all tests are passing. To verify that all the tests pass:

```
python2 lots_test.py
python2 wash_test.py
python2 run_integ_tests.py
```

