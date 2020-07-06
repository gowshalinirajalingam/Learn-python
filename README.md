# Learn-python

Analytics vidya
-----------------
https://www.analyticsvidhya.com/blog/2018/05/24-ultimate-data-science-projects-to-boost-your-knowledge-and-skills/

Basic
---------
rename columns: df = df.rename(columns={"A": "a", "B": "c"})
Define the date format while changing date column type: df_salary['Paid_Date'] = pd.to_datetime(df_salary['Paid_Date'],format="%d/%m/%Y")
create another column with given date format: df_salary['paid_Month_Year'] = df_salary['Paid_Date'].dt.strftime('%m-%Y')

replace df column value pandas: df['value'].replace({'cluster':Cluster,'0:1'},inplace=True) #inplace =True means no need to assign the result to another dataframe
replace column name character:     df.columns = df.columns.str.replace(" ", "")

#make 1 st row as header
header = temp.iloc[0]
temp = temp.iloc[1::]
temp.columns = header

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

**Working with Excel**
print(pd.DataFrame(ws.values))      #convert excel sheet values to dataframe

cell.column                         # gives the cell column letter cell
cell.row                            # gives the cell row number
get_column_letter(cell.column - 1)      # get n -1th column's letter

  **Append dataframe to a excel workbook**
  from openpyxl.utils.dataframe import dataframe_to_rows
  wb = Workbook()
  ws = wb.active

  for r in dataframe_to_rows(df, index=True, header=True):
      ws.append(r)

--------------------
import pandas as pd
import numpy as np

file = "C:\\Users\\GowshaliniR\\Documents\\Brandix\\TASK5_understanding KPI\\KPI Docs\\"

import openpyxl
book = openpyxl.load_workbook(file+"KPI-SUMMARY APPAREL SECTOR-2019-12 V2.xlsm",data_only=True)      #data_only return only the cell value in the code 'a1.value'. Else it will return the cell formula
sheet_names = book.sheetnames

df = pd.DataFrame()


factories =["BCR","BCRD","BCG","BCS","BCI","BFL","BCW-SL","BCB","BCB Washing","BCW-BCB","BFFW","BFFMR1",
            "BFFMR2","BFFM","BFFAV1","BFFAV2","BBOS","SBU-FF","OSFF","Katu","Cambodia","DD_SUB",
            "DD","BEK","BEN","BERA","BEKA","BER","BEB","BRANDM","BELOS","BEL","BELBAI3",
            "SBUBAI1","SBUBAI2","Plant","Outsources - BLI","BLI","QI","Lingerie","BrandMKNIT","SEEDS"]

#factories =["BCR","BCRD","BCG","BCS"]

for f in factories:
    sheet = book[f]               #OR     sheet = book.get_sheet_by_name("BCRD") 
    dfFacMonthsKPI = pd.DataFrame()
    dfKPINames = pd.DataFrame()
    dfKPI_value = pd.DataFrame()
    KPIs =[sheet['D9':'O9'],sheet['D44':'O44'],sheet['D54':'O54'],sheet['D57':'O57'],sheet['D59':'O59'],sheet['D93':'O93'],sheet['D96':'O96']] 
    month_cells = sheet['D115': 'O115']     #get months of the year
    KPI_names = ['On-Time Delivery (OTD) %','Efficiancy% (Overall)','SAH(Acheieved/Projection %)','Lost Time(Hrs)','Cut Ship Ratio(With Trf Qty & Sample)','Labour Turnover','Absenteeism']   #get months of the year
    
#if iterating through same row and different column just use this function.
#        for c1 in cells[0]:  
#            print(c1.value)
#If there are rows and columns use this function 
#        for c1, c2 in cells:
#            print("{0:8} {1:8}".format(c1.value, c2.value))
    

    for k,kn in zip(KPIs,KPI_names):
        for month in month_cells[0]:  
            print(month.value)
#           print(pd.DataFrame({'Date':month.value},index=[0]))  #create dataframe with a column
            dfFacMonthsKPI = dfFacMonthsKPI.append(pd.DataFrame({'Factory':f,'Date':month.value,'KPI':kn},index=[0]),ignore_index=True) #append dataframe
        for cell in k[0]:  
            print(cell.value)
            dfKPI_value = dfKPI_value.append(pd.DataFrame({'Value':cell.value},index=[0]),ignore_index=True)
    df1 = pd.concat([dfFacMonthsKPI,dfKPI_value],sort=False,axis=1)
        
    df = df.append(df1)

df['Value'] = df['Value'].astype('float64')   #without converting to float can't round that column
df['Value'] = (round(df['Value']*100,2)).where((df['KPI']=='Efficiancy% (Overall)') | (df['KPI']=='Cut Ship Ratio(With Trf Qty & Sample)') | (df['KPI']=='Labour Turnover') | (df['KPI']=='Absenteeism')  ,df['Value'])
df['Value'] = (round(df['Value']*100,0)).where((df['KPI']=='SAH(Acheieved/Projection %)') | (df['KPI']=='On-Time Delivery (OTD) %') ,df['Value'])
df['Value'] = (round(df['Value'],0)).where( (df['KPI']== 'Lost Time(Hrs)') ,df['Value'])

df['Value'] = df['Value'].fillna('-')

#df.to_csv(file+"KPI-SUMMARY APPAREL SECTOR-2019-12 V2 Filtered.csv",index=False,header=True)
