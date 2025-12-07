# Individual Contribution: Swire Coca-Cola Cart Abandonment Project  

## Project Context  
This project was completed as part of a collaborative analytics initiative with **Swire Coca-Cola**, using **real business data** from the company’s B2B ordering platform, **MyCoke360**.  

The main goal was to use data analytics and machine learning to **predict which customers are most likely to abandon their carts** within the first 30% of their order window, allowing Swire to take proactive actions to reduce revenue loss.  

---

## Business Problem and Objective  
Cart abandonment occurs when a customer adds products to their cart but does not complete the purchase before the order window closes.  

For Swire Coca-Cola, this behavior represents a significant business problem:
- A large share of customers begin orders but never check out.
- This leads to millions of dollars in lost potential sales and missed operational opportunities.  

**Objective:**  
To build a predictive model that can identify customers at risk of abandoning their carts early. The Google Analytics Data is captured within the first 30% of the remaining time within an order window, from the first Google Analytic Activity. This information is run through our model to predict if the rest of 70% going to have an purchase activity. 
---

## Solution Overview  
To address this problem, our team built a **supervised machine learning model** using XGBoost to predict the probability of cart abandonment.  
The model achieved:  
- **Accuracy:** 81.8%  
- **Recall:** 71.2%  

We focused on maximizing **recall** to ensure the model correctly identified as many potential abandoners as possible.  
This approach allows Swire to target high-risk customers efficiently, increasing recovery opportunities for lost orders.  

The model was supported by extensive **data engineering**, **Google Analytics event integration**, and **customer-level feature construction** that reflected purchasing behavior across changing order frequencies and cutoff times.  

---

## My Individual Contributions  

### 1. Data Preparation and EDA  
- Conducted exploratory data analysis in `EDA.ipynb` and `eda_use_abandon_table.ipynb`, examining trends in abandonment by customer type, shipping condition, and distribution mode.  
- Validated data consistency across GA4, Orders, Sales, and Visit Plans tables.  
- Assisted in defining the “abandon” label logic based on add-to-cart and cutoff behavior.  

### 2. Master Table Creation  
- Contributed to the development of `master.csv`, which merged and exploded order windows for each customer.  
- Helped create a repeatable process that accounted for **changing order frequencies and cutoff times**, ensuring each customer’s window was correctly represented.  

### 3. Modeling and Feature Engineering  
- Developed multiple modeling iterations in:
  - `hung_modeling.ipynb`
  - `modeling with new csv.ipynb`
  - `modeling_new_logic.ipynb`
  - `modeling_new_logic copy.ipynb`
  - `modeling_new_logic copy 2.ipynb`
- These notebooks included:
  - Feature construction for timing, cart events, and frequency metrics.  
  - Model evaluation using precision, recall, and ROC-AUC.  
  - Parameter tuning and interpretation of feature importance.  

My contributions were particularly focused on **testing new variables derived from GA4 event sequences** and **balancing recall versus accuracy tradeoffs** in the XGBoost model.  

---

## Business Value of the Solution  
The model gives Swire Coca-Cola a practical way to reduce financial losses from incomplete orders.  

- By predicting cart abandonment early, Swire can send targeted reminders or incentives.  
- Even a **25% success rate** in intervention could recover **over $500,000** per order cycle.  
- The findings also highlight operational opportunities, such as optimizing shipping options and improving the Tell Sell distribution process.  

This predictive solution provides measurable business impact while improving customer satisfaction through timely engagement.  

---

## Difficulties Encountered  

Building the **master table** was one of the most technically demanding parts of the project. The process required integrating multiple tables—Orders, Visit Plans, Cutoff Times, and Google Analytics (GA)—each with different structures and timestamp logic. The challenge came from aligning these sources into a consistent format while keeping every customer’s local time zone and order frequency accurate.  

We also discovered that **GA data alone was not sufficient** to confirm whether a purchase actually occurred. Some transactions happened outside the online platform, so the GA events did not include all purchases. To address this, we merged the **Orders.csv** table into our dataset to capture both online and offline activity, ensuring that any order completed within a customer’s active window was properly recognized.  

Once the master dataset was built, we ran into a serious **data leakage issue** in our first modeling attempt. Our model used the entire GA event window—including events after a purchase—to predict whether a purchase occurred in that same window. This meant the model was learning from future data, which made performance unrealistically high but useless for real-time prediction. Our professor, **Jeff Webb**, identified this issue and helped us understand why the approach was flawed.  

To fix this, we introduced the concept of a **capturing period**, which restricted GA data to only the **first two days** of each order window. This approach better simulated how Swire would make predictions in production. However, this introduced a new limitation: not every customer uses MyCoke360 within the first two days. Many customers interact later in the window, which caused a large number of missing cases and reduced our training data.  

We then experimented with **variable capture lengths**—for example, three, four, or five days depending on the customer’s typical ordering frequency—but these adjustments did not meaningfully increase model coverage.  

Finally, we adopted a **flexible, behavior-based capture approach**. Instead of defining a fixed number of days, we captured GA data dynamically based on the **first user activity** within each order window. This made the dataset more representative of real customer behavior and allowed the model to adapt to different engagement patterns. This final solution balanced accuracy, practicality, and data integrity.  
<img width="1269" height="601" alt="image" src="https://github.com/user-attachments/assets/3832e918-04e3-497e-bc9a-1c9d27820d84" />


---

## What I Learned  
Through this project, I gained hands-on experience in several key areas:  
- **Advanced Data Engineering:** I loved to play around with complex logic and I hop this will be a big part of my career. 
- **Predictive Modeling:** Understanding how to balance accuracy and recall based on business priorities.  
- **Google Analytics Integration:** Using behavioral event data to enrich predictive features.  
- **Collaboration in Data Science Projects:** Juggling around each team member's idea of importance and yourself is not easy, this is a skill of politic that I could better mastered.
- **Data Ethics and Model Validation:** Recognizing and resolving data leakage to ensure valid and trustworthy predictions.  

---


## File Summary  

| File Name | Description |
|------------|-------------|
| `EDA.ipynb` | Initial exploratory analysis of customer behavior and cart abandonment patterns |
| `eda_use_abandon_table.ipynb` | EDA focused on identifying abandonment through event logs |
| `hung_modeling.ipynb` | Early model development using logistic regression and XGBoost |
| `modeling with new csv.ipynb` | Revised modeling pipeline using engineered master dataset |
| `modeling_new_logic.ipynb` to `modeling_new_logic copy 2.ipynb` | Iterative improvements of final XGBoost model logic |
| `master.csv` | Final engineered dataset with customer-level window and frequency features |

---

## Summary  
This project demonstrates how data analytics and machine learning can solve real-world business problems.  
Through individual experimentation and group synthesis, I helped build a predictive model that improves revenue retention and customer engagement for Swire Coca-Cola’s MyCoke360 platform.  
My work focused on exploratory data analysis, dataset engineering, and model optimization, all documented in this repository for transparency and reproducibility.
