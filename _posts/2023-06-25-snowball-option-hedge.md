---
layout: article
title: Research on Snowball Option--Trader Hedge
mathjax: true
tags: Quantitative
---

This is the second article of the series, discussing how traders can profit from this structure and how to hedge against potential risks.
<!--more-->

---

Snowball options are a type of path-dependent exotic derivative that has been gaining increasing traction in the recent Chinese market. One contributing factor to its popularity is its extraordinarily high structured coupon payments, which are particularly appealing in the current low-interest-rate environment in China. However, elevated returns come with increased risks. From the client's perspective, it's imperative to have a thorough understanding of these risks; from a trader's standpoint, it's crucial to comprehend the sources of risk and appropriate hedging strategies. This article serves as a part of a series that ranges from snowball to rainbow options, and will initially focus on snowball options and hedge from the trader's perspective.

## Menu

- [1. Snowball, a free gift from traders? ](#1-snowball-a-free-gift-from-traders-)
  - [1.1 Why are snowball options usually free? ](#11-why-are-snowball-options-usually-free-)
    - [1.1.1 A traditional option](#111-a-traditional-option)
    - [1.1.2 A snowball option](#112-a-snowball-option)
- [2. How to model it? ](#2-how-to-model-it-)
  - [2.1 Modeling Options:](#21-modeling-options)
  - [2.2 MC Method](#22-mc-method)
- [3. Theoretical result of snowball option ](#3-theoretical-result-of-snowball-option-)
  - [3.1 Results Analysis](#31-results-analysis)
  - [3.2 Option Price](#32-option-price)
  - [3.3 Computational Efficiency Issues](#33-computational-efficiency-issues)
- [4. My comments on snowball option](#4-my-comments-on-snowball-option)


# 1. Snowball, a free gift from traders? <a id="1-what-is-snowball-option-"></a>
## 1.1 Why are snowball options usually free? <a id="structure"></a>
Most market participants understand that there's no such thing as a free lunch. Still, few can precisely elucidate why a trader might be willing to sell a derivative with a **positive present value (PV)** on Day 0 for zero cost. This seems akin to an arbitrage opportunity of the first degree. However, the reality is that if such a lucrative opportunity genuinely existed, traders would preemptively exhaust the liquidity of that product. (As financial professionals, it might be time to dispense with the notion of 'infinity'.) The willingness of traders to seemingly give away these options for free lies in the profitable hedging opportunities they present.

### 1.1.1 A traditional option
Reflecting upon standard calls or puts, when traders sell these options, they often face losses in hedging. The core issue stems from **buying high and selling low**.

To illustrate this:

1. Selling a Put Option: Let's assume a trader sells a put option on a stock currently priced at 51 with a strike price of 50. **Assume he/she gets X for selling it.**
2. Initiating the Hedge: To hedge the **negative delta** of the sold put, the trader might buy a certain amount of the stock at the current price of 51.
3. Price Decline: Suppose the stock price declines to 48. The put option is now closer to being in-the-money, increasing its **negative delta**. To adjust the hedge and maintain a delta-neutral position, the trader would need to sell some of the stock they had previously bought at 51.


Result: This forces the trader to sell the stock at the reduced price of 48, realizing a loss. The trader effectively bought high at 51 and had to sell low at 48 due to the hedging adjustment, even though the initial intent of the hedge was to mitigate risk.

Given that hedging can result in losses, when not considering any other factors, traders would naturally demand a positive price to sell the product.

### 1.1.2 A snowball option
Even though the above example doesn't take into account transaction costs, which would make actual hedging losses even greater, it does provide a perspective. If traders had access to a hedging product that allowed them to sell high and buy low, would they be willing to "pay" for it?


# 2. How to model it? <a id="model"></a>
## 2.1 Modeling Options:
Modeling financial options is a matter of prime concern for both investors and financial institutions, as it directly impacts investment returns. When it comes to Snowball options, there are generally two approaches: the Monte Carlo (MC) method and the Partial Differential Equation (PDE) method. In this series of articles, I will mainly use the MC method for modeling, which is quite straightforward and offers the most intuitive insight into our results. The PDE method, on the other hand, has its unique advantages in hedging strategies, and I plan to delve into this in a separate, specialized topic.

Recently, machine learning techniques are gaining increased attention in the field. This is an area of personal interest for me, and should there be noteworthy developments in the future, I intend to share them through articles.

## 2.2 MC Method

Suppose we have:

$$
dS_t = \mu S_tdt+\sigma S_t dB_t
$$

where $S_t$ is the stock price at t, $\mu$ is the drift rate, $\sigma$ is the volatility and $dB_t = \epsilon * \sqrt{dt}$, $\epsilon$ is the standard normal distribution.

We can get from BS equation:

$$
S_T = S_t e^{(\mu-\sigma^2/2)(T-t) + \sigma \epsilon \sqrt{T-t}}
$$

The remaining part of the modeling will be quite straightforward. Given the set of parameters, we randomly sample $\epsilon$ to generate a simulated stock price path. Utilizing this path, we can clearly identify the corresponding returns. By repeating this experiment multiple times, we will arrive at an average value for the expected returns.

From the perspective of the investor alone, hedging considerations are irrelevant; we simply need to calculate the expected return and its variance, and then discount these back to the initial time.

Of course, many simplifications are made in this process. A typical one is assuming constant volatility. On the other hand, we could incorporate short-selling costs, like a constant or piecewise constant rate, without adding significant complexity to our model.

Given the simplicity of the parameters involved, advanced coding techniques—such as extensive vectorization, parallelization, or rewriting the computation in a more efficient language—are not strictly necessary, though they could be useful in more complex calculations down the line.

The complete code can be found in the linked document, which includes both the pure Python code as well as a more readable Jupyter Notebook version. Right now, C++ or Matlab may not be necessary, but we will use them soon on Rainbow option.


# 3. Theoretical result of snowball option <a id="result"></a>
## 3.1 Results Analysis
Based on the Monte Carlo simulation results, the return on this option is approximately 5.07%. This implies that its value is about 5.07% of the notional principal amount. Upon observing the return profile, it can be discerned that it consists of three parts: knock-out returns that are Poisson-like distributed, knock-in losses that resemble a normal distribution, and a "smooth landing" that constitutes its own category. In fact, this third component is a significant part of the overall return, contributing roughly +3%.

After considering discounting, the maximum return stands at around 19%, while the maximum loss could be as high as 38%. As evident, the variance of this instrument is quite high. This is further supported by its high standard deviation of 10%, which aligns with the principle behind such structured products: exploiting high volatility to achieve high structured yields while simultaneously concealing the potential for massive losses under certain circumstances.

## 3.2 Option Price
So, the question arises, what is the option pricing for this instrument? Surprisingly, it's often zero. That is to say, if we don't consider any hedges, the seller of the Snowball option is typically operating at a loss. Generally, under a zero pricing premise, the product can be sold at a minimal loss by fine-tuning the returns. Of course, investors are usually required to post a certain amount of margin, say 25%, which remains in the seller's account during the life of the option, thereby generating risk-free returns. However, even when considering this, the Snowball option still holds a value of 4.7%. The allure of the Snowball option is precisely this opportunity to earn money almost 'for free.'

Why can the Snowball option be sold at zero cost? This aspect makes it even more interesting than conventional vanilla call or put options.

Let's illustrate this with an example: suppose you believe that a piece of geologically unstable land contains gold, and you own an expensive automated mining machine. You may be eager to remotely control the machine to dig but are concerned that it may get damaged underground. At this moment, an investor offers to sell you an insurance contract. In exchange, you pay a fixed sum, an offer that you might find tempting.

Indeed, the investor, acting as the put option provider, bears the risk of honoring the insurance contract. As the seller of the option, they don't have to worry about the machine getting damaged but do need to assess whether enough gold can be extracted to make the operation worthwhile. In reality, hedging a Snowball option often involves buying high and selling low, which is usually accompanied by a positive return. This is precisely why it can be sold at a loss; what's needed is essentially the put option as a form of insurance.

In our next article, we will delve deeply into the issue of hedging, which is arguably one of the most critical considerations for anyone working in quantitative finance

## 3.3 Computational Efficiency Issues
In the corresponding IPython notebook, a rudimentary calculation of this problem shows that it takes approximately 20 seconds to compute 100,000 Monte Carlo paths on my computer. This level of efficiency is clearly unacceptable. The primary reason for this slow performance is the lack of any optimization techniques, coupled with Python's inherently low efficiency when it comes to loops. As we delve deeper into the issue, we will employ various methods to enhance computational speed and make the calculations as fast as possible.


# 4. My comments on snowball option
For any international investor, the snowball option is undoubtedly familiar. It is essentially a non-capital-guaranteed autocall. In this respect, China's financial development is merely imitating and following the international path. In subsequent articles, we will also see that, in order to achieve higher coupon rates, we will employ a "worst-of" strategy by combining multiple underlying assets to attain what is referred to as "target high yield."

However, apart from these tricks, what really appeals to me about these types of options is that the trading parties are not in an adversarial relationship, but rather hold non-conflicting views on the market. They leverage the trader's hedging abilities to progressively realize these views.

In the most straightforward call and put options, the parties could be considered engaged in a zero-sum game; one party's gain largely comes from the other party's loss. This is not the case with snowball options. In fact, investors believe the market is stable and that there is a probability of exceeding the initial price level during several observation dates (expecting the market to be entirely event-free in the next 1-3 years is unrealistic). Financial institutions, on the other hand, believe that the market volatility will be sufficiently high. The only scenario that could result in substantial losses for financial institutions is if stock prices slowly decline but don't drop enough to reach the knock-in level, signifying inadequate volatility.

Lastly, as financial derivatives without an option premium, what do financial institutions actually find attractive? Two aspects: a "buy low, sell high" hedging strategy and the valuable put options embedded within the structure. We will see them in later articles.