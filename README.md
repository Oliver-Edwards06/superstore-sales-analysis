# Superstore Sales Analysis

This is a project I did looking at 4 years of order data from the Sample Superstore dataset. I wanted to actually dig into where the business is making money vs losing it, not just look at total sales, because sales going up doesn't actually mean profit is increasing.

## What this is about

Basically the question I kept coming back to was: is high sales actually a good thing here, or is it hiding some categories that are quietly losing money? It turns out its a bit of both. I also built a small classification model at the end to predict if an order will be profitable, mainly because I wanted to try it and see how far discount alone could get me.

## Dataset

Sample Superstore data, 9994 rows, 2014-2017. Has order details, customer segment, region/geography, product category and the financial columns (Sales, Quantity, Discount, Profit).

## What I did

Loaded everything into SQLite first and checked the dataset for any nulls, duplicates or dates that didn't make sense.

From there I did most of the actual analysis in SQL:
- profitability by category and sub-category
- how the regions compare
- ranking top products with window functions 
- monthly sales trends
- some basic customer segmentation by value

Then I went into the discount side of things as I wanted to know if discount level actually drives profitability or if that's too simple an explanation. And whether the sub-categories that lose money are just getting discounted harder than everyone else.

Last part was the model I used a Gradient Boosting Classifier to predict whether an order will be profitable or not. I had to be a bit careful with the evaluation here since the classes weren't balanced as there was a lot more profitable orders than loss-making ones, so I looked at precision/recall/F1 instead of just accuracy.

## Findings

- Tables, Bookcases and Supplies are the only sub-categories losing money outright. Tables and Bookcases are also the best sellers in Furniture, annoyingly, so more sales in these two = more losses, not less. Supplies sits inside Office Supplies which looks healthy overall, so its loss is easy to miss unless you break it down to sub-category level.
- Once discount goes past around 20%, margin turns negative, and it gets a lot worse past 40%.
- Discount is doing most of the work in the classification model as over 80% of what it relies on to make predictions.
- Central region has a weaker margin (7.92%) than the other regions even though sales volume is pretty similar to them.

Discount isn't the full story though as binders gets discounted more than Tables on average but stays profitable, so something else is going on with Tables/Bookcases specifically (probably cost/markup, which isn't in this dataset).

## Recommendations

- Cap discounts around 20% as a general rule
- Look into Tables, Bookcases and Supplies pricing specifically as is the loss intentional (clearance) or not
- Figure out why Central's margin is lagging the other regions

## Tools

Python (pandas, scikit-learn), SQL (SQLite), Jupyter Notebook

## Running it

1. clone the repo
2. `pip install pandas xlrd scikit-learn`
3. open `superstore_sales_analysis.ipynb` and run the cells top to bottom

Notebook has all the actual SQL queries and the model build in it if you want to see the working.