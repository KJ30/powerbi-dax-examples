SelectedFilterText = 
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
