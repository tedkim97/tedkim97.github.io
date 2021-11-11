---
layout: page
title: Curriculum Vitae
hide_link: true
permalink: cv
---

# Ted Kim

I am a software engineer specializing in back-end systems with experience ranging from programming to deployments and monitoring, but at night I'm also interested in working with high performance computing environments and machine learning applications.  

I currently work as a SDE II on McDonald's "order-core" services that handle **70 millions digital orders annually** (approximately 200,000 digital orders per day), resulting in **\~$250 million in yearly revenue**. I also work on a global deployment strategy for replacing the legacy core order infrastructure (orders 1.0) with new microservices for better scalability, market specific compatibility, maintainability, and reliability. In my previous roles I worked as a systems engineer at "McMaster-Carr Supply Company" where my major contribution was engineering a new search suggestion engine for the product catalog. The new engine used customer searches to serve "better" suggestions (opposed to the traditional method of using two SQL tables), and **Version 1 increased search success rate by 10%** (a "successful search" being defined as a search that directly results in "adding product to cart", "downloading product CAD", or "purchasing product"). 

As an undergrad research assistant, I was the "full stack" engineer on "MobileMedicine" (informal name) an adaptive diagnostic and electronic medical record system for public health in low-resource areas. The goal was to create a SMS-based, light-weight platform to help mimic the diagnostic process while also creating high-quality medical documentation for patients in low-internet areas. The assistant system was trained using multiple data sets and open-source medical literature, and tested by primary care providers in Pakistan with additional trials in Rwanda and Nigeria. During my undergrad I've explored projects such as bio-metric authentication with sensor fusion and machine learning, music generation with GRUs.

# Projects
### Orders 2.0
"Orders 2.0" is a solution aiming to simplify McDonald's software roll out - which is a side effect of its franchise business model. Software updates and deployments needs to be extensively modified and tested by their respective "markets" (US, FR, EE, UK, JP, etc). These markets modify the global version of the ordering services because they need to implement region specific features and promotions that are impractical to support on a global scale. For instance, the U.K needs to modify code to accept the "monopoly" promotion because it's drastically increases sales (while the other markets have abandoned it). In the U.S we've added a new microservice to support the McRewards program.  

Since the original order core services were not designed for these purposes and have "rotted" from high developer turnover, modifications have resulted in even slower deployments and increased hesitation from franchisees accepting software updates. Orders 2.0 is aimed at reducing this headache and speeding up updates by providing better abstractions for the core services (that can be customized more cleanly), as well as leveraging modern technology to address automatic scaling, reliability, and logging.

In terms of software development, I program micro-services in C#, analyze and document legacy services, implement compatibility on "non-core" services (such as the gateway and orchestrator), and test with Gremlin. For deployments & operations, I create docker images, update Kubernetes configurations, make jenkins build pipelines, and create monitoring dashboards and alerts using NewRelic. 

### Improving Search Suggestions:
Improving search suggestions was a part of a larger project to increase the success rate of searching through the McMaster-Carr catalog. User-studies and interviews conducted by the company showed that customers often need to manually search through the catalog to find the product they need. This is because there are a surprising amount of difficulties when working with search for industrial parts/equipment. Sometimes the terminology and semantics the customer understands differ from the official spec, and other times the suggestions engine doesn't rank the most popular products for a search trie ("sc") with the most popular product ("1/4-20 inch screw"). Version 1 of the new search engine resulted in a **10% increase in success rate** (searches that directly result in "adding to cart", "downloading cad", or "purchasing"). 

My project was to engineer a new search suggestion engine using customer searches to serve "better" suggestions. This project consisted of efforts structuring queries on Neo4J databases (a graphDB of customer interactions) as well as confirming reliability and accuracy of the results. I also needed to experiment and test with different cleaning and grouping algorithms using Python and C#. For operations, I needed to configure migration jobs in C#, configuring and launch the search suggestion Elasticsearch indices (cloning, replicating, priming caches, etc). Something that was not required, but I deemed essential was documenting the code, processes, and design decisions for future maintainability. 

