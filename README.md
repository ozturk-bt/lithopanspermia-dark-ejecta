# Mars–Earth lithopanspermia: continuous biological survival landscapes

**Bilal Talha Özturk** | September 2026

Preprint + code for a paper updating the Mileikowsky (2000) lithopanspermia
estimate with modern experimental data, and asking which Mars rocks we're
probably missing.

Zenodo: https://doi.org/10.5281/zenodo.22982602

---

## The basic idea

Mileikowsky et al. (2000) estimated that about 2×10⁷ viable rock fragments
could have made the Mars-to-Earth trip over geological time. Their key
biological assumption was a step function: rocks shocked below 1 GPa survive
fine, everything above that is dead. That was a reasonable call in 2000 when
there wasn't much data.

The problem is that Meyer et al. (2011) actually measured *B. subtilis* spore
survival at different shock pressures in gabbro — 60% at 5 GPa, 12% at 10 GPa.
Not zero. So I replaced the step function with that curve, added a
radiation shielding model, and convolved with Gladman's (1997) transfer time
distribution. The correction factor relative to Mileikowsky comes out to
about 10×. That's the main result.

The side finding I think is more interesting: the pressure range where
biology says "this is survivable" and the pressure range where geology says
"this is how we spot Mars rocks" are almost completely opposite. Maskelynite —
the glassy mineral that's basically our main way to identify Martian meteorites —
only forms above about 20 GPa. But most of the biological weight in the
model sits below 15 GPa. Which means there's probably a population of Martian
material sitting in collections that nobody's identified yet because it looks
like ordinary basalt. I'm calling this dark ejecta.

---

## Results summary

| | Value |
|---|---|
| Correction factor C | 10.8× (90% CI: 6.7–22.7) |
| Y_bio center | ~6 GPa |
| Meteorite collection center | ~39 GPa |
| Offset (Delta_P) | 33 GPa, robust across all tested models |
| Dark ejecta fraction | 91% of Y_bio below 15 GPa |
| Model P_cross | 19.3 GPa |
| Maskelynite range (Yu 2024) | 17–22 GPa |
| NWA 8159 shock pressure | 15–23 GPa (Sharp 2019) |

Those last three rows all converge near 19–20 GPa. That's what Figure 1 shows.

---

## Figures

`fig1_hero_crossing_pressure.png` — the main one. Biological weight Y(P) and
geological detection D_geo(P) crossing at 19.3 GPa, with the Yu 2024
maskelynite range and NWA 8159's shock pressure both landing right there.

`fig2_correction_factor_distribution.png` — Monte Carlo distribution of C.
Every single run came out above 5.

`fig3_dark_ejecta_zones.png` — the mismatch between what the model says
matters biologically and what actually ends up in the meteorite catalog.

---

## Running the analysis

Needs Python 3.10+ and numpy, scipy, matplotlib.

```bash
pip install numpy scipy matplotlib
python analysis/main.py
```

Takes a few minutes because of the MC iterations. Figures go to `figures/`.

Main parameters at the top of `main.py` — radiation model values are straight
from Mileikowsky, gamma is the fragment size exponent, Gladman distribution
uses log-mean = ln(5) Myr.

---

## What I'm uncertain about

The 1–5 GPa gap is the biggest issue. Meyer's measurements start at 4.6 GPa,
so below that I'm interpolating linearly toward S = 1 at zero pressure.
Sensitivity analysis suggests this changes C by at most 1.5×, which isn't
huge, but it would be nice to have actual data there. Nobody seems to have
measured B. subtilis survival in gabbro below 5 GPa. If you know of data
I've missed please open an issue.

The fragment size coupling r(P) ∝ P^(-γ) is also a guess. The physical
intuition is that lower-shock spallation produces bigger fragments, which is
mentioned qualitatively in Meyer et al. (2011, p. 716), but I don't have
hydrocode numbers to back up the specific exponent.

The D_geo model (logistic centered at 22 GPa) is a simplification. Nakhlites
were identified without maskelynite through geochemistry, so in reality the
geological detection function doesn't go to zero below 22 GPa — it's just
much lower. This means the actual sampling bias is real but probably not as
extreme as the model implies. The blind prediction test confirms this: the
model predicts a collection center of ~42 GPa, the actual collection is
closer to 30 GPa.

---

## The NWA 8159 thing

