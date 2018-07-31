


## This changes the folder structure so I an see that it worked but figured I would share this in case anyone was interested.
## Tabular Editor - Advanced Script for creating Time Calcs based on Data Scenario Measure v0.1-alpha 

    foreach(var measure in Selected.Measures)
    {
        var measureName = measure.Name;
        var measureNameLength = measure.Name.IndexOf(" for Data Scenario", StringComparison.InvariantCultureIgnoreCase);
        var measureFullName = measure.DaxObjectFullName;

        if (measureNameLength > 0)
        {
            var timeCalcExpression = @"VAR SelectedTimeCalculationKey = 
        SELECTEDVALUE(a 'Time Calculation'[TimeCalculationKey], 1)
    RETURN " + Environment.NewLine;
            timeCalcExpression += "        1, [" + measureName + "]," + Environment.NewLine;

            measureName = measure.Name.Left(measureNameLength);
            var timeCalcMeasure = Model.DefaultMeasure;
            // LY
            var name = measureName + " LY";
            var displayFolder = "Measures\\Time Calculation\\Selection\\LY";
            var expression = string.Format(@"CALCULATE(
        {0},
        ALL ( 'Date' ),
        TREATAS(
            SELECTCOLUMNS(
                'Date',
                ""Full Date LY"", LOOKUPVALUE(
                    'Date'[Full Date],
                    'Date'[Fiscal Day Of Year Number], 'Date'[Fiscal Day of Year Number],
                    'Date'[Fiscal Year Number], 'Date'[Fiscal Year Number] - 1
                )
            ), 
            'Date'[Full Date]
        )
    )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        2, [" + name + "]," + Environment.NewLine;



            // LY Variance
            name = measureName + " v LY";
            displayFolder = "Measures\\Time Calculation\\Selection\\LY Variance";
            expression = string.Format(@"VAR TY = [{0} for Data Scenario]
    VAR LY = [{0} LY]
    RETURN
        IF ( ISBLANK ( TY ) || ISBLANK ( LY ), BLANK (), TY - LY )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        3, [" + name + "]," + Environment.NewLine;


            // LY Variance Pct
            name = measureName + " v LY Pct";
            displayFolder = "Measures\\Time Calculation\\Selection\\LY Variance Percent";
            expression = string.Format(@"IF (
        NOT ISBLANK ( [{0} v LY] ),
        DIVIDE ( [{0} v LY], [{0} LY], BLANK () )
    )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        4, [" + name + "]," + Environment.NewLine;


            // WTD
            name = measureName + " WTD";
            displayFolder = "Measures\\Time Calculation\\WTD";
            expression = string.Format(@"VAR FiscalWeekNumber =
        SELECTEDVALUE ( 'Date'[Fiscal Week Number] )
    VAR FiscalDayOfWeekNumber =
        MAX ( 'Date'[Fiscal Day Of Week Number] )
    RETURN
        CALCULATE (
            {0},
            ALL ( 'Date' ),
            'Date'[Fiscal Week Number] = FiscalWeekNumber,
            'Date'[Fiscal Day Of Week Number] <= FiscalDayOfWeekNumber
        )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        5, [" + name + "]," + Environment.NewLine;


            // WTD LY
            name = measureName + " WTD LY";
            displayFolder = "Measures\\Time Calculation\\WTD\\LY";
             expression = string.Format(@"VAR FiscalWeekNumber =
        FORMAT( VALUE( SELECTEDVALUE ( 'Date'[Fiscal Week Number] ) ) - 100, ""0"" )
    VAR FiscalDayOfWeekNumber =
        MAX ( 'Date'[Fiscal Day Of Week Number] )
    RETURN
        CALCULATE (
            {0},
            ALL ( 'Date' ),
            'Date'[Fiscal Week Number] = FiscalWeekNumber,
            'Date'[Fiscal Day Of Week Number] <= FiscalDayOfWeekNumber
        )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        6, [" + name + "]," + Environment.NewLine;


            // WTD LY Variance
            name = measureName + " WTD v LY";
            displayFolder = "Measures\\Time Calculation\\WTD\\LY Variance";
            expression = string.Format(@"VAR TY = [{0} WTD]
    VAR LY = [{0} WTD LY]
    RETURN
        IF ( ISBLANK ( TY ) || ISBLANK ( LY ), BLANK (), TY - LY )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        7, [" + name + "]," + Environment.NewLine;


            // WTD LY Variance Pct
            name = measureName + " WTD v LY Pct";
            displayFolder = "Measures\\Time Calculation\\WTD\\LY Variance Percent";
            expression = string.Format(@"IF (
        NOT ISBLANK ( [{0} WTD v LY] ),
        DIVIDE ( [{0} WTD v LY], [{0} WTD LY], BLANK () )
    )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        8, [" + name + "]," + Environment.NewLine;


            // PTD
            name = measureName + " PTD";
            displayFolder = "Measures\\Time Calculation\\PTD";
             expression = string.Format(@"VAR FiscalPeriodNumber =
        SELECTEDVALUE ( 'Date'[Fiscal Period Number] )
    VAR FiscalDayOfPeriodNumber =
        MAX ( 'Date'[Fiscal Day Of Period Number] )
    RETURN
        CALCULATE (
            {0},
            ALL ( 'Date' ),
            'Date'[Fiscal Period Number] = FiscalPeriodNumber,
            'Date'[Fiscal Day Of Period Number] <= FiscalDayOfPeriodNumber
        )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        9, [" + name + "]," + Environment.NewLine;


            // PTD LY
            name = measureName + " PTD LY";
            displayFolder = "Measures\\Time Calculation\\PTD\\LY";
             expression = string.Format(@"VAR FiscalPeriodNumber =
        FORMAT( VALUE( SELECTEDVALUE ( 'Date'[Fiscal Period Number] ) ) - 100, ""0"" )
    VAR FiscalDayOfPeriodNumber =
        MAX ( 'Date'[Fiscal Day Of Period Number] )
    RETURN
        CALCULATE (
            {0},
            ALL ( 'Date' ),
            'Date'[Fiscal Period Number] = FiscalPeriodNumber,
            'Date'[Fiscal Day Of Period Number] <= FiscalDayOfPeriodNumber
        )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        10, [" + name + "]," + Environment.NewLine;


            // PTD LY Variance
            name = measureName + " PTD v LY";
            displayFolder = "Measures\\Time Calculation\\PTD\\LY Variance";
            expression = string.Format(@"VAR TY = [{0} PTD]
    VAR LY = [{0} PTD LY]
    RETURN
        IF ( ISBLANK ( TY ) || ISBLANK ( LY ), BLANK (), TY - LY )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        11, [" + name + "]," + Environment.NewLine;


            // PTD LY Variance Pct
            name = measureName + " PTD v LY Pct";
            displayFolder = "Measures\\Time Calculation\\PTD\\LY Variance Percent";
            expression = string.Format(@"IF (
        NOT ISBLANK ( [{0} PTD v LY] ),
        DIVIDE ( [{0} PTD v LY], [{0} PTD LY], BLANK () )
    )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        12, [" + name + "]," + Environment.NewLine;


            // QTD
            name = measureName + " QTD";
            displayFolder = "Measures\\Time Calculation\\QTD";
             expression = string.Format(@"VAR FiscalQuarterNumber =
        SELECTEDVALUE ( 'Date'[Fiscal Quarter Number] )
    VAR FiscalDayOfQuarterNumber =
        MAX ( 'Date'[Fiscal Day Of Quarter Number] )
    RETURN
        CALCULATE (
            {0},
            ALL ( 'Date' ),
            'Date'[Fiscal Quarter Number] = FiscalQuarterNumber,
            'Date'[Fiscal Day Of Quarter Number] <= FiscalDayOfQuarterNumber
        )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        13, [" + name + "]," + Environment.NewLine;


            // QTD LY
            name = measureName + " QTD LY";
            displayFolder = "Measures\\Time Calculation\\QTD\\LY";
             expression = string.Format(@"VAR FiscalQuarterNumber =
        SELECTEDVALUE ( 'Date'[Fiscal Quarter Number] ) - 10
    VAR FiscalDayOfQuarterNumber =
        MAX ( 'Date'[Fiscal Day Of Quarter Number] )
    RETURN
        CALCULATE (
            {0},
            ALL ( 'Date' ),
            'Date'[Fiscal Quarter Number] = FiscalQuarterNumber,
            'Date'[Fiscal Day Of Quarter Number] <= FiscalDayOfQuarterNumber
        )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        14, [" + name + "]," + Environment.NewLine;


            // QTD LY Variance
            name = measureName + " QTD v LY";
            displayFolder = "Measures\\Time Calculation\\QTD\\LY Variance";
            expression = string.Format(@"VAR TY = [{0} QTD]
    VAR LY = [{0} QTD LY]
    RETURN
        IF ( ISBLANK ( TY ) || ISBLANK ( LY ), BLANK (), TY - LY )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        15, [" + name + "]," + Environment.NewLine;


            // QTD LY Variance Pct
            name = measureName + " QTD v LY Pct";
            displayFolder = "Measures\\Time Calculation\\QTD\\LY Variance Percent";
            expression = string.Format(@"IF (
        NOT ISBLANK ( [{0} QTD v LY] ),
        DIVIDE ( [{0} QTD v LY], [{0} QTD LY], BLANK () )
    )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        16, [" + name + "]," + Environment.NewLine;


            // YTD
            name = measureName + " YTD";
            displayFolder = "Measures\\Time Calculation\\YTD";
             expression = string.Format(@"VAR FiscalYearNumber =
        SELECTEDVALUE ( 'Date'[Fiscal Year Number] )
    VAR FiscalDayOfYearNumber =
        MAX ( 'Date'[Fiscal Day Of Year Number] )
    RETURN
        CALCULATE (
            {0},
            ALL ( 'Date' ),
            'Date'[Fiscal Year Number] = FiscalYearNumber,
            'Date'[Fiscal Day Of Year Number] <= FiscalDayOfYearNumber
        )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        17, [" + name + "]," + Environment.NewLine;


            // YTD LY
            name = measureName + " YTD LY";
            displayFolder = "Measures\\Time Calculation\\YTD\\LY";
             expression = string.Format(@"VAR FiscalYearNumber =
        SELECTEDVALUE ( 'Date'[Fiscal Year Number] ) - 1
    VAR FiscalDayOfYearNumber =
        MAX ( 'Date'[Fiscal Day Of Year Number] )
    RETURN
        CALCULATE (
            {0},
            ALL ( 'Date' ),
            'Date'[Fiscal Year Number] = FiscalYearNumber,
            'Date'[Fiscal Day Of Year Number] <= FiscalDayOfYearNumber
        )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        18, [" + name + "]," + Environment.NewLine;


            // YTD LY Variance
            name = measureName + " YTD v LY";
            displayFolder = "Measures\\Time Calculation\\YTD\\LY Variance";
            expression = string.Format(@"VAR TY = [{0} YTD]
    VAR LY = [{0} YTD LY]
    RETURN
        IF ( ISBLANK ( TY ) || ISBLANK ( LY ), BLANK (), TY - LY )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        19, [" + name + "]," + Environment.NewLine;


            // YTD LY Variance Pct
            name = measureName + " YTD v LY Pct";
            displayFolder = "Measures\\Time Calculation\\YTD\\LY Variance Percent";
            expression = string.Format(@"IF (
        NOT ISBLANK ( [{0} YTD v LY] ),
        DIVIDE ( [{0} YTD v LY], [{0} YTD LY], BLANK () )
    )", measureFullName);

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = expression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);
            timeCalcExpression += "        20, [" + name + "]," + Environment.NewLine;


        // Time Calculation
            name = measureName + " for Time Calc";
            displayFolder = "Measures\\Time Calculation";
            timeCalcExpression = timeCalcExpression.Substring(0, timeCalcExpression.Length - 3) + Environment.NewLine + "    )";

            if ( Model.AllMeasures.Any(m => m.Name == name) )
            {
                timeCalcMeasure = Model.AllMeasures.First(m => m.Name == name);
                timeCalcMeasure.Expression = timeCalcExpression;
                timeCalcMeasure.DisplayFolder = displayFolder;
            }
            else
                measure.Table.AddMeasure(name, expression, displayFolder);

        }
    }
