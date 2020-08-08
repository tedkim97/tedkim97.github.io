---
layout: post
title: Price Discrimination in (and with) the Tech Industry
subtitle: A description of a clever trick applied to the tech sector (with examples), and an exploration of how technology makes it "better"
published: true
enable_latex: true
permalink: /price_differential
frontpage: true
technical: false
funstuff: true
---

<!-- * TOC
{:toc} -->

# Introduction
This writeup is about **price discrimination** - the practice of selling the same* product at prices unique to a customer or group (Professionals seem to label it **price segmentation** or **differential pricing**). The innovation of technology makes the economics of it seem unimportant or invisible, but I think it's worth learning some constants in the industry. I'll describe the theory, use in the tech industry, and a peek of how technology and big data is used to augment its efficiency.
- <a href="#examples-of-price-discrimination-in-tech">If you just want to skip to examples</a>
- <a href="#examples-of-price-discrimination-with-tech  ">if you want to skip to price discrimination with tech</a>

\**The word "same" may not be literal, but effectively the same. For example, a super-sized big-mac and a regular big-mac are different, but they are basically the same thing*

##### Why Price Discrimination (Segmentation, Differential, etc) is interesting
Price segmentation is interesting because it's simple in theory, but clever when its implementation. We're accustomed to this strategy because it's ubiquitous, and as a result it's almost invisible to us. However, once someone points out the illusion, you can't help but see it. 

Once you've seen some examples and how it works, you'll see price discrimination all around you. 

# The Theory: Price Discrimination
Since this is supposed to be about ***technology***, I'll try to keep the economics to a minimum. I'll try to include auxiliary information or sources in a different appendix-post if you want more details. If you want to learn more, I recommend reading an introductory econ textbook (Mankiw's "Principle of Economics"). If you want more math and proofs, a more advanced textbook is for you (Varian's "Intermediate Microeconomics: A Modern Approach" is very popular). 

### What is Price Discrimination?
Before we get into "how or why", let's start with the basic vocabulary. **Price discrimination** is "the practice of selling the same good at different prices to different consumers" (Mankiw 837). "**Willingness to pay**" (WTP) is the "maximum amount that a buyer will pay for a good" (Mankiw 838).

In layman's term, willingness to pay (WTP) is the limit to how much you would pay for a product - and people tend to have different WTP for different things. For example, Alice is addicted to sprite so her WTP for a sprite is \\$5. On the other hand, Bob prefers other drinks to sprite so his WTP for a sprite is \\$1.

Due to "supply and demand" dictating a single price - people rarely pay as much as they would for a product. Let's say that a supermarket's price for a sprite is \\$2. As a result, Alice pays \\$3 less than what she was willing to pay, and Bob doesn't buy the sprite! 

Despite the outcome, the supermarket isn't happy with that. Management knows that Alice is willing to pay a lot more for sprite. Moreover, management buys each can of sprite for \\$0.50 from the manufacturer and understands they can sell sprite for \\$1 and still profit from Bobby. The supermarket wants to be able to get every dollar that they can and there exists a way to do it! Price discrimination is the supermarket charging Alice and Bob different prices for soda through direct (or indirect) means. 

In fact, there are classifications for price discrimination that describe to what "degree" that they can identify buyer's and their WTP. These levels of price discrimination are categorized as "first", "second", and "third" degree price discrimination. If you've read "Information Rules: A Strategic Guide to the Network Economy" the authors refer to these as "personalized pricing", "versioning", and "group pricing".

**1) First Degree** (also known as perfect price discrimination):
A company can identify each customer and charge the consumer's WTP. This is almost impossible and largely considered impossible... for now.

> In our example with Alice & Bob, perfect price discrimination is when the supermarket charges \\$5 to Alice, \\$1 to Bob, and the willing-to-pay for any other customer. At market price, the supermarket would have net profit of \\$1.50. Under perfect price discrimination, they're able to make \\$5 in profit (5-0.5 + 1-0.5).

**2) Second Degree** - 
The traditional description of 2nd degree price discrimination involves quantity or quality - giving "bulk-buy" discounts to consumers or having first-class vs economy class airline tickets. Textbooks normally give an example of "bulk-buy" discounts as an example of price discrimination, where the price per good varies with the quantity you buy (i.e buying more is cheaper). Note that the consumer **chooses** which product to pick, and that a high WTP customer may opt to purchase in bulk or the "value-brand". 

In tech, we'll focus on the variations in quality (as this is a very popular choice). Shapiro and Varian describe second degree price discrimination as "offering a product line that lets users choose the version of the product that is appropriate for them."

> **Quantity example:** The supermarket offers individual cans of sprite for \\$4, but offers 24-packs of sprite for \\$24 (\\%1 per can). The net profit from Alice and Bob would be \\$15 (3.50 + 24 x 0.5)

> **Quality Example:** The supermarket offers "Sprite-Premium" that comes in a glass bottle for \\$4.50, but offers a "Sprite-value" that comes in a paper cup for \\$0.75. 

**3) Third Degree** 
A company will set different prices for groupings of their customers (such as senior discounts, student discounts, kids-eat free, grocery-members, etc). 

> If Alice were a student and Bob were an adult, maybe the supermarket has an "adult discount" for Bob, or maybe the supermarket employs membership discounts. Students need to pay \\$3 for a sprite, while adults have the privilege of paying \\$1 for a can.

> Similarly, our supermarket can run a "member-exclusive" deal that allows people who register with their phone numbers to get a special discount on sprite 

