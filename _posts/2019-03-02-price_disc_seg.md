---
layout: post
title: Price Discrimination in (and with) the Tech Industry
subtitle: An exploration of a clever trick applied to the tech sector with a compilation of examples, and a driver to a dystopian future (and trying to get value out of my degree)
published: true
enable_latex: true
permalink: /price_differential
frontpage: false
technical: false
funstuff: true
---

<!-- * TOC
{:toc} -->
<a href="#corrections-or-complaints">testlink</a>

# Introduction
This writeup is about **price discrimination** - the practice of selling the same* product at prices unique to a customer or group (Professionals seem to label it **price segmentation** or **differential pricing**). 

People think that the constant evolution and innovation in technology means that economics can't possibly be used to describe it (spoiler alert: it still does).

Working in Tech makes the economics of it seem unimportant or invisible, but I think it's worth getting a ground-up understanding of a very common strategy. 

I'll try to be inclusive so that non-techies can still understand the examples

I'll describe the theory, use in the tech industry, and a peek of how technology and big data is used to augment its efficiency. 

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

However, the supermarket isn't happy with that. Management knows that Alice is willing to pay a lot more for sprite. Moreover, management buys each can of sprite for \\$0.50 from the manufacturer and understands they can sell sprite for \\$1 and still profit from Bobby. The supermarket wants to be able to get every dollar that they can and there exists a way to do it! Price discrimination is the supermarket charging Alice and Bob different prices for soda through direct (or indirect) means. 

In fact, there are classifications for price discrimination that describe to what "degree" that they can identify buyer's and their WTP. These levels of price discrimination are categorized as "first", "second", and "third" degree price discrimination. If you've read "Information Rules: A Strategic Guide to the Network Economy" the authors refer to these as "personalized pricing", "versioning", and "group pricing".

**1) First Degree** (also known as perfect price discrimination):
A company can identify each customer and charge the consumer's WTP. 

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
Here are some examples of Price discrimination that you'll find in any textbook or investopedia!

>**Elderly & Children Movie Tickets - 3rd degree**  
Very young and old people (normally) don't have a disposable income from a job source, and are sensitive to prices as a result. As a movie theater adding an additional person to a half-filled theater comes at no extra cost. You can offer a special discount to these price-sensitive customers. This is also true for senior discounts in (Japanese) public transportation.

>**Books (hardcover vs paperback) - 2nd degree**  
I have experienced this example personally. When I was 10, I was confused why I needed to pay \\$20 for the new Percy Jackson book and that they were no cheaper, paperback options. One might argue that hardcover is more expensive to the produce (heavier books means more shipping, hardcover books are usually higher quality, etc).  
The reality is that even though cost is higher, they usually aren't "enough" to justify a \\$10 additional markup. The publishing company knows that fans are desperate for the new book, and can charge more as a result. The "Arbitrage" in this situation is borrowing my friend's copy after he was done reading it.

>**Coupons - 3rd (or 1st) degree price discrimination**
Traditionally, coupons have been distributed broadly so that anyone could use them. If you were picky about prices, you would wait until the coupons were valid to buy your stuff. If you didn't really care, you could be effectively paying a "premium" to buy it whenever. However, some coupon schemes have become targeted. Companies will try to target specific goods at certain customers depending on their zipcode or online purchasing habits, but we'll talk about that more later. 

>**Airfare tickets (first/business/economics) - 2nd degree**
First class is expensive, and economy class is cheap(er). Customers don't need to be forced into one or the other, but they choose to. 


# Pricing Parable: Pricing is never about cost, but willingness to pay
You might see some of these examples and think "That's not fair. You can't call everything price discrimination, you're not an insider." While this is a fair critique, people fall into this trap when they try to justify a purchase by the "cost of the materials or service". **Pricing isn't about cost, but the market's willingness to pay**. 

I'll give an anecdote from Tim Hartford's "The Undercover Economist" to convince you that higher costs does not mean higher prices. 

Costa Coffee (a British coffee chain) offered "Fair Trade" coffee that came from a fair trade brand called Cafédirect. Fairtrade coffee promises a lot of good things, one of which is offering coffee farmers good prices for their beans. People who asked for fair trade coffee were charged an extra 10p (which is around \\$0.13). This seems fair! Since coffee farmers were being paid more for their coffee, the customer should also pay more for their coffee.

However, if you did the math something didn't add up. Cafédirect paid farmers between 40p to 55p (\\$0.53 - \\$0.72) per pound of coffee. The cost increase to a cup of coffee would be less than a penny for each cup of coffee. There is this unaccounted 90% premium that isn't going to the farmers or Cafédirect. In fact, the \~9p was all going to Costa Coffee's bottom line!

