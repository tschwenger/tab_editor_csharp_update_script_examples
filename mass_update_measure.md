# Code for mass update to a measure within the cube

The following is c# code that will update the SSAS model for a large number of measures. 

Use case: performance issues. This is code update looks for the following strings in the model and replaces them with an integer. The theory is to speed up the query engine. 

      // This script will fix measures to use new standard of TimeCalculationKey instead of the Time Calcualtion Name
      // IMPORTANT:
      // After running this script paste the following into the filter of Tabular Editor and assign the correct "Selection" measure 
      // ":Name.EndsWith("for Time Calc") && Expression.Contains("BLANK()")"
      ///////////////////////////////////////////////////////////////////////////////////////////////////////////////
      foreach( var measure in Model.AllMeasures.Where(m => m.Name.EndsWith("for Time Calc")) ) 
      {
          var mxName = measure.Name.Left(measure.Name.IndexOf(" for Time Calc"));
          var expression = "VAR SelectedTimeCalculationKey = " + Environment.NewLine;
          expression += "    SELECTEDVALUE( 'Time Calculation'[TimeCalculationKey], 1)" + Environment.NewLine;
          expression += "RETURN // " + mxName + Environment.NewLine;
          expression += "    SWITCH ( SelectedTimeCalculationKey," + Environment.NewLine;
          if (Model.AllMeasures.Any(m => m.Name == mxName + " for Data Scenario"))
          {
              expression += "        1, [" + mxName + " for Data Scenario]," + Environment.NewLine;
          }
          else if (Model.AllMeasures.Any(m => m.Name == mxName + " for Company Rollup"))
          {
              expression += "        1, [" + mxName + " for Company Rollup]," + Environment.NewLine;
          }
          else 
          {
              expression += "        1, BLANK()," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " LY"))
          {
              expression += "        2, [" + mxName + " LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " v LY"))
          {
              expression += "        3, [" + mxName + " v LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " v LY Pct"))
          {
              expression += "        4, [" + mxName + " v LY Pct]," + Environment.NewLine;
          }
          else if (Model.AllMeasures.Any(m => m.Name == mxName + " v LY %"))
          {
              expression += "        4, [" + mxName + " v LY %]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " WTD"))
          {
              expression += "        5, [" + mxName + " WTD]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " WTD LY"))
          {
              expression += "        6, [" + mxName + " WTD LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " WTD v LY"))
          {
              expression += "        7, [" + mxName + " WTD v LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " WTD v LY Pct"))
          {
              expression += "        8, [" + mxName + " WTD v LY Pct]," + Environment.NewLine;
          }
          else if (Model.AllMeasures.Any(m => m.Name == mxName + " WTD v LY %"))
          {
              expression += "        8, [" + mxName + " WTD v LY %]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " PTD"))
          {
              expression += "        9, [" + mxName + " PTD]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " PTD LY"))
          {
              expression += "        10, [" + mxName + " PTD LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " PTD v LY"))
          {
              expression += "        11, [" + mxName + " PTD v LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " PTD v LY Pct"))
          {
              expression += "        12, [" + mxName + " PTD v LY Pct]," + Environment.NewLine;
          }
          else if (Model.AllMeasures.Any(m => m.Name == mxName + " PTD v LY %"))
          {
              expression += "        12, [" + mxName + " PTD v LY %]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " QTD"))
          {
              expression += "        13, [" + mxName + " QTD]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " QTD LY"))
          {
              expression += "        14, [" + mxName + " QTD LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " QTD v LY"))
          {
              expression += "        15, [" + mxName + " QTD v LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " QTD v LY Pct"))
          {
              expression += "        16, [" + mxName + " QTD v LY Pct]," + Environment.NewLine;
          }
          else if (Model.AllMeasures.Any(m => m.Name == mxName + " QTD v LY %"))
          {
              expression += "        16, [" + mxName + " QTD v LY %]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " YTD"))
          {
              expression += "        17, [" + mxName + " YTD]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " YTD LY"))
          {
              expression += "        18, [" + mxName + " YTD LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " YTD v LY"))
          {
              expression += "        19, [" + mxName + " YTD v LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " YTD v LY Pct"))
          {
              expression += "        20, [" + mxName + " YTD v LY Pct]," + Environment.NewLine;
          }
          else if (Model.AllMeasures.Any(m => m.Name == mxName + " YTD v LY %"))
          {
              expression += "        20, [" + mxName + " YTD v LY %]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " Rolling 12 Periods"))
          {
              expression += "        21, [" + mxName + " Rolling 12 Periods]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " Rolling 12 Periods LY"))
          {
              expression += "        22, [" + mxName + " Rolling 12 Periods LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " Rolling 12 Periods v LY"))
          {
              expression += "        23, [" + mxName + " Rolling 12 Periods v LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " Rolling 12 Periods v LY Pct"))
          {
              expression += "        24, [" + mxName + " Rolling 12 Periods v LY Pct]," + Environment.NewLine;
          }
          else if (Model.AllMeasures.Any(m => m.Name == mxName + " Rolling 12 Periods v LY %"))
          {
              expression += "        24, [" + mxName + " Rolling 12 Periods v LY %]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " Rolling 13 Weeks"))
          {
              expression += "        25, [" + mxName + " Rolling 13 Weeks]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " Rolling 13 Weeks LY"))
          {
              expression += "        26, [" + mxName + " Rolling 13 Weeks LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " Rolling 13 Weeks v LY"))
          {
              expression += "        27, [" + mxName + " Rolling 13 Weeks v LY]," + Environment.NewLine;
          }

          if (Model.AllMeasures.Any(m => m.Name == mxName + " Rolling 13 Weeks v LY Pct"))
          {
              expression += "        28, [" + mxName + " Rolling 13 Weeks v LY Pct]," + Environment.NewLine;
          }
          else if (Model.AllMeasures.Any(m => m.Name == mxName + " Rolling 13 Weeks v LY %"))
          {
              expression += "        28, [" + mxName + " Rolling 13 Weeks v LY %]," + Environment.NewLine;
          }
          expression = expression.Substring(0, expression.Length - 3) + Environment.NewLine + "    )";
          measure.Expression = expression;
      }
