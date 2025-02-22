# 🚗 Miles Carsharing Data Analysis

## 🛠️ Project Overview
This project focuses on analyzing **Miles Carsharing data** to understand **user behavior, conversion drop-offs, and optimization opportunities** in the app's **gift feature funnel**.

The main objective is to identify **bottlenecks in user engagement**, determine **drop-off points**, and provide **data-driven recommendations** for improving conversion rates.

**Relevance:** This project aligns with **data analytics and business intelligence roles**, focusing on **SQL-based funnel analysis** and **product feature optimization**.

---

## 🔗 Resources & Links
- **Google Colab Notebook**: *(If applicable, add a link here)*
- **Dataset Source**: *(Specify if from Kaggle, company data, etc.)*
- **GitHub Repository**: *(Your GitHub link here)*

---

## 🎯 Project Goals
- **Analyze user behavior** in the Miles app's **gift feature funnel**.
- **Identify major drop-off points** and analyze why users abandon the gift feature.
- **Optimize conversion rates** by providing actionable recommendations.
- **Compare engagement trends** across different platforms (iOS vs. Android).

---

## 🗂️ Dataset Description
The dataset consists of event logs capturing user interactions within the Miles Carsharing app. The **gift feature funnel** is divided into three main stages:

- **Upper Funnel:** Users interacting with the wallet but not proceeding further.
- **Middle Funnel:** Users selecting a gift credit but not completing the purchase.
- **Lower Funnel:** Users who finalize the gift transaction.

### **Key Columns in the Dataset**
- **`user_id`**: Unique identifier for each user.
- **`event_name`**: Specific action taken (e.g., `menu_wallet`, `wallet_gift`, `gift_item_click`).
- **`platform`**: Device used (iOS or Android).
- **`event_date`**: Date of event occurrence.
- **`trip_creation_time`**: Time the user initiated an action.
- **`gift_funnel_stage`**: Categorization of users into different funnel stages (Upper, Middle, Lower).
- **`drop_off_reason`**: Identified friction points where users leave the funnel.

---

## 📊 Key Insights & Visuals

### 📊 **1. Funnel Drop-Off Analysis**
```sql
WITH gift_funnel AS ( 
    SELECT DISTINCT user_id, 'upper' AS funnel_stage 
    FROM `karuns-miles-project.miles.karun_miles` 
    WHERE event_name IN ('menu_wallet', 'wallet_gift', 'wallet_topup_quick')
    UNION ALL 
    SELECT DISTINCT user_id, 'middle' AS funnel_stage 
    FROM `karuns-miles-project.miles.karun_miles` 
    WHERE event_name IN ('gift_item_click_giftcredit_5', 'gift_item_click_giftcredit_10',  
                         'gift_item_click_giftcredit_50', 'gift_item_click_giftcredit_100',  
                         'confirm_gif', 'go_gif')
    UNION ALL 
    SELECT DISTINCT user_id, 'lower' AS funnel_stage 
    FROM `karuns-miles-project.miles.karun_miles` 
    WHERE event_name IN ('gif_send', 'gif_copy', 'cancel_gif', 'cancel_go_gif') 
) 
SELECT 
    COUNT(DISTINCT CASE WHEN funnel_stage = 'upper' THEN user_id END) AS upper_funnel_users, 
    COUNT(DISTINCT CASE WHEN funnel_stage = 'middle' THEN user_id END) AS middle_funnel_users, 
    COUNT(DISTINCT CASE WHEN funnel_stage = 'lower' THEN user_id END) AS lower_funnel_users
FROM gift_funnel;
```
**Summary:**
- 33,723 users interacted with the wallet but did not proceed to explore gift-related features.
- 448 users started preparing a gift but dropped off before purchasing.
- Only 224 users (0.65%) completed the full gift purchase flow, highlighting a low conversion rate.

### 📊 **2. Conversion Rate Analysis by Platform**
```sql
SELECT platform, 
       COUNT(DISTINCT user_id) AS total_users, 
       COUNT(DISTINCT CASE WHEN event_name = 'gif_send' THEN user_id END) AS completed_gift_users, 
       COUNT(DISTINCT CASE WHEN event_name = 'gif_send' THEN user_id END) / COUNT(DISTINCT user_id) * 100 AS conversion_rate
FROM `karuns-miles-project.miles.karun_miles` 
GROUP BY platform;
```
**Summary:**
- Android users interact with the wallet more but convert at lower rates than iOS users.
- Possible reasons: UI/UX differences, friction in payment methods, or missing incentives on Android.

### 📊 **3. Time-Based Engagement Trends**
```sql
SELECT event_date, 
       event_name, 
       COUNT(DISTINCT user_id) AS user_count 
FROM `karuns-miles-project.miles.karun_miles` 
GROUP BY event_date, event_name 
ORDER BY event_date;
```
**Summary:**
- Wallet interactions are consistent across all days, but gift completions are much lower.
- Gift purchases slightly increase during holiday periods, suggesting a seasonal impact.

---

## 🔍 Overall Insights & Business Impact
### **Key Findings**
1. **Major Drop-Offs in the Upper Funnel**
   - 98% of users interact with the wallet but do not explore gift features.
   - Possible causes: Lack of awareness, unclear navigation, or no incentive to proceed.

2. **Friction in the Middle Funnel**
   - Users start the gift process but do not finalize purchases.
   - Potential reasons: Complicated flow, unclear value proposition, or difficult payment process.

3. **Android vs. iOS Disparities**
   - Android users have higher engagement but lower conversion rates.
   - Possible solutions: Optimize the Android UI/UX or provide tailored incentives.

4. **Seasonal Trends Affect Gift Purchases**
   - Gift transactions increase slightly during holidays.
   - **Business Opportunity**: Leverage seasonal campaigns and special offers.

---

## 📌 Final Recommendations
1. **Increase Engagement Between the Upper and Middle Funnel**
   - Add contextual prompts when users interact with the wallet.
   - Introduce push notifications to nudge users towards gift options.

2. **Simplify the Gift Purchase Flow**
   - Reduce the number of steps required to complete a gift transaction.
   - Introduce a progress bar to keep users engaged.

3. **Leverage Promotions & Seasonal Offers**
   - Offer bonus credits for first-time gift purchases.
   - Run holiday campaigns to boost conversions.

4. **Optimize Android User Experience**
   - Investigate potential UI/UX frictions causing Android users to drop off.
   - Conduct A/B testing on Android-specific incentives.

---

## 🚀 Project Completion & Conclusion
- All planned analyses have been successfully conducted, and insights have been structured into key action points.
- The dashboard and SQL analysis provide a clear roadmap for optimization.
- **Next Steps**: Implementing and testing solutions to improve conversion rates.

✅ **Project Completed!** 🚀