NWA 8159 is the first Martian meteorite found with preserved crystalline
plagioclase alongside partial maskelynite (Herd et al. 2017). Its shock
pressure range (15–23 GPa, Sharp et al. 2019) sits right at the model
P_cross. That's either a meaningful coincidence or just a coincidence.

I want to be clear that NWA 8159 was found in 2015, years before this
analysis. So this isn't a prediction I made — it's a consistency check.
The actual prediction is that there should be Martian basalt with *no*
maskelynite at all (P < 15 GPa), fully crystalline plagioclase, which
nobody's found yet. Finding that would actually confirm the model.
NWA 8159 is just sitting at the boundary of the zone I'm predicting.

---

## Submission plan

Going to Astrobiology first. They accept format-neutral first submissions
so the LaTeX cleanup can wait until after review.

**Cover letter:**

Dear Editors of Astrobiology,

I'm submitting "Continuous Biological Survival Landscapes Replace Binary
Pressure Thresholds in Mars–Earth Lithopanspermia Models" as a Research Article.

The Mileikowsky et al. (2000) estimate of viable Mars-to-Earth ejecta has been
the field's quantitative baseline for 25 years. It rests on a binary biological
assumption that experimental data published since then doesn't support. Meyer
et al. (2011) measured *B. subtilis* spore survival continuously across shock
pressures in gabbro — 60% at 5 GPa, 12% at 10 GPa. Yu et al. (2024) showed
that maskelynite forms under single-shock conditions at 17–22 GPa, substantially
lower than assumed from earlier reverberant experiments.

Integrating these with the Gladman (1997) transfer-time distribution gives a
correction factor C = 10.8 (90% CI: 6.7–22.7) relative to Mileikowsky, robust
across 100,000 Monte Carlo samples, seven ejecta models, and six microbial
datasets. Worth et al. (2013) also documented a factor-of-10 typographic error
in the Mileikowsky F(2l) value; combining both corrections gives roughly 430×
relative to the published number.

The structural finding is a 33 GPa offset between the biological weight center
of the ejecta distribution and the geological detection center of the known
collection. This offset is robust across all tested model variations. Three
independent constraints converge near 19–20 GPa: the model crossing pressure
(19.3 GPa), the Yu et al. (2024) maskelynite range (17–22 GPa), and the shock
pressure of NWA 8159 (15–23 GPa), the first Martian meteorite with partial
crystalline plagioclase. This is a consistency test rather than a prospective
prediction. The actual falsifiable prediction — Martian basalt with fully
crystalline plagioclase at P < 15 GPa — hasn't been found yet. Table 1 gives
a five-criterion identification protocol.

Astrobiology is the right venue given the combination of shock biology, ejecta
impact physics, and Mars petrology involved. The search protocol connects
directly to ongoing Mars Sample Return planning and Antarctic meteorite programs.

No competing interests. Code and data at https://doi.org/10.5281/zenodo.22982602

Sincerely,
Bilal Talha Özturk

---

## Suggested reviewers

Hard to find one person who knows impact physics, shock biology, and Mars
petrology all at once, so mix-and-match works fine.

- Charles Cockell (Edinburgh) — shock survival, astrobiology generally
- Natasha Artemieva (PSI) — Mars ejecta simulations
- Caroline Smith (NHM London) — Mars meteorites
- Vera Assis Fernandes (MfN Berlin) — meteorite geochronology

Avoid: Meyer et al. 2011 group and Mileikowsky 2000 group (used their work
or correcting it). Chris Herd (Alberta) would be ideal for the NWA 8159
context but wrote the NWA 8159 paper so there's an obvious conflict.

---

## Things I'd do differently with more time

- **1–5 GPa experiment** — this is the most important thing. Someone needs to
  measure B. subtilis in gabbro at 1, 2, 3, 4 GPa. I've tried to frame it as
  an experimental priority in the paper.

- **Turyshev (2026) comparison** — there's a recent preprint (arXiv 2604.03916)
  that also revisits the Mileikowsky framework. I couldn't get the full text in
  time for a proper comparison. They use a different approach (F_bur parameter
  rather than continuous S(P)) so the results aren't directly comparable, but
  a side-by-side would be useful.

- **ANSMET catalog** — worth going through the anomalous basaltic achondrites in
  the Meteoritical Bulletin database and checking Fe/Mn ratios. Might already
  be some candidates sitting there unrecognized.

---

## Citation

Özturk, B. T. (2026). Continuous biological survival landscapes replace binary
pressure thresholds in Mars and Earth lithopanspermia models. Preprint.
https://doi.org/10.5281/zenodo.22982602