Isn't that a bit scummy? Pocketing the "premium" price for a fair-trade product that was supposed to help others? Well at the end of 2004, the company admitted this practice was "misleading" and made it so that people can request fair trade coffee without an extra charge.

The takeaway is that Costa Coffee didn't increase the price of their "fair trade" option because it was more expensive, they increased the price because they were convinced that consumers were willing to pay that much!

**So what are the takeaways?**

>We should abandoned pro-social behavior? &#10060;  
We should haggle the barista? &#10060;  
Seize the means of production? &#10067; &#10071;  
**Pricing is not about costs, but willingness to pay?** &#9989;  

# The Practice: Price Segmentation in Tech
Before we get into fun examples of price discrimination, we need to discuss "consumer vs enterprise" market segments. In all the examples we gave, price discrimination have been about people. Naturally businesses are not sensitive to prices like consumers. Enterprises purchase goods to increase productivity for their workers (like chairs, printers, computers, etc) which increases their revenue as well. People purchase goods to "consume" and live with. 

Unlike humans, businesses have metrics to determine whether a purchase will increase revenue by \\$X or increase workforce happiness by Y%. For example, if upgrading your existing IT infrastructure could increase productivity and save time, netting the company an additional \\$1 million in revenue, how much would the company pay? Any reasonable amount less than \\$1 mil!

Understanding this difference is important because when you're in the realm of tech, products can be sold to anyone - hobbyists, start-ups, enterprises, etc. In the examples I'll describe, price discrimination is used more to charge enterprises more money, while charging regular people much less- rather than sorting their customers as "premium vs discount". Although that does happen a lot. 

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

I suspect that since Lenovo laptops are bought in bulk from enterprises, the company maintains higher prices to get every dollar they can from these companies. However, in order to sell to regular consumers, Lenovo uses sales as a "coupon" to let them buy these laptops as well.

##### ECC support on Intel and AMD CPUs
One of Intel & AMD's core products is a [CPU](https://en.wikipedia.org/wiki/Central_processing_unit). Essentially, the CPU is the "brains" of the computer is responsible for performing the math, logic, input/output on your computer. Error-correcting code memory (ECC) is a special feature to some memory sticks (RAM) that can correct some kinds of data corruption. These data corruptions are imperceivable to people reading articles or watching TV, but can be catastrophic for servers or scientific computing. 

For some reason, Intel's consumer processors don't offer ECC-support, while their enterprise server chips do. You could argue that ECC support is a new feature, and that is very costly. Adding ECC support would only increase the cost for consumers. In addition, it could not be up to Intel, but the motherboard manufacturers to bake-in ECC support. However, I suspect this reasoning isn't true because the AMD consumer processors series offers ECC memory support along with their server-grade chips (at similar prices as well)! 

