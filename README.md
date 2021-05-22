# The Power of Plots

## Background

What good is data without a good plot to tell the story?

So, let's take what you've learned about Python Matplotlib and apply it to a real-world situation and dataset:

![Laboratory](Images/Laboratory.jpg)

While your data companions rushed off to jobs in finance and government, you remained adamant that science was the way for you. Staying true to your mission, you've joined Pymaceuticals Inc., a burgeoning pharmaceutical company based out of San Diego. Pymaceuticals specializes in anti-cancer pharmaceuticals. In its most recent efforts, it began screening for potential treatments for squamous cell carcinoma (SCC), a commonly occurring form of skin cancer.

As a senior data analyst at the company, you've been given access to the complete data from their most recent animal study. In this study, 249 mice identified with SCC tumor growth were treated through a variety of drug regimens. Over the course of 45 days, tumor development was observed and measured. The purpose of this study was to compare the performance of Pymaceuticals' drug of interest, Capomulin, versus the other treatment regimens. You have been tasked by the executive team to generate all of the tables and figures needed for the technical report of the study. The executive team also has asked for a top-level summary of the study results.

## Instructions

Your tasks are to do the following:

* Before beginning the analysis, check the data for any mouse ID with duplicate time points and remove any data associated with that mouse ID.

* Use the cleaned data for the remaining steps.

* Generate a summary statistics table consisting of the mean, median, variance, standard deviation, and SEM of the tumor volume for each drug regimen.

* Generate a bar plot using both Pandas's `DataFrame.plot()` and Matplotlib's `pyplot` that shows  the number of total mice for each treatment regimen throughout the course of the study.

  * **NOTE:** These plots should look identical.

* Generate a pie plot using both Pandas's `DataFrame.plot()` and Matplotlib's `pyplot` that shows the distribution of female or male mice in the study.

  * **NOTE:** These plots should look identical.

* Calculate the final tumor volume of each mouse across four of the most promising treatment regimens: Capomulin, Ramicane, Infubinol, and Ceftamin. Calculate the quartiles and IQR and quantitatively determine if there are any potential outliers across all four treatment regimens.

* Using Matplotlib, generate a box and whisker plot of the final tumor volume for all four treatment regimens and highlight any potential outliers in the plot by changing their color and style.

  **Hint**: All four box plots should be within the same figure. Use this [Matplotlib documentation page](https://matplotlib.org/gallery/pyplots/boxplot_demo_pyplot.html#sphx-glr-gallery-pyplots-boxplot-demo-pyplot-py) for help with changing the style of the outliers.

* Select a mouse that was treated with Capomulin and generate a line plot of tumor volume vs. time point for that mouse.
```python
#generate a line plot of tumor volume vs. timepoint for a mouse treated with Capomulin
mouse_id = "b128"
capo_df = de_dup_df.loc[de_dup_df["Drug Regimen"] == "Capomulin", :]
b128_df = capo_df.loc[capo_df["Mouse ID"] == mouse_id, :]
x_axis = b128_df["Timepoint"]
y_axis = b128_df["Tumor Volume (mm3)"]
plt.plot(x_axis, y_axis)
plt.xlim((0, 46))
plt.ylim((37,46))
plt.title(f"Capomulin Treatment for mouse #{mouse_id}")
plt.xlabel("Time (days)")
plt.ylabel("Tumor Volume (mm3)")
plt.savefig("Analysis/TumorVolumeLine_pyplot.png")
plt.show()
```

![tumorvolumeb128](Analysis/TumorVolumeLine_pyplot.png)

* Generate a scatter plot of mouse weight versus average tumor volume for the Capomulin treatment regimen.
```python
# Generate a scatter plot of average tumor volume vs. mouse weight for the Capomulin regimen
capo_average_tumor_vol = capo_df.groupby(["Mouse ID"]).mean()
x_axis = capo_average_tumor_vol["Weight (g)"]
y_axis = capo_average_tumor_vol["Tumor Volume (mm3)"]
plt.scatter(x_axis, y_axis)
plt.xlim((13, 27))
plt.ylim((34, 47))
plt.title("Average Tumor Volume vs. Mouse Weight for the Capomulin Regimen")
plt.xlabel("Weight (g)")
plt.ylabel("Average Tumor Volume (mm3)")
plt.savefig("Analysis/TumorVolumeScatter_pyplot.png")
plt.show()
```

![tumorvolumesp](Analysis/TumorVolumeScatter_pyplot.png)

* Calculate the correlation coefficient and linear regression model between mouse weight and average tumor volume for the Capomulin treatment. Plot the linear regression model on top of the previous scatter plot.
```python
# Calculate the correlation coefficient and linear regression model 
# for mouse weight and average tumor volume for the Capomulin regimen
corr=st.pearsonr(x_axis,y_axis)[0]
print(f"The correlation between mouse weight and average tumor volume is {corr:.2f}")
(slope, intercept, rvalue, pvalue, stderr) = linregress(x_axis, y_axis)
print(f"Linear Regression: slope={slope:.2f}, y-intercept={intercept:.2f}, r-value={rvalue:.2f}, p-value={pvalue:.2f}, stderr={stderr:.2f}")
regress_values = x_axis * slope + intercept

plt.scatter(x_axis, y_axis)
plt.xlim((13, 27))
plt.ylim((34, 47))
plt.plot(x_axis, regress_values, "r-")
plt.title("Average Tumor Volume vs. Mouse Weight for the Capomulin Regimen")
plt.xlabel("Weight (g)")
plt.ylabel("Average Tumor Volume (mm3)")
plt.savefig("Analysis/TumorVolumeScatterRegress_pyplot.png")
plt.show()
```

![TumorVolumeScatterRegress_pyplot](Analysis/TumorVolumeScatterRegress_pyplot.png)