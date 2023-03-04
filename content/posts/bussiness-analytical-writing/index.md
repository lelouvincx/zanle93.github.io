---
title: "Business Analytical Writing: Thailand’s Growing Battle with Childhood Obesity: A Wake-Up Call"
author: "Van Le"
# date: '2021-12-28'
showToc: yes
TocOpen: no
draft: no
hidemeta: no
comments: no
disableHLJS: no
disableShare: no
hideSummary: no
searchHidden: yes
ShowReadingTime: yes
ShowBreadCrumbs: yes
ShowPostNavLinks: yes
categories: ["python"]
tags: []
series: []
cover:
  image: "00.jpg"
  responsiveImages: true
  # hidden: false
  relative: true
---

|                |                                                                                               |
| -------------- | --------------------------------------------------------------------------------------------- |
| Visualization  | Python (Matplotlib, Seaborn, Plotly) for graphs, charts, and world map, InDesign, Power Point |
| Data wrangling | Python, SQL, and Excel                                                                        |

> **Disclaimer**  
> This project is a personal initiative aimed at showcasing my critical thinking and analytical abilities using various programming languages and tools. This project is not intended to provide professional advice or endorse any particular viewpoint. The information contained in this project is for informational purposes only.

---

# Obesity rates have risen in every country around the world since 1975

The number of obese adults has seen a substantial increase globally between 1975 and 2016. In 1975, 4.3% of the world’s population aged 18 years and older, equivalent to 93 million adults, were considered obese.

By 2016, this rate had risen to 13%, which equates to 641 million adults. This is a 5.5-fold increase considering the world population increased from 4 billion to 7.5 billion between 1975 and 2016.