[Some people on enthusiast forums grumble and call this an "Intel Tax"](https://news.ycombinator.com/item?id=10029341), but the reality is that it's just price discrimination. Intel understands that customers who need ECC memory support do high-stakes work. These people tend to be coders, researchers, or data centers, and that means their organization is willing to pay a pretty penny for it to be done properly. 

##### Graphics Processing Units (GPU)
A Graphics Processing Unit (GPU) is a specialized processor used to accelerated calculations used in graphics generation (i.e gaming). In the past 20 years, people have realized that they could use the GPU's parallel structure to accelerate  calculations in other fields (popular examples include machine learning, cryptocurrencies, image processing, etc).

The [Titan XP](https://www.nvidia.com/en-us/titan/titan-xp/#) and the [GTX 1080TI](https://www.nvidia.com/en-sg/geforce/products/10series/geforce-gtx-1080-ti/) were similar GPUs with drastically different prices. For example, both GPUs  used the same chip (the GP102), but the Titan XP was \\$1200 and the 1080TI was \\$699 (a \\$500 price gap). The other features of the cards were similar. The only difference was the number of "cuda cores" (3840 vs 3584), memory bandwidth (354 bit bus vs 384 bit bus) which resulted in a theoretical difference of 547.7 GB/S vs 484 GB/s. Otherwise, the cards had identical base clocks, and a marginal difference in memory capacity. In fact, various gaming benchmarks showed that the Titan XP and 1080TI had (almost) identical game performance. 

For reference, upgrading from a lower-tier GPUs offered a much greater jump in performance for less money(\\$599 for the 1080, \\$449 for the 1070TI). **Why is their such a large price discepancy between the Titan XP & 1080TI?** The answer is once again, price discrimination. People who would want a Titan XP (it's higher memory, more cuda cores, higher bandwidth) makes it ideal for advanced computing. A typical video game does not need these specs to play a game "better". As a result, Nvidia can use these features to let their serious consumers separate from the hobbyists - and charge them more for it.

##### Overclocked Processors
"[Overclocking](https://en.wikipedia.org/wiki/Overclocking)" is the process of increasing the frequency (the speed) over the stock, pre-configured speed that your CPU was validated for. Faster is always better (but your chip will also produce more heat)! How much a processor can overclock is determined by the quality of the silicon during the manufacturing process. If you got lucky, then your processor could overclock significantly better (called the "Silicon Lottery") than a "losing chip".

The aptly named [Silicon Lottery.com](https://siliconlottery.com/) was a site dedicated to selling chips that were validated to reach certain overclocked frequencies - allowing customers to pay a premium to avoid the gambling. This arbitrage seemed to indicate that there was decent money in further splitting up your premium processors by their overclock ability. As a result, Intel released the i9-9900KS and the i9-9900K processor. The only difference was that the "KS" model could reach 5GHz on all 8 cores while the "K" model could not. The release price for these chips were \\$560 and \\$499 respectively. However, the price discrepancies quickly increased, and[by 2020 the price of the "K" model was \\$438.99, and the "KS" model was \\$ 1099.99.](https://camelcamelcamel.com/product/B005404P9I?context=search)

##### Mining Cards
The Nvidia GTX 1060 and the Nvidia P106-100 seem to be different cards - one is for gaming while the other is for mining cryptocurrencies. The P106 had drivers locked, and their was no physical output to put in a cable. [However, hacking the 106 drivers and finding hardware workarounds indicated that they could perform similarly to the GTX 1060!](https://youtu.be/TY4s35uULg4). Nvidia locked down the features of a traditional "gaming" card and marketed it towards miners (who are sensitive to their mining returns). To prevent gamers from buying a cheaper GTX 1060, Nvidia locked down the features that could make this card play video games.

# Software
**What makes software unique** 
An interesting quirk of software products is that coding is a massive cost, but selling additional licenses (ignoring acquisition costs) is basically free. 

Another detail that's complicating the software landscape is that open-source has become popular. For instance [Stata](https://en.wikipedia.org/wiki/Stata) used to be decently popular statistical software. However, the license fees have turned away new individuals from using the tool. Moreover, Statacorp couldn't add new features or performance improvements unlike a whole international community in R, where people could contribute improvements. As a result, Stata has died as a popular statistical programming language (as it should). Of course, I'm not capturing every reason why Stata is not as popular, and people still use it - but hopefully the audience gets the idea. 

### Adobe's Pricing
Let's examine Adobe pricing strategy through their "All Apps" package for adobe subscriptions. The table was transcribed from their [plans page](https://www.adobe.com/creativecloud/plans.html).

|Plan target          	| Price (per month) |
|:--------------------: |:-----------------:|
| Individuals         	| \\$52.99   		|
| Business            	| \\$79.99   		|
| Students & Teachers 	| \\$29.99 or \\$19.99 (first year)|
| Schools & Universities| \\$34.99			|

All of these packages offer the same thing (the adobe suite) with minimal differences between them (from my understanding the "School & University" option offers tech support). Everyone downloads the same software (with all the features) and uses their license key to access their tools. This seems like a pretty clear example of Group Pricing- Adobe is offering the same product at different prices depending on the group. Arbitrage is relatively infeasible for Adobe, as the software is validated through licenses (there aren't millions of university or teacher email addresses to create discount keys for Adobe) - and you can't 

It is important to note that pricing adobe suite for students is more than just "maximizing profits". By making Adobe accessible to students, Adobe is trying to "lock-in" young users into their platform. Since learning adobe proficiently takes a lot of effort, students will be reluctant to switch out for alternatives in the future.  

### Database Services (Mongodb)
MongoDB offers so many different services and products, it's hard to keep track of everything. For now, let's focus on their software. MongoDB segments their prices based on versioning.

MongoDB offers both a "Community" and "Enterprise" version of their database software - that are almost identical feature. The core database system and its features are the same among both features (same encryption, memory & storage limits, scalability, etc). However, the "enterprise" software has add-ons and extra services that greatly increase the quality of life for enterprises. [The full list is here](https://www.mongodb.com/products/mongodb-enterprise-advanced), but some examples include monitoring software, 24/7 support team, visualization tools, security tools, training, etc. 


### Text Editors (Sublime Text)  
Sublime text doesn't suffer from versioning we might see in other software and offers the full-featured version for free! The only downside being that an occasional pop-up appears that suggests you purchase a license (as a long-time user I think it's not that annoying). Paying a full-license only removes the pop-up! However, sublime isn't free because the developers are generous people (they could be). Sublime needs to compete with other free text editors that have plenty of community support and plugins - keeping Sublime free is a matter of survival. 

# An Important Aside:

>**Counterpoint: Versioning your product creates new products which breaks the definition**  
This is true. The definition of "price discrimination" is selling the same product and different prices - NOT different products at different prices. However, a lot of businesses start from one product, and chop it up into multiple different ones to target different markets. Moreover, we can treat differences as variations on "quality" because these versions never have trade-offs in features, the premium version is strictly better than all other versions. Finally, Varian (who is also the "chief economist" at Google) classifies versioning as a price discrimination.

>**Is everything price discrimination? (no)**  
Not everything is price discrimination! Sometimes, the materials and costs of items genuinely warrant a higher price
They're are genuine reasons for a business to raise or lower prices. For example, higher than expected demand can raise prices. Lower than expected demand can also lower costs (the cost of holding inventory may be too high, which will cause the company to try to get rid of it ASAP).  
>Pricing has a strategic component beyond taking as much money as they can. We saw some of this strategy in some of our software examples. You might point to sublime having a fully-featured free version as an example of price discrimination! This might be partially true - there are strategic component to pricing. Offering a competent free-tier can be thought of as a way of acquiring users and developers in to create a network effect for your product. In addition, the more people that use your software, the more "stuck" they become in the ecosystem (also known as "Lock In"). These practices are very popular in other software as well (like the Microsoft suite, Adobe products, etc).   
>For the sublime example, you need to realize that the company competes with other great, free, open-source options. SublimeText might be free because the developers are generous (they are awesome people), but it's also because they are competing with open-source editors such as VIM, EMACS, Notepad++ (VSCode, atom, etc).



# How Tech Helps Price Discrimination
With the spotlight on giant tech companies in the past few years for abuses in privacy, democracy, and competition in the industry, people do not yell things like "tech companies are abusing their power to charge me 2 dollars more for gloves".

**"The internet is a medium for personalized pricing"** (Shaprio, Varian, "Information Rules: A Strategic Guide to the Network Economy", 42)

>**My Personal Vendetta: Lyft**  
I used to use Lyft quite a fair bit, I didn't bother using Uber because the ride-share prices were cheaper on Lyft (when I checked). One summer, my friend and I were trying to get a ride from the annual Chicago jazz festival to a nearby restaurant. His 2-person lyft ride was $8, while my ride was a whopping $10! It was only two dollars, but I felt betrayed - I swore to never use their services again (and I have not since then).

> At the same time, I was amused at how clever rideshare companys have been. My friend was a religious Uber user, and only tried using lyft because I suggested it. This might be a wrong assumption (but not a blind guess), but I assumed Lyft was giving an increased discount to my friend to get them to switch over. Uber & Lyft were using technology to create personalized pricing schemes to compete with each other. 

I haven't kept track if practices like these were still allowed (with secuirty on mobile devices increasing) 



Ebate & Honey - You think these companies are just a platform for uploading coupons and getting rebates? They're studying your behavior and stealing your data 
Big Data - They're using SQL Joins on your online spending habits using your personal information as the key (i.e matching your information on different platforms to build a complete profile of you), 
Cookies - The store cookies in your browser to track who you arte and where you are
Web Pages - 
VPN & Locations for Plane Tickets - You used to be able to get discount tickets using a VPN! 
DRM (Digital Rights Management) - DRM is used to prevent arbitrage, in addition to being very annoying to use.

My friend got me a certain game on steam for $10 less by purchasing it within his home country (thailand).

# Pros of Price Segmentation
It seems that price discrimination is just a loss for consumers where every customer is unable to find "good deals" on anything they want anymore. 

In addition, it's a big win for "businesses" as they can extract every single dollar from each company that they can. 

Despite the cynical presentation, price discrimination has upsides as well - mainly that it increases the accessibility of a good for (some) people. 

Whether you benefit or lose from this segmentation depends on where you are on the demand curve. If your willingness to pay is above the equilibrium price, you benefit from no price discrimination. If your WTP is below the equilibrium price, then you're out of luck. With price discrimination, the people who aren't able to afford the product, will be able to at their WTP. If we could not price discriminate, people would have to pay the "supply and demand" cost and pay. Suddenly less wealthy individuals cannot afford the newest hardware or adobe suite, while businesses will be happy that their margins have gone up!

### Welfare Economics
Your first impression is that price discrimination is a greedy move by companies to suck out each dollar from its  consumers, and you're right.

However, price discrimination can increase effiency in **some** (this part is important) market situations, as well being more equitable giving

The example you might think of is that Alice paid \$5 for her pizza while Bob paid \$3 (which is just unfair!).
However, another example is that Alice pays \$50 for a life-saving drug, and Bob in a poorer-country pays \$10 in his country (which may be proportionate to \$50 in his country's currency)\*.

\* However, the situation is more nuanced and there are some cases worth discussing later. 


# Cons of Cashing out Willingness to Pay (Has it gone to far?)
The idea behind price discriminatino to grab every dollar that the customer is willing to pay. People argue that this desire to maximize profit is natural and important for keeping the free hand of the market yadda yadada. 

However, can this diea go too far?

### Insulin
Even though segmentation sounds wonderful, there are still egregious cases where this segmentation and desire to maximize profits have gone too far. 

A prominent example is the healthcare system (at least in the US). Insulin is like $300 in America, while in other first to third world countries, Insulin is much cheaper. 

(Don't even argue, insulin is so old and nothing new has fundamentally changed)

It's good that people in poorer countries that have an easy to access quality 

however, this isn't economics faults - its politics (although businesses being able to lobby politicans make it a business fault).  

https://youtu.be/7Ycd8zEdoVk
https://youtu.be/9CdydQNfAXE

Insulin is expensive, just because it can be in the US

You could argue that insane medicine prices are a result of America's failure in the medical sector.

However, people forget that hospital's charge high intitial prices for treatment because the insurance companies were trying to negotiate lower WTP.

In short, it's because hospitals know that Insurance companies have such high WTP that they try to take as much money as possible

### Big Data and Privacy
In addition to healthcare, this desire for information has d

# The Future 
Price discrimination/segmentation is a new business idea in the slighest 
"QUOTE FROM THE BOOK ABOUT UNDERCOVER ECONOMIST"
"QUOTE FROM THE BOOK BIG DATA BOOK"

# Conclusion
Takeaways, if you have a good product odds are you can sell it without no troubles and your company will do fine. However, if you want ever dollar you can squeeze out of your customers, than you want to version your product (by adding or removing features) such that they would be unable to buy different versions. 


### Links

https://wearecitizensadvice.org.uk/markets-dont-work-like-they-used-to-and-people-are-starting-to-notice-af00ed38014d

https://obamawhitehouse.archives.gov/sites/default/files/whitehouse_files/docs/Big_Data_Report_Nonembargo_v2.pdf?utm_source=Bruegel+Updates&utm_campaign=656e7da39b-Blogs+review+11%2F02%2F2017&utm_medium=email&utm_term=0_eb026b984a-656e7da39b-278510293

https://www.bruegel.org/2017/02/big-data-and-first-degree-price-discrimination/

https://theconversation.com/buyer-beware-online-shopping-prices-vary-user-to-user-33439

# Readings & Sources

**Fun & Informative**
Underground Economist (Fun Reads). A lot of these examples are from britain, so I personally was very confused. 
Naked Economics
Weapons of Math Destruction (Fun Read. Not about economics, but flaws in big data)

**Dry & Informative**
Principles of Economics (A classic econ-101 beginner textbook, but it has all the fundamentals. I would never reccommend reading the whole thing, but treating it as a resource if you need to consult about basic concepts.)

Information Rules: A Strategic Guide to the Network Economy (an interesting formalization/guide to the economics of business in the digital age. The book may "seem old" in an industry that is defined by rapid change (its age reflects in its examples), but as the authors state "technology changes. Economic laws do not.") For example, chapter 2 of the book talks about hwo companies can tracke user behavior through cookies, but it had its limitations (before the explosion in popularity of javascript). 

The "Observation" section of chapter 2 (around page 36 and onwards) makes me laugh

The Strategy and Tactics of Pricing (A different book about pricing, but more general )

**Articles and Links I've looked at**
https://hbr.org/2012/05/can-there-ever-be-a-fair-price

https://www.economist.com/free-exchange/2016/02/29/disney-discovers-peak-pricing
https://www.economist.com/free-exchange/2011/11/22/the-adult-book-premium
https://theconversation.com/the-economics-behind-ubers-new-pricing-model-78180#:~:text=Economists%20generally%20refer%20to%20three,extracting%20all%20of%20their%20WTP.

https://news.ycombinator.com/item?id=10029341

https://youtu.be/A_1lGkbIx7M?t=191

https://www.investopedia.com/terms/v/versioning.asp

# Corrections or Complaints?
Email me at nablags97 [at] gmail [dot] com 