### Lightweight Mobile Automated Assistant-to-physician for Global Lower-resource Areas (MobileMedicine)
MobileMedicine is a healthcare experiment to provide infrastructure-lacking countries better diagnostic and electronic medical record (EMR) systems. Healthcare in these communities suffer because modern tools (like diagnosis references and medical histories) are crippled without internet infrastructure. By using existing medical datasets as a training set & SMS as a communication protocol, MobileMedicine aims to improve healthcare outcomes with low-compute inference and more ubiquitous communication protocols. The project has completed deployments with trials and user studies in Pakistan with an accuracy of 68% with very positive user-studies. More trials are projected with Rwanda and Nigeria in the near future.

I built the SMS-based server system using Python as the serving language, and MongoDB as the database. I adapted an Huawei E8372 to send SMS data over USB, as well as encrypting SMS messages for security. I implemented the diagnostics engine that uses simplistic models to calculate probabilities of patient diseases from a set of symptoms (in Java) while using Android Studio and Java to create the frontend.  

- ArXiv Preprint: https://arxiv.org/abs/2110.15127
- Publication: PLOS Global Public Health (February 2021)
- Repository: https://github.com/humancomputerintegration/realtime-diagnostics-app
- Repository: https://github.com/imechaozhang/Mead-Android-App

### Semantic Code Search through Unsupervised Learning
This project was a final project for CS229 at Stanford University. The motivation behind this project is that software in "non-tech" environments such as enterprises where software is not he main product, have convoluted structures that make contributing as a new team member difficult. Theoretically, the process of fixing bugs or adding features to existing software involves understanding the structure of the software's codebase. After an individual acquires an understanding "the code", the individual needs to (1) find the correct areas to modify/add code (2) understand the behavior of the code to measure the effect of the changes. External factors like high developer churn (rate at which software developer leave a project), code churn (rate at which code is change), increasing lines of code, or incentives to skimp on tests and documentation make this difficult. 

"Semantic Code Search Through Codebases" intends to use the semantic meaning of code through naming and function abstractions (functions named "abs", "average", "log", "PostPurchase", "UpdateClientInfo", etc) to search code through its semantic meaning. This technique processed code bases by using lexers & parsers to parse code, convert files into vectors using TF-IDF, and classifying using K-Means & KNN. 

### GaitKeep: Gait Authentication w/ Sensor Fusion
Gaitkeep was a final project for "CMSC23400: Mobile Computing" at UChicago. Gait-Keep was a machine-learning system for developing more robust biometric authentication methods. Previous methods to gait authentication suffered from a trade-off accessibility and security. Our method builds off academia’s research in sensing by using multiple sensors (accelerometers and gyroscopes in a smartphone) along with a video camera to reduce the mentioned trade-off. These methods use ConvNets to both classify accelerometer and gyroscope data along with video feed.
- Methodology & Retrospective: https://nablag.com/gaitkeep
- Repository: https://github.com/tedkim97/immobile-computing-project
- Repository #2: https://github.com/aprilyw/immobile_computing 

### K-Means Based Movie Visualizations
KMeans Based Movie Visualizations is a critique of popular subgenre of movie visualizations on reddit. These visualizations used incorrect techniques (such as down-sampling or color averages) to represent frames within a movie which brought inaccurate representations of color within these visualizations. My solution used K-Means to quantize the colors within a frame of a movie, to generate perceptually correct colors. I've extended the project my leveraged CUDA supported libraries to speed up the fitting process by 150% speedup. 
- Critique & Retrospective: https://nablag.com/better_barcode_vis
- Repository: https://github.com/tedkim97/movie_visualizations

# Education & Employment
### **University of Chicago**: B.A. Computer Science and Economics
- Specialization in Machine Learning
- *magna cum laude* (GPA 3.7/4.0)
- Dean's List 2016-2020

