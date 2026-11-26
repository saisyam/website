---
title: "My Watch Said 96%. The Minute-by-Minute Data Said Severe Sleep Apnea."
description: "For 50 nights my watch app showed blood oxygen averages between 94% and 98%. The per-minute readings underneath showed drops below 90% on 49 of those nights. A sleep test confirmed severe sleep apnea. A personal case study in what averages hide, and why the grain of the data matters."
date: 2026-11-26T08:00:00-06:00
slug: "averages-hide-sleep-apnea"
kicker: "Field case study"
featured: true
image: /images/t21-cover.png
categories: ["Data analysis", "Consumer AI", "Time series"]
---

For 50 nights in a row, my Garmin app told me my overnight blood oxygen was fine. The nightly average sat between 94% and 98%, 96.3% across all 50 nights. Nothing about those numbers suggested a problem.

The minute-by-minute readings underneath told a different story. On 49 of those 50 nights my oxygen dropped below 90%. On 34 of them it dropped below 85%. The lowest reading was 77%.

A home sleep test confirmed it: severe obstructive sleep apnea, 43 breathing events an hour.

This article is about how I found that, and about the lesson that matters to anyone who works with data: **an average can be correct and still hide the one thing you need to see.**

## The average was right. It just wasn't the answer.

Here is what 50 nights looked like at two levels of detail.

| What I looked at | What it said |
|---|---|
| Nightly average in the app | 94% to 98% every night; 96.3% overall |
| Nights with any minutes below 90% | 49 of 50 |
| Minutes below 90% per night | median 23, worst night 106 |
| Nights with any minutes below 85% | 34 of 50 |
| Lowest single reading | 77% |
| Separate drops below 90% | 544, three out of four lasting 1 to 2 minutes |

The last row explains why the average stayed calm. The drops were short and frequent. A night has about 400 minutes of readings. If 26 of them fall to around 87% instead of 95%, the night's average moves by about half a point. The app's summary rounds that away.

Anyone who builds dashboards has seen the same thing. A pipeline's average runtime looks normal while three runs a day time out. An API's mean latency is fine while one request in fifty takes ten seconds. A daily SLA is green while a twenty-minute outage sits inside it. Aggregation simplifies; it also hides outliers, frequency and timing, which is often exactly where the problem lives.

## One night, at full detail

This is the night of my sleep test, as the watch recorded it, one reading per minute.

![Per-minute blood oxygen readings for one night. The night average is 94%, but readings drop below 90% in repeated short dips through the night: 26 minutes in total, lowest 81%.](/images/t21-sleep-test-night.png)

The dashed line is what a summary shows: 94%. The red dots are what it leaves out: short drops below 90%, in clusters, all night.

## How I got to the per-minute data

The Garmin Connect app shows a nightly average. The per-minute readings exist, but the app doesn't show them.

