_Hvordan vælger vi EU cloud med GPU for undervisning og læring? _

## Problemet med Hetzner GEX

Hetzners dedikerede GPU-servere (GEX-serien, fx GEX131) har:

- **engangs-opsætningsgebyr**, og
- **månedlig fakturering** — instansen kan ikke "slukkes" for at spare penge; den koster, så længe den eksisterer (en glemt GEX131 ≈ 640 €/måned).

Til en 6-timers lab betyder det, at man betaler for langt mere, end man bruger, og at "ansvarlig nedlukning" reelt kun kan ske ved at **slette** hele serveren.

## Den vigtige faldgrube: "stop" ≠ gratis

På de fleste GPU-clouds stopper **strøm-af / stop** _ikke_ faktureringen: instansen reserverer stadig GPU'en, så du betaler videre, så længe den eksisterer.

Den reelle besparelsesstrategi er derfor ikke "tænd/sluk", men:

> **Timefakturering + intet opsætningsgebyr + slet-og-genskab** (evt. fra et snapshot/custom image, så genopsætning tager minutter).

Det er præcis det, Hetzners dedikerede GEX ikke giver.

## EU-udbydere der passer til mønstret

|Udbyder|HQ / dataresidens|Fakturering|Opsætningsgebyr|Noter til laben|
|---|---|---|---|---|
|**Scaleway**|Frankrig (Iliad), Paris|Pr. time, ingen binding|Ingen|H100, L40S, L4. GDPR-native, ingen US CLOUD Act-eksponering. Tætteste drop-in.|
|**OVHcloud**|Frankrig, multi-EU|Pr. time (~€0,88–1,80/time)|Ingen|H100/A100. ⚠️ Vælg **timefakturering** ved opstart — kan ikke skiftes fra månedlig senere.|
|**Exoscale**|Schweiz (EU-fokus)|Pr. time|Ingen|Suveræn EU-cloud; simpel start/slet.|
|**DataCrunch (Verda)**|Finland|Pr. time + spot|Ingen|Billig H100/A100, vedvarende energi; nogle typer kan pauses.|
|**Nebius**|Holland|Pr. time|Ingen|H100/H200 i skala; mest til større jobs.|
|**Genesis Cloud**|Tyskland|Pr. time|Ingen|EU grøn energi, on-demand GPU.|

## Anbefaling

Til en 6-timers session med én delt vLLM-instans er **Scaleway** eller **OVHcloud** de tætteste erstatninger for manualens setup:

1. Spin en H100/L40S op.
2. Kør den samme kommando: `vllm serve … --host 0.0.0.0 --port 8000 …`
3. Udlevér `http://<ip>:8000/v1` til holdet.
4. **Slet** instansen til sidst.

Ingen opsætningsgebyr, og du betaler kun for de timer, du faktisk bruger.

## Praktiske tips (bevarer "ansvarlig nedlukning"-læringsmålet)

- Gem et **custom image/snapshot** efter første opsætning, så genskabelse næste gang tager minutter i stedet for fuld vLLM-installation.
- Hold øje med efterladt **block storage / reserverede IP-adresser** efter sletning af VM'en — de fakturerer videre hos alle udbydere (samme advarsel som manualens §Omkostninger).

## Kilder

- [Scaleway vs OVH-sammenligning](https://gartsolutions.com/scaleway-vs-ovhcloud/)
- [Cloud GPU Europe & GDPR](https://cloudgputracker.com/cloud-gpu-europe/)
- [OVHcloud GPU-priser](https://deploybase.ai/articles/ovhcloud-gpu-pricing)
- [Exoscale GPU Europe](https://www.exoscale.com/gpu/)
- [DataCrunch/Verda EU-alternativer](https://www.spheron.network/blog/datacrunch-verda-alternatives/)
- [GPU cloud pricing 2026](https://www.spheron.network/blog/gpu-cloud-pricing-comparison-2026/)
