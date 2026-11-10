# Raw data

The raw files are **not included in this repository**. SteamDB's FAQ says publishing its data in its entirety is prohibited, and Steam Charts pages are third-party content. This page records exactly what was used so the analysis can be reproduced.

## How to download the files

1. **SteamDB player counts:** open each game's charts page below. In the chart's download menu (arrow icon, top right of the chart), choose **Download CSV**. Save the file in this folder under the name shown. SteamDB may require you to be logged in.
2. **SteamDB prices:** open the game's main SteamDB page (Price history tab), select the Canadian store, and use the same **Download CSV** menu on the price chart. Rename the file as shown.
3. **Steam Charts pages:** save `https://steamcharts.com/app/<app_id>` as HTML to `steamcharts/steamcharts_<app_id>.html`. These are used only for the cross-check.
4. Run the notebook. It stops with a clear message if any file is missing.

Downloads made after 10 Nov 2026 will contain more recent rows, and older rows can be revised, so checksums won't match exactly. The analysis periods (up to October 2026) should be unaffected.

## Files used (downloaded 2026-11-10 UTC, unmodified)

| File | Game | Steam App ID | Source page | SHA-256 |
|---|---|---|---|---|
| steamdb_chart_1811260.csv | FIFA 23 | 1811260 | https://steamdb.info/app/1811260/charts/ | 5a09b3f5962735d212f293ca59775569198959814b06f4e70ba8ef899afe86f7 |
| steamdb_chart_2195250.csv | FC 24 | 2195250 | https://steamdb.info/app/2195250/charts/ | f16f17f7f51c701270109c32e231f22ae386a01ffd4922679f729b9228895701 |
| steamdb_chart_2669320.csv | FC 25 | 2669320 | https://steamdb.info/app/2669320/charts/ | 2693d30431fc7dd78ce433b56c1cbfe43946da3e2ee8ebde56124625915e9dd5 |
| steamdb_chart_3405690.csv | FC 26 | 3405690 | https://steamdb.info/app/3405690/charts/ | 1ebe95417e19f2143b779f33a042a24e23cfced8dcec3f9f1c6417ef06f149ce |
| steamdb_price_3405690_CAD.csv | FC 26 price history (Canadian store, CAD) | 3405690 | https://steamdb.info/app/3405690/ (Price history) | 95cc050c84a55f8c570ade68e37984a1e5fc91540c1a7d659640c36c8b60bb6c |
| steamdb_price_1811260_CAD.csv | FIFA 23 price history (Canadian store, CAD) | 1811260 | https://steamdb.info/app/1811260/ (Price history) | dfe4786d15f3dd05b520893b212383d13a2c97f25b4783d84309fa87dad8733b |

The browser saved the price files as `steamdb_chart_<app_id> (1).csv`; they were renamed here with contents unchanged. The FIFA 23 price file was downloaded on 2026-11-10 at about 08:20 UTC, after the other files.

## Player-count files

Columns: `DateTime` (UTC), `Players`, `Average Players`.

- **Mixed resolution.** Up to 2026-10-10 there is one row per day at 00:00. From 2026-10-11 the export switches to hourly and then 10-minute snapshots, because SteamDB keeps fine detail only for recent weeks. The daily `Average Players` value is still given on each 00:00 row in that period. Where 10-minute snapshots exist it equals their daily mean exactly, so daily averages are available for every complete day through 2026-11-09. Daily peaks are only reliable up to 2026-10-10.
- **`Players` = daily peak concurrent players.** Checked: the maximum of the daily values in a month equals the monthly peak in SteamDB's own monthly table (e.g. FC 26, June 2026: 142,074).
- **`Average Players` = daily average concurrent players**, as computed by SteamDB. Checked: the mean of daily values per month reproduces SteamDB's monthly averages (e.g. FC 26: June 2026 74,099; July 2026 102,075).
- These are concurrent counts on Steam (PC) only, not unique users, not sales and not retention.

Coverage (daily rows), all with zero missing days and no empty values:

| Game | First day | Last daily row |
|---|---|---|
| FIFA 23 | 2022-09-29 | 2026-10-10 |
| FC 24 | 2023-09-28 | 2026-10-10 |
| FC 25 | 2024-09-27 | 2026-10-10 |
| FC 26 | 2025-09-25 | 2026-10-10 |

Cross-check against Steam Charts monthly averages (full months only): the median difference is +0.1% to +0.4% depending on the game. Larger gaps (up to about 6%) appear only for FIFA 23 in 2026, when it has fewer than 1,000 players, and for FC 26 in November 2025 (-4.8%). The two sites sample and average differently.

## Steam Charts pages (cross-check only)

`steamcharts/steamcharts_<app_id>.html` are the Steam Charts pages (https://steamcharts.com/app/<app_id>), saved unmodified on 2026-11-10. The notebook reads their monthly "Avg. Players" table to cross-check the SteamDB monthly averages.

| File | SHA-256 |
|---|---|
| steamcharts_1811260.html | 31ba55bc3698734c8e3ab7de202202d3855bf73a5ba279400245585eaad62971 |
| steamcharts_2195250.html | 251296111a2dacd1c7a02f210628c4705477e29f5e3607a9ee8bac2d46ecddc3 |
| steamcharts_2669320.html | eb85a28197552810f95e3db68bff917d10fd0ddecbdb7adf7afac1b380519bec |
| steamcharts_3405690.html | 9bc633c3f84cd42aa0469e32e629b94820b4eb56f18b82bd01cec2144b3a6d09 |

## Price files

Columns: `DateTime` (UTC), `Final price`, `Historical Low` (CAD, Canadian Steam store). Each row is a price change, not a daily observation. The list price is CA$89.99 for both games. Canadian sale timing usually matches other regions, but these files don't prove US pricing.

FIFA 23 notes: a CA$109.99 entry at 03:55 on 30 Sep 2022 lasted 5 minutes (launch-day correction). A price of 0 from 21 Sep 2023 marks the game's removal from sale, not a free offer.
