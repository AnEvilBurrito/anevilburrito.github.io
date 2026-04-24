---
title: "Optimising 8-th Gen Intel CPUs for Power Efficiency"
date: 2026-04-24
permalink: /posts/2026/04/optimizing-xps-15-9570-efficiency/
categories:
  - work log
tags:
  - hardware
  - optimisation
  - linux
  - battery life
---

I have a old Dell XPS 15 9570, which has certainly lived past its era. Still, as a general interest I decided to see how far this old machine can still be pushed. 

For some background, this laptop has already been optimised at the software, OS and hardware level. 

## The Baseline Optimisations
1. It has gone through a recent battery change (97Wh)
2. For OS, it runs on Ubuntu 24.04 LTS, with appropriate `tlp` and `auto-cpufreq` configurations to ensure no conflict between them.
3. The CPU core and package is undervolted at -130mV, the embedded GPU is undervolted at -100mV. 
4. The dedicated Nvidia video card is fully turned off to maximise power savings when the laptop is not plugged in.
5. Where graphics are used, the majority of the load is hardware-accelerated (if supported).  
6. The stock Killer Wi-Fi card was replaced with Intel AX210, which reduces CPU wakeups significantly. 

With some further `grub` loader flag adjustments, the laptop can achieve an impressive idle wattage floor at ~5-6w with no applications in background, which translate to a near 18-hour pure idle battery life. The display is the 4K OLED variant, set at 30% screen brightness for the wattage draw measurement. (Note: only impressive relative to its era, where modern systems could see a <2w total system idle draw).

## The Problem: Active Power Draw
However, in active use, the 8-th Intel H-series CPU still tend to be a 'watt-guzzler', flooring up to 13-16w package power draw, subsequently raising the total system power draw to around ~20-25w. This happens when loading content heavy pages or intensive scrolling long conversation logs across ChatGPT sessions and Claude. The same happens when YouTube and Reddit is initially loaded.  

Assuming a research heavy session with many page jumping and constant scrolling, I can realistically expect a 6-8 hour real world battery life by experience. This is acceptable for me most of the times, but curiosity has pushed me to wonder if I can do even better. 

## Finding the Efficiency Sweet Spot
The efficiency of 8-th Intel CPUs have been well studied, and general power scaling law has determined that with increased voltage there is diminishing returns: 

$$Power = Voltage^2 \times Frequency$$

Prompting Claude about the optimal 8-th Gen Intel chip power efficiency has led to an initial starting point: 

> The **optimal power efficiency point** for 8th Gen Intel (Coffee Lake/Kaby Lake-R) CPUs is typically in the range of **800 MHz – 1.2 GHz** under the Intel Speed Shift / HWP (Hardware P-state) framework.
> 
> Here's a practical breakdown:
> 
> **Idle / Light Load:** 800 MHz – 1.2 GHz at ~0.7–0.8V. This is the sweet spot where performance-per-watt is highest — the CPU does useful work while drawing minimal power (often under 3–5W)
> 
> **Efficiency Range:** 1.2 GHz – base clock (e.g., 2.2–3.6 GHz depending on SKU). Voltage scales up gradually; still reasonable efficiency but watts climb faster than performance.
> 
> **Beyond Base / Turbo Boost:** Once the CPU enters turbo frequencies (e.g., 3.9–4.7 GHz for i7-8700K), power consumption rises steeply — often 2–4× the wattage for a modest performance gain, which is the least efficient zone.

A following prompt about the H-series chip: 

> **Optimal Efficiency Sweet Spot: ~1.0 – 1.8 GHz**

> At this range, the chip sits in a low-voltage state (~0.75–0.85V) drawing roughly **6–12W**, which is well within efficient territory for a 45W-class part.

> **Why higher than U-series?** H chips have more cores (6c/12t vs 4c/8t on U), so the per-core workload at idle is distributed differently — the efficiency island sits a bit higher on the frequency curve.

The stock maximum frequency of the i7 8750H is at 2.2GHz, this is markedly higher than what Claude has claimed. **We should not be trusting LLMs blindly**, so let's verify it with some benchmark testing. 

