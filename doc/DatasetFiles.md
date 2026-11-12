---
title: "Dataset Files"
subtitle: "for Slice Master 6000"
author:
- Donald Beaudry (K1DBO)
- Slice Master 1.0
...

## Introduction

A dataset puts spots into layers, and gives the spots their properties.  You
can make a dataset on Slice Master's Datasets page, or you can write it as
a file.  This page tells you how to write the file.

## File location

Each dataset of your own is one file in the `datasets` folder.  This folder is
in Slice Master's data folder:

```
C:\Users\<your user name>\AppData\Local\K1DBO\slice-master-data\datasets
```

The name of the file is the name of the dataset: `My spots.yaml`.

- Slice Master reads a file again when you save it.  You do not have to stop
  Slice Master.
- A change on the Datasets page goes into the file at once.  Your comments
  in the file stay.
- To delete a dataset, delete its file.

The file is YAML: each key is followed by a colon and its value, and the
indent shows what holds what.

## An example

```
format: sm-band-plan/1
name: My spots
fields: {club: ''}
display: {max_age: 30}
layers:
  - name: spots
    from: {dataset: All spots, layer: spots}
    display: {show: always}
  - name: cw
    from: {layer: spots, field: mode, test: is, value: CW}
    display: {color: blue}
  - name: ft8
    from: {layer: spots, field: mode, test: is, value: FT8}
    display: {color: green, max_age: 5}
  - name: friends
    from: {layer: spots, field: call, test: is, value: 'K1ABC, N2DEF'}
    fields: {club: NEARC}
  - name: friends on cw
    rule: friends also in cw
    display: {score: 10, background: yellow}
```

This dataset shows all the spots, and it makes the CW spots blue and the FT8
spots green.  It keeps an FT8 spot for 5 minutes, and the other spots for 30
minutes.  Two stations are friends.  When a friend is on CW, the spot has a
yellow background, and it goes to the top.

## The top of the file

```
KEY       VALUE
format    sm-band-plan/1.  Always this text.
name      the name of the dataset.  It is the name on the Datasets page.
fields    your own properties, each with its default value.  Optional.
display   the properties for all the spots.  Optional.
layers    the layers, in a list.  Each item of the list starts with "-".
```

The `display` at the top is the same as **Defaults** on the Datasets page.  A
layer can give a different value.

## A layer

Each layer has a `name`, and one of `from`, `rule` or `items`:

```
KEY       VALUE
name      the name of the layer.  The rules and the other datasets use it.
from      the spots that the layer takes from a different layer.
rule      the spots that the layer makes from layers above it.
items     the entries of a band plan (see "Items" below).
display   the properties that the layer gives its spots.  Optional.
fields    the values that the layer gives your own properties.  Optional.
```

The order of the layers is important:

- A `from` or a `rule` can use only the layers above it.
- When two layers give a spot the same property, the lower layer applies.
  Only `score` adds up.

A name can have spaces, but it cannot be `also`, `in` or `not`.  Two layers
cannot have the same name.  If a name starts with `_`, other datasets cannot
use the layer.

## from

`from` takes the spots of a layer, and can test a field of each spot:

```
KEY       VALUE
dataset   the dataset that has the layer.  Leave it out for a layer of
          this file.
layer     the name of the layer.
field     the field to test, for example call, mode, band, spotter.
          Leave it out to take all the spots of the layer.
test      is, is not, matches, or matches regex.  By default: is.
value     the value to test for.
```

The tests:

```
TEST            CONDITION
is              the field is one of the values: 'CW, RTTY'
is not          the field is none of the values
matches         the field matches one of the patterns: 'K1*'.  * is any
                text, ? is one character, [ABC] is one of A, B and C.
matches regex   the field contains the regular expression: '^W[0-9]'
```

Upper and lower case are the same in all the tests.  Put a value with a comma
or a space in quotes.

To find the names of the fields, select a layer on the Datasets page, and
click the **Field** box.  It shows the fields of the spots of that layer.  To
find the layers of a different dataset, click the **From** box.

## rule

A `rule` joins the names of layers above it:

```
WORD      MEANING      EXAMPLE                  RESULT
,         either       pota, dx                 the POTA spots and the DX spots
also in   narrow to    pota also in cw          the POTA spots in CW
not in    leave out    pota not in worked       the POTA spots not worked
( )       group        pota, (dx also in cw)    all POTA spots, and DX spots in CW
```

- The words read from left to right.
- A list after `also in` or `not in` is one set: `pota also in cw, ssb`.
- Put a name with a space in quotes: `'DX window' not in cw`.
- Between spots and other items, `in` means at the same frequency:
  `spots also in slicea` is the spots in the passband of slice A.

## display

The display properties.  Give them at the top of the file for all the spots,
or in a layer for the spots of that layer.

```
KEY          VALUE                          EFFECT
show         always, optional or never      always: the display shows the
                                            spots, and a map opens up for
                                            them.  optional: where there is
                                            space.  never: not shown.
score        a number                       a higher score goes higher on a
                                            map, and first in a list.  The
                                            scores of all the layers add up.
rank         a number                       puts in order the spots of the
                                            same score.  Programs give it.
place        top, bottom or own             where a map puts the bars of a
                                            band plan, or a DAX IQ channel.
color        a color, or own                the color of the text.
background   a color, or own                the color behind the text.
onclick      words: tune or announce,       what a click and a double-click
             and select, tx, mode           do.  'announce select tx'
max_age      minutes                        how long the dataset keeps a
                                            spot after its last report.
```

A color is a name, for example `blue`, `green`, `orange`, or a number, for
example `'#3cafd5'`.  Put a number in quotes.  `own` is the color that the
source gave the spot.

A dataset shows nothing until a `show` shows it.

## fields: your own properties

You can give a spot a property of your own, for example a club or a note.

1. In the `fields` at the top of the file, give the property and its default:
   `fields: {club: ''}`.
2. In the `fields` of a layer, give the value for the spots of that layer:
   `fields: {club: NEARC}`.

A layer can then test the property with `from`, as it tests a field.

## Items

A layer with `items` holds the entries of a band plan.  A dataset with items is
a band plan too.  Each item gives where the entry is, in `display`, and what it
is, in `fields`:

```
- {display: {frequency_lo: 3.790, frequency_hi: 3.800}, fields: {use: dx}}
- {display: {frequency: 3.950, offset_lo: -2700, offset_hi: 0},
   fields: {use: net, note: Thursday 8 PM}}
```

Where the entry is:

```
KEYS                                      RANGE
frequency_lo and frequency_hi (MHz)       the range between them
frequency_lo or frequency_hi, and         the range that wide, up from the
  width (Hz)                                low end or down from the high end
frequency and width                       the range that wide, with the
                                            frequency at its center
frequency, offset_lo and offset_hi (Hz)   from frequency + offset_lo to
                                            frequency + offset_hi
frequency                                 that one frequency
```

What the entry is: `use` (one, or a list in brackets), and if you want,
`classes`, `power`, `region`, `note`, and fields of your own.

Give each use a layer that takes it, with its color:

```
  - name: Net
    from: {layer: entries, field: use, test: is, value: net}
    display: {color: '#3cafd5'}
```

Add `pattern: hatch` to the display to draw the use as lines.  The guide
[Spectrum Maps](SpectrumMaps.md) tells more about band plans.

## Mistakes

A mistake in a file does not stop Slice Master.  Slice Master reads the rest of
the file.  On the Datasets page, a row with a mistake has a red border, and the
reason shows under it.