### Crux of Price Discrimination: No Arbitrage
Price discrimination cannot work if there is arbitrage. Price discrimination fails if your "low WTP" customer can buy cheaply and sell it to your "high WTP" customers. If Bob could buy his sprite for \\$1 and sell it to Alice for \\$3 - the supermarket would never be able to sell to Alice again. While our supermarket in our example can't prevent arbitration like this, our supermarket would also not implement such drastic price discrimination schemes. 

Popular examples of enforcing this "no arbitrage" restriction is using a ticket based on your ID (i.e you can't board your plane unless you have a ticket that matches your passport), DRM (digital rights management), or geographical locks. This is why only certain people can get coupons, or there is a limit to how many discounted toilet paper rolls you can buy. 

>**Micro Center's approach to "No Arbitrage"**  
>A clever way of preventing arbitrage is to gouge bulk buyers. Microcenter (a computer-equipment store) will offer sizable discounts of certain products (Rasberry Pi's and Processors), but charge more per unit if a buyer tries to buy more than one. For example, Micro Center listed the prices for a "raspberry pi zero w" for each quantity and average cost. One raspberry pi was $5 (special sale), but if you try to buy incrementally more, they charged a higher cost per unit (higher than market price), the more units you bought!  
>The goal is to prevent any individual from buying all the raspberry pi's at a discounted price, and selling them to other people at market price. In addition, it offers a cute incentive to buy a raspberry pi, and the much pricier "accessories" that come along with it.

{% assign caption1 = "An example of Micro Center pricing on Raspberry Pi Zero(s)" %}
{% include caption_image.html imgpath="/figures/microcenter.jpg" alt="fig1" caption=caption1 %}

>There are ways to beat this tactic (like having 15 different people come in and buy 1 raspberry pi), but they're a  waste time and resources. In addition, even though I like this strategy adding a "one discount per customer" rule is equally as offering the discount. 

### Classic Examples of Price Discrimination 
Here are some examples of Price discrimination that you'll find whenever price discrimination is mentioned (textbooks, books, articles, investopedia, etc)- these examples are essentially common knowledge.

>**Elderly & Children Movie Tickets - 3rd degree**  
Very young and old people (normally) don't have a disposable income from a job source, and are sensitive to prices as a result. As a movie theater adding an additional person to a half-filled theater comes at no extra cost. You can offer a special discount to these price-sensitive customers. This is also true for senior discounts in (Japanese) public transportation.

>**Books (hardcover vs paperback) - 2nd degree**  
I have experienced this example personally. When I was 10, I was confused why I needed to pay \\$20 for the new Percy Jackson book and that they were no cheaper, paperback options. One might argue that hardcover is more expensive to the produce (heavier books means more shipping, hardcover books are usually higher quality, etc).  
The reality is that even though cost is higher, they usually aren't "enough" to justify a \\$10 additional markup. The publishing company knows that fans are desperate for the new book, and can charge more as a result.

>**Coupons - 3rd degree price discrimination**
Traditionally, coupons have been distributed broadly so that anyone could use them. If you were picky about prices, you would wait until the coupons were valid to buy your stuff. If you didn't really care, you could be effectively paying a "premium" to buy it whenever. However, some coupon schemes have become targeted. Companies will try to target specific goods at certain customers-groupings depending on their zipcode or online purchasing habits. There's a limit to how well they can target customers, and so it's still 3rd degree. 

>**Airfare tickets (first/business/economics) - 2nd degree**
People might argue that third-class seats are narrow and uncomfortable so that the flight can fit as many individuals in the flight as possible. This reasoning is true to a certain extent, but airlines frequently make the "economy" option uncomfortable to disincentive business or first-class passengers from getting a cheaper flight ticket. The airline doesn't want to lose the larger margins on their high WTP customers, and so they make the discount option as undesirable as possible. 

# Pricing Parable: Pricing is never about cost, but willingness to pay
You might see some of these examples and think "That's not fair. You can't call everything price discrimination, you're not an insider." While this is a fair critique, people fall into this trap when they try to justify a purchase by the "cost of the materials or service". **Pricing isn't about cost, but the market's willingness to pay**. 

I'll give an anecdote from Tim Hartford's "The Undercover Economist" to convince you that higher costs does not mean higher prices. 

Costa Coffee (a British coffee chain) offered "Fair Trade" coffee that came from a fair trade brand called Cafédirect. Fairtrade coffee promises a lot of good things, one of which is offering coffee farmers good prices for their beans. People who asked for fair trade coffee were charged an extra 10p (which is around \\$0.13). This seems fair! Since coffee farmers were being paid more for their coffee, the customer should also pay more for their coffee.

Something was wrong though. If you did the math... something didn't **add** up. Cafédirect paid farmers between 40p to 55p (\\$0.53 - \\$0.72) per pound of coffee. The cost increase to a cup of coffee would be less than a penny for each cup of coffee. There is this unaccounted 90% premium that isn't going to the farmers or Cafédirect. In fact, the \~9p was all going to Costa Coffee's bottom line!

Isn't that a bit scummy? Pocketing the "premium" price for a fair-trade product that was supposed to help others? Well at the end of 2004, the company admitted this practice was "misleading" and made it so that people can request fair trade coffee without an extra charge.

The takeaway is that Costa Coffee didn't increase the price of their "fair trade" option because it was more expensive, they increased the price because they were convinced that consumers were willing to pay that much!

>**So what are the takeaways?**   

We should abandoned pro-social behavior? &#10060;  
We should haggle the barista? &#10060;  
Seize the means of production? &#10067; &#10071;  
**Pricing is not about costs, but willingness to pay?** &#9989;  

# The Practice: Price Segmentation in Tech
Before we get into fun examples of price discrimination, we need to discuss "consumer vs enterprise" market segments. In all the examples we gave, price discrimination have been about people. Businesses differ in price sensitivity compared to people. Companies purchase goods or material to deliver a product or service (silicon, metals, plastics, etc) or
to increase productivity (computers, office equipment, etc). For businesses, buying is for increasing revenue rather than personal consumption. 

Unlike humans, businesses can estimate the effect of a purchase and judge if its necessary. For example buying a new tool will increase revenue by \\$X, increase productivity by Y%, or reduce expenses by Z%. For example, if upgrading your existing IT infrastructure could increase productivity and save time, netting the company an additional \\$5 million in revenue, how much would the company pay? Any reasonable amount less than \\$5 mil!

Understanding this difference is important because when you're in the realm of tech, products can be sold to anyone - hobbyists, start-ups, enterprises, etc. A decent chunk of these examples involves charging people who use the tool more (and therefore are more willing to pay), while charging regular people much less - rather than sorting their customers as "premium vs discount". Although that does happen a lot. 

##### Versioning:
Price discrimination in tech is closely to "quality versioning" (or second degree price discrimination). The idea is that a business will created slightly different models of the same product, adding or removing (usually removing) features and changing the price as a result. The company will choose their features carefully, as they can easily separate their hobbyist vs enterprise market segments on enterprise-critical features. 

> **How is this clever?**  
Versioning the quality of the product (2nd degree price discrimination) is more elegant than personal or group pricing. The other pricing schemes requires effort to collect information on their customers or enforce groupings and restrictions to prevent arbitrage (and is not even enforceable in some situations)! Versioning is clever because by adding or removing features (or adding a few bells and whistles), the **customers will sort themselves**. We'll see examples and details later!

# Examples of Price Discrimination in Tech
### Hardware
There are a lot of examples, I could be missing way too many. I've compiled examples that I thought were interesting to me. Some of these examples may seem dated, as they were taken from "Information Rules: A Strategic Guide to the Network Economy". However, just like the book says "technology changes. Economic laws do not" (pages 8-9).

##### IBM Laser Printers ("Information Rules", 59)
An example that is older than me is the IBM LaserPrinter Series. The discount "LaserPrinter Series E" model was almost identical to the standard "LasterPrinter" model. The Series E printer printed at half the speed of the standard model (5 pages per minute vs 10 pages per minute). If the printers used the same parts, how could they print as such different speeds? 

The "Series E" model had an additional chip that **deliberately slowed down printing speed**. The two printers were **identical, but the slower model had an additional component meant to hinder performance**! This is actually an instance, where the cost of producing and engineering the discount product is **higher** than the standard/premium product! If the "lower quality" model printed pages at a comparable speed, enterprise customers would not have a reason to purchase the professional model. As a result, IBM needed to hinder the "consumer" version so that it wasn't viable for enterprises to cheap out and use it.

##### Lenovo Thinkpad Laptops
An important thing to establish is that Lenovo X1 Carbon laptops tend to be expensive compare to the competition. Lenovo offers good quality laptops that seem to be overpriced in terms of hardware quality to dollar. Other brands (dell, HP, etc) offer similar quality laptops for \\$200-500 less. This is important to understand as Lenovo also launches "special" sales frequently to discount their laptops by \\$200 to \\$500-600. Some of these sales made sense (Black Friday, Christmas, Labor day, etc) but other events were not-real reasons to host a sale (July sale, Spring Sale, graduation sale, etc). 

I suspect that since Lenovo laptops are bought in bulk from enterprises, the company maintains higher prices to get every dollar they can from these companies. In order to sell to regular consumers, Lenovo uses sales as a "coupon" to let them buy these laptops as well.

##### ECC support on Intel and AMD CPUs
One of Intel & AMD's core products is a [CPU](https://en.wikipedia.org/wiki/Central_processing_unit). Essentially, the CPU is the "brains" of the computer is responsible for performing the math, logic, input/output on your computer. Error-correcting code memory (ECC) is a special feature to some memory sticks (RAM) that can correct some kinds of data corruption. These data corruptions are imperceivable to people reading articles or watching TV, but can be catastrophic for servers or scientific computing. 

For some reason, Intel's consumer processors don't offer ECC-support, while their enterprise server chips do. You could argue that ECC support is a new feature, and that is very costly. Adding ECC support would only increase the cost for consumers. In addition, it could not be up to Intel, but the motherboard manufacturers to bake-in ECC support. I suspect this reasoning isn't true because the AMD consumer processors series offers ECC memory support along with their server-grade chips (at similar prices as well)!

{% assign caption_memory = "An example of a ECC memory stick"%}
{% include caption_image.html imgpath="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a2/Micron_PC2700_DDR_ECC_REG.JPG/300px-Micron_PC2700_DDR_ECC_REG.JPG" alt="a nvidia gpu" caption=caption_memory%}

[Some people on enthusiast forums grumble and call this an "Intel Tax"](https://news.ycombinator.com/item?id=10029341), but the reality is that it's just price discrimination. Intel understands that customers who need ECC memory support do high-stakes work. These people tend to be coders, researchers, or data centers, and that means their organization is willing to pay a pretty penny for it to be done properly. 

##### Graphics Processing Units (GPU)
A Graphics Processing Unit (GPU) is a specialized processor used to accelerated calculations used in graphics generation (i.e gaming). In the past 20 years, people have realized that they could use the GPU's parallel structure to accelerate  calculations in other fields (popular examples include machine learning, cryptocurrencies, image processing, etc).

{% assign caption_gpu = "An example of a newer generation of GPU's from Nvidia"%}
{% include caption_image.html imgpath="https://www.nvidia.com/content/dam/en-zz/Solutions/shop/nvidia-consumer-store-portal-geforce-2080-ti-um@2x.png" alt="a nvidia gpu" caption=caption_gpu%}

The [Titan XP](https://www.nvidia.com/en-us/titan/titan-xp/#) and the [GTX 1080TI](https://www.nvidia.com/en-sg/geforce/products/10series/geforce-gtx-1080-ti/) were similar GPUs with drastically different prices. For example, both GPUs  used the same chip (the GP102), but the Titan XP was \\$1200 and the 1080TI was \\$699 (a \\$500 price gap). The other features of the cards were similar. The only difference was the number of "cuda cores" (3840 vs 3584), memory bandwidth (354 bit bus vs 384 bit bus) which resulted in a theoretical difference of 547.7 GB/S vs 484 GB/s. Otherwise, the cards had identical base clocks, and a marginal difference in memory capacity. In fact, various gaming benchmarks showed that the Titan XP and 1080TI had (almost) identical game performance. For reference, upgrading from a lower-tier GPUs offered a much greater jump in performance for less money

|Graphic Card SKU 		| Price 			|
|:--------------------: |:-----------------:|
| TitanXP         		| \\$1200.00   		|
| 1080TI            	| \\$699.00  		|
| 1080				 	| \\$599.00			|
| 1070TI				| \\$449.00			|

**Why is their such a large price discepancy between the Titan XP & 1080TI?** The answer is once again, price discrimination. People who would want a Titan XP (it's higher memory, more cuda cores, higher bandwidth) makes it ideal for advanced computing. A typical video game does not need these specs to play a game "better". As a result, Nvidia can use these features to let their serious consumers separate from the hobbyists - and charge them more for it.

##### Overclocked Processors
"[Overclocking](https://en.wikipedia.org/wiki/Overclocking)" is the process of increasing the frequency (the speed) over the stock, pre-configured speed that your CPU was validated for. Faster is always better (but your chip will also produce more heat)! How much a processor can overclock is determined by the quality of the silicon during the manufacturing process. If you got lucky, then your processor could overclock significantly better (called the "Silicon Lottery") than a "losing chip".

The aptly named [Silicon Lottery.com](https://siliconlottery.com/) was a site dedicated to selling chips that were validated to reach certain overclocked frequencies - allowing customers to pay a premium to avoid the gambling. This arbitrage seemed to indicate that there was decent money in further splitting up your premium processors by their overclock ability. As a result, Intel released the i9-9900KS and the i9-9900K processor. The only difference was that the "KS" model could reach 5GHz on all 8 cores while the "K" model could not. The release price for these chips were \\$560 and \\$499 respectively. However, the price discrepancies quickly increased, and[by 2020 the price of the "K" model was \\$438.99, and the "KS" model was \\$ 1099.99.](https://camelcamelcamel.com/product/B005404P9I?context=search)

The target market for "overclocked" chips is actually not enterprises. The tradeoffs, risks, and additional costs with overclocking every processor is generally not worth it for most companies. Overclocked chips seemed to be more targetted towards the "enthusiast" market.

##### Mining Cards
The Nvidia GTX 1060 and the Nvidia P106-100 seem to be different cards - one is for gaming while the other is for mining cryptocurrencies. The P106 had drivers locked, and their was no physical output to put in a cable. [Hacking the 106 drivers and finding hardware workarounds indicated that they could perform similarly to the GTX 1060!](https://youtu.be/TY4s35uULg4). Nvidia locked down the features of a traditional "gaming" card and marketed it towards miners (who are sensitive to their mining returns). To prevent gamers from buying a cheaper GTX 1060, Nvidia locked down the features that could make this card play video games.

### Software
"Information Rules: A Strategic Guide to the Network Economy" notes that software products face a massive, development cost, but can distribute licenses for (almost) free! Unfortunately, the book was written in an era of proprietary software dominance. The book may seem old with respect to how frequently the software landscape changes: introduction of new development frameworks (Scrum, Agile, Waterfall, etc), new monetization schemes (SAAS), or the introduction of open-source software. Despite the changes, the strategies they describe are still practical and exist today.  

> **Open-source versus proprietary software**    
Software faces a unique problem in that some of its "competitors" can offer a product for free. For instance [Stata](https://en.wikipedia.org/wiki/Stata) used to be a popular statistical software; however, the license fees have turned away new individuals from using the tool and encouraged them to seek alternatives. With the introduction of the open-source [R (the programming language)](https://en.wikipedia.org/wiki/R_(programming_language)), individuals could add features that benefit the community as a whole. Statacorp couldn't keep up with new features or performance improvements and gradually faded away. These aren't the only reasons why Stata fell behind (people still use the software), but Stata is no longer a popular statistical programming software it once was. 

> **Open-source with proprietary software**
On a more positive note, instead of competing against open-source software, companies have begun to adopt it. By incorporating open-source into their own products, companies can reduce costs and errors of their software while focusing on improving it for their customers. In fact, a majority of the big tech companies make huge contributions to the open-source community. Even Microsoft (I hope you get the irony) has made multiple contributions to open source and acquired github.

> **Proprietary Software Still Strives**  
Proprietary software and services will strive - and that's natural. Bloomberg terminals (software that provides financial data) can't be out-competed with open-source (as easily). Collecting, transcribing, and recording financial information cannot be done better (yet) - it's purely a function of resources invested into the collection of information and infrastructure.

##### Adobe's Pricing

Let's examine Adobe pricing strategy through their "All Apps" package for adobe subscriptions. The table was transcribed from their [plans page](https://www.adobe.com/creativecloud/plans.html).

|Plan target          	| Price (per month) |
|:--------------------: |:-----------------:|
| Individuals         	| \\$52.99   		|
| Business            	| \\$79.99   		|
| Students & Teachers 	| \\$19.99 (first year) - \\$29.99 (afterwards) |
| Schools & Universities| \\$34.99			|

All of these subscription plans offer the same thing (the adobe suite) with minimal differences between them (the "School & University" option offers 24/7 tech support). Everyone downloads the same software (with all the features) and uses their license key to access their tools. This seems like a pretty clear example of Group Pricing- Adobe is offering the same product at different prices depending on the group. Arbitrage is relatively infeasible for Adobe, as the software is validated through licenses (there aren't millions of university or teacher email addresses to create discount keys for Adobe) - and you can't 

It is important to note that pricing adobe suite for students is more than just "maximizing profits". By making Adobe accessible to students, Adobe is trying to "lock-in" young users into their platform. Since learning adobe proficiently takes a lot of effort, students will be reluctant to switch out for alternatives in the future.  

##### Operating Systems (Windows, Redhat)

###### Windows 10
Microsoft sells it's non-server operating systems in three tiers: Windows 10 Home, Pro, and "Pro for workstations". In fact, not only does Microsoft price discriminate by offering different versions of their operating system, but also through differential pricing in different geographic locations!

###### Differentiating Windows Pricing by Versioning

Below is a basic table of the operating system versions and their "features". [The data was taken from the Microsoft Store](https://www.microsoft.com/en-us/store/b/windows?activetab=tab%3ashopwindows10). I'm not going to list all of the differences, but it's important to know that the most expensive tier has "all" of the features.

|OS Version          			 | Price 	 | Features                     |
|:------------------------------:|:---------:| :---------:                  | 
| Windows 10 Home         		 | \\$139.00 | The base operating system    |
| Windows 10 Pro    	 		 | \\$199.00 | All of Home Features + More* |
| Windows 10 Pro For Workstations| \\$309.00 | All of Pro Features + More*  |

Essentially their versions are divided by Home users, Businesses, and Businesses that need more compute. Windows 10 Home is a good OS, and the users probably only browse the internet, watching TV, or playing video games. These users don't need advanced features like Hyper-V virtualization, Bitlocker device encryption, or remote desktop (although these would be nice to have!). People who run companies and could be managing hundreds of machines would be at risk if they used Windows 10 Home as their licenses and must use the Pro version. As a result, the business would choose the "Pro" version. Most businesses will not need hardware and file system optimizations that the "Pro for Workstation" license offers. The page states that this OS is targeted for "researchers, engineers, video editors, graphics artist, teams that work with big data .." i.e where time and output can make a huge difference on the bottom line. 

Did it cost Microsoft a ton of money to add these features to their operating system? Maybe. It's more likely that they started with the highest quality version of the operating system, and stripped away features to create the discrepancy. 

###### Differential Windows Pricing by Location

Microsoft **also** segments their Windows 10 licenses through **geography**. To be fair, the price of Windows 10 can differ across borders because of international tax differences (i.e Windows 10 is more expensive because of the government). Other times, Microsoft offers cheaper Windows 10 license keys where the retail price is disproportionate to the cost of living. Residents in some of these countries don't have enough money (or enough willingness) to pay the full American retail price. As a disclaimer, I cannot find any primary evidence of this - I've only heard of this through "word of mouth". There are suspicious services that sell discounted Windows 10 keys (illegally). They acquire these keys through several means (stolen credit cards, volume licenses, education versions, etc), but they also buy keys from countries that are offered windows 10 at a much cheaper price. 

###### Redhat Linux
Price Differentials can still exist with open source software! For example, Red Hat (now an IBM subsidiary) offers
a free version (a "no-cost developer subscription") of their "Red Hat Enterprise Linux" Distro in addition to having their complete source code online. In this situation, the differences between the free version and the paid version is: 
- **Legality:** You're only allowed to use the free version for personal use or development purposes - NOT for running a company. Although, there doesn't seem to be any binary that tracks your usage)
- **Support:** Red Hat will offer longer life-cycle support on the operating system (rather than abandoning a version after 2-3 years). In addition, they'll experts that you can consult, professional training materials, etc. 

##### Database Services (MongoDB)
MongoDB offers many different services and products (so much that I can't keep track of everything). For now, let's focus on their main database software. MongoDB segments their prices based on versioning.

MongoDB offers both a "Community" and "Enterprise" version of their database software - that are almost identical feature. The core database system and its features are the same among both features (same encryption, memory & storage limits, scalability, etc). The "enterprise" software has add-ons and extra services that greatly increase the quality of life for enterprises. [The full list is here](https://www.mongodb.com/products/mongodb-enterprise-advanced), but some examples include monitoring software, 24/7 support team, visualization tools, security tools, training, etc. 

##### Text Editors (Sublime Text)  
Sublime text doesn't suffer from versioning we might see in other software and offers the full-featured version for free! The only downside being that an occasional pop-up appears that suggests you purchase a license (as a long-time user I think it's not that annoying). Paying a full-license only removes the pop-up! 

Don't mistake sublime's pricing as generosity. sublime isn't free because the developers are generous people (they could be). Sublime needs to compete with other free text editors that have plenty of community support and plugins - keeping Sublime free is a matter of survival. 

# An Important Aside:

>**Counterpoint: Versioning your product creates new products which breaks the definition**  

This is true. The definition of "price discrimination" is selling the same product and different prices - NOT different products at different prices. However, a lot of businesses start from one product, and chop it up into multiple different ones to target different markets. Moreover, we can treat differences as variations on "quality" because these versions never have trade-offs in features, the premium version is strictly better than all other versions. Finally, Varian (who is also the "chief economist" at Google) classifies versioning as a price discrimination.

>**Is everything price discrimination? (no)**  

Not everything is price discrimination! Sometimes, the materials and costs of items genuinely warrant a higher price
They're are genuine reasons for a business to raise or lower prices. For example, higher than expected demand can raise prices. Lower than expected demand can also lower costs (the cost of holding inventory may be too high, which will cause the company to try to get rid of it ASAP).  

Pricing has a strategic component beyond taking as much money as they can. We saw some of this strategy in some of our software examples. You might point to sublime having a fully-featured free version as an example of price discrimination! This might be partially true - there are strategic component to pricing. Offering a competent free-tier can be thought of as a way of acquiring users and developers in to create a network effect for your product. In addition, the more people that use your software, the more "stuck" they become in the ecosystem (also known as "Lock In"). These practices are very popular in other software as well (like the Microsoft suite, Adobe products, etc).   

For the sublime example, you need to realize that the company competes with other great, free, open-source options. SublimeText might be free because the developers are generous (they are awesome people), but it's also because they are competing with open-source editors such as VIM, EMACS, Notepad++ (VSCode, atom, etc).

# Pros of Price Segmentation & Welfare Economics
My first impression is that price discrimination is a greedy move by companies to suck out each dollar from its customers, and it's true. Despite my cynical take, price discrimination has its benefits as well - mainly that it's economically "efficient" and that it increases the accessibility of a good for people.

In the earlier single pricing scheme example, there are certain market situations where a business could sell a Sprite to a customer (Bob) above the product's marginal cost, but below the market price. As a result, the business couldn't sell Bob a sprite and lose \\$0.50 profit. Economists consider this a "deadweight loss" - which is considered an inefficiency in the market. 

With price discrimination, the business is able to make the sale to both Alice and Bob - removing the inefficiency. One downside is that the Alice doesn't get a great deal on her sprite anymore. Her \\$3 of "consumer surplus" - the difference between the price she paid and her willingness to pay - has become the supermarket's net profit. On the other hand, Bob is able to purchase a sprite for himself at his \\$1 price. 

**It's important to note that only perfect price discrimination offers the "ultimate" efficiency.** Second and third degree price discrimination **reduce** inefficiency, but can't eliminate it. 

This may seem trivial with soda, but this matters for essential goods like medicine. You might dislike the fact that Alice paid \\$5 for her sprite while Bob paid \\$1. However, another example is that Alice pays \\$100 for a life-saving drug, and Bob in a poorer-country pays \\$10 in his country. If Bob could not afford his medicine at \\$100 and the medicine were cheap to produce, wouldn't the pharmaceutical company be ethically obligated to find some way to save Bob's life? **The pricing of medicine is situation is more nuanced than this, so take this example with a grain of salt**. 

**In theory**, price discrimination results in the win for the world! Whether an individual benefits or loses from this segmentation depends on where you are on the demand curve. 

**Under No Price Discrimination:**
- If ${WTP} \geq {Market Price}$, you benefit from no price discrimination.
- If ${WTP} < {Market Price}$, you are not able to buy the product.

**With Price Discrimination:**  
This will depend on the degree of price discrimination, but the people who had $WTP < {Market Price}$ can now buy the product (if their willingness to pay is above the marginal cost). If price discriminate were illegal, people would have to pay the "supply and demand" cost and pay. Now, less wealthy individuals cannot afford the newest hardware or software, while giant corporations will be happy that equipment costs have gone down!

# Examples of Price Discrimination With Tech  
With the focus on tech companies abuses in privacy, democracy, and competition in the industry, people do not complain about "tech companies are abusing their power to charge me 2 dollars more for gloves". Despite this, technology still plays an increasingly larger role in pricing today. 

A personal anecdote I had with Lyft is below, but feel free to ignore it.

>**A Personal Vendetta: Lyft**  
I used to use Lyft quite a fair bit, I didn't bother using Uber because (I remembered that) the ride-share prices were cheaper on Lyft. One summer, my friend and I were trying to get a ride from the annual Chicago jazz festival to a nearby restaurant. His 2-person lyft ride was \\$8, while my ride was a whopping \\$10! After confirming that we had all the same coupons/deals - there was still a two dollar in our rides. I swore to never use their services again (and I have not since then). 

> Despite feeling betrayed, I was impressed at how clever these rideshare companies have been. My friend was a religious Uber user and rarely used Lyft because I suggested it. I speculated that Lyft was giving an increased discount to my friend to get them to switch from Uber to Lyft. 

> You might think this is a bit "crazy", but there is evidence that Uber [has collected "more than necessary" information using it's "God View" tool](https://www.wired.com/insights/2015/01/uber-privacy-woes-cautionary-tale/). In fact, the [Uber got into trouble with Apple by including a snippet of code that tagged user's iphones after deleting the app (to prevent fraud)](https://www.nytimes.com/2017/04/23/technology/travis-kalanick-pushes-uber-and-himself-to-the-precipice.html?_r=1). If you think this is only an "Uber" thing, [Lyft has had it's own privacy issues as well](https://www.theinformation.com/articles/lyft-moves-to-restrict-employee-access-to-customer-data). If you don't trust this source, the [BBC](https://www.bbc.com/news/technology-42827636) and [techcrunch](https://techcrunch.com/2018/01/25/lyft-god-view/) have seem to report it as well. I'm not sure if these practices continue given efforts to increase security and privacy requirement on mobile OS's, but I'm sure there will always be workarounds. 

### "The internet is a medium for personalized pricing"** 
The above quote is (once again) from "Information Rules: A Strategic Guide to the Network Economy" page 42. Even before "Big Data" became popular, people understood that technology had the potential to augment price segmentation. Even in 2003, [people worried about the movement to erode privacy]((https://www.economist.com/finance-and-economics/2003/10/16/theyre-watching-you)) for the sake of price discrimination. The full paper states that "powerful movement to reduce privacy that is coming from the private sector is motivated by the incentives to price discriminate" (Andrew Odlyzko. 2003. Privacy, economics, and price discrimination on the Internet. In Proceedings of the 5th international conference on Electronic commerce (ICEC ’03). Association for Computing Machinery, New York, NY, USA, 355–366. DOI:https://doi.org/10.1145/948005.948051)

### Other Uses of Technology in Price Discrimination

##### DRM (Digital Rights Management)
This section is more about pricing rather than enforcement, but DRM is implemented to prevent arbitrage. This is essential in software, where people can literally copy-paste your program. There are countless ways that DRM has been proposed or implemented - this is [one of my favorite ways](https://www.youtube.com/watch?v=ccneE_gkSAs).

##### Airline Industry & Locations (Cookies)
The airline industry sure does come appear in these examples a lot &#128064;&#128064;... 

Anyways, ticket booking websites use the cookies stored in your browser or your account information to "dynamically price" your plane ticket to you. They can do it the silliest of ways, as in [Orbitz showing Mac users pricier hotel rooms](https://www.wsj.com/articles/SB10001424052702304458604577488822667325882) or more devilish ways like subtly increasing the flight ticket price each time you revisit the booking site. 

[An exploit](https://youtu.be/Utsnt6GFrKo) to beat this dynamic pricing model is to constantly delete the cookies in your brwoser, and use a VPN (virtual private network) to fake your location in a different country. LTT were able to get book a flight from Denver to London for \~\\$2800 less. I can't verify how well this exploit works because companies dislike location spoofing. Advertisers claim that VPNs can play region-locked content, but fail to mention that streaming services will actively sniff out VPN addresses and blacklist them from region-locked content. 

##### Big Data & PERFECT PRICE DISCRIMINATION
As an oversimplification "Big Data" systems allows organizations to stitch together information about you from different platforms using some identifier or key (like your email address or username). When you sign-up for a loyalty or reward program, the company can use your email address to find additional information about you from other sources to send targeted ads.

Despite my ominous for shadowing, there isn't really any perfect price discrimination. [A Febuary 2015 report from the whitehouse shows that there was little evidence for personalized pricing at the time](https://obamawhitehouse.archives.gov/sites/default/files/whitehouse_files/docs/Big_Data_Report_Nonembargo_v2.pdf?utm_source=Bruegel+Updates&utm_campaign=656e7da39b-Blogs+review+11%2F02%2F2017&utm_medium=email&utm_term=0_eb026b984a-656e7da39b-278510293). The report does outline other suspicious activities companies will do try to make more money (showing different demographics different prices, or targeting ads to users). 

Just because price discrimination seems infeasible, doesn't mean researchers aren't trying to increase its viability. [This article contains a nice summary of research in the field of perfect price discrimination](https://www.bruegel.org/2017/02/big-data-and-first-degree-price-discrimination/)

##### ECommerce & Shopping Behavior [Ebates (Rakuten) and Honey (Paypal)]
Ebates (parent company: Rakuten) and Honeys (parent company: Paypal) are tools that help you get discount items you buy online through rebates and coupons respectively. They normally use app extensions to help look for and apply discounts, as well as collecting some of your activity while shopping. I **suspect** (i.e not confirmed) that these companies are monitoring your shopping behavior in order to profile shopping behavior and price sensitivity. Although you can verify the information [Rakuten collects and Why](https://www.rakuten.com/help/article/what-data-rakuten-collects-and-why-360041231553). 

### A Tangent: Big Data and Health care
A common misconception is that the different quotes you get from insurance is price discrimination - it's not. The quotes you get from insurance are based on how likely you are to file a claim. Given what the insurer knows about you and your habits (age, number of serious car crashes, smoking habits, etc) they get an idea of what it costs to insure you and proportionally charges you for that risk. However, insurance companies look for more factors that can help determine that risk such as credit score, employment, etc. [One examples is car insurers using your email domain as a factor](https://uk.finance.yahoo.com/news/email-address-can-see-pay-car-insurance-103744431.html?guce_referrer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS8&guce_referrer_sig=AQAAAMSjLAf5e_jmjJz0MRG03MUyyuN670BcpmWwxMhfWe5furQa29lrIMHUADo3DoDXJoBExy_QGfSLnv97cZg2LHf01f9o9OXuAFPdRHgah0eDjpzuJB5iQgsZb9qAF6qT304vLmzha_GAug4GDM9vCYIReO4Hw_H47c7-NkIPDddl&_guc_consent_skip=1596872406). Overall, these factors are incredibly flawed and problematic, if you want to understand why this might be an issue, read chapter 8 and 9 of "Weapons of Math Destruction".

A concern issue that people consider is that companies can uncover medical conditions that are prohibited by the government, or discover a health issues that the consumer is unaware - [a common anecdote is how Target's marketing team discovering a girl was pregnant](https://www.nytimes.com/2012/02/19/magazine/shopping-habits.html). I personally dislike this Target anecdote because I don't know how accurate this marketing system works. It might have sent a million pregnacy-related ads to people, and it happened to be right once. This anecdote is still valid because using unclear, unverifiable classification systems shouldn't be used to determine such important things (and what Cathey O'Neil argues as well). Read chapter 9 of "Weapons of Math Destruction" if you want details on Big Data and insurance.

<!-- ### Convincing Consumers to buy: targetting and behavioral nudging 

##### Sweatcoin
I got an advertisement for "Sweatcoin" an app/virtual currency that rewarded you for walking (this was when cryptocurrencies were wildly popular). I suspected that they were trying to aggregate walking pattern of their users to sell to some data vendor. The website promises that they do not sell their data and they keep their information private - the company's main form of monetization was showing advertisements to the users while they have the app open/walking. However, this is a classic example of targeting users to encourage them to buy the advertised goods. -->

# Cons of Price Discrimination (and getting a little political)
Traditionally, price discrimination is described as having no major "cons". Sure the consumers lose a bit, but the business will capture it as profit ([AND SURELY THOSE PROFITS WILL TRICKLE DOWN RIGHT?!](https://en.wikipedia.org/wiki/Trickle-down_economics#Criticisms)) and the economy becomes more efficient. 

Has this idea of charging your customers their maximum willingness to pay gone too far? I think the free market can solve a lot of problems, but aren't there some situations where someone needs to step in?

### Insulin
Even though segmentation sounds nice, there are still egregious cases where this segmentation and desire to maximize profits have gone too far. A prominent example is Insulin in the US. In 2016, the average out of pocket monthly diabetes cost was [\\$360 in the US, \\$112 in India, \\$70 in Japan, \\$65 in the UK, and \\$19 in Italy.](https://www.bbc.com/news/world-us-canada-47491964#:~:text=Now%2C%20retail%20prices%20in%20the,have%20been%20making%20national%20headlines.) If you want a more indepth description of why insulin is so expensive, watch these: [businessinsider](https://youtu.be/7Ycd8zEdoVk), [verge](https://youtu.be/9CdydQNfAXE).

##### Common "devil's advocate" responses:
**Companies need to be rewarded with high profits for how high-risk developing medicine**
> This is true for a lot of medicines, and I think that medicine-patent is okay for encouraging live-saving medicine research. This isn't the case with Insulin. It's almost a 100 years old. There are "innovations" in the production and quality of the medicine, but there does not seem to be such jarring differences in the quality of it to justify this egregious case.

**"It's not an economic problem, but a political one"**
> I agree that "politics" interferes with a "better" market outcome in the US, but it's clearly someone's fault when the [players (pharmaceutical companies) can actively lobby politicians](https://www.cnn.com/2019/01/23/health/phrma-lobbying-costs-bn/index.html) 

**"Insulin prices (and other healthcare costs) aren't high because of price discrimination"**
> None of the links or articles I have shown you have explicitly talked about price discrimination (and it's not). I would argue that the idea is the same. A company sells Insulins to people at a higher price wherever they can. While this is more "efficient" and makes sense with printers, it's cruel to do this for life-saving medicine. 

# The Future & Conclusion
If you have a good product you can sell it at a single price and your company will do fine. If you want ever dollar you can squeeze out of your customers, than you can version your product such that your customers choose the appropriate version. Given how I dislike pricing strategies today, I am not looking towards the future of price segmentation. I already believe that current e-commerce pricing schemes are too invasive, and will only become more invasive without regulation. In addition, group pricing feels unfair due to its arbitrary nature (as a consumer, it feels like paying a "premium" for being in a specific group). If you couldn't tell, my favorite alternative is "versioning". Overall it feels more "fair" because the company has to be smart about setting up their product line, and this strategy allows the consumer choose for themselves. 

# Readings & Sources

### Fun & Informative  
**Underground Economist**: A lot of these examples are about Britain, so the examples were a bit confusing. It's still an engaging exploration of economics with minimal jargon.  
**Weapons of Math Destruction**: Not about economics, but the flaws in big data. There's nothing "fun" (it's actually sort of depressing) but a very engaging book. 

### Dry & Informative  
**Principles of Economics**: This is a classic econ-101 beginner textbook, but it has all the fundamentals. I would never recommend reading the whole thing, but treating it as a resource if you need to consult about basic concepts.

**Information Rules: A Strategic Guide to the Network Economy**: An interesting formalization and guide to the economics of business in the digital age. The book may "seem old" in an industry that is defined by rapid change its age reflects in its examples), but as the authors state "technology changes. 

**The Strategy and Tactics of Pricing**. A very different book about pricing that is more general. 


# Corrections, Complaints, Questions?
Did I get something wrong, misinterpret something, or do you have a question? I'll gladly chat - email me at nablags97[at]gmail.com.