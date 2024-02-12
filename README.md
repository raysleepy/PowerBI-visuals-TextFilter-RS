# Text Filter (RS) PowerBI Visual

# Why

Text Filter (RS) is based on Microsoft's Text Filter custom visual for Power BI.

Text Filter provides a search (really, a filtering) capability on a specific table.
OOTB, filtering can be targeted towards a specific column. If you need to target multiple columns,
you can create a column that combines (concatenate) values from multiple columns (fields).
It's rudimentary, but works.

In my case, I had a specific need to search across multiple tables. Initially, I thought I could
write some cute DAX expressions to accomplish what's needed, but soon realized that that's not possible.

The reason is actually quite simple. Power BI reports and visuals are meant to show things that are related.
What I wanted is to have ONE visual to act on TWO (or more) things that are not related.

Nonetheless I needed that functionality, hence this fork.

# How

The how part is not too bad once I understood what the original code was doing.
However, there is one issue cannot be resolved with the current Power BI visuals API.

It's really preferred to have the capability to drag-n-drop fileds onto the data section of the visual's setting.
Text Filter has a limit of 1 for the number of fields allowed, which again, reflects the design philosophy that
the visual should only target one column from one table.

This limit has been bumped up to 10, which should be sufficient for most use cases.
However, Power BI doesn't take it kindly when multiple fields from multiple (unrelated) tables are configured in the visual
setting and will balk at you with an error message complaining that it doesn't understand the relationship between the tables.

I experimented with adding an "always true" column (alwaysTrue = True()) on all tables in question and establishing
a many-to-many relationship between all the respective alwaysTrue columns. This hack almost works, except when search doesn't
yield any results in one table. i.e., null's many-to-many relationship is still null -- null neither equals nor not equals to null.

In the end, I made the somewhat painful compromise of changing the field type to measure.
This allows for adding multiple fields from multiple (unrelated) tables. The downside is that when you add a field
(say, SearchText from FooBar table), it will show "First SearchText". While you can easily rename it to something more sensible
(e.g., I recommend to rename it to FooBar.SearchText so it clearly shows the table and the column), it does introduce a bit of
confusion, especially for first time users. But, I feel it's a worthwhile compromise. The alternative would have been
something like reading a list of delimited key/val pairs from a string in a config setting.

Overall, I am pleased with the result. After all, I am forcing Power BI to do something it wasn't designed to do.

# Tutorial
In the samples dir, there is a sample PBIX file with both Text Filter and Text Filter (RS) configured side by side for comparisions.
A dozen or so screenshots are also included. They should be self-explanatory, if you have some experience with Power BI visuals.
BTW, you may wonder why those filters are formatted in different colors. Well, when you mix red and blue, you get purple.
(We are talking about Power BI visuals, not politics...)

# NB
As much as I'd like to think this visual provides a useful feature (for me at least), it's really something grafted onto Text Filter
in a way that the original author probably never envisioned or now plans. Hence, it's unlikely that it should ever be PR'd.

# Contributing
(Carried over from MS' Text Filter)

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