![](./01.jpg#center)

```python
import plotly.express as px
import matplotlib.pyplot as plt
df10 = pd.read_csv('obese_pop.csv')
fig = px.area(df10, x="year", y="obese_pop", color="country",color_discrete_sequence= ['#9970ab','#d9f0d3','#1b7837','blue'],
              labels={
                     "country": "Country Classification",
                     "year": "Year",
                     "obese_pop": "Number of Obese Adults"
                 },
                title="Obese population from 1975 to 2016")
fig.update_layout({'plot_bgcolor': 'rgba(0,0,0,0)', 'paper_bgcolor': 'rgba(0,0,0,0)'}, #this for transparent
    legend=dict(
        x=0.05,  # value must be between 0 to 1.
        y=0.9,   # value must be between 0 to 1.
        traceorder="normal",
        font=dict(
            family="sans-serif",
            size=12,
            color="black"
        ),
    )
)
fig.show()
```

# Disparity in Obesity Rates: Women Outnumber Men in Most Countries

Upon investigating countries with populations greater than 10 million, I discovered a pattern where a higher number of women are obese compared to men. This disparity is particularly pronounced in countries such as South Africa and Algeria.

![](./02.jpg#center)

```python
gender = pd.read_csv("obesity_gender02.csv")
gender.dropna()

countries = ['United States','Egypt','Turkey','Chile','Argentina','Mexico','Canada','Australia','United Kingdom',
'South Africa','Venezuela','Irag','Czechia','Spain','Iran','Poland','Russia','Algeria','Kazakhstan','Hungary','France','Italy','Morocco','Germany','Brazil','Colombia','Peru','Ukraine','Netherlands','Uzbekistan','Malaysia','Nigeria','Thailand','Sudan','China','Kenya','Tanzania','Sri Lanka','Pakistan','Indonesia','Philippines','South Korea',
'Mozambique','Congo','Uganda','Nepal' ,'Myanmar','India','Ethiopia','Japan','Afghanistan','Bangladesh','North Korea','Vietnam']

gender_2016 = gender[["country",'gender','pct_obese','total']][gender["year"] == 2016]
gender_2016 = gender_2016.loc[gender_2016["country"].isin(countries)]
gender_2016 = gender_2016.sort_values(by= 'total',ascending=False)

sns.set(style="ticks")
sns.set_style("darkgrid",{'axes.grid' : False})
g = sns.catplot(data=gender_2016,x="pct_obese", y="country" , hue="gender", s = 8, palette="PRGn", height=10, aspect = 0.8)
g.set(xlabel='Prevalence of Obesity')
g.set(ylabel= None)
plt.show()
```

# Rising Obesity Rates: Southeast Asia No Longer Immune to the Trend

As illustrated in the graph below, the obesity rates in all Southeast Asian countries, with the exception of Malaysia, are lower than the global average and fall on the lower end of the spectrum.

![](./03.png#center)

```python
df3 = pd.read_csv("country_obesity.csv")
country_16 = df3.loc[df3['year'] == 2016]
country_16 = country_16.dropna(axis=1)
country_16 = country_16.sort_values(by= 'pct_obese',ascending=True)
country_16['country_duplicate'] = country_16.loc[:,'country']
country_16 = country_16.set_index('code')

plt.figure(figsize=(12,5))
ax=sns.barplot(x = 'country',
            y = 'pct_obese',
            data = country_16,edgecolor='none')
ax.set(ylabel= None)
ax.set(xlabel = None)
ax.yaxis.set_label_position("right")
ax.xaxis.set_ticklabels ([]) #remove x axis ticks
ax.yaxis.tick_right()
for bar in ax.patches:
    bar.set_facecolor('#bfbfbf')

#highlight
pos01 = country_16.index.get_loc('VNM')
ax.patches[pos01].set_facecolor('#762a83')
#Annotate: https://mode.com/example-gallery/python_chart_annotations/
ax.axvline(x=0, linestyle='--', alpha=0.5,color='#762a83')
ax.text(x=1, y=40, s='Vietnam (2.1%)', alpha=0.7, color='#762a83')

pos02 = country_16.index.get_loc('KHM')
ax.patches[pos02].set_facecolor('#762a83')


pos03 = country_16.index.get_loc('LAO')
ax.patches[pos03].set_facecolor('#762a83')


pos04 = country_16.index.get_loc('MYS')
ax.patches[pos04].set_facecolor('#762a83')
ax.axvline(x=71, linestyle='--', alpha=0.5,color='#762a83')
ax.text(x=72, y=40, s='Malaysia (15.3%)', alpha=0.7, color='#762a83')

pos05 = country_16.index.get_loc('PHL')
ax.patches[pos05].set_facecolor('#762a83')

pos06 = country_16.index.get_loc('SGP')
ax.patches[pos06].set_facecolor('#762a83')

pos07 = country_16.index.get_loc('IDN')
ax.patches[pos07].set_facecolor('#762a83')

pos08 = country_16.index.get_loc('THA')
ax.patches[pos08].set_facecolor('#762a83')

pos09 = country_16.index.get_loc('MMR')
ax.patches[pos09].set_facecolor('#762a83')

pos09 = country_16.index.get_loc('OWID_WRL')
ax.patches[pos09].set_facecolor('#5ca964')

ax.axvline(x=63, linestyle='-', alpha=0.5, color ='#018837')
ax.text(x=43, y=30, s='World (13.2%)', alpha=0.7, color='#018837')

ax.legend()
#Show the plot
plt.tight_layout()
plt.ylabel('Prevalence of obese adults')
plt.title("SEA countries vs Others (2016)")
plt.show()

```

Despite this, obesity in Southeast Asian countries is growing at a rapid pace. Indonesia, Thailand, and Malaysia are among the frontrunners, with CAGR of 7.19%, 6.9%, and 6.2% respectively, outpacing the world’s CAGR of 2.77%.

![](./04.png#center)

```python
df3 = pd.read_csv("/Users/vanle/Documents/Obesity data/country_obesity.csv")
df4 = df3.loc[df3["code"].isin(['VNM','KHM','MMR','IDN','LAO','MYS','SGP','THA','PHL'])]

#fig = px.area(df6, x='year', y='pct_obese', color='country') #for area chart
fig = px.line(df4, x='year', y='pct_obese', color='country', title = 'Obesity growing rate in SEA countries',
             color_discrete_sequence= ['#9970ab','#5ca964','#9970ab','#5ca964','#9970ab','#9970ab','#9970ab','#5ca964','#9970ab'],
              labels={
                     "country": "Country",
                     "year": "Year",
                     "pct_obese": "Prevalence of obese adults"
                 },
                )
fig.add_annotation(x=2016, y=10,
            text="THA-CAGR: 6.9%",
            showarrow=False,
            yshift=10)
fig.add_annotation(x=2016, y=15.6,
            text="MYS-CAGR: 6.2% ",
            showarrow=False,
            yshift=10)
fig.add_annotation(x=2016, y=6.9,
            text="IDN-CAGR: 7.19%",
            showarrow=False,
            yshift=10)
#fig.update_layout(showlegend=False)
fig.update_layout(
#     {'plot_bgcolor': 'rgba(0,0,0,0)',
#     'paper_bgcolor': 'rgba(0,0,0,0)'},
    legend=dict(
        x=0.05,  # value must be between 0 to 1.
        y=0.9,   # value must be between 0 to 1.
        traceorder="normal",
        font=dict(
            family="sans-serif",
            size=12,
            color="black"
        ),
    )
)
fig.show()
```

Despite having a diverse population, Indonesia, Thailand, and Malaysia still have the largest number of obese adults.

![](./05.png#center)

```python
fig = px.line(tha, x="year", y="pct_obese", color = 'age',color_discrete_sequence= ['#9970ab','blue','#1b7837'], labels = {'year': 'Year','pct_obese' : 'prevalence of obesity', 'age': 'Age Group'},title='Obesity rate in Thailand for different age groups')
fig.update_layout(
    #{'plot_bgcolor': 'rgba(0,0,0,0)',
    #'paper_bgcolor': 'rgba(0,0,0,0)'},
    legend=dict(
        x=0.05,  # value must be between 0 to 1.
        y=0.9,   # value must be between 0 to 1.
        traceorder="normal",
        font=dict(
            family="sans-serif",
            size=12,
            color="black"
        ),
    )
)
fig.show()
```

# Thailand’s Future: The Oncoming Concern of Childhood Obesity

Thailand’s Alarming Trend: Growing Childhood Obesity Rates Continue to Raise Concerns.

![](./06.png#center)

```python
import plotly.express as px
from plotly.subplots import make_subplots

# data for px.sunburst1
data1 = dict(character=[ "Total GDP 2016", ' ',"Obesity"],
             parent=["", "Total GDP 2016",   "Total GDP 2016"],
             value=[519.6,513,6.6])

# extract data and structure FROM px.sunburst1
sb1 = px.sunburst(data1,
                  names='character',
                  parents='parent',
                  values='value',
                  branchvalues="total",
                  color_discrete_sequence = ['#9970ab','#d9f0d3','#5aae61']
                  )

# data for px.sunburst2
# data for px.sunburst2
data2 = dict(character=[ "Total GDP 2056", ' ',"Obesity"],
             parent=["", "Total GDP 2056", "Total GDP 2056"],
             value=[519.6,494.24,25.36])

# extract data and structure FROM px.sunburst2
sb2 = px.sunburst(data2,
                  names='character',
                  parents='parent',
                  values='value',
                  branchvalues="total",
                  color_discrete_sequence = ['#9970ab','#d9f0d3','#5aae61']
                  )

fig = make_subplots(rows=1, cols=2, specs=[
    [{"type": "sunburst"}, {"type": "sunburst"}]
])
fig.add_trace(sb1.data[0], row=1, col=1)
fig.add_trace(sb2.data[0], row=1, col=2)
fig.show()
```

# Obesity Results in Economic Losses Equivalent to 1.27% of Thailand’s GDP

The associated health complications and economic loss have been well documented.

According to Dr. Tanupon Wirunhakarun, President of the Bangkok Association of Regenerative Medicine and Obesity Education Promotion (BARSO) and Chief Executive Officer of BDMS Wellness Clinic said that obesity may cause economic losses accounted for more than 6.6 billion USD (200 billion baht) or 1.27% of the country’s GDP. If this problem is not solved, in next 40 years obesity could affect Thai economy up to 90 billion USD (2.7 trillion baht) or 4.88% of GDP, which is a huge economic loss to Thailand.

![](./07.png#center)

```python
sea_country = ['Vietnam','Cambodia','Myanmar','Laos','Philippines','Indonesia','Singapore','Thailand','Malaysia']
obese_pop = [1355023,368182,1952005,206979,3884363,11727354,283080,5379703,3266162]
data = pd.DataFrame(list(zip(sea_country, obese_pop)),
               columns =['country', 'obese_pop'])
fig = px.treemap(data,
                 path= [px.Constant("SEA"),'country'],
                 values='obese_pop',
                 color='obese_pop',
                 color_continuous_scale='PRGn',
                  )

fig.update_layout(title="SEA Obesity Popuplation Distribution",
                  width=1000, height=600
                  #, uniformtext=dict(minsize=8, mode='hide')
                 )

fig.show()
```

Direct costs for medical care are 1.6 billion USD (50 billion baht) and indirect costs are 5 billion USD (150 billion baht), which are the effects of premature death. Including reduced productivity of labor (Productivity losses; absenteeism and presenteeism) cost 99 USD (2,970 baht) per person.

# Main components for Obesity in Thailand: High unhealthy food consumption and insufficient active

Obesity is commonly caused by taking in more calories through food and drinks (dietary intake), while not using enough energy through physical activity and exercise (energy expenditure). This results in the body storing excessive energy as fat. I will examine two key factors, dietary intake and energy
expenditure.

To understand the root causes of high caloric intake and low energy expenditure in Thai children and youth, I initially researched their dietary habits and physical activity levels.

## A. High unhealthy food consumption by Thai children and adolescents

A study investigated the impact of food types on the weight status of preadolescents in high-obesity urban settings. Below is a summary of key findings.

![](./08.png#center)

My observation indicated that preadolescents who are obese have a higher consumption of processed foods and drinks as side dishes during their main meal compared to their non-obese counterparts. The main dish for obese preadolescents often consists of ready-to-eat foods, including convenience foods and Thai traditional fast foods sold in open markets. Additionally, they tend to have between- meal snacks that consist of Western or local fast foods, which they consume as a pre-dinner meal on their way home from school.

## B. Compared to Japan, Thai children have a lower level of physical activity

Sariya and I conducted an investigation into the physical activity of Thai children and youth to gain insights into their current level of physical activity and health. I compared the results of the Thailand Report Card for Physical Activity for Children and Youth (aged 6 to 17) with the Japan Report Card. The results showed that, compared to Japanese children and youth, Thai children and preadolescents have an insufficient level of physical activity, with low levels of moderate-rigorous physical activity, limited physical activity and a more sedentary lifestyle.

![](./09.png#center)

# The Triggers of Childhood Obesity in Thailand: Unhealthy Food Access, Adver- tisements, and Busy Parenting

After identifying the direct causes of high caloric intake and low energy expenditure in Thai children and youth, I conducted an analysis to uncover the root causes. This analysis revealed three critical root causes contributing to the issue.

![](./10.png#center)

## 1. Poor Food Choices Linked to High Availability of Unhealthy Food Options

The Thai fast-food industry can be classified into three distinct historical periods, each of which has a widespread distribution and a significant impact on the Thai eating culture. This has significantly increased the availability of unhealthy food options for Thai children.

![](./11.png#center)

## 2. F&B advertising on Thai TV is predominantly unhealthy

Results from a study reveal that for Free TV, around 997 food advertisements were identified, with on average 2.9 unhealthy food advertisements per hour per channel whereas with only 0.2 advertisements per channel-hour for healthy food.

![](./12.png#center)

```python
# Read food_ads DataFrame
food_ads = pd.read_csv('Food_ads.csv')
fig = px.sunburst(food_ads,
                  path=["F&B category", "F&B types"],
                  values='Rate of advertisements (ads per channel hour (n))',
                  title="Starbucks Store Count Distribution World Wide [Country, State, City]",
                  width=750, height=750)
fig.show()
```

## 3. Busy parents have little time for their children

Thailand has the 10th longest annual working hours globally, with an average of 2185.45 hours. Due to busy parents who cannot spend enough time with their children, there is a possibility that the kids may lead a low-quality lifestyle, including consuming ready-to-eat food and having limited physical activity.

![](./13.jpg#center)

# Opportunities for New Business Ventures and Foreign Players Arise from the Lack of Solution Providers

In Thailand, the number of providers addressing the root causes of high childhood obesity rates is limited. Devers, one of the few Food Tech startups in the country, offers an easy solution for busy parents by providing healthy food options that are readily accessible.
Established in Bangkok in 2020, Devers is a full-service food delivery startup that has received support and development from the National Innovation Agency under the Ministry of Science and Technology, aimed at tackling the obesity crisis in Thailand.
This opens up opportunities for foreign players or new business ventures to address the underlying causes of the issue.

---

**Data**: World Bank, WHO The Global Health Observatory, NCD Data Portal

**Source**:

- The Thailand Report Card Survey 2022
- The 2022 Japan Report Card on Physical Activity for Children and Youth
- Jaichuen, N., Vandevijvere, S., Kelly, B. et al. Unhealthy food and non-alcoholic beverage advertising on children’s, youth and family free-to-air and digital television pro grammes in Thailand. BMC Public Health 18, 737 (2018).
- Boonchoo W, Takemi Y, Hayashi F, Koiwai K, Ogata H. Dietary intake and weight status of urban Thai preadolescents in the context of food environment. Prev Med Rep. 2017 Sep 28;8:153-157. doi: 10.1016/j.pmedr.2017.09.009.
- Chavasit, Visith & Kriengsinyos, Wantanee & Tangsuphoom, Nattapol & Photi, Juntima. (2014). Fast foods in transition and nutrition problems in Thailand.

**Credit**: Sariya Achawananthakul for local research
