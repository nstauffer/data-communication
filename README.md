# Communicating Ecological Data With Land Managers: Lessons Learned
This poster summarizes the work and efforts to share ecological data by the national Assessment Inventory and Monitoring (AIM) team formed from a partnership between the Bureau of Land Management (BLM), the United States Department of Agriculture Agricultural Research Service (ARS), and the National Aquatic Monitoring Center (NAMC).

For more information, contact Nelson Stauffer at nelson.stauffer@ars.usda.gov.

## Guiding Principles
The guiding principles applied to any presentation of data are:
* **Clarity**: Figures should be parsable at a glance or with minimal explanatory text.
* **Scientific integrity**: Figures and tables must report values in ways that do not misrepresent data (e.g. include confidence intervals for statistics).
* **Flexibility**: Figures and tables should not be single-purpose unless necessary and should be reusable in multiple documents and decisions.
* **Empowerment**: All data should be useful but should not imply a decision or judgement (e.g. indicate that values are “bad”) and land manages should feel confident that they can explain them.

## Example Code
### Figure 1
This is the code used to create figure 1 for the poster, which is the general purpose, default style of figure used to display most analyzed AIM data. Normally for AIM, the data used are weighted areal estimates for indicator values binned into condition classes with confidence intervals. However for the sake of clarity in the poster, a dummy data set was used and is included in the code block below.

This code is not currently generalized into a function, and has multiple hardcoded values (variables, labels, scales, and colors) that need to be manually changed in order to use it for other purposes. Its only non-base R dependency is the [ggplot2 package](https://ggplot2.tidyverse.org/).
``` r
data_example <- data.frame(category = c("Unsuitable", "Marginal", "Suitable"),
                           value = c(21, 41, 38),
                           ci = c(3, 8, 4))

example_plot <- ggplot2::ggplot(data = data_example,
                                ggplot2::aes(x = category,
                                             y = value,
                                             color = category)) +
  # This adds the diamond at each value and the confidence interval bars extending from them
  ggplot2::geom_pointrange(ggplot2::aes(ymax = value + ci,
                                        ymin = value - ci),
                           size = 3.5,
                           shape = 18,
                           fatten = 3.5) +
  # This adds the end caps to the confidence intervals, which improves legibility
  ggplot2::geom_errorbar(ggplot2::aes(ymax = value + ci,
                                      ymin = value - ci),
                         width = 0.25,
                         size = 3.5) +
  # Ensuring that the values are ordered the intuitive way
  ggplot2::scale_x_discrete(limits = c("Unsuitable", "Marginal", "Suitable")) +
  # Setting colors manually, following the idea of "neutral" symbology
  ggplot2::scale_color_manual(values = c("Unsuitable" = "dodgerblue3", "Marginal" = "darkslategray4", "Suitable" = "goldenrod1")) +
  # The plots tend to be more readable when extending from left to right, rather than bottom to top
  ggplot2::coord_flip() +
  # This sets the scale to display the values comfortably.
  # Automating the upper limit selection is strongly recommended for batching figures
  # e.g. ggplot2::ylim(0, (1.05 * max(data_example[["value"]] + data_example[["ci"]])))
  ggplot2::ylim(0, 75) +
  ggplot2::labs(title = "Percentage of Reporting Unit by Category: \nAverage Sagebrush Height (cm, 80% conf.)",
                y = "Estimated Percent of Reporting Unit Area",
                x = "Category",
                fill = "Category") +
  # Mostly stripping out unnecessary elements (and removing the feeling of "default ggplot figure")
  ggplot2::theme(panel.background = ggplot2::element_rect(fill = "gray95"),
                 panel.grid = ggplot2::element_line(color = "gray90"),
                 panel.grid.major.y = ggplot2::element_blank(),
                 panel.grid.minor = ggplot2::element_blank(),
                 legend.position = "none",
                 axis.title = ggplot2::element_text(size = 25),
                 axis.text = ggplot2::element_text(size = 23),
                 title = ggplot2::element_text(size = 29),
                 aspect.ratio = 1/3)

```
