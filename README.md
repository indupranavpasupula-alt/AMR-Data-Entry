# AMR Data Entry

This is a little Python script I made to type in AMR (antimicrobial
resistance) lab results by hand, one isolate at a time, instead of using a
spreadsheet. It's based on the layout I used for a hospital AMR surveillance
project — organism, sample type, patient gender, ESBL flag, and sensitivity
(S / I / R) results for a bunch of antibiotics.

It's nothing fancy, just a menu-driven command line program, but it saves
everything as you go and does the boring calculations for you.

## What it can do

- Asks you questions one at a time and won't let you move on until you give
  a valid answer
- Saves every isolate straight away into `private_data/amr_records.csv`, so
  you don't lose anything if you close the terminal
- Automatically figures out flags for each isolate: ESBL, MDR (resistant to
  3 or more antibiotic classes), carbapenem-resistant, and MSSA / MRSA
- Works out antibiotic sensitivity percentages, split by sample type
- Can save summary tables and a chart into an `outputs/` folder
- Gives you a warning if you mark something ESBL-positive but also say it's
  sensitive to ceftriaxone, since that combination is usually a typo

## Organisms and antibiotics

I included 10 organisms: E. coli, Klebsiella spp., Pseudomonas spp.,
Staphylococcus aureus, Streptococcus spp., Proteus spp., Enterococcus spp.,
Acinetobacter spp., Citrobacter spp., and Enterobacter spp. There's also an
"Other" option if you need to type in something that's not on the list.

For antibiotics there are 24 in total, covering the main classes: penicillins,
cephalosporins, aminoglycosides, fluoroquinolones, carbapenems, glycopeptides,
and a few more (nitrofurantoin, cotrimoxazole, doxycycline, azithromycin,
colistin, aztreonam, linezolid). The program only asks about the antibiotics
that actually make sense for the organism you picked (e.g. it asks about
cefoxitin for Staph aureus so it can work out MSSA/MRSA), but you can say yes
if you want to enter results for other antibiotics too.

## What you need

Just Python 3. If you want the chart at the end, you'll also need
matplotlib:

```bash
pip install matplotlib
```

If you don't have matplotlib installed, the program still works fine — it
just skips the chart and tells you it did.

## Running it

```bash
python3 amr_entry.py
```

You'll get a simple menu:

```
Menu
  1. Add a new isolate
  2. View all entered isolates
  3. Delete an isolate
  4. Show summary on screen
  5. Save summary tables and chart to files
  6. Quit
```

Just type the number for whatever you want to do.

## Where everything gets saved

Your entered isolates go into `private_data/amr_records.csv` (created
automatically the first time you add something). When you use option 5, it
also writes a few summary files into `outputs/`:

- `resistance_profile.csv` — counts and percentages of ESBL, MDR,
  carbapenem-resistant, MSSA and MRSA isolates
- `sensitivity_by_sample_type.csv` — percent sensitive for each antibiotic,
  broken down by sample type
- `isolates_by_sample_and_gender.csv` — how many isolates per sample type,
  split by gender
- `organism_distribution.csv` — which organisms showed up in which sample
  types
- `sensitivity_chart.png` — bar charts for the first four sample types
  (only if matplotlib is installed)


## Why the code looks the way it does

I kept things simple on purpose — plain loops and if/else statements rather
than fancier Python tricks — mostly because that's how I actually code, but
also so it's easy for me (or anyone else) to come back later and understand
what's going on.
