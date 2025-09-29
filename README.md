# Startup Success & Failure Prediction Project

## 📌 Loyihaning maqsadi
Ushbu loyihaning asosiy maqsadi **ikki turli startup dataset** bilan deyarli bir xil preprocessing va modeling pipeline ishlatib, natijalarni ilmiy asosda solishtirishdir. Shu orqali, datasetning hajmi, feature turlari va modeling yondashuvlar natijalarga qanday ta'sir qilishi tahlil qilinadi.

---

## 🗂 Datasetlar
1. **Global Startup Success Dataset**
   - Manba: [Kaggle link](https://www.kaggle.com/datasets/hamnakaleemds/global-startup-success-dataset)
   - Qatorlar soni: 5,000
   - Ustunlar soni: 15
   - Maqsad ustuni: `Success Score` → 3 ta sinfga aylantirildi (Fail, Average, Success)

2. **Big Startup Success & Failure Dataset (Crunchbase)**
   - Manba: [Kaggle link](https://www.kaggle.com/datasets/yanmaksi/big-startup-secsees-fail-dataset-from-crunchbase)
   - Qatorlar soni: 66,368
   - Ustunlar soni: 14
   - Maqsad ustuni: `status` → binary (`operating` = 1, `failed` = 0)

---

## 🛠 Preprocessing Pipeline
Har ikki dataset uchun quyidagi qadamlar ishlatildi:

1. **Target ustunini yaratish**  
   - Dataset 1: `Success Score` → 0,1,2 (Fail, Average, Success)  
   - Dataset 2: `status` → 0 yoki 1

2. **Feature Engineering**  
   - Dataset 1: Startup yoshi, revenue/funding per employee, valuation-to-revenue ratio, followers per customer  
   - Dataset 2: Company age, funding delay/span, average funding per round, main category, month/year founded  

3. **Categorical Encoding**  
   - Kam kartinalik: One-Hot Encoding  
   - Yuqori kartinalik: Target Encoding  

4. **Skewness transform**  
   - Ijobiy skew → `log1p()`  
   - Salbiy skew → `(max-val)-log1p()`

5. **Feature Selection**  
   - Random Forest yordamida eng muhim 10–20 feature tanlandi  

6. **NaN larni to‘ldirish**  
   - Median imputation ishlatildi  

---

## 📊 Model Training
Har ikki dataset uchun quyidagi klassifikatorlar ishlatildi:

- RandomForestClassifier  
- GradientBoostingClassifier  
- LogisticRegression  
- SVC  
- KNeighborsClassifier  
- DecisionTreeClassifier  
- GaussianNB  

### Hyperparameter tuning:
- RandomizedSearchCV va GridSearchCV (faqat 1-dataset uchun kattaroq tuning)
- 5-fold Stratified CV ishlatildi

---

## 🔬 Natijalar solishtirish

| Model | Dataset 1: Macro F1 | Dataset 1: Best F1 | Dataset 2: F1 | Dataset 2: Best Accuracy |
|-------|------------------|------------------|---------------|-------------------------|
| RandomForest | 0.334 | 0.341 | 0.8916 | 0.8219 |
| GradientBoosting | 0.332 | 0.339 | 0.8956 | 0.8253 |
| LogisticRegression | 0.331 | 0.328 | 0.8890 | 0.8053 |
| SVC | 0.328 | 0.321 | 0.8960 | 0.7991 |
| KNeighbors | 0.331 | 0.338 | 0.8807 | 0.8018 |
| DecisionTree | 0.355 | 0.315 | 0.8410 | 0.8039 |
| GaussianNB | 0.315 | 0.315 | 0.8356 | 0.7428 |

### 🔑 Observations
1. **Dataset hajmi**: Dataset 2 (66k+ qator) ancha yuqori F1 va Accuracy natijalarni berdi, dataset 1 kichikligi sababli klass balanssizligi va multi-class nature natijasida past F1.  
2. **Feature turlari**:  
   - Dataset 1 numeric va engineered features (e.g., per employee, per customer) ishlatildi → tree-based modellarda stabil feature importances.  
   - Dataset 2 datetime va funding features orqali yangi informative features yaratildi → log-based skew transform va encoding bilan Accuracy yuqori bo‘ldi.  
3. **Model yondashuvi**:  
   - Tree-based modellarda (RF, GB) har ikkala datasetda eng yaxshi natija ko‘rsatdi.  
   - LogisticRegression va SVC kichik datasetda osonlik bilan macro F1ni oshirmadi.  
4. **Hyperparameter tuning**: Kichik dataset uchun batafsil tuning kerak, katta datasetda esa RandomizedSearchCV yetarli.  
5. **Ilmiy tahlil**:  
   - Multi-class prediction (dataset 1) qiyinroq va F1 past, lekin feature importance analizi orqali startup muvaffaqiyatini prognozlash uchun qaysi metriklar muhimligi ko‘rindi (`Valuation`, `Employees`, `Funding per Employee`).  
   - Binary classification (dataset 2) ancha barqaror va yuqori metrics ko‘rsatdi → dataset hajmi va maqsad sinfining soddaligi bu natijaga ta'sir qilgan.  

---

## 📌 Xulosa
- **Hajm va maqsad sinfi**: Dataset hajmi va maqsad sinfi model natijalariga bevosita ta’sir qiladi.  
- **Feature Engineering**: Startup yoshi, revenue/funding per employee, valuation-to-revenue ratio kabi features tree-based modellarda muhim.  
- **Model tanlovi**: Random Forest va Gradient Boosting har ikkala datasetda barqaror.  
- **Pipeline replikatsiyasi**: Bir xil preprocessing pipeline bilan ikki datasetni solishtirish, model natijalarining dataset strukturasiga bog‘liqligini ilmiy asosda ko‘rsatdi.

---

## 📝 Foydalanilgan resurslar
- [Global Startup Success Dataset – Kaggle](https://www.kaggle.com/datasets/hamnakaleemds/global-startup-success-dataset)  
- [Big Startup Success & Failure Dataset – Kaggle](https://www.kaggle.com/datasets/yanmaksi/big-startup-secsees-fail-dataset-from-crunchbase)  

