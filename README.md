# E-commerce Funnel Performance

**English** · [Українська](README.uk.md)

![E-commerce Funnel Performance dashboard](images/dashboard.png)

**Interactive dashboard:** [Tableau Public](https://public.tableau.com/views/EcommerceFunnelPerformance/E-commerceFunnelPerfomance)

An analysis of an online store's conversion funnel: where users drop off, what caused the sharp revenue drop at the end of the period, and which traffic sources perform best.

| | |
|---|---|
| **Tools** | Tableau, SQL (BigQuery) |
| **Data** | Google Analytics 4, Google Merchandise Store (public obfuscated GA4 sample dataset) |
| **Period** | November 2, 2020 – January 25, 2021 (13 weeks) |
| **Dimensions** | week, device, country, traffic source, funnel stage |

## Dashboard features

- **KPIs compared with the previous period:** Sessions, Purchase CVR, Orders, Revenue, AOV.
- **Period selection:** a single week or several consecutive weeks, compared with the previous period of the same length. The period can be set in the filters or by selecting bars on the trend chart.
- **6-stage funnel** (Session Start → Product View → Add to Cart → Begin Checkout → Add Payment Info → Purchase) with cumulative and step conversion.
- **Revenue and Purchase CVR trend** with the selected and previous periods highlighted.
- **Breakdowns** by device (with a metric switch) and by traffic source.
- **Filters:** Device, Source, Country.

## Data notes and limitations

- **Funnel definition.** The funnel counts sessions that went through all defined stages in sequence. The Orders and Purchase CVR KPIs also include purchases made without passing every funnel stage, so the number of purchases at the Purchase stage may differ from Orders.
- **Incomplete tracking at the start of the period.** Event tracking shows anomalies in the first weeks: no Add to Cart events and no purchases were recorded in the week of Nov 2, no Add to Cart in the week of Nov 9, and in the week of Nov 16 Add to Cart was only 8% of product views. Tracking completeness should be considered when comparing weeks; trend analysis is reliable from late November onwards.

---

## 1. Key findings

The funnel analysis revealed three main problems: a sharp revenue drop at the end of the period, large user losses at the early funnel stages, and a gradual decline in product-view-to-cart conversion.

- **Critical revenue drop in the week of Jan 25.** Revenue fell by 80%, from $24.9K to $4.9K, while sessions decreased by only 6%. The main deterioration happened at the Add Payment Info → Purchase step, which may indicate a technical problem with payments or with purchase tracking in GA4.
- **The biggest losses are at the top of the funnel.** Only 21.2% of sessions reached the product view stage. Site navigation, landing page relevance and event tracking should be reviewed.
- **Product View → Add to Cart conversion is declining.** It dropped from 33.7% at the end of November to 17.5% in the week of Jan 25. This may be related to changes in demand, assortment, prices or user experience.
- **Google brings a large share of traffic but underperforms.** It accounts for about a third of sessions, but its share of revenue is lower. The main gap is at the step from product view to add to cart.
- **The Unknown source has the highest conversion.** However, without clear attribution it is impossible to tell which campaigns or channels drive this result.

The top priority is to verify tracking accuracy and the causes of the conversion drop at the end of January, while investigating the early-funnel losses and the decline at the Add to Cart stage in parallel.

---

## 2. Critical drop in the week of January 25, 2021

Comparing the last two weeks shows a sharp deterioration in commercial results with relatively stable traffic.

| Metric | 18-Jan | 25-Jan | Change |
|---|---|---|---|
| Sessions | 27,120 | 25,543 | −6% |
| Purchase CVR | 1.27% | 0.36% | −71% |
| Orders | 362 | 94 | −74% |
| Revenue | $24,947 | $4,936 | −80% |
| AOV | $68.91 | $52.51 | −24% |

The revenue drop far exceeds the decline in traffic. The main problem is therefore not the number of visitors but how well they convert into purchases, combined with a lower average order value.

### Funnel stage analysis

| Stage | Typical range in other weeks | Jan 25 | Assessment |
|---|---|---|---|
| Session Start → Product View | 15–25% | 22.8% | Within the typical range |
| Product View → Add to Cart | 19–34% | 17.5% | Below the typical range |
| Add to Cart → Begin Checkout | 25–41% | 38.1% | Within the typical range |
| Begin Checkout → Add Payment Info | 63–79% | 75.6% | Within the typical range |
| **Add Payment Info → Purchase** | **64–74%** | **24.5%** | **Critical deviation** |

The sharpest deterioration is at the last stage: the share of users who complete a purchase after entering payment information. Add Payment Info → Purchase conversion fell to 24.5%, while in other weeks it was mostly within 64–74%.

The problem appears in every analysed segment:

- **Devices:** Desktop — 23.2% · Mobile — 25.8% · Tablet — 28.6%
- **Sources:** Unknown — 20.5% · Google — 22.5% · Direct — 23.7% · Shop — 27.0% · Other — 28.8%
- **Countries:** Canada — 15.8% · Spain — 25.0% · India — 27.3% · USA — 27.6%

A similar deviation across different segments supports the hypothesis of a system-wide problem rather than a local drop in traffic quality.

### Possible causes

1. **Payment process failure:** payment gateway errors, declined transactions or issues with the order confirmation page.
2. **GA4 tracking issue:** the `purchase` event stopped being recorded or sent correctly after a site update.
3. **Incomplete data:** the last week may cover an incomplete observation period or have delayed events.

To identify the cause, GA4 data, actual orders, payment system logs and recent site releases need to be checked.

---

## 3. Seasonality and demand dynamics

The best results were recorded in the week of Dec 7: revenue reached $57.8K and Purchase CVR was 1.91%.

After that, commercial metrics gradually declined until early January. Revenue fell to $9.2K in the week of Jan 4.

The likely explanation is seasonal demand: the end of Black Friday and Cyber Monday, followed by the end of the main holiday shopping wave. Seasonality is still a hypothesis that should be verified against historical data and demand trends.

### Anomaly in the week of January 4

Sessions grew by 42% compared with the previous week, but Purchase CVR dropped to 0.53%. Only about 15% of visitors reached a product view.

Growing traffic combined with lower engagement and conversion may indicate a change in audience mix, a higher share of non-target visits, or tracking issues.

**Recommendation:** review the sources of the traffic growth, landing pages and user behaviour during this period.

---

## 4. The biggest funnel loss: Session Start → Product View

Over the whole period, only 74,736 of 352,263 sessions reached the product view stage, about 21.2%. Accordingly, about 79% of sessions had no recorded transition to this stage.

This is the largest loss in absolute number of sessions. At the same time, the `view_item` event setup and the funnel logic should be verified.

### Results by segment

| Dimension | Segment | Session → Product View |
|---|---|---|
| **Total** | | **21.2%** |
| Device | Desktop | 21.3% |
| | Mobile | 21.1% |
| | Tablet | 21.0% |
| Source | Unknown | 25.0% |
| | Shop | 22.8% |
| | Direct | 21.0% |
| | Google | 20.8% |
| | Other | 20.7% |
| Country | Canada | 21.7% |
| | India | 21.3% |
| | USA | 21.2% |
| | France | 20.7% |
| | Spain | 20.6% |

The rates barely differ across devices and countries. This supports the assumption that the problem lies in the overall user journey or in event measurement.

### Possible causes

- **Navigation issues:** the home page, categories or search do not lead users to products effectively.
- **Landing pages that miss expectations:** visitors land on pages that do not match their search query or intent.
- **Incomplete event collection:** `view_item` may not fire on all relevant pages or may not be sent to GA4 correctly.
- **Funnel definition:** some users may interact with the store without passing all stages in the defined order.

### Recommended actions

1. Check the `view_item` event setup with DebugView and GTM Preview.
2. Compare landing pages, session counts and clicks through to product pages.
3. Analyse engagement, exits and click-throughs by page type.
4. Review navigation, internal search, and the visibility of products, prices and purchase buttons.

Before optimising the site, it is necessary to confirm that the data reflects user behaviour correctly.

---

## 5. Declining Product View → Add to Cart conversion

At the end of November, product-view-to-cart conversion was 33.7%. By the week of Jan 25 it had fallen to 17.5%, almost by half.

| Week | 30.11 | 07.12 | 14.12 | 21.12 | 28.12 | 04.01 | 11.01 | 18.01 | 25.01 |
|---|---|---|---|---|---|---|---|---|---|
| Product View → Add to Cart | 33.7% | 33.3% | 29.7% | 24.0% | 21.0% | 21.6% | 22.4% | 18.6% | 17.5% |

The main trend is a gradual decline after the December peak. In early January the rate stabilised at 21–22%, but from the week of Jan 18 it fell again.

Even after overall purchase conversion recovered in the week of Jan 18, add-to-cart efficiency stayed below December levels.

### What to check

- changes in prices and assortment;
- availability of popular products and sizes;
- quality of product pages, photos and descriptions;
- visibility and functionality of the Add to Cart button;
- shipping costs and terms;
- technical correctness of the `view_item` and `add_to_cart` events.

**Optimisation target:** keep Product View → Add to Cart at or above 22.0%, the median for the fully tracked period (Nov 23, 2020 – Jan 25, 2021).

---

## 6. Traffic source performance

Sources are compared over the fully tracked period: November 23, 2020 – January 25, 2021.

| Source | Share of sessions | Purchase CVR | AOV | Revenue per Session |
|---|---|---|---|---|
| Google | 36.1% | 1.01% | $70.0 | $0.76 |
| Direct | 23.2% | 1.13% | $70.6 | $0.88 |
| Other | 27.4% | 0.99% | $67.1 | $0.75 |
| Shop | 7.7% | 1.88% | $72.0 | $1.56 |
| Unknown | 5.6% | 3.03% | $60.2 | $2.09 |

### Key observations

**Google: large traffic volume, weaker monetisation.** Purchase CVR is 1.01% and revenue per session is $0.76. For comparison, Shop delivers 1.88% CVR and $1.56 per session, and Unknown delivers 3.03% CVR and $2.09 per session. This does not automatically mean Google traffic is unprofitable: that conclusion requires data on ad spend, margins and customer acquisition cost.

**Shop: a promising source to scale.** High CVR and revenue per session suggest higher commercial value. It is worth exploring its structure and checking whether traffic can grow without a significant drop in quality.

**Unknown: the best metrics, but unclear attribution.** This source has the highest conversion and revenue per session. However, without channel-level detail it is impossible to tell which marketing activities drive this result.

### Where Google loses users

| Stage | Google | Direct | Unknown | Shop | Other |
|---|---|---|---|---|---|
| Session Start → Product View | 19.9% | 20.3% | 25.1% | 22.2% | 19.9% |
| Product View → Add to Cart | 23.3% | 25.3% | 34.2% | 28.4% | 23.9% |
| Add to Cart → Checkout | 33.4% | 35.5% | 44.5% | 39.9% | 33.7% |
| Begin Checkout → Payment | 69.8% | 70.3% | 81.8% | 77.2% | 66.8% |
| Payment → Purchase | 66.5% | 65.2% | 67.5% | 70.0% | 67.2% |

Google's main gap is at the early stages, especially the step from product view to add to cart. At the final stage its conversion is close to other sources.

This may indicate a weaker match between user intent, search queries and the store's assortment. For example, some visitors may be looking for information about the Google brand or its electronics and end up in a merchandise store.

### Recommendations for Google

1. Analyse search queries, campaign types and landing pages.
2. Separate informational queries from queries with commercial intent.
3. Check that ads match the Google Merchandise Store assortment.
4. Send users to relevant product pages with a clear price, description and add-to-cart button.
5. Evaluate performance by revenue per session, ROAS and margin, not just by traffic volume.

---

## 7. Priority action plan

| Priority | Task | Expected outcome |
|---|---|---|
| High | Analyse the week of Jan 25 day by day; reconcile GA4, actual orders, payment provider logs and site releases | Identify the cause of the 80% revenue drop |
| High | Check the `purchase`, `add_payment_info`, `view_item` and `add_to_cart` events | Confirm that the funnel is measured correctly |
| High | Set up daily monitoring of Payment Info → Purchase conversion with anomaly alerts | Detect technical failures faster |
| High | Investigate the decline of Product View → Add to Cart from 33.7% to 17.5% | Identify barriers to adding products to the cart |
| High | Check UTM tags and identify the traffic sources that end up in Unknown | Improve attribution and channel evaluation |
| Medium | Analyse Google search queries and how well landing pages match audience intent | Improve traffic quality and revenue per session |
| Medium | Launch remarketing to users who viewed products but did not add them to the cart | Bring back interested users and increase purchases |

### Monitoring recommendations

To keep the funnel under control, set up daily monitoring of:

- Purchase CVR;
- Revenue and Orders;
- Add Payment Info → Purchase;
- Product View → Add to Cart;
- share of sessions with a `view_item` event;
- share of traffic from an undefined source.

For the critical Payment Info → Purchase step, a preliminary alert threshold of below 50% can be used. The threshold should then be refined based on historical data, daily event volume and statistical variability to avoid too many false alerts.

---

## 8. Summary

The analysis shows that funnel performance is limited by at least three separate factors: user losses at the first stage, declining interest in products, and an abnormal drop in purchase conversion at the end of the period.

First, the technical correctness of the data for the week of Jan 25 must be verified to determine whether the Purchase CVR drop reflects a real payment problem or a tracking issue. In parallel, the low share of sessions with product views and the gradual decline of Add to Cart should be investigated.

On the marketing side, the priorities are improving attribution for Unknown, analysing Google traffic quality and assessing the potential to scale Shop.

**Expected outcome:** more reliable analytics, faster detection of technical issues, higher conversion at key funnel stages and more efficient use of the marketing budget.