I retrieved them with [python-garminconnect](https://github.com/cyberjunky/python-garminconnect), an open-source Python wrapper for Garmin Connect that signs in through [garth](https://github.com/matin/garth). The per-minute oxygen readings come back inside the sleep data, in a field called `wellnessEpochSPO2DataDTOList`, with a timestamp, a value and a reading-confidence score for each minute. One caution: the library is unofficial. It uses the same access the Garmin website uses, not a Garmin-supported API, so it can break when Garmin changes things.

I wired that into Claude Code so I could ask questions about my own history instead of clicking through dashboards. The analysis itself was simple, the kind any data engineer would write:

- **Time below a threshold:** minutes below 90% and below 85%, per night.
- **Events, not averages:** consecutive minutes below 90% grouped into separate drops, each with a length and a lowest value.
- **Frequency across nights:** how many nights had any drop at all.

None of these calculations is hard. What made the difference was that the AI did them without being asked, at the level of detail where the problem was. While reviewing my sleep and heart rate variability, Claude flagged both as worth attention and suggested turning on overnight Pulse Ox. When the readings came in, it went straight past the reassuring average to the recurring drops, and recommended I see a doctor and ask for a sleep study. I did.

## What the sleep test found

A sleep specialist ordered a home sleep apnea test (WatchPAT) for the night of November 3. The key results:

| Result | Value |
|---|---|
| Diagnosis | Severe obstructive sleep apnea |
| Apnea-hypopnea index (breathing events per hour) | 43.3 (severe is 30 or more) |
| Oxygen desaturation index (drops of 4% or more per hour) | 35.2 |
| Lowest oxygen | 61% |
| Minutes below 90% | 16.2 |
| Events per hour on my back vs left side | 58.9 vs 16.3 |

From there it was in my doctor's hands.

## The watch caught the pattern, not the depth

The test night gave me a rare thing: the watch and a clinical device measuring the same night.

| Same night | Watch | Sleep test |
|---|---|---|
| Minutes below 90% | 26 | 16.2 |
| Minutes below 85% | 7 | 7.8 |
| Lowest reading | 81% | 61% |

The watch got the time below the thresholds roughly right. It badly missed the depth: 81% against 61%. That is the same lesson one level down. The watch reports one value per minute, and a drop that lasts 20 seconds gets averaged into its minute. So the watch is good for spotting that a pattern exists. It is not good for measuring how severe it is. That is what the clinical test is for.

## Who did what

It matters to be precise about this:

- **The watch** collected the measurements.
- **AI** helped me look at the right level of detail, spotted the pattern and suggested a medical evaluation.
- **A sleep specialist** evaluated the evidence and ordered the test.
- **A clinical sleep test** established the diagnosis.

AI was not my doctor. It was an analytical layer that got me to one sooner.

## Conclusion: averages hide, and AI makes looking underneath routine

The data lesson is simple to state: **averages hide crucial details.** Mine said 96% for 50 nights. Underneath it were 544 drops below 90%, on 49 of those nights, and severe sleep apnea.

The harder part is that people rarely look underneath. Fifty nights at one reading a minute is about 20,000 readings. No one scrolls through that by hand, and the app's summary gives no reason to. I had all the data for weeks and saw only the average.

That is where AI changed the outcome. It went to the minute-level data by default, ran the checks a careful analyst would run (time below a threshold, separate events, how often across nights), and noticed a pattern I wasn't looking for. The calculations are ordinary; doing them on every reading, unprompted, is what a person doesn't do. The value was not a clever model. It was an analyst that always looks at the grain.

The same holds for any data team:

1. **Keep the grain.** Store the detailed data, not just the daily rollup. The answer was in data I already had; the summary just didn't show it.
2. **Measure time below a threshold, not only the mean.** Minutes below 90% told the story; the average never did.
3. **Count events.** 544 separate drops is a different fact from "average 96%."
4. **Watch the edge of the measurement.** One-minute averaging hid how deep the drops went. Every metric has a resolution; know what falls below it.
5. **Let AI do the looking.** Point it at the detailed data, not the dashboard, and ask it what the summary hides.

## Limits

- This is one person's data, not a study. It says nothing about how reliably wrist oximeters detect sleep apnea in general.
- Wrist oximeter readings can be wrong: movement, fit and skin contact all affect them. That is why the watch only prompted a test; it didn't replace one.
- The data access uses an unofficial library.

## What's next

My first analysis sent my health data to a cloud model. For wearable data I was comfortable with that. For blood tests and medical records, I'm not. The next step is to see how much of this analysis a small model running on my own laptop can do, so the most sensitive data never leaves my machine. That question, what it takes for a small local model to use tools reliably, is the subject of next week's article.

---

*This article describes my personal experience and is not medical advice. A wrist oximeter cannot diagnose sleep apnea, and an AI interpretation is not a clinical evaluation. If your readings show repeated drops, or you have symptoms such as loud snoring, gasping at night or daytime sleepiness, talk to a doctor about a sleep study.*