### McDonald's Corporation (May 2021 - Present)
#### Software Engineer II
- Maintaining the "Orders 1.0" microservices - a core set of services that handles 70 million orders annually,  resulting in $250 million in revenue yearly in the US.
- Working on Orders 2.0 - a rollout initiative with a new core services better scalability, market specific compatibility, maintainability, and reliability. 
- Programmed features and deployed the McDonald’s Loyalty micro-service (McRewards Program)  
- Refined Logging and built dashboards for monitoring performance and reliability of microservices responsible for "orders" and "loyalty" with NewRelic
- Improved latency, traffic, and state metrics for robust representation of services’ performance
- Refactored service architecture and communication to reduce processing time on exchanges by 6% 
- Tested reliability of loyalty architecture through load simulations and chaos engineering with Gremlin

### McMaster-Carr Supply Company (August 2020 - May 2021)
#### Systems Engineer
- Implemented and released a new search suggestion engine using data-driven suggestions and replacing legacy search suggestions
- Migrated search suggestions engine and processes from legacy SQL tables to Elasticsearch indices
- Documenting code, architecture, and design decisions for future maintainability by other engineers
- Developed and released an internal analytics tool with my cohort for company analytics and sales department

### University of Chicago: Human Computer Integration Lab (June 2019 - Dec 2019)
#### Research Assistant
- Engineered a healthcare initiative for infrastructure poor countries (Mobile Medicine) as a collaboration between the Pritzker School of Medicine and the Computer Science Department
- Collaborated with medical researchers to pick ideal patient features to collect in the data-collecting pipeline
- Developed an SMS-based app and server ecosystem along with communication protocols
- Configured MongoDB database and integrated database functions into server with MongoDB drivers
- Co-author of paper "Lightweight Mobile Diagnostic System for Global Lower-resource Areas" (publication February 2021)

### Oberon Securities (May 2018 - August 2018)
#### Investment Banking Summer Analyst 
I edited presentations, filled in spreadsheets,and sat in meetings. I appreciate the experience and learned a lot about the industry, but I realized I was not a good fit for the role.

### Rebellion Research (May 2017 - August 2017)
#### Data Science Intern
- Researched investment factors and returns at an artificial intelligence-based hedge fund with $50 million AUM
- Collaborated with other interns to create company reports with an analyst’s recommendation
- Research and engineered an experimental sentiment-based trading program for company testing

# Skills & Courses

##### Computer Science
- CMSC 25025: Machine Learning and Large-Scale Data Analysis
- CMSC 29700: Reading and Research in Computer Science
- CMSC 25040: Introduction to Computer Vision
- CMSC 23900: Data Visualization
- CMSC 23400: Mobile Computing
- CMSC 22300: Functional Programming
- CMSC 22100: Programming Languages

##### Statistics
- STAT 23400: Statistical Models/Method
- CMSC 21800: Data Science for Computer Scientists
- ECON 21020: Econometrics
- SOSC 13100-13300: Social Science Inquiry

##### Math & Theory
- MATH 19520: Math Methods for Social Sciences
- MATH 19620: Linear Algebra
- MATH 15300: Calculus-3
- CMSC 27100: Discrete Mathematics
- CMSC 27200: Theory of Algorithms
- CMSC 28400: Introduction to Cryptography
- CMSC 25300: Mathematical Foundations of Machine Learning

#### Programming Languages
- C# (.NET Ecosystem)
- Python (numpy, torch, scitkit, networkx, pandas)
- C++ (C++17 & Above) 
- C
- Golang
- F# 
- SML N/J

#### Technologies/Skills
- Kubernetes
- Docker
- Terraform
- Jenkins
- Deployment
- Monitoring
- Amazon Web Services & Architecture (SQS, SNS, Dynamo, S3, etc), MongoDB, Elasticsearch, Neo4J, MySQL, Gremlin, and ReactJS
- Systems Architecture 
- Performance & Reliability testing
- Reading Legacy Enterprise Code
- Data Visualizations
- Object Oriented & Functional Programming
- Public Speaking


# Hobbies
- I enjoy making good ice cream flavors (Thai Tea, Molasses, Black Seasame) and weird ice cream flavors (like Szechuan & Black Garlic)
- I review restaurants/foods on Instagram (@foodawkward)
- I walk around and play Pokemon Go
- I used to play Starcraft, League of Legends, and Hearthstone (not so much anymore)