Efficiency is usually measured by how much battery life you use to complete a specific task, just because we are running at a lower clock (e.g. 1.5GHz) doesn't mean it is more efficient at power draws. With the 'Race to sleep' principle, if the processor can finish its task much earlier at a higher power draw, it can return to its idle state a lot faster and therefore draw even less power compared to underclocking where the task will run for longer. With this base assumption, our null hypothesis is that 2.2GHz is already optimally designed for maximum power efficiency. 

## Benchmarking
Regardless, for curiosity I have performed the benchmark as outlined below: 

For measuring power draw: 

`sudo turbostat --Summary --show Bzy_MHz,PkgWatt,CorWatt,PkgTmp`

this is ran constantly in the background, with stats refreshing every 5 seconds. 

For the CPU-stress benchmark: 

`sudo apt install sysbench`

`sysbench cpu --threads=12 run`

The total time of the test is around 10 seconds, and the `event/second` statistic is extracted.

To ensure consistency, three power consumption rows are extracted from `turbostat` following the stress test is ran. Since each interval is equal (~5 seconds), the power consumption wattage is simply summed. 

1.5GHz was found to be the optimal for performance per watt under this simple test, as measured by events/second/W: 

## Results and Analysis

| GHz | W₁    | W₂    | W₃   | Total W | events/s | events/s/W | vs 1.5GHz |
| --- | ----- | ----- | ---- | ------- | -------- | ---------- | --------- |
| 1.4 | 5.32  | 6.53  | 1.82 | 13.67   | 42,250   | 3090.7     | -0.8%     |
| 1.5 | 5.4   | 6.97  | 2.24 | 14.61   | 45,527   | 3116.2     | 0.0%      |
| 1.6 | 6.45  | 7.34  | 1.91 | 15.7    | 48,615   | 3096.5     | -0.6%     |
| 1.7 | 7.27  | 8.57  | 2.08 | 17.92   | 51,089   | 2850.9     | -8.5%     |
| 1.8 | 7.96  | 8.99  | 1.95 | 18.9    | 54,370   | 2876.7     | -7.7%     |
| 2   | 8.67  | 11.07 | 2.46 | 22.2    | 61,102   | 2752.3     | -11.7%    |
| 2.2 | 10.56 | 12.86 | 2.51 | 25.93   | 67,224   | 2592.5     | -16.8%    |

Given the trivial difference in power efficiency between 1.5 and 1.6GHz, we can conclude that 1.6GHz is the best choice for this particular stress test given its higher performance in events per second. It draws slightly more power, but it is also able to get more done quickly without sacrificing power efficiency. 

I performed everyday tasks on the laptop at 1.6GHz. As expected it is noticeably slower, but not to an unusable extent. But, the power draw from opening content-heavy new pages / heavy scrolling in browser has also dropped to around 15-16w. If the power efficiency statistic is to be extracted, there should be around 15-25% increase in battery life during active use, since each watt is now simply doing more work. 

## Real-World Experience
For personal use, I found this set-up to be 'okay', not great, and I do envy the efficiency and performance gain of the new lunar-lake and panther-lake intel architectures (or Apple Silicon). Electron applications start-up quite slowly when the CPU is at 2.2GHz, it is even slower at 1.6GHz. I'd presume most people will not be happy with this trade-off when system responsiveness is preferred. 

This benchmark set-up is also pretty rudimentary, it probably isn't robust enough to lead to any conclusions. Perhaps in other benchmarks, 2.2GHz can be found to be the most power efficient. 

So maybe there is more time for me to stare at Claude doing work or to do a few more google searches? But that's about it? 

## Final Thoughts
But, with `auto-cpufreq`, the switch between '1.6GHz' to '2.2GHz' is pretty toggle-able, this can lead to some interest use cases (e.g. use stock setting for opening up applications, and then cruise at 1.6GHz later). 

Perhaps the findings here could be extrapolated to other legacy hardware running 8-th Gen H-series Intel chips, especially in the Linux community where used ThinkPad laptops such as the T480 usually have a 8-th Gen intel CPU inside. 
