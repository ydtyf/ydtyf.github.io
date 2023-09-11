---
layout: article
title: Research on Snowball Option--Introduction
mathjax: true
---




You may want to [config the site](https://kitian616.github.io/jekyll-TeXt-theme/docs/en/configuration) or [writing a post](https://kitian616.github.io/jekyll-TeXt-theme/docs/en/writing-posts) next. Please feel free to [create an issue](https://github.com/kitian616/jekyll-TeXt-theme/issues) or [send me email](mailto:kitian616@outlook.com) if you have any questions.

<!--more-->

---

Snowball options are a type of path-dependent exotic derivative that has been gaining increasing traction in the recent Chinese market. One contributing factor to its popularity is its extraordinarily high structured coupon payments, which are particularly appealing in the current low-interest-rate environment in China. However, elevated returns come with increased risks. From the client's perspective, it's imperative to have a thorough understanding of these risks; from a trader's standpoint, it's crucial to comprehend the sources of risk and appropriate hedging strategies. This article serves as a part of a series that ranges from snowball to rainbow options, and will initially focus on snowball options and risk management from the client's perspective.

## Menu

- [1. What is snowball option? ](#1-what-is-snowball-option-)
  - [1.1 Snowball structure ](#11-snowball-structure-)
  - [1.2 Why it's popular and what might be the risk? ](#12-why-its-popular-and-what-might-be-the-risk-)
- [2. How to model it? ](#2-how-to-model-it-)
  - [2.1 Modeling Options:](#21-modeling-options)
  - [2.2 MC Method](#22-mc-method)
- [3. Theoretical result of snowball option ](#3-theoretical-result-of-snowball-option-)
- [4. My comments on snowball option](#4-my-comments-on-snowball-option)


# 1. What is snowball option? <a id="1-what-is-snowball-option-"></a>
## 1.1 Snowball structure <a id="structure"></a>
A typical snowball option structure encompasses the following components:

1. Underlying Asset: This can be an individual stock, an index, or another financial instrument.

2. Knock-Out Level: Generally set extremely close to the initial asset price to entice investors. Knocking out typically leads to extraordinarily high coupon payments, which constitute the primary avenue for investor profit.

3. Knock-Out Events: These are usually observed on a monthly basis to determine if the asset price has exceeded the knock-out level, thereby reducing the likelihood of a knock-out (and consequently increasing the coupon payment).

4. Knock-In Level: Typically set around 80% of the asset's initial price. In essence, the investor sells a put option to the financial institution at a 100% price level. **Once the asset hits the knock-in level, it is usually already in a deep-in-the-money state.**

5. Knock-In Events: These are observed daily to ascertain whether the asset price is below the knock-in level, thereby increasing the likelihood of a knock-in (and consequently increasing the coupon payment).

In addition to the fundamental elements outlined above, there are also advanced conditions that can be incorporated into the snowball option structure. For example, a lock-up period could be specified to preclude knock-out observations in the initial months. Another feature could be a diminishing knock-out level, which gradually reduces over time. Moreover, the knock-out level can be significantly lowered under certain circumstances, such as upon a knock-in event.

Below it's a typical product parameter. Suppose start price is $S_0$.

| Parameter    | Value   |
|-----------|--------|
| Term    | 12 months | 
| Index   | 000905.SH |
| Knock-out level     | 103%$S_0$ |
| Knock-in level   | 85%$S_0$ |
| Knock-out event     | Monthly check whether the index current price exceeds the knock-out level |
| Knock-in event   | Daily check whether the index current price is below the knock-in level |
| Knock-out coupon     | 20% |
| bonus coupon     | 20% |

I will use the following example to discuss the investment returns associated with this snowball option:

- Knock-Out Scenario: If a knock-out occurs in any month, i.e., the closing price for the day exceeds 103% of $S_0$, the option will immediately expire. The investor will receive the knock-out coupon payment proportional to the time elapsed. For instance, if the option ends after six months, the coupon payment would be 10%.

- No Knock-Out, No Knock-In Scenario: If neither a knock-out nor a knock-in occurs, the investor will be awarded a bonus coupon, which, in this case, is 20%.

- Knock-In Only Scenario: If only a knock-in event takes place, the investor is considered to have sold a put option. **There would be no return, and the investor bears the loss equivalent to the difference between the initial and terminal prices if the terminal price is lower than the initial price.**

## 1.2 Why it's popular and what might be the risk? <a id='popular'></a>
In the recent Chinese market, snowball options have become exceedingly popular. While there may be multiple factors contributing to this surge in popularity, the primary one appears to be the extraordinarily high returns that these options promise, with their associated risks seemingly being dismissed as inconsequential. Indeed, the typical duration of a snowball option ranges from 1 to 3 years, and the prospect of the underlying asset declining by 20% within this time frame seems implausible to many Chinese investors. This, coupled with China's low-interest-rate environment, explains the allure of snowball options for many. 

While some institutional players are involved, looking to exploit perceived mispricing, the overwhelming majority of investors are entering the market with either sophisticated or naïve expectations of market direction. Interestingly, there tends to be more buyers of snowball options when stock prices are rising than when they are falling, despite the fact that, based on the initial asset prices, purchasing during a downturn would ostensibly offer greater value.

So, what are the risks involved? Fundamentally, snowball options architect their structure to widen the variance, simultaneously offering structured high returns and uncontrollable, potentially catastrophic losses.

Take, for instance, the market plunge in April 2022, a time when Shanghai, China's largest city, was contemplating a lockdown to deal with an outbreak. The CSI 1000 index plummeted, reaching a low closing price of 5431.61. On that particular day, approximately half of the snowball options purchased within the previous two years would have triggered a knock-in event, exposing many investors to enormous losses through the embedded put options. While the actual losses were mitigated to some extent due to favorable market performance in 2021—leading many snowball options to knock out earlier—those acquired during the peak period of 2021 were almost universally subjected to heavy losses. This vulnerable period spanned nearly four months, and investors who purchased snowball options during this time are likely to incur staggering losses, potentially amounting to around 40%. In other words, while snowball options may appear to be risk-neutral products in expectation, they inherently embody a high likelihood of large gains offset by a low probability tail risk of devastating losses, resulting in extremely high variance.



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

Content for section 3.


# 4. My comments on snowball option
For any international investor, the snowball option is undoubtedly familiar. It is essentially a non-capital-guaranteed autocall. In this respect, China's financial development is merely imitating and following the international path. In subsequent articles, we will also see that, in order to achieve higher coupon rates, we will employ a "worst-of" strategy by combining multiple underlying assets to attain what is referred to as "target high yield."

However, apart from these tricks, what really appeals to me about these types of options is that the trading parties are not in an adversarial relationship, but rather hold non-conflicting views on the market. They leverage the trader's hedging abilities to progressively realize these views.

In the most straightforward call and put options, the parties could be considered engaged in a zero-sum game; one party's gain largely comes from the other party's loss. This is not the case with snowball options. In fact, investors believe the market is stable and that there is a probability of exceeding the initial price level during several observation dates (expecting the market to be entirely event-free in the next 1-3 years is unrealistic). Financial institutions, on the other hand, believe that the market volatility will be sufficiently high. The only scenario that could result in substantial losses for financial institutions is if stock prices slowly decline but don't drop enough to reach the knock-in level, signifying inadequate volatility.

Lastly, as financial derivatives without an option premium, what do financial institutions actually find attractive? Two aspects: a "buy low, sell high" hedging strategy and the valuable put options embedded within the structure. We will see them in later articles.