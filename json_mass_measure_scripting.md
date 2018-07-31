# Json scripting for addition of measures to a cube

## note the code is in json and not c#

* this the developer can just copy and replace with the name of the measure
* note, developer still needs to create and code the base measure and everything else will cascade down
* also note, the ytd, ptd, and qtd work for a non contiguous date table, note that it is better practice to use a contiguous date table
so that standard time intelligence functions can be used (ie totalytd, sameperiodaslastyear, datesytd etc)

            ,
                      {
                        "name": "xMeasure for Company Rollup",
                        "expression": "",
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Utility Measures\\Actual Measures"
                      },
                      {
                        "name": "xMeasure Plan for Company Rollup",
                        "expression": "",
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Utility Measures\\Plan Measures"
                      },
                      {
                        "name": "xMeasure Plan Var",
                        "expression": [
                          "VAR Actual = [xMeasure for Company Rollup]",
                          "VAR Plan = [xMeasure Plan for Company Rollup]",
                          "RETURN",
                          "    IF ( ISBLANK ( Actual ) || ISBLANK ( Plan ), BLANK (), Actual - Plan )"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Data Scenario Measures\\Variance Measures"
                      },
                      {
                        "name": "xMeasure Plan Var Pct",
                        "expression": [
                          "IF (",
                          "    ISBLANK ( [xMeasure Plan Var] ),",
                          "    BLANK (),",
                          "    DIVIDE ( [xMeasure Plan Var], [xMeasure Plan for Company Rollup], BLANK () )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Data Scenario Measures\\Variance Measures"
                      },
                      {
                        "name": "xMeasure for Data Scenario",
                        "expression": [
                          "VAR SelectedDataScenarioKey = ",
                          "    SELECTEDVALUE( 'Data Scenario'[DataScenarioKey], 1)",
                          "RETURN",
                          "    SWITCH ( SelectedDataScenarioKey,",
                          "        1, [xMeasure for Company Rollup],",
                          "        2, [xMeasure Plan for Company Rollup],",
                          "        3, [xMeasure Plan Var],",
                          "        4, [xMeasure Plan Var Pct]",
                          "    )"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Data Scenario Measures"
                      },
                      {
                        "name": "xMeasure LY",
                        "expression": [
                          "VAR FiscalYear =",
                          "    SELECTEDVALUE ( 'Date'[Fiscal Year Number] )",
                          "VAR DayOfYear =",
                          "    SELECTEDVALUE ( 'Date'[Fiscal Day Of Year Number] )",
                          "VAR WeekOfYear =",
                          "    SELECTEDVALUE ( 'Date'[Fiscal Week Of Year Number] )",
                          "VAR PeriodOfYear =",
                          "    SELECTEDVALUE ( 'Date'[Fiscal Period Of Year Number] )",
                          "VAR QuarterOfYear =",
                          "    SELECTEDVALUE ( 'Date'[Fiscal Quarter Of Year Number] )",
                          "RETURN",
                          "    SWITCH (",
                          "        TRUE (),",
                          "        NOT ISBLANK ( DayOfYear ), CALCULATE (",
                          "            [FTE Sum for Data Scenario],",
                          "            FILTER (",
                          "                ALL ( 'Date' ),",
                          "                AND (",
                          "                    'Date'[Fiscal Year Number]",
                          "                        = FiscalYear - 1,",
                          "                    'Date'[Fiscal Day Of Year Number] = DayOfYear",
                          "                )",
                          "            )",
                          "        ),",
                          "        NOT ISBLANK ( WeekOfYear ), CALCULATE (",
                          "            [FTE Sum for Data Scenario],",
                          "            FILTER (",
                          "                ALL ( 'Date' ),",
                          "                AND (",
                          "                    'Date'[Fiscal Year Number]",
                          "                        = FiscalYear - 1,",
                          "                    'Date'[Fiscal Week Of Year Number] = WeekOfYear",
                          "                )",
                          "            )",
                          "        ),",
                          "        NOT ISBLANK ( PeriodOfYear ), CALCULATE (",
                          "            [FTE Sum for Data Scenario],",
                          "            FILTER (",
                          "                ALL ( 'Date' ),",
                          "                AND (",
                          "                    'Date'[Fiscal Year Number]",
                          "                        = FiscalYear - 1,",
                          "                    'Date'[Fiscal Period Of Year Number] = PeriodOfYear",
                          "                )",
                          "            )",
                          "        ),",
                          "        NOT ISBLANK ( QuarterOfYear ), CALCULATE (",
                          "            [FTE Sum for Data Scenario],",
                          "            FILTER (",
                          "                ALL ( 'Date' ),",
                          "                AND (",
                          "                    'Date'[Fiscal Year Number]",
                          "                        = FiscalYear - 1,",
                          "                    'Date'[Fiscal Quarter Of Year Number] = QuarterOfYear",
                          "                )",
                          "            )",
                          "        ),",
                          "        NOT ISBLANK ( FiscalYear ), CALCULATE (",
                          "            [FTE Sum for Data Scenario],",
                          "            FILTER ( ALL ( 'Date' ), 'Date'[Fiscal Year Number] = FiscalYear - 1 )",
                          "        )",
                          "    )"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Selection Measures"
                      },
                      {
                        "name": "xMeasure v LY",
                        "expression": [
                          "VAR TY = [xMeasure for Data Scenario]",
                          "VAR LY = [xMeasure LY]",
                          "RETURN",
                          "    IF ( ISBLANK ( TY ) || ISBLANK ( LY ), BLANK (), TY - LY )"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Selection Measures"
                      },
                      {
                        "name": "xMeasure v LY Pct",
                        "expression": [
                          "IF (",
                          "    ISBLANK ( [xMeasure v LY] ),",
                          "    BLANK (),",
                          "    DIVIDE ( [xMeasure v LY], [xMeasure LY], BLANK () )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Selection Measures"
                      },
                      {
                        "name": "xMeasure WTD",
                        "expression": [
                          "CALCULATE (",
                          "    [xMeasure for Data Scenario],",
                          "    FILTER (",
                          "        ALL ( 'Date' ),",
                          "        AND (",
                          "            'Date'[Fiscal Week Sequence Number]",
                          "                = SELECTEDVALUE ( 'Date'[Fiscal Week Sequence Number] ),",
                          "            'Date'[Fiscal Day Of Week Number] <= MAX ( 'Date'[Fiscal Day Of Week Number] )",
                          "        )",
                          "    )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Week Measures"
                      },
                      {
                        "name": "xMeasure WTD LY",
                        "expression": [
                          "CALCULATE (",
                          "    [xMeasure for Data Scenario],",
                          "    FILTER (",
                          "        ALL ( 'Date' ),",
                          "        'Date'[Fiscal Year Number]",
                          "            = SELECTEDVALUE ( 'Date'[Fiscal Year Number] ) - 1",
                          "            && 'Date'[Fiscal Week Of Year Number]",
                          "                = SELECTEDVALUE ( 'Date'[Fiscal Week Of Year Number] )",
                          "            && 'Date'[Fiscal Day Of Week Number]",
                          "                <= MAX ( 'Date'[Fiscal Day Of Week Number] )",
                          "    )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Week Measures"
                      },
                      {
                        "name": "xMeasure WTD v LY",
                        "expression": [
                          "VAR TY = [xMeasure WTD]",
                          "VAR LY = [xMeasure WTD LY]",
                          "RETURN",
                          "    IF ( ISBLANK ( TY ) || ISBLANK ( LY ), BLANK (), TY - LY )"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Week Measures"
                      },
                      {
                        "name": "xMeasure WTD v LY Pct",
                        "expression": [
                          "IF (",
                          "    ISBLANK ( [xMeasure WTD v LY] ),",
                          "    BLANK (),",
                          "    DIVIDE ( [xMeasure WTD v LY], [xMeasure WTD LY], BLANK () )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Week Measures"
                      },
                      {
                        "name": "xMeasure PTD",
                        "expression": [
                          "CALCULATE (",
                          "    [xMeasure for Data Scenario],",
                          "    FILTER (",
                          "        ALL ( 'Date' ),",
                          "        AND (",
                          "            'Date'[Fiscal Period Sequence Number]",
                          "                = SELECTEDVALUE ( 'Date'[Fiscal Period Sequence Number] ),",
                          "            'Date'[Fiscal Week Sequence Number]",
                          "                <= MAX ( 'Date'[Fiscal Week Sequence Number] )",
                          "        )",
                          "    )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Period Measures"
                      },
                      {
                        "name": "xMeasure PTD LY",
                        "expression": [
                          "CALCULATE (",
                          "    [xMeasure for Data Scenario],",
                          "    FILTER (",
                          "        ALL ( 'Date' ),",
                          "        'Date'[Fiscal Year Number]",
                          "            = SELECTEDVALUE ( 'Date'[Fiscal Year Number] ) - 1",
                          "            && 'Date'[Fiscal Period Of Year Number]",
                          "                = SELECTEDVALUE ( 'Date'[Fiscal Period Of Year Number] )",
                          "            && 'Date'[Fiscal Day Of Period Number]",
                          "                <= MAX ( 'Date'[Fiscal Day Of Period Number] )",
                          "    )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Period Measures"
                      },
                      {
                        "name": "xMeasure PTD v LY",
                        "expression": [
                          "VAR TY = [xMeasure PTD]",
                          "VAR LY = [xMeasure PTD LY]",
                          "RETURN",
                          "    IF ( ISBLANK ( TY ) || ISBLANK ( LY ), BLANK (), TY - LY )"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Period Measures"
                      },
                      {
                        "name": "xMeasure PTD v LY Pct",
                        "expression": [
                          "IF (",
                          "    ISBLANK ( [xMeasure PTD v LY] ),",
                          "    BLANK (),",
                          "    DIVIDE ( [xMeasure PTD v LY], [xMeasure PTD LY], BLANK () )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Period Measures"
                      },
                      {
                        "name": "xMeasure QTD",
                        "expression": [
                          "CALCULATE (",
                          "    [xMeasure for Data Scenario],",
                          "    FILTER (",
                          "        ALL ( 'Date' ),",
                          "        'Date'[Fiscal Quarter Sequence Number]",
                          "            = SELECTEDVALUE ( 'Date'[Fiscal Quarter Sequence Number] )",
                          "            && 'Date'[Fiscal Day Of Quarter Number]",
                          "                <= MAX ( 'Date'[Fiscal Day Of Quarter Number] )",
                          "    )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Quarter Measures"
                      },
                      {
                        "name": "xMeasure QTD LY",
                        "expression": [
                          "CALCULATE (",
                          "    [xMeasure for Data Scenario],",
                          "    FILTER (",
                          "        ALL ( 'Date' ),",
                          "        'Date'[Fiscal Year Number]",
                          "            = SELECTEDVALUE ( 'Date'[Fiscal Year Number] ) - 1",
                          "            && 'Date'[Fiscal Quarter Of Year Number]",
                          "                = SELECTEDVALUE ( 'Date'[Fiscal Quarter Of Year Number] )",
                          "            && 'Date'[Fiscal Day Of Quarter Number]",
                          "                <= MAX ( 'Date'[Fiscal Day Of Quarter Number] )",
                          "    )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Quarter Measures"
                      },
                      {
                        "name": "xMeasure QTD v LY",
                        "expression": [
                          "VAR TY = [xMeasure QTD]",
                          "VAR LY = [xMeasure QTD LY]",
                          "RETURN",
                          "    IF ( ISBLANK ( TY ) || ISBLANK ( LY ), BLANK (), TY - LY )"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Quarter Measures"
                      },
                      {
                        "name": "xMeasure QTD v LY Pct",
                        "expression": [
                          "IF (",
                          "    ISBLANK ( [xMeasure QTD v LY] ),",
                          "    BLANK (),",
                          "    DIVIDE ( [xMeasure QTD v LY], [xMeasure QTD LY], BLANK () )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Quarter Measures"
                      },
                      {
                        "name": "xMeasure YTD",
                        "expression": [
                          "CALCULATE (",
                          "    [xMeasure for Data Scenario],",
                          "    FILTER (",
                          "        ALL ( 'Date' ),",
                          "        'Date'[Fiscal Year Number]",
                          "            = SELECTEDVALUE ( 'Date'[Fiscal Year Number] )",
                          "            && 'Date'[Fiscal Day Of Year Number] <= MAX ( 'Date'[Fiscal Day Of Year Number] )",
                          "    )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Year Measures"
                      },
                      {
                        "name": "xMeasure YTD LY",
                        "expression": [
                          "CALCULATE (",
                          "    [xMeasure for Data Scenario],",
                          "    FILTER (",
                          "        ALL ( 'Date' ),",
                          "        'Date'[Fiscal Year Number]",
                          "            = SELECTEDVALUE ( 'Date'[Fiscal Year Number] ) - 1",
                          "            && 'Date'[Fiscal Day Of Year Number] <= MAX ( 'Date'[Fiscal Day Of Year Number] )",
                          "    )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Year Measures"
                      },
                      {
                        "name": "xMeasure YTD v LY",
                        "expression": [
                          "VAR TY = [xMeasure YTD]",
                          "VAR LY = [xMeasure YTD LY]",
                          "RETURN",
                          "    IF ( ISBLANK ( TY ) || ISBLANK ( LY ), BLANK (), TY - LY )"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Year Measures"
                      },
                      {
                        "name": "xMeasure YTD v LY Pct",
                        "expression": [
                          "IF (",
                          "    ISBLANK ( [xMeasure YTD v LY] ),",
                          "    BLANK (),",
                          "    DIVIDE ( [xMeasure YTD v LY], [xMeasure YTD LY], BLANK () )",
                          ")"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures\\Year Measures"
                      },
                      {
                        "name": "xMeasure for Time Calc",
                        "expression": [
                          "VAR SelectedTimeCalculationKey = ",
                          "    SELECTEDVALUE( 'Time Calculation'[TimeCalculationKey], 1)",
                          "RETURN",
                          "    SWITCH ( SelectedTimeCalculationKey,",
                          "        1, [xMeasure for Data Scenario],",
                          "        2, [xMeasure LY],",
                          "        3, [xMeasure v LY],",
                          "        4, [xMeasure v LY Pct],",
                          "        5, [xMeasure WTD],",
                          "        6, [xMeasure WTD LY],",
                          "        7, [xMeasure WTD v LY],",
                          "        8, [xMeasure WTD v LY Pct],",
                          "        9, [xMeasure PTD],",
                          "        10, [xMeasure PTD LY],",
                          "        11, [xMeasure PTD v LY],",
                          "        12, [xMeasure PTD v LY Pct],",
                          "        13, [xMeasure QTD],",
                          "        14, [xMeasure QTD LY],",
                          "        15, [xMeasure QTD v LY],",
                          "        16, [xMeasure QTD v LY Pct],",
                          "        17, [xMeasure YTD],",
                          "        18, [xMeasure YTD LY],",
                          "        19, [xMeasure YTD v LY],",
                          "        20, [xMeasure YTD v LY Pct]",
                          "    )"
                        ],
                        "formatString": "#,##0.000000;(#,##0.000000)",
                        "displayFolder": "Time Calc Measures"
                      },
                      {
                        "name": "xMeasure",
                        "expression": "[xMeasure for Time Calc]",
                        "formatString": "#,##0.000000;(#,##0.000000)"
                      }
