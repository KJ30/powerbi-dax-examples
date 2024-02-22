Metrics = 

// Notes:
// This measure allows a user to select between % or Dollar amount in one measure
// A filter table for the measure type needs to be created in the report

VAR _dollar =
    SELECTEDVALUE ( 'Filter'[Filter] ) = "Dollar $"
VAR _sales =
    SELECTEDVALUE ( 'Filter'[Filter] ) = "Sales %"

VAR _measure =
    IF (
        _sales,
        FORMAT (
            DIVIDE (
                SUM ( 'SalesData'[Sales] ),
                SUM ( 'SalesData'[TotalSales] )
            ),
            "0%"
        ),
        IF (
            _dollar,
            CONCATENATE (
                FORMAT (
                    IF (
                        ISBLANK ( SUM ( 'SalesData'[SalesAmountUSD] ) ),
                        0,
                        SUM ( 'SalesData'[SalesAmountUSD] )
                    ),
                    "$#,##0,,M / "
                ),
                FORMAT ( SUM (  'SalesData'[TotalSalesAmountUSD]), "$#,##0,,M" )
            )
        )
    )

RETURN
   _measure
