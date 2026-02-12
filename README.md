# Cosmetic-Reformulation-ML
## The Mission
In the lab, a surprise ingredient ban is a crisis. It wastes R&D hours, expensive raw materials, and creates unnecessary stress for the formulation team. I built this tool to provide a 6–12 month "early warning" for ingredient risks, allowing chemists to plan reformulations before they become emergencies.

📊The Project at a Glance
Dataset: 114,000+ entries from the California Safe Cosmetics Program (CSCP).

The Reality:📉 92.1% of products are stable (not reformulated). This is great for safety, but it creates a Class Imbalance that makes typical models "blind" to the high-risk cases.

The Strategy: I used SMOTE (Synthetic Minority Over-sampling Technique) to teach the model how to recognize the rare signatures of a product about to be updated.🧬

# Visualizations

1. High-Volatility Categories
My EDA shows that the Makeup category has the highest concentration of hazardous chemical flags. This highlights exactly where labs need to focus their "clean beauty" pivot.

<img width="981" height="584" alt="image" src="https://github.com/user-attachments/assets/33484eae-c76f-40ea-a756-79b697e665ba" />
*Makeup remains the highest-risk category due to complex pigment formulations and frequent regulatory updates.*

2. Engineering "Chemist Intuition"
To make the data meaningful for a lab setting, I created custom scores that reflect a chemist's priorities:
* **Safety Score:** Based on Prop 65 and carcinogen lists.
* **Efficacy:** Does the ingredient actually do its job well?
* **Sustainability:** Is it eco-friendly?

## **🛠️Technical Methodology**

To help the lab avoid "fire drills," I built a pipeline that moves from messy regulatory data to a predictive risk score. 

### **1. Data Engineering & Preprocessing**
Real-world lab data is rarely clean. I focused on standardizing the data so it could be used for modeling:
* **Cleaning:** Standardized inconsistent chemical names using **CAS Numbers** to ensure one chemical didn't appear under multiple names.🔍
* **Feature Creation:** Engineered numerical features like **Chemical Age** and **Product Lifespan** to track how long a formula stays "safe" before a regulatory update.⏳
* **Scoring:** Assigned custom **Safety, Efficacy, and Sustainability** scores based on my background in cosmetic chemistry.⭐
<img width="984" height="484" alt="image" src="https://github.com/user-attachments/assets/eeb86b58-6c54-4acb-99e8-d67180f41874" />

### **2. Modeling Strategy**
I compared multiple models to see which one could best predict a "Need to Reformulate" event.

| Stage | Model | PR-AUC | Note |
| :--- | :--- | :--- | :--- |
| **Baseline** | Logistic Regression | 0.54 | Good for simple trends, but missed too many complex risks. |
| **Ensemble** | Random Forest | 0.63 | **Top Performer.** Balanced precision and recall perfectly. |
| **Advanced** | XGBoost | 0.61 | Strong, but required more tuning to avoid overfitting. |

### **3. Addressing Class Imbalance**
In this dataset, **92.1% of products are stable**, while only **7.9% are reformulated**. 
* **The Challenge:** A standard model would "cry wolf" or simply ignore the 8% of risks to stay "accurate."
* **The Solution:** I implemented **SMOTE** (Synthetic Minority Over-sampling Technique) to create a balanced training set, allowing the model to learn the specific chemical patterns of at-risk products.
<img width="1151" height="539" alt="image" src="https://github.com/user-attachments/assets/246ca39d-6b03-41c7-b99a-d54c64ecb383" />

### **4. Model Evaluation (The "No False Alarms" Goal)**
In a lab setting, a "False Alarm" costs money, but a "Missed Risk" costs the whole brand. I optimized for a balance:

* **Precision (0.68):** Ensures we don't flag "safe" products, which saves the lab money and R&D resources.
* **Recall (0.72):** Ensuring we catch ~72% of reformulation risks 6–12 months in advance to avoid "fire drills."
* **PR-AUC (0.63):** This was my primary metric because of the class imbalance. It proves the model is significantly better than a "random guess" (which would be 0.08 in this dataset).
