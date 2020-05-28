# Learn-python

Analytics vidya
-----------------
https://www.analyticsvidhya.com/blog/2018/05/24-ultimate-data-science-projects-to-boost-your-knowledge-and-skills/

Basic
---------
rename columns: df = df.rename(columns={"A": "a", "B": "c"})
Define the date format while changing date column type: df_salary['Paid_Date'] = pd.to_datetime(df_salary['Paid_Date'],format="%d/%m/%Y")
create another column with given date format: df_salary['paid_Month_Year'] = df_salary['Paid_Date'].dt.strftime('%m-%Y')

**Scatter plot**
#Visualise data points
plt.figure(figsize=(10,10))
plt.scatter(df_salary['EMP_NUMBER'],df_salary['PAY_NETPAY'] ,c='blue')
plt.title("Net salary distribution")
plt.xlabel('Emp No')
plt.ylabel('NET_SALARYY')
plt.show()

**box plot**
plt.figure(figsize=(10,10))
df_salary.boxplot(column=['PAY_NETPAY'])
plt.show()

**bar chart**
count_classes.plot(kind = 'bar')
plt.title("Transaction Class Distribution")
LABELS = ["Normal", "Fraud"]
plt.xticks(range(2), LABELS)
plt.xlabel("Class")
plt.ylabel("Frequency")

**Correlation matrix**
#Using Pearson Correlation

#print with correlation values
plt.figure(figsize=(50,50))
cor1 = data1.corr()
sns.set(font_scale=2)
sns.heatmap(cor, annot=True, cmap="RdYlGn")
plt.show()

#plot without correlation value
correlation_matrix = data.corr()
fig = plt.figure(figsize=(12,9))
sns.heatmap(correlation_matrix,vmax=0.8,square = True)
plt.show()
