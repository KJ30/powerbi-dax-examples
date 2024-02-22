SelectedFilterText = 


// Notes :
// Below dax alows you to display a different text messages based on what filters are applied (measure and filter)
// Create a filters table in order to reference inside your report
// Create a measures table in order to reference inside your report 

VAR _filter =
    IF (
        SELECTEDVALUE ( 'Filter'[Filter] ) = "Dollar $",
        " - $ Value / $ Total",
        IF ( SELECTEDVALUE ( 'Filter'[Filter] ) = "Sales %", " - Sales %" )
    )
RETURN
    IF (
        SELECTEDVALUE ( 'Measure'[Measure] ) = "Total Users",
        CONCATENATE (
            FIRSTNONBLANKVALUE ( 'Measure'[Measure], MAX ( 'Measure'[Measure] ) ),
            " - User %"
        ),
        IF (
        SELECTEDVALUE ( 'Measure'[Measure] ) = "Total Deals",
        CONCATENATE (
            FIRSTNONBLANKVALUE ( 'Measure'[Measure], MAX ( 'Measure'[Measure] ) ),
            " - Total Deal %"
        ),
        IF (
            ISFILTERED ( 'Filter'[Filter] ) = FALSE (),
            "Please Select Breakdown",
            IF (
                ISFILTERED ( 'Measure'[Measure] ) = FALSE (),
                "Please Select A Measure",
                CONCATENATE (
                    FIRSTNONBLANKVALUE ( 'Measure'[Measure], MAX ( 'Measure'[Measure] ) ),
                    _filter
                )
            )
        )
    )
    )
