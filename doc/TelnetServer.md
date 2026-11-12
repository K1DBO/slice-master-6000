---
title: "Telnet Aggregation Server"
subtitle: "with Slice Master 6000"
author:
- Donald Beaudry (K1DBO)
- Slice Master 1.0
...

## Introduction

Slice Master sends spots to your logger on one telnet port, as a DX cluster
does.  This page tells you how to:

* connect a logger to the port.
* select which spots the logger gets, and in which format.
* send your skimmers' spots to the Reverse Beacon Network with RBN Aggregator.
* send a spot from the logger to Slice Master.

## Connection

1. On Slice Master's **...** tab, click **Telnet Aggregation Server** to
   open the section.

   ![Slice Master Telnet Aggregation Server settings](../screenshots/settings-telnet-aggregation-server-1-0-0.png)

2. Select **Enable aggregation server**.  Write down the **Telnet port**: 7300
   by default.
3. In your logger, connect its DX cluster window to **localhost**, at that
   port.
4. When the logger asks for a login, give your callsign.  Slice Master asks
   for no password.

Many loggers can connect at the same time.  Each connection has its own
settings: the commands below change only the connection that sends them.

## Spot selection

By default, a connection gets all the spots that Slice Master collects: the
spots of the programs that it launches -- CW Skimmer, WSJT-X, JTDX, JS8Call,
MSHV, GRITTY -- and of Skim Master, the spots that you type, and the spots of
your spot sources -- DX clusters, DX Summit, POTA, and your loggers.

To get only the spots of Slice Master's programs, Skim Master and the spots
that you type, send **unset/sm_external**.  Send **set/sm_external** to get
all the spots again.

To get the spots of one of your views, send **set/sm_view** and the name of
the view, for example `set/sm_view Friends`.  The connection then gets the
spots that the view shows on a map, and `sh/dx` shows them too.  You make and
change views on the View tab: the guide [Spectrum Maps](SpectrumMaps.md) tells
you how.  Send **unset/sm_view** to go back to the spots that you got before.

The view holds for the connection only.  To get it at each login, add
`set/sm_view` and the name to the commands that your logger sends after the
login.  If you delete the view, the connection goes back to the spots that it
got before, and Slice Master sends the line "View NAME deleted."

## RBN Aggregator

RBN Aggregator sends the spots of your skimmers to the Reverse Beacon Network
(RBN).  Slice Master can be its primary skimmer.  RBN Aggregator then gets the
CW, RTTY, FT8 and FT4 spots of Skim Master and of the CW Skimmer that Slice
Master launches, and no other spots.

1. In Slice Master, start the aggregation server (see
   [Connection](#connection)).
2. In RBN Aggregator, on the **Connections** tab, set the primary skimmer:
   - address: **127.0.0.1**
   - port: Slice Master's telnet port
   - callsign: your callsign
   - password: **set/sm_rbn**
3. On the **Combined Skimmers** tab, select the check box at the top ("If you
   are using a program such as AR Cluster Server...").  Type your callsign,
   name, QTH and grid square.  Select the bands that you skim and the
   validation level.
4. To try it first, select **Don't send spots to RBN server** on the
   **Connections** tab.  Then connect the primary skimmer.  The **Skimmer
   Traffic** tab shows the spots, with no "Spot rejected" lines.
5. When the status says "Connected to the RBN Server", clear **Don't send
   spots to RBN server**.

When Skim Master skims CW, Slice Master sends RBN Aggregator the bands that
your skimmers skim and their validation level.  RBN Aggregator then shows them
in place of the ones on the Combined Skimmers tab.

The RBN shows CW and RTTY spots on its web pages.  It sends FT8 spots only on
its FT8 telnet port.

### FT8 by WSJT-X's messages

RBN Aggregator can also get FT8 and FT4 decodes on its **FT#** tab, as
WSJT-X's UDP messages.  Send them by one way only: by the telnet port as above,
or by the FT# tab.  If you use both, the RBN gets each decode two times.

1. On Slice Master's **...** tab, in **Status Broadcast**, add a row for each
   DAX IQ channel or slice, with one mode, for example:

   ```
   wsjtx://127.0.0.1:2250?slice=DAXIQ1&mode=FT8
   ```

   Use a port that no other program on the computer listens on.  Many loggers
   listen on 2237.
2. On RBN Aggregator's **FT#** tab, select **Use** on a row, type the same port,
   and click **Apply Changes**.  The row shows the dial frequency when the
   decodes come.

## Commands

Your logger sends a command as a line of text.  Most loggers let you add
commands to the script that they send after the login.

| COMMAND | RESULT |
|---|---|
| `set/sm_external` | The connection gets all the spots.  This is the default. |
| `unset/sm_external` | The connection gets only the spots of Slice Master's programs, Skim Master and the spots that you type. |
| `set/sm_view NAME` | The connection gets the spots of the view NAME.  A name that no view has gets the names of the views. |
| `unset/sm_view` | The connection gets the spots that it got before `set/sm_view`. |
| `sh/sm_views` | The names of the views. |
| `set/ve7cc` | The spots come as CC Cluster lines (CC11), for CC User and loggers that read them. |
| `unset/ve7cc` | The spots come as DX cluster lines. |
| `set/sm_rbn` | The connection gets only the spots of Skim Master and of Slice Master's skimmers, as RBN Aggregator reads them.  See [RBN Aggregator](#rbn-aggregator). |
| `unset/sm_rbn` | The connection gets the spots that it got before `set/sm_rbn`. |
| `sh/dx` | The last 20 spots that the connection gets. |
| `sh/dx/N` | The last N spots that the connection gets, for example `sh/dx/50`. |
| `sm/json` | All the spots that Slice Master holds, one JSON object a line. |
| `dx FREQ CALL COMMENT` | Sends a spot to Slice Master.  See [Spots you send](#spots-you-send). |
| `echo TEXT` | Slice Master sends TEXT back.  `echo #ready#` also does what `set/sm_external` does. |
| `skimmer/status` | The status of each CW Skimmer that Slice Master launched. |
| `skimmer/sett` | The settings line of your skimmers: the validation level, CQ if they spot only CQ, and the bands that they skim. |

Slice Master answers a command that it does not know with its prompt only.
It does not close the connection on `bye`: close it from the logger.

## Spots you send

Send a spot with the **dx** command:

* `dx 14074.0 K1ABC FT8 -10` -- a spot of K1ABC on 14074.0 kHz, with the
  comment "FT8 -10".  Type the frequency in kHz, with a decimal point.
* `dx @A K1ABC` -- a spot on the frequency of slice A.  Use `@B` for slice B,
  and so on.  The spot also takes the mode of the slice.
* `dx by W1XYZ 7030.5 K1ABC` -- a spot with W1XYZ as the spotter.  With no
  **by**, the spotter is the callsign that logged in.

Slice Master shows the spot on the maps and lists, and sends it to every
connection.  A spot that you send stays for one hour.

## Announced spots

When you announce a spot, Slice Master tells each connection, as CW Skimmer
does.  To announce a spot, double-click it on a map, or click a spot whose
dataset has **Click** set to **Tune and announce**.  The guide
[Spectrum Maps](SpectrumMaps.md) tells you more.

```
To ALL de K1ABC/B <0112Z> : Clicked on "W1XYZ" at 7026.64
```

The line gives your callsign and the slice, the time, the callsign of the spot
and its frequency in kHz.
