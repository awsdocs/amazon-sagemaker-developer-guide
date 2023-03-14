# Parameterize your notebook<a name="notebook-auto-run-troubleshoot-override"></a>

To pass new parameters or parameter overrides to your scheduled notebook job, you must make some modifications to your notebook\. When you pass a parameter, Studio uses the methodology enforced by Papermill\. Studio searches for a Jupyter cell tagged with the `parameters` tag and applies the new parameters or parameter overrides immediately after this cell\. If you don’t have any cells tagged with `parameters`, Studio applies the parameters at the beginning of the notebook\. If you have more than one cell tagged with `parameters`, Studio applies the parameters after the first cell tagged with `parameters`\.

To tag a cell in your notebook with the `parameters` tag, complete the following steps:

1. Select the cell to parameterize\.

1. Choose the **Property Inspector** icon \( ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/studio/icons/gears.png) \) in the right sidebar\.

1. Type **parameters** in the **Add Tag** box\.

1. Choose the **\+** sign\.

1. The `parameters` tag appears under **Cell Tags** with a check mark, which means the tag is applied to the cell\.