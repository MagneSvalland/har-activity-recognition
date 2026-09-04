# Maskinlæring for aktivitetsgjenkjenning

**Machine learning for activity recognition: a comparison of performance, robustness and explainability**

Bacheloroppgave i Informasjonsteknologi, Høgskulen på Vestlandet (HVL), vår 2026.

> Denne oppgaven undersøker hvordan ulike maskinlæringsmodeller presterer på Human Activity Recognition (HAR) med hensyn til **ytelse**, **robusthet** og **forklarbarhet**, og hvilke modeller som gir best balanse mellom disse.

## Sammendrag

Prosjektet bruker det multimodale [SPHERE-datasettet](https://irc-sphere.ac.uk/) (akselerometer, PIR-bevegelsessensorer og RSSI-signaler) til å klassifisere dagliglivsaktiviteter i et realistisk smarthjemsmiljø.

Arbeidet startet med en **baseline-pipeline** (klassiske ML-modeller + en første 1D-CNN på aggregerte data), der ensemble-modeller som Random Forest og Gradient Boosting presterte best, mens overgangsaktiviteter, klasseubalanse og usikre annotasjoner var de klart største utfordringene.

Basert på disse funnene ble det utviklet en **V3-pipeline** med:
- **Temporal feature engineering** (lag/lead-variabler for tidskontekst)
- **Clean semantic class mapping** (sammenslåing av overlappende overgangsklasser)
- **Confidence-filtering og -weighting** basert på datasettets label-confidence (`max_prob`)
- En sekvensbasert **1D-CNN V2** som bruker rå tidsseriedata i stedet for aggregerte features

**Hovedfunn:** Forbedringer i datapipeline og temporal kontekst hadde større betydning for klassifikasjonsytelsen enn økt modellkompleksitet alene. Gradient Boosting på V3-pipelinen oppnådde best samlet resultat i prosjektet. Robusthet ble undersøkt gjennom random-seed-eksperimenter, sensorablasjon og støyeksperimenter, mens forklarbarhet ble undersøkt med SHAP og permutation importance — begge pekte konsistent på akselerometerdata som viktigste kilde til modellenes beslutninger.

## Innhold i repoet

```
├── notebook/
│   └── DAT191_H3.ipynb      Fullstendig Colab-notebook: dataanalyse, pipelines,
│                             modelltrening, evaluering, robusthet og forklarbarhet
└── docs/
    ├── rapport.pdf           Hovedrapport (bacheloroppgaven, 56 sider)
    ├── prosjekthandbok.pdf   Vedlegg A – prosjektplanlegging, metodikk og fremdrift
    └── presentasjon.pdf      Vedlegg B – lenke til videopresentasjon av prosjektet
```

Notebooken er selvstendig strukturert i seksjoner for dataforståelse, evalueringsmetodikk, baseline-modeller/resultater, V3-pipeline/resultater, og en avsluttende sammenligning — se innholdsfortegnelsen i selve notebooken eller kapittel 4–5 i rapporten for full detaljering.

## Metode i korte trekk

| | Baseline-pipeline | V3-pipeline |
|---|---|---|
| Tidsoppløsning | Ett sekund, isolert | Lag/lead-variabler for temporal kontekst |
| Klasser | Alle originale klasser separat | Semantisk sammenslåtte, mer robuste klasser |
| Annotasjonsusikkerhet | Hard-label (argmax) | Confidence-filtering + confidence-weighting |
| Modeller | Dummy, Logistic Regression, Linear SVM, Random Forest, Gradient Boosting, 1D-CNN V1 | Samme klassiske modeller + sekvensbasert 1D-CNN V2 |

Evaluering ble gjort langs tre akser:
1. **Klassifikasjonsytelse** — accuracy, precision, recall, macro-F1, weighted-F1
2. **Robusthet** — random seed, sensorablasjon, gaussisk støy
3. **Forklarbarhet** — SHAP (global og lokal), permutation importance, innebygd feature importance

## Forfattere

Hans Christian Gustafsson, Jonatan Dam og Magne Svalland — Institutt for datateknologi, elektroteknologi og realfag, HVL.
Veileder: Faiga Alawad.

## Lisens

Rapporten, prosjekthåndboken og presentasjonen er akademisk arbeid levert til HVL og deles her for portefølje-/referanseformål — alle rettigheter forbeholdes forfatterne. Koden i notebooken er delt under [MIT-lisensen](LICENSE) med mindre annet er spesifisert i selve filen.